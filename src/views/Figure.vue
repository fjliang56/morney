<template>
  <layout>
  <div class="chart-wrapper" ref="chartWrapper">
    <Chart class="chart" :options="chartOptions"/>
  </div>
  </layout>
</template>


<script lang="ts">
import {Component} from 'vue-property-decorator';
import Tabs from '@/components/Tabs.vue';
import Vue from 'vue';
import Chart from '@/components/Chart.vue';
import _ from 'lodash';
import day from 'dayjs'
import clone from '@/lib/clone';
import dayjs from 'dayjs';
import recordTypeList from '@/constants/recordTypeList';



@Component({
  components: {Tabs,Chart}
})
export default class Figure extends Vue {
mounted() {
  const div = (this.$refs.chartWrapper as HTMLDivElement);
  div.scrollLeft = div.scrollWidth;
}

get keyValueList() {
  const today = new Date();
  const array = [];
  console.log(this.groupedList);
  for (let i=0; i <= 29; i++) {
    const dateString = day(today)
      .subtract(i,'day').format('YYYY-MM-DD');
    const found = _.find(this.groupedList,{
      title: dateString
    });
    array.push({
      key: dateString,value: found ? found.total : 0
    })
  }
  array.sort((a,b)=>{
    if(a.key > b.key){
      return 1;
    } else if (a.key === b.key) {
      return 0;
    }
    else{
      return -1;
    }
  });
  return array;
}

get chartOptions() {
  const keys = this.keyValueList.map(item => item.key);
  const values = this.keyValueList.map(item => item.value);
    return {
        title: {
          text: '最近30天收入图',
          right: "140px"
        },

        grid: {
          left: 0,
          right: 0,
        },
        xAxis: {
         type: 'category',
          data: keys,
          axisTick: {alignWithLabel: true},
          axisLine: {lineStyle: {color: '#666'}},
          axisLabel: {
           formatter: function (value:string,index:number){
             return value.substr(5)
  }
          }
        },
        yAxis: {
          type: 'value',
          show:false
        },
        series: [{
          symbol: 'circle',
          symbolSize: 12,
          itemStyle: {borderWidth: 1, color: '#666', borderColor: '#666'},
          data: values,
          type: 'line'
        }],
      tooltip: {
          show: true, trigger: 'click',
        position: 'top',
        formatter: '{c}'
      }
      };
  }
  get groupedList() {
    const {recordList} = this;
    if (recordList.length === 0) {
      return [];
    }

    const newList = clone(recordList)
        .filter(r => r.type === this.type)
        .sort((a, b) => dayjs(b.createdAt).valueOf() - dayjs(a.createdAt).valueOf());

    if (newList.length === 0) {
      return [] as Result;
    }
    type Result = { title: string, total?: number, items: RecordItem[] }[]
    const result: Result = [{title: dayjs(newList[0].createdAt).format('YYYY-MM-DD'), items: [newList[0]]}];
    for (let i = 1; i < newList.length; i++) {
      const current = newList[i];
      const last = result[result.length - 1];
      if (dayjs(last.title).isSame(dayjs(current.createdAt), 'day')) {
        last.items.push(current);
      } else {
        result.push({title: dayjs(current.createdAt).format('YYYY-MM-DD'), items: [current]});
      }
    }

    result.map(group => {
      group.total = group.items.reduce((sum, item) => {
        // console.log(sum);
        // console.log(item);
        return sum + item.amount;
      }, 0);
    });
    return result;
  }
  type = '-';
  recordTypeList = recordTypeList;

  get recordList() {
    return (this.$store.state as RootState).recordList;
  }

  beforeCreate() {
    this.$store.commit('fetchRecords');
  }
}


</script>

<style lang="scss" scoped>
 .echarts {
   max-width: 100%;
   height: 400px;
 }
 .chart {
   width: 430%;
   &-wrapper {
     overflow: auto;
     &::-webkit-scrollbar {
       display: none;
     }
   }
 }
</style>