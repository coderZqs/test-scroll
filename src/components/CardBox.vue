<template>
  <div class="card-box">
    <img :src="cardInfo.url" v-if="cardInfo.type === 'image'" :style="{ height: cardInfo.media_height + 'px' }" alt="" style="width: 100%" />
    <video :id="'video-' + index" controls="controls" :src="cardInfo.url" v-else :style="{ height: cardInfo.media_height + 'px' }" alt="" style="width: 100%" @click="handleClick" />
    <div class="text-overflow card-title">{{ cardInfo.title }}</div>
    <div class="tag">{{ cardInfo.tag }}</div>

    <div class="handle-bar">
      <div class="user-info">
        <div class="avatar"></div>
        <span class="nickname">恐龙岛</span>
      </div>

      <div class="like-num">1</div>
    </div>
  </div>
</template>

<script setup lang="ts">
defineProps({
  cardInfo: {
    type: Object,
    required: true,
  },

  index: {
    type: String,
  },
});

const handleClick = () => {
  console.log(31221);
};
</script>

<style scoped>
.card-box {
  overflow: hidden;
  background: white;
  padding: 10px;
  margin: 10px;
}

.text-overflow {
  overflow: hidden;
  text-overflow: ellipsis;
  display: -webkit-box;
  -webkit-box-orient: vertical;
  -webkit-line-clamp: 2;
}

.card-title {
  font-size: 14px;
}

.tag {
  font-size: 12px;
  color: gray;
  margin-top: 4px;
  display: inline-block;
  padding: 2px;
  background: #f6f6f6;
}

.avatar {
  height: 16px;
  width: 16px;
  border-radius: 50%;
  background: lightgray;
}

.user-info {
  display: flex;
  align-items: center;
}

.nickname {
  font-size: 12px;
  color: gray;
  margin-left: 4px;
}

.handle-bar {
  margin-top: 6px;
  display: flex;
  align-items: center;
  justify-content: space-between;
}

.like-num {
  font-size: 13px;
}
</style>
