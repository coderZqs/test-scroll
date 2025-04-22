<script setup lang="ts">
import BetterScroll from "./components/BetterScroll.vue";
import CardBox from "./components/CardBox.vue";
import WaterFall from "./components/WaterFall.vue";
import { ref, onMounted } from "vue";

const bScroll = ref(null);

const resource = [
  { title: "網友狂推超療癒！十大必看動物動畫電影| 遠見雜誌", tag: "必看电影", type: "video", url: "http://vjs.zencdn.net/v/oceans.mp4", media_height: 100 },
  { title: "这些不同形式的大海图片，满足你对那一抹蓝的无限幻想", tag: "海边", type: "image", url: "https://static-cse.canva.cn/blob/239388/e1604019539295.jpg", media_height: 150 },
  { title: "爱图片_爱素材_爱高清图片_摄图网图片下载", tag: "TTTTTTT", type: "image", url: "https://imgs.699pic.com/images/500/465/562.jpg!list1x.v2", media_height: 100 },
  { title: "风景图片大全_风景图片汇总- 三千图片网", tag: "风景", type: "image", url: "https://img.win3000.com/m00/55/8b/6bf1b0e0f4a0f47c7660e80a28363460_c_345_458.jpg", media_height: 300 },
];

const getList = (timer = 2000, isPullDown) => {
  return new Promise((resolve) => {
    setTimeout(() => {
      const list = [];
      const num = isPullDown ? Math.random() * 5 : 5;
      for (let i = 0; i < num; i++) {
        list.push(resource[Math.floor(Math.random() * 4)]);
      }

      if (isPullDown) {
        waterfall.value.refresh();
      }

      waterfall.value.addRecords(list);
      resolve();
    }, timer);
  });
};

const onPullingUp = async () => {
  await getList(1000);
  bScroll.value._setPullUpEndStatus();
};

const onPullingDown = async () => {
  await getList(1000, true);
  bScroll.value._setPullDownEndStatus();
};

const debounce = (fn, wait) => {
  var timeout = null;
  return function () {
    if (timeout !== null) clearTimeout(timeout);
    timeout = setTimeout(fn, wait);
  };
};

const onScroll = debounce(() => {
  waterfall.value.listenVideoOverflow();
}, 500);

const waterfall = ref(null);
</script>

<template>
  <BetterScroll @pullingUp="onPullingUp" @pullingDown="onPullingDown" @scroll="onScroll" ref="bScroll">
    <WaterFall ref="waterfall"></WaterFall>
    <!-- <CardBox v-for="item in lists" :key="item"></CardBox> -->
  </BetterScroll>
</template>

<style scoped></style>
