<template>
  <div class="date-range" @click="showCalendar = true">
    <div class="start">
      <div class="date">
        <span class="tip">入住</span>
        <span class="time">{{ startDateStr }}</span>
      </div>
      <div class="stay">共{{ stayCount }}晚</div>
    </div>
    <div class="end">
      <div class="date">
        <span class="tip">离店</span>
        <span class="time">{{ endDateStr }}</span>
      </div>
    </div>
  </div>
  <van-calendar 
    v-model:show="showCalendar" 
    type="range" 
    color="#ff9854" 
    :round="false" 
    :show-confirm="false"
    @confirm="onConfirm" 
    style="--van-calendar-popup-height: 100%" 
  />
</template>

<script setup>
import { ref } from 'vue';
import { storeToRefs } from 'pinia';
import { formatMonthDay, getDiffDays } from "@/utils/format_date"
import useMainStore from '@/stores/modules/main';
import { computed } from '@vue/reactivity';

// 日期范围的处理
const mainStore = useMainStore()
const { startDate, endDate } = storeToRefs(mainStore)

const startDateStr = computed(() => formatMonthDay(startDate.value))
const endDateStr = computed(() => formatMonthDay(endDate.value))
const stayCount = ref(getDiffDays(startDate.value, endDate.value))

// 日历
const showCalendar = ref(false)

const onConfirm = (value) => {
  // 1.设置日期
  const selectStartDate = value[0]
  const selectEndDate = value[1]
  mainStore.startDate = selectStartDate
  mainStore.endDate = selectEndDate
  stayCount.value = getDiffDays(selectStartDate, selectEndDate)

  // 2.隐藏日历
  showCalendar.value = false
}
</script>

<style lang="less" scoped>
.date-range {
  display: flex;
  height: 44px;
  align-items: center;
  padding: 0 20px;
  height: 44px;
  border-bottom: 1px solid var(--line-color);

  .start {
    flex: 1;
    height: 44px;
    display: flex;
    align-items: center;

    .stay {
      flex: 1;
      text-align: center;
      font-size: 12px;
      color: #666;
    }
  }

  .date {
    display: flex;
    flex-direction: column;

    .tip {
      font-size: 12px;
      color: #999;
    }

    .time {
      margin-top: 3px;
      color: #333;
      font-size: 15px;
      font-weight: 500;
    }
  }

  .end {
    min-width: 30%;
    padding-left: 20px;
  }
}
</style>