<template>
  <svg
    xmlns="http://www.w3.org/2000/svg"
    :style="{
      left: props.leftOffset + 'px',
      top: props.height - 5 + 'px',
      width: props.widthOffset + 'px',
      height: props.height / 2 + 'px',
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
  height: number;
  leftOffset: number;
  stroke: number;
  widthOffset: number;
}>();

const path = ref("");

onMounted(() => {
  path.value = `M 1.5 6 L ${props.widthOffset - props.stroke} 6 L ${
    props.widthOffset - props.stroke
  } 3`;
});
</script>

<style lang="scss" scoped></style>
