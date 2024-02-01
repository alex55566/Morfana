<template>
  <svg
    xmlns="http://www.w3.org/2000/svg"
    :style="{
      left: props.leftOffset + 'px',
      top: props.mobile ? -19.42 + 'px' : -21.42 + 'px',
      width: props.widthOffset + 'px',
      height: props.mobile ? 23.42 + 'px' : 25.2 + 'px',
      position: 'absolute',
      pointerEvents: 'none',
    }"
  >
    <path
      :d="path"
      :style="{ stroke: props.color, strokeWidth: props.stroke }"
      fill="transparent"
      fill-opacity="0"
    ></path>
  </svg>
</template>

<script setup lang="ts">
import { onMounted, ref } from "vue";
const props = defineProps<{
  color: string;
  leftOffset: number;
  mobile: boolean;
  stroke: number;
  widthOffset: number;
}>();

const path = ref("");

onMounted(() => {
  path.value = `M 2 23.2 L ${props.widthOffset / 2} 12.6 L ${
    props.widthOffset - props.stroke
  } 23.2`;
});
</script>

<style lang="scss" scoped></style>
