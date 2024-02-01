<template>
  <div class="morfana-wrapper">
    <div v-for="(item, index) in arr" :key="index">
      <MorfanaWord
        :wordText="item.wordText"
        :morfana="item.datamorfana"
        :color="item.color"
        :stroke="item.stroke"
        :order="index"
      />
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref } from "vue";
import MorfanaWord from "./MorfanaWord.vue";
const arr = ref([
  {
    wordText:
      '<span style="color: #FF430F; font-weight: 700; letter-spacing: 10px">во</span>до<span style="color: #FF430F; font-weight: 700; letter-spacing: 10px">кач</span>ка',
    datamorfana: "ko:1-3; ko:5-8",
    color: "#FF430F",
    stroke: 2,
  },
  {
    wordText: "птицефабрика",
    datamorfana: "ko:1-3; ko:5-8",
    color: "#FF430F",
    stroke: 2,
  },
  {
    wordText: "северо-запад",
    datamorfana: "ko:1-4;ko:7-11",
    color: "#FF430F",
    stroke: 2,
  },
  {
    wordText: "соцветие",
    datamorfana: "pr:1-2;su:7-7",
    color: "#FF430F",
    stroke: 2,
  },
  {
    wordText: "нарукавник",
    datamorfana: "pr:1-2;su:8-10",
    color: "#FF430F",
    stroke: 2,
  },
  {
    wordText: "подстаканник",
    datamorfana: "pr:1-3;su:10-12",
    color: "#FF430F",
    stroke: 2,
  },
  {
    wordText: "подгруппа",
    datamorfana: "pr:1-3",
    color: "#FF430F",
    stroke: 2,
  },
  { wordText: "правнук", datamorfana: "pr:1-3", color: "#FF430F", stroke: 2 },
  { wordText: "привкус", datamorfana: "pr:1-3", color: "#FF430F", stroke: 2 },
  { wordText: "синеть", datamorfana: "su:4-5", color: "#FF430F", stroke: 2 },
  { wordText: "тишина", datamorfana: "su:4-6", color: "#FF430F", stroke: 2 },
  { wordText: "стрелок", datamorfana: "su:6-7", color: "#FF430F", stroke: 2 },
  { wordText: "глушь", datamorfana: "", color: "#FF430F", stroke: 2 },
  { wordText: "мучное", datamorfana: "", color: "#FF430F", stroke: 2 },
  {
    wordText: "пять",
    datamorfana: "ko:1-4;ok:0;os:1-4",
    color: "#FF430F",
    stroke: 2,
  },
  {
    wordText: "безбилетный",
    datamorfana: "ko:4-8;ok:1-3;  su:9-9; ok:10-11",
    color: "#FF430F",
    stroke: 2,
  },
  {
    wordText: "подберезовик",
    datamorfana: " pr:1-3;  ok:11-12; ko:4-8;su:9-10",
    color: "#FF430F",
    stroke: 2,
  },
  {
    wordText: "лебеде́й",
    datamorfana: "ko:1-5;ok:6-8;os:1-5",
    color: "#FF430F",
    stroke: 2,
  },
  {
    wordText: "себялюбец",
    datamorfana: "  su:8-9;ok:0;  ko:1-3;ok:4-4;ko:5-7; osL:1-3;osR:5-9",
    color: "#FF430F",
    stroke: 2,
  },
  {
    wordText:
      '<span style="color: #FF430F; font-weight: 700; letter-spacing: 10px">десяти</span><span style="color: #FF430F; font-weight: 700; letter-spacing: 10px">этаж</span>н<span style="color: #FF430F; font-weight: 700; letter-spacing: 10px">ый</span>',
    datamorfana: "ko:1-5;ok:6-6;ko:7-10;su:11-11;ok:12-13;osL:1-5;osR:7-11",
    color: "#FF430F",
    stroke: 2,
  },
  {
    wordText:
      '<span style="color: #FF430F; font-weight: 700; letter-spacing: 10px">де</span>сяти<span style="color: #FF430F; font-weight: 700; letter-spacing: 10px">этаж</span>ный',
    datamorfana: "ko:1-5;ok:6-6;ko:7-10;su:11-11;ok:12-13;osL:1-5;osR:7-11",
    color: "#FF430F",
    stroke: 2,
  },
  {
    wordText:
      '<span style="color: #FF430F; font-weight: 700; letter-spacing: 10px">десятиэтажный</span>',
    datamorfana: "ko:1-5;ok:6-6;ko:7-10;su:11-11;ok:12-13;osL:1-5;osR:7-11",
    color: "#FF430F",
    stroke: 2,
  },
  {
    wordText:
      '<span style="color: #FF430F; font-weight: 700; letter-spacing: 10px">д</span>есятиэтажный',
    datamorfana: "ko:1-5;ok:6-6;ko:7-10;su:11-11;ok:12-13;osL:1-5;osR:7-11",
    color: "#FF430F",
    stroke: 2,
  },
  {
    wordText: "десятиэтажный",
    datamorfana: "ko:1-5;ok:6-6;ko:7-10;su:11-11;ok:12-13;osL:1-5;osR:7-11",
    color: "#FF430F",
    stroke: 2,
  },
]);
</script>
