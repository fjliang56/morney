<template>
  <layout>
  <div class="kingdom">
  <Echart :option="option"></Echart>
  </div>
  </layout>
</template>


<script lang="ts">
import Echart from '@/components/Echart.vue';
import dayjs from 'dayjs';

export default {
  data() {
    return {
      option: {
        title: {
          text: '收入支出对比图'
        },
        tooltip: {},
        legend: {
          data: ['支出', '收入']
        },
        xAxis: {
          data: ['周一', '周二', '周三', '周四', '周五', '周六', '周日']
        },
        yAxis: {},
        series: [{
          name: '支出',
          type: 'line',
          data: [5, 20, 36, 10, 10, 20,50]
        },
          {
            name: '收入',
            type: 'line',
            data: [5, 5, 1, 5, 10, 7,40]
          }]
      }
    }
  },
  components: {Echart},
}


</script>

<style lang="scss" scoped>

</style>