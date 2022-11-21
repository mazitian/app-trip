<template>
  <div class="home" ref="homeRef">
    <NavBar></NavBar>
    <Banner></Banner>
    <Location></Location>
    <DateRange></DateRange>
    <Section></Section>
    <Categories></Categories>
    <div class="search-bar" v-if="isShowSearchBar">
      <search-bar />
    </div>
    <Content></Content>
  </div>
</template>


<script>
  export default { name: 'home' }
</script>
<script setup>
import { ref, onActivated, watch } from 'vue'
import NavBar from './cpns/home-nav-bar.vue';
import Banner from './cpns/banner.vue'
import Location from './cpns/location.vue'
import DateRange from './cpns/date-range.vue'
import Section from './cpns/section.vue'
import Categories from './cpns/categories.vue';
import Content from './cpns/content.vue';
import SearchBar from '@/components/search-bar.vue'

import useScroll from '@/hooks/useScroll'
import { computed } from '@vue/reactivity';

// 网络请求
import useHomeStore from '@/stores/modules/home';
const homeStore = useHomeStore()
homeStore.fetchHotSuggestData()
homeStore.fetchCategoriesData()
homeStore.fetchHouselistData()

const homeRef = ref()
const { isReachBottom, scrollTop } = useScroll(homeRef)
watch(isReachBottom, (newValue) => {
  if (newValue) {
    homeStore.fetchHouselistData().then(() => {
      isReachBottom.value = false
    })
  }
})

// 定义的可响应式数据, 依赖另外一个可响应式的数据, 那么可以使用计算函数(computed)
const isShowSearchBar = computed(() => {
  return scrollTop.value >= 360
})

// 保留当前位置
onActivated(() => {
  homeRef.value?.scrollTo({
    top: scrollTop.value
  })
})
</script>

<style lang="less" scoped>
.home {
  height: 100vh;
  overflow-y: auto;
  .search-bar {
    position: fixed;
    z-index: 9;
    top: 0;
    left: 0;
    right: 0;
    height: 45px;
    padding: 16px 16px 10px;
    background-color: #fff;
  }
}
</style>