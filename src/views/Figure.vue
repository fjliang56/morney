<template>
  <layout>
  <div class="wrapper">
    <Chart :options="x"/>
  </div>
  </layout>
</template>


<script lang="ts">
import {Component} from 'vue-property-decorator';
import Tabs from '@/components/Tabs.vue';
import Vue from 'vue';
import Chart from '@/components/Chart.vue';


@Component({
  components: {Tabs,Chart}
})
export default class Figure extends Vue {
get x() {
    return {
        title: {
          text: '收入支出对比图'
        },
        tooltip: {show: true},
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
          data: [5, 20, 36, 10, 10, 20, 50]
        },
          {
            name: '收入',
            type: 'line',
            data: [5, 5, 1, 5, 10, 7, 40]
          }],
      }
  }
}


</script>

<style lang="scss" scoped>
 .echarts {
   max-width: 100%;
 }
</style>