<template>
  <div ref="container"></div>
</template>

<script >
import * as echarts from 'echarts'

export default {
  props: ['option'],
     mounted() {
       const width = document.documentElement.clientWidth
       this.$refs.container.style.width = `${width - 20}px`
       this.$refs.container.style.height = `${(width - 20)*1.6}px`
       this.chart = echarts.init(this.$refs.container,'dark')
       this.chart.setOption(this.option)
     }
}
</script>

<style lang="scss" scoped>

</style>