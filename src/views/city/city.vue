<template>
  <div class="city top-page">
    <div class="top">
      <!-- 1、搜索框 -->
      <van-search v-model="searchValue" placeholder="城市/区域/位置" shape="round" show-action @cancel="cancelClick" />
      <!-- 2、tab切换 -->
      <van-tabs v-model:active="tabActive" color="#ff9854" line-height="3">
        <template v-for="(value, key, index) in allCities" :key="key">
          <van-tab :title="value.title" :name="key"></van-tab>
        </template>
      </van-tabs>
    </div>
    <!-- 3、城市内容 -->
    <div class="content">
      <template v-for="(value, key, index) in allCities">
        <CityGroup v-show="tabActive === key" :group-data="value"></CityGroup>
      </template>
    </div>
  </div>
</template>

<script setup>
import { useRouter } from 'vue-router';
import { storeToRefs } from 'pinia';
import { ref } from 'vue';
import CityGroup from "./cpns/group.vue"

// 1、从store中引入useCityStore
import useCityStore from '@/stores/modules/city';
// 2、调用useSityStore
const cityStore = useCityStore()
// 3、从cityStore中调用获取数据方法
cityStore.fetchAllCitiesData()
// 4、储存获取的数据
const { allCities } = storeToRefs(cityStore)

// tab的切换
const tabActive = ref()

const router = useRouter()

// 搜索框功能
const searchValue = ref("")
const cancelClick = () => {
  router.back()
}
</script>

<style lang="less" scoped>

</style>