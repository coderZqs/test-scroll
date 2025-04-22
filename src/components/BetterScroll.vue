<script setup lang="ts">
import { ref } from "vue";
import BScroll from "better-scroll";
import { onMounted } from "vue";

let emits = defineEmits(["pullingUp", "pullingDown", "scroll"]);

const bs = ref(null);
const TIME_BOUNCE = 800;
const REQUEST_TIME = 1000;
const THRESHOLD = 70;
const STOP = 56;
let STEP = 0;

const isPullingDown = ref(false);
const isPullUpLoad = ref(false);
const beforePullDown = ref(true);
const bsWrapper = ref(null);

const initScroll = () => {
  bs.value = new BScroll(bsWrapper.value, {
    scrollY: true,
    bounceTime: TIME_BOUNCE,
    useTransition: false,
    pullUpLoad: true,
    autoPullUpLoad: true,
    // click: true,
    preventDefault: false,
    pullDownRefresh: {
      threshold: THRESHOLD,
      stop: STOP,
    },
  });

  bs.value.on("pullingDown", onPullDown);
  bs.value.on("scroll", onScroll);
  bs.value.on("pullingUp", onPullingUp);
  bs.value.on("enterThreshold", () => {
    beforePullDown.value = true;
  });
};

const _setPullUpEndStatus = () => {
  bs.value.finishPullUp();
  bs.value.refresh();
  isPullUpLoad.value = false;
};

const onScroll = () => {
  emits("scroll");
};

const _setPullDownEndStatus = () => {
  bs.value.finishPullDown();
  bs.value.refresh();
  isPullingDown.value = false;
};

const onPullingUp = async () => {
  isPullUpLoad.value = true;
  emits("pullingUp");
};

const onPullDown = () => {
  beforePullDown.value = false;
  isPullingDown.value = true;

  emits("pullingDown");
};

onMounted(() => {
  initScroll();
  bs.value.autoPullUpLoad();
});

defineExpose({
  _setPullUpEndStatus,
  _setPullDownEndStatus,
});
</script>

<template>
  <div class="pulldown">
    <div class="pulldown-bswrapper" ref="bsWrapper">
      <div class="pulldown-scroller">
        <div class="pulldown-wrapper">
          <div v-show="beforePullDown">
            <span>下拉刷新</span>
          </div>
          <div v-show="!beforePullDown">
            <div v-show="isPullingDown">
              <span>刷新中...</span>
            </div>
            <div v-show="!isPullingDown">
              <span>加载成功</span>
            </div>
          </div>
        </div>

        <slot></slot>

        <div class="pullup-tips">
          <div v-if="!isPullUpLoad" class="before-trigger">
            <span class="pullup-txt">上拉加载更多</span>
          </div>
          <div v-else class="after-trigger">
            <span class="pullup-txt">加载中...</span>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
.pulldown {
  background: #f7f6f2;
  height: 100vh;
  overflow: hidden;
}

.pulldown-bswrapper {
  position: relative;
  height: 100%;
  overflow: hidden;
}

.pulldown-wrapper {
  position: absolute;
  width: 100%;
  padding: 20px;
  box-sizing: border-box;
  transform: translateY(-100%) translateZ(0);
  text-align: center;
  color: #999;
}

.pullup-tips {
  padding: 20px;
  text-align: center;
  color: #999;
}
</style>
