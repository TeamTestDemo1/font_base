<template>
  <div :id="id" :class="className" :style="{height:height,width:width}" />
</template>

<script>
import * as echarts from 'echarts'

export default {
  props: {
    className: {
      type: String,
      default: 'chart'
    },
    id: {
      type: String,
      default: 'chart'
    },
    width: {
      type: String,
      default: '99%'
    },
    height: {
      type: String,
      default: '500px'
    }
  },
  data() {
    return {
      chart: null
    }
  },
  mounted() {
    this.initChart()
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
      this.chart = echarts.init(document.getElementById(this.id))
      const xData = (function() {
        const data = []
        for (let i = 2017; i < 2022; i++) {
          data.push(i + '年')
        }
        return data
      }())
      this.chart.setOption({
        backgroundColor: '#fff', //背景颜色
        //标题调整
        title: {
          text: '历年工作量统计',
          // x: '20',
          top: '20',
          left: 'center',
          textStyle: {
            color: '#fff',
            fontSize: '22'
          }
        },
        //提示框
        tooltip: {
          trigger: 'axis',
          axisPointer: {
            textStyle: {
              color: '#fff'
            }
          }
        },
        //图标距离边框距离
        grid: {
          left: '5%',
          right: '5%',
          borderWidth: 0,
          top: 150,
          bottom: 50,
          textStyle: {
            color: '#fff'
          }
        },
        legend: {
          // x: '5%',
          top: '10%',
          left:'center',
          textStyle: {
            color: '#90979c'
          },
          data: ['课程', '指导学生', '教学总工作量']
        },
        calculable: true,
        // 坐标
        xAxis: [{
          type: 'category',
          axisLine: {
            lineStyle: {
              color: '#90979c'        //x轴颜色
            }
          },
          splitLine: {        //不显示竖线
            show: false
          },
          // axisTick: {
          //     show: false
          // },
          // splitArea: {
          //     show: false
          // },
          // axisLabel: {
          //     interval: 0
          // },
          data: xData
        }],
        yAxis: [{
          type: 'value',
          splitLine: {    //不显示横线
            show: false
          },
          axisLine: {
            show: true,
            lineStyle: {
              // 设置y轴颜色
              // color: '#87CEFA'
              color: '#90979c'
            }
          },
          axisTick: {       //无影响
            show: false
          },
          splitArea: {       //很丑不显示
            show: false
          },
          // min:0, // 设置y轴刻度的最小值
          // max:1800,  // 设置y轴刻度的最大值
          // splitNumber:9 // 设置y轴刻度间隔个数

        }],
        series: [
          {
            name: '课程',
            type: 'bar',
            stack: 'total',
            barMaxWidth: 35,
            barGap: '10%',
            itemStyle: {
              normal: {
                color: 'rgba(255,144,128,1)',
                label: {
                  show: true,
                  textStyle: {
                    color: '#fff'
                  },
                  position: 'insideTop',
                  formatter(p) {
                    return p.value > 0 ? p.value : ''
                  }
                }
              }
            },
            data: [
              709,
              1917,
              2455,
              2610,
              1719,
            ]
          },
          {
            name: '指导学生',
            type: 'bar',
            stack: 'total',
            itemStyle: {
              normal: {
                color: 'rgba(0,191,183,1)',
                barBorderRadius: 0,
                label: {
                  show: true,
                  position: 'top',
                  formatter(p) {
                    return p.value > 0 ? p.value : ''
                  }
                }
              }
            },
            data: [
              327,
              1776,
              507,
              1200,
              800,
            ]
          }, {
            name: '教学总工作量',
            type: 'line',
            stack: 'total',
            symbolSize: 10,
            symbol: 'circle',
            itemStyle: {
              normal: {
                color: 'rgba(252,230,48,1)',
                barBorderRadius: 0,
                label: {
                  show: true,
                  position: 'top',
                  formatter(p) {
                    return p.value > 0 ? p.value : ''
                  }
                }
              }
            },
            data: [
              1036,
              3693,
              2962,
              3810,
              2519,
            ]
          }
        ]
      })
    }
  }
}
</script>
