<template>
  <div class="waterfall-container">
    <div v-for="(lane, index) in lanes" :key="index" class="lane" :style="{ width: laneWidth + '%' }">
      <div v-for="(flowItem, flowIndex) in lane.children" :key="flowIndex" class="item" ref="cardBoxGroup">
        <CardBox :index="index + '-' + flowIndex" :cardInfo="flowItem"></CardBox>
      </div>
    </div>
  </div>
</template>

<script lang="ts" setup>
import CardBox from "./CardBox.vue";
import { onMounted, ref, nextTick, watch, computed } from "vue";
const images = ref([]);
const laneCount = ref(2);

const GAP = 135;

const addRecords = (lists) => {
  images.value = images.value.concat(lists);
};

const cardBoxGroup = ref();

watch(
  () => images.value,
  (e) => {
    e.forEach((v) => {
      // 如果两个高度都为0，则给第一个添加

      if (lanes.value.every((lane) => lane.height === 0)) {
        lanes.value[0].children.push(v);
        lanes.value[0].height += v.media_height + GAP;
      } else {
        const maxIndex = lanes.value.reduce((maxIndex, obj, index, array) => {
          return obj.height < array[maxIndex].height ? index : maxIndex;
        }, 0);

        lanes.value[maxIndex].children.push(v);
        lanes.value[maxIndex].height += v.media_height + GAP;
      }
    });

    nextTick(() => {
      let video = document.querySelectorAll("video");
      video.forEach((v) => {
        v.addEventListener("play", () => {
          video.forEach((ele) => {
            console.log(ele.getAttribute("id"), v.getAttribute("id"));
            if (ele.getAttribute("id") !== v.getAttribute("id")) {
              ele.pause();
            }
          });
        });
      });
    });
  }
);

const lanes = ref(
  new Array(laneCount.value).fill().map(() => {
    return {
      height: 0,
      children: [],
    };
  })
);

const listenVideoOverflow = () => {
  cardBoxGroup.value.forEach((cardBox) => {
    let { top } = cardBox.getBoundingClientRect();
    if (top < 0) {
      let video = cardBox.querySelector("video");
      if (video) {
        video.pause();
      }
    }
  });
};

const refresh = () => {
  images.value = [];
  lanes.value = new Array(laneCount.value).fill().map(() => {
    return {
      height: 0,
      children: [],
    };
  });
};

const laneWidth = computed(() => {
  return 100 / laneCount.value;
});

defineExpose({
  refresh,
  images,
  addRecords,
  listenVideoOverflow,
});
</script>
<style>
.waterfall-container {
  display: flex;
  flex-wrap: wrap;
  background: #f7f6f2;
}

.lane {
  display: flex;
  flex-direction: column;
}

.item {
  box-sizing: border-box;
}
</style>
