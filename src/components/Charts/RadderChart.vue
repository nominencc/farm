<template>
  <div :class="className" style="width:550px;height: 300px;" />
</template>

<script>
import echarts from '/Gitee/cc/vue-element-admin/node_modules/echarts/index'
require('/Gitee/cc/vue-element-admin/node_modules/echarts/theme/macarons') // echarts theme

const animationDuration = 3000

export default {
  name:'RadderChart',
  props: {
    className: {
      type: String,
      default: 'chart'
    },
    width: {
      type: String,
      default: '100%'
    },
    height: {
      type: String,
      default: '300px'
    }
  },
  data() {
    return {
      chart: null
    }
  },
  mounted() {
    this.$nextTick(() => {
      this.initChart()
    })
  },
  beforeDestroy() {
    if (!this.chart) {
      return
    }
    this.chart.dispose()
    this.chart = null
  },
  methods: {
    initChart() {
      this.chart = echarts.init(this.$el, 'macarons')

      this.chart.setOption({
        tooltip: {
          trigger: 'axis',
          axisPointer: { // 坐标轴指示器，坐标轴触发有效
            type: 'shadow' // 默认为直线，可选为：'line' | 'shadow'
          }
        },
        radar: {
          radius: '66%',
          center: ['50%', '42%'],
          //网格数
          splitNumber: 8,
          splitArea: {
            areaStyle: {
              color: 'rgba(127,95,132,.3)',
              opacity: 1,
              shadowBlur: 45,
              shadowColor: 'rgba(0,0,0,.5)',
              shadowOffsetX: 0,
              shadowOffsetY: 15
            }
          },
          indicator: [
            { name: '土壤温度', max: 40 },
            { name: '土壤湿度', max: 70 },
            { name: '土壤PH', max: 9 },
            { name: '土壤氮', max: 350 },
            { name: '土壤磷', max: 500 },
            { name: '土壤钾', max: 1000 },
            { name: '土壤电导率', max: 70 }
          ]
        },
        legend: {
          left: 'center',
          bottom: '10',
          data: ['土地1', '土地2', '土地3'],
          textStyle: {
            color:'#fff'
          }
        },
        series: [{
          type: 'radar',
          symbolSize: 0,
          areaStyle: {
            normal: {
              shadowBlur: 13,
              shadowColor: 'rgba(0,0,0,.2)',
              shadowOffsetX: 0,
              shadowOffsetY: 10,
              opacity: 1
            }
          },
          data: [
            {
              value: [
              (Math.random() * 40.0).toFixed(1), 
              (Math.random() * 65 + 5).toFixed(0), 
              (Math.random() * 5 + 4).toFixed(1), 
              (Math.random() * 300 + 50).toFixed(0), 
              (Math.random() * 480 + 20).toFixed(0),
              (Math.random() * 800 + 200).toFixed(0),
              (Math.random() * 65 + 5).toFixed(0),
              ],
              name: '土地1'
            },
            {
              value: [
              (Math.random() * 40.0).toFixed(1), 
              (Math.random() * 65 + 5).toFixed(0), 
              (Math.random() * 5 + 4).toFixed(1), 
              (Math.random() * 300 + 50).toFixed(0), 
              (Math.random() * 480 + 20).toFixed(0),
              (Math.random() * 800 + 200).toFixed(0),
              (Math.random() * 65 + 5).toFixed(0),
              ],
              name: '土地2'
            },
            {
              value: [
              (Math.random() * 40.0).toFixed(1), 
              (Math.random() * 65 + 5).toFixed(0), 
              (Math.random() * 5 + 4).toFixed(1), 
              (Math.random() * 300 + 50).toFixed(0), 
              (Math.random() * 480 + 20).toFixed(0),
              (Math.random() * 800 + 200).toFixed(0),
              (Math.random() * 65 + 5).toFixed(0),
              ],
              name: '土地3'
            }
          ],
          animationDuration: animationDuration
        }]
      })
    }
  }
}
</script>
