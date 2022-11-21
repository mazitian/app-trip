<template>
  <div class="tab-bar">
    <van-tabbar 
      v-model="currentIndex" 
      active-color="#ff9854" 
      route
    >
      <template v-for="(item, index) in tabbarData">
        <van-tabbar-item :to="item.path">
          <span>{{ item.text }}</span>
          <template #icon>
            <img v-if="currentIndex !== index" :src="getAssetURL(item.image)" alt="">
            <img v-else :src="getAssetURL(item.imageActive)" alt="">
          </template>
        </van-tabbar-item>
      </template>
    </van-tabbar>
  </div>
</template>

<script setup>
import { getAssetURL } from "@/utils/load_assets.js"
import { ref, watch } from "vue";
import { useRoute } from "vue-router";

// 监听路由改变时, 找到对应的索引, 设置currentIndex
const route = useRoute()
const currentIndex = ref(0)
watch(route, (newRoute) => {
  const index = tabbarData.findIndex(item => item.path === newRoute.path)
  if (index === -1) return
  currentIndex.value = index
})

const tabbarData = [
  {
    text: "首页",
    image: "tabbar/tab_home.png",
    imageActive: "tabbar/tab_home_active.png",
    path: "/home"
  },
  {
    text: "收藏",
    image: "tabbar/tab_favor.png",
    imageActive: "tabbar/tab_favor_active.png",
    path: "/favor"
  },
  {
    text: "订单",
    image: "tabbar/tab_order.png",
    imageActive: "tabbar/tab_order_active.png",
    path: "/order"
  },
  {
    text: "消息",
    image: "tabbar/tab_message.png",
    imageActive: "tabbar/tab_message.png",
    path: "/message"
  }
]

</script>

<style lang="less" scoped>
.tab-bar {
  // 局部定义一个变量: 只针对.tab-bar子元素才生效
  // --van-tabbar-item-icon-size: 30px !important;

  // 找到类, 对类中的CSS属性重写
  // :deep(.class)找到子组件的类, 让子组件的类也可以生效
  :deep(.van-tabbar-item__icon) {
    font-size: 50px;
  }

  img {
    height: 26px;
  }
}

</style>
