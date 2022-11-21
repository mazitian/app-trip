<template>
  <div class="app">
    <router-view v-slot="props">
      <keep-alive include="home">
        <component :is="props.Component"></component>
      </keep-alive>
    </router-view>
    <TabBar/>
    <Loading></Loading>
  </div>
</template>

<script setup>
import TabBar from "@/components/tab-bar.vue"
import Loading from "./components/loading.vue";
// text1
</script>

<style lang="less" scoped>

</style>
