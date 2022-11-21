<template>
  <div class="section">
    <!-- 价格/人数选择 -->
    <div class="price-counter">
      <div class="start">价格不限</div>
      <div class="end">人数不限</div>
    </div>
    <!-- 关键字 -->
    <div class="keyword">关键字/位置/民宿名</div>

    <div class="hot-suggests">
      <template v-for="(city, index) in hotSuggests" :key="index">
        <div class="item" :style="{ color: city.tagText.color, background: city.tagText.background.color }">
          {{ city.tagText.text }}
        </div>
      </template>
    </div>

    <div class="search-btn">
      <div class="btn" @click="searchBtnClick">开始搜索</div>
    </div>
  </div>
</template>

<script setup>
import { useRouter } from 'vue-router';
import useHomeStore from '@/stores/modules/home';
import { storeToRefs } from 'pinia';

const homeStore = useHomeStore()
const { hotSuggests } = storeToRefs(homeStore)

const router = useRouter()

// 开始搜索
const searchBtnClick = () => {
  router.push({
    path: "/search",
    query: {
      a: 123
    }
  })
}
</script>

<style lang="less" scoped>
.price-counter,
.keyword {
  display: flex;
  align-items: center;
  padding: 0 20px;
  color: #999;
  height: 44px;
  border-bottom: 1px solid var(--line-color);

  .start {
    flex: 1;
    display: flex;
    height: 44px;
    align-items: center;
    border-right: 1px solid var(--line-color);
  }

  .end {
    min-width: 30%;
    padding-left: 20px;
  }
}

.hot-suggests {
  display: flex;
  flex-wrap: wrap;
  align-items: center;
  padding: 0 20px;
  margin: 10px 0;

  .item {
    padding: 4px 8px;
    margin: 4px;
    border-radius: 14px;
    font-size: 12px;
  }
}

.search-btn {
  display: flex;
  align-items: center;
  height: 44px;
  padding: 0 20px;
  .btn {
    width: 342px;
    height: 38px;
    font-size: 18px;
    font-weight: 500;
    line-height: 38px;
    text-align: center;
    border-radius: 20px;
    color: #fff;
    background: linear-gradient(90deg, #fa8c1d, #fcaf3f);
  }
}
</style>