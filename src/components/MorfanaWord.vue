<template>
  <div class="morfana-top"></div>
  <span :style="{ position: 'relative' }" class="morfana-wrapper__word">
    <span
      v-html="props.wordText"
      class="morfana-word"
      ref="word"
      :data-morfana-markup="props.morfana"
    ></span>
    <component
      v-for="morf in keyMap"
      :key="morf.id"
      :is="comparArr[morf.key]"
      :leftOffset="morf.left"
      :widthOffset="morf.width"
      :color="props.color"
      :stroke="props.stroke"
      :height="heightWord"
      :mobile="mobile"
    />
  </span>
</template>

<script setup lang="ts">
import isMobile from "ismobilejs";
import { computed, onMounted, ref, shallowRef } from "vue";

import Ko from "./morfanaModels/Ko.vue";
import Ok from "./morfanaModels/Ok.vue";
import Os from "./morfanaModels/Os.vue";
import Osl from "./morfanaModels/Osl.vue";
import Osr from "./morfanaModels/Osr.vue";
import Pr from "./morfanaModels/Pr.vue";
import Su from "./morfanaModels/Su.vue";
const word = ref<HTMLDivElement>();
const heightWord = ref(0);
const widthtWord = ref(0);
const markup = ref();
//разбили строку в массив
const markupArray = ref([]);

const key = ref("");
const id = ref("");

//флаг наличия окончания при разборе слова
const checkOk = ref(false);

//подсчет количества вхождений в окончание
const countEnd = ref(0);

//для смещения из-за паддингов у окончания
const leftOk = ref(0);

//стилизованный контейнер
const currentContainer = ref<HTMLElement>();
const styledContainer = ref(Array<HTMLElement>());

const attributeContainer = ref(Array<IAttribute>());

//массив сопоставления

const comparArr: { [key: string]: any } = shallowRef({
  ko: Ko,
  ok: Ok,
  os: Os,
  osL: Osl,
  osR: Osr,
  pr: Pr,
  su: Su,
});

interface IAttribute {
  end: number;
  start: number;
}

interface IKeyMap {
  coordEnd: number;
  coordStart: number;
  id: string;
  key: string;
  left: number;
  width: number;
}

interface IWordMap {
  end: number;
  key: string;
  start: number;
}

//Данные для рендеринга морфем
const keyMap = ref(Array<IKeyMap>());

const getWordMap = ref(Array<IWordMap>());

const props = defineProps<{
  color: string;
  morfana: string;
  order: number;
  stroke: number;
  wordText: string;
}>();

onMounted(() => {
  document.fonts.ready.then(() => {
    // в сафари все равно без timeout ломается
    setTimeout(() => {
      init();
    }, 200);
  });
});

function init() {
  if (word.value) {
    if (word.value.attributes) {
      markup.value = word.value.attributes.getNamedItem("data-morfana-markup");
    }
    markupArray.value = markup.value.value.split(" ").join("").split(";");
    //сортировка
    markupArray.value.sort((a: string, b: string) => {
      const numA = parseInt(a.split(":")[1].split("-")[0]);
      const numB = parseInt(b.split(":")[1].split("-")[0]);

      if (numA === 0) return 1; // Переместить элемент с 0 в конец
      if (numB === 0) return -1; // Переместить элемент с 0 в конец

      return numA - numB;
    });

    addAttribute();

    getWord();
    //проверка контейнера с инлайн стилями
    getStyledContainer();
    widthtWord.value = word.value.offsetWidth;
    heightWord.value = word.value.offsetHeight;
  }

  markupArray.value.forEach((item: string) => {
    const index = item.indexOf(":");
    const k = item.slice(0, index);
    if (index !== -1) {
      id.value = getId();
      key.value = k;
      getPosition(item, index, k);
    }
  });
}

const mobile = computed(() => {
  return isMobile().phone;
});

function getId() {
  return "xxxxxxxx-xxxx-4xxx-yxxx-xxxxxxxxxxxx".replace(/[xy]/g, function (c) {
    const r = (Math.random() * 16) | 0,
      v = c === "x" ? r : (r & 0x3) | 0x8;
    return v.toString(16);
  });
}

function getPosition(item: string, index: number, k: string) {
  const position = item.substring(index + 1);
  calculatePosition(position, k);
}

