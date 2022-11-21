<template>
  <div class="content">
    <div class="title">热门精选</div>
    <div class="list">
      <template v-for="(item, index) in houselist" :key="item.data.houseId">
        <HouseItemV9 v-if="item.discoveryContentType === 9" :item-data="item.data" @click="itemClick(item.data)"></HouseItemV9>
        <HouseItemV3 v-else="item.discoveryContentType === 3" :item-data="item.data" @click="itemClick(item.data)"></HouseItemV3>
      </template>
    </div>
  </div>
</template>

<script setup>
import HouseItemV3 from '@/components/house-item-v3.vue';
import HouseItemV9 from '@/components/house-item-v9.vue';
import useHomeStore from '@/stores/modules/home';
import { storeToRefs } from 'pinia';
import { useRouter } from 'vue-router';

const homeStore = useHomeStore()
const { houselist } = storeToRefs(homeStore)

// 函数在组件的跟元素上触发 所以有多个跟元素要明确指定$attr
const router = useRouter()
const itemClick = (item) => {
  router.push("/detail/" + item.houseId)
}
</script>

<style lang="less" scoped>
.content {
  padding: 10px 8px;
  margin-bottom: 40px;
  .title {
    font-size: 22px;
    padding: 10px;
  }
  .list {
    display: flex;
    flex-wrap: wrap;
  }
}
</style>