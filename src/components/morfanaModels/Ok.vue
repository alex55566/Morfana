<template>
  <svg
    xmlns="http://www.w3.org/2000/svg"
    :style="{
      left: props.leftOffset - 2 + 'px',
      top: -2 + 'px',
      width: props.widthOffset + 4 + 'px',
      height: props.height + 'px',
      overflow: 'visible',
      position: 'absolute',
      pointerEvents: 'none',
    }"
  >
    <rect
      :style="{
        stroke: props.color,
        strokeWidth: props.stroke,
        width: props.widthOffset + 4 + 'px',
        height: props.height + 'px',
      }"
      fill="transparent"
      fill-opacity="0"
    ></rect>
  </svg>
</template>

<script setup lang="ts">
import { onMounted, ref } from "vue";
const props = defineProps<{
  color: string;
  height: number;
  leftOffset: number;
  stroke: number;
  widthOffset: number;
}>();

const path = ref("");

onMounted(() => {
  path.value = `M 2 23.2 C ${props.widthOffset / 3} 10.08, ${
    (props.widthOffset / 3) * 2
  } 10.08 ${props.widthOffset - props.stroke} 23.2`;
});
</script>

<style lang="scss" scoped></style>