function calculatePosition(position: string, k: string) {
  const coord = position.split("-");
  const startCoord = Number(coord[0]);
  const endCoord = Number(coord[1]);
  let widthOffset = 0;
  let leftOffset = 0;

  const text = word.value?.textContent;

  if (word.value && text) {
    for (let i = 0; i < text.length; i++) {
      const letter = text[i];
      const span = document.createElement("span");
      span.textContent = letter;
      //прохожусь по стилизованным контейнерам
      styledContainer.value.forEach((item: HTMLElement) => {
        const max = Number(item.attributes.getNamedItem("data-max")?.value);
        const min = Number(item.attributes.getNamedItem("data-min")?.value);
        if (i >= min && i <= max) {
          currentContainer.value = item;
        }
      });
      currentContainer.value
        ? currentContainer.value.appendChild(span)
        : word.value.appendChild(span);
      const rect = span.getBoundingClientRect();
      const width = rect.width;
      currentContainer.value
        ? currentContainer.value.removeChild(span)
        : word.value.removeChild(span);

      if (i + 1 >= startCoord && i + 1 <= endCoord) {
        widthOffset += width;
      }
      if (i + 1 < startCoord) {
        leftOffset += width;
      }
      currentContainer.value = undefined;
    }
  }

  //дополнительные расчет из-за окончания

  if (k === "ok") {
    countEnd.value += 1;
    const el = document.querySelector(
      `.morfana-word__inner${startCoord}-${props.order}`
    ) as HTMLElement;
    if (el) {
      const computedStyles = window.getComputedStyle(el);
      const paddingLeft = parseFloat(
        computedStyles.getPropertyValue("padding-left")
      );
      const paddingRight = parseFloat(
        computedStyles.getPropertyValue("padding-right")
      );
      leftOk.value += paddingLeft + paddingRight;
    }
  }

  if (startCoord > 1) {
    if (
      countEnd.value === 1 &&
      k === "ok" &&
      markupArray.value.length - 1 !== keyMap.value.length
    ) {
      leftOffset += leftOk.value / 2;
    } else {
      leftOffset += leftOk.value;
    }
  }

  if (startCoord === 0) {
    leftOffset = widthtWord.value + 5;
    widthOffset = 25;
  }

  if (keyMap.value)
    keyMap.value.push({
      coordEnd: endCoord,
      coordStart: startCoord,
      id: id.value,
      key: key.value,
      left: leftOffset,
      width: widthOffset,
    });
}

function getWord() {
  markupArray.value.forEach((item: string) => {
    const index = item.indexOf(":");
    const k = item.slice(0, index);
    const position = item.substring(index + 1);
    const coord = position.split("-");
    const startCoord = Number(coord[0]);
    const endCoord = Number(coord[1]);
    if (getWordMap.value) {
      getWordMap.value.push({
        end: endCoord,
        key: k,
        start: startCoord,
      });
    }
  });

  let newText: string;

  getWordMap.value.forEach((k) => {
    const text = newText ? newText : props.wordText;
    if (k.key === "ok" && word.value && word.value?.textContent !== null) {
      checkOk.value = true;
      const cutText = word.value.textContent.slice(k.start - 1, k.end);
      let cutWrapper;
      if (k.start === 0) {
        return;
      }
      if (k.start === 1) {
        cutWrapper =
          `<span class="morfana-word__inner start morfana-word__inner${k.start}-${props.order}">` +
          cutText +
          "</span>";
      } else if (k.end === word.value.textContent.length) {
        cutWrapper =
          `<span class="morfana-word__inner end morfana-word__inner${k.start}-${props.order}">` +
          cutText +
          "</span>";
      } else {
        cutWrapper =
          `<span class="morfana-word__inner between morfana-word__inner${k.start}-${props.order}">` +
          cutText +
          "</span>";
      }

      newText = text.replace(cutText, cutWrapper);
      if (word.value) {
        word.value.innerHTML = newText;
      }
    }
    if (!checkOk.value) {
      if (word.value) {
        word.value.innerHTML = props.wordText;
      }
    }
  });
}

function getStyledContainer() {
  //проверка контейнера с инлайн стилями
  if (word.value) {
    const childElements = word.value.querySelectorAll("*");
    const childArray: Element[] = Array.from(childElements);
    for (let i = 0; i < childArray.length; i++) {
      const childElement = childElements[i] as HTMLElement;

      if (childElement.style.length > 0) {
        styledContainer.value.push(childElement);
      }
    }

    for (let i = 0; i < styledContainer.value.length; i++) {
      styledContainer.value[i].setAttribute(
        "data-min",
        String(attributeContainer.value[i].start)
      );
      styledContainer.value[i].setAttribute(
        "data-max",
        String(attributeContainer.value[i].end)
      );
    }
  }
}

function addAttribute() {
  if (word.value && word.value.textContent !== null) {
    const text = word.value.innerHTML;

    const pattern = /<span style>(.*?)<\/span>|[^<]+/g;
    const matches = text.match(pattern);

    let count = 0;

    if (matches !== null) {
      for (const match of matches) {
        if (match.startsWith("span style")) {
          const indexOfGreaterThan = match.indexOf(">");
          if (indexOfGreaterThan !== -1) {
            // Вырезаем текст после ">"
            const extractedText = match.substring(indexOfGreaterThan + 1);
            if (extractedText) {
              const start = count;
              count += extractedText.length;
              const end = count - 1;
              attributeContainer.value.push({
                end: end,
                start: start,
              });
            }
          }
        } else {
          const indexOfGreaterThan = match.indexOf(">");
          if (indexOfGreaterThan !== -1) {
            // Вырезаем текст после ">"
            const extractedText = match.substring(indexOfGreaterThan + 1);
            if (
              extractedText.length &&
              extractedText.length !== word.value.textContent.length
            ) {
              count += extractedText.length;
            }
          }
        }
      }
    }
  }
}
</script>
<style lang="scss">
.morfana-word__inner {
  padding-left: 3px;
  padding-right: 3px;
  &.start {
    padding-left: 0px;
  }
  &.end {
    padding-right: 0px;
  }
}
.morfana-top {
  height: 15px;
}
</style>
