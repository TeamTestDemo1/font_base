<template>
    <div :id="id" :class="className" :style="{height:height,width:width}" />
</template>

<script>
    import * as echarts from 'echarts'

    export default {
        name :"mixcharts",
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
                default: '810px'
            },
            height: {
                type: String,
                default: '400px'
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
                    backgroundColor: '#fff',
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
                        left: '10%',
                        right: '5%',
                        borderWidth: 0,
                        top: 150,
                        bottom: 50,
                        textStyle: {
                            color: '#fff'
                        }
                    },
                    // 标签
                    legend: {
                      x: '5%',
                      top: '10%',
                      textStyle: {
                        color: '#90979c'
                      },
                      data: ['教学', '科研', '总工作量']
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
                    //范围拖动   太累赘了感觉
                    // dataZoom: [{
                    //   show: true,
                    //   height: 30,
                    //   xAxisIndex: [
                    //     0
                    //   ],
                    //   bottom: 30,
                    //   start: 10,
                    //   end: 80,
                    //   handleIcon: 'path://M306.1,413c0,2.2-1.8,4-4,4h-59.8c-2.2,0-4-1.8-4-4V200.8c0-2.2,1.8-4,4-4h59.8c2.2,0,4,1.8,4,4V413z',
                    //   handleSize: '110%',
                    //   handleStyle: {
                    //     color: '#d3dee5'
                    //
                    //   },
                    //   textStyle: {
                    //     color: '#fff' },
                    //   borderColor: '#90979c'
                    //
                    // }, {
                    //   type: 'inside',
                    //   show: true,
                    //   height: 15,
                    //   start: 1,
                    //   end: 35
                    // }],
                    series: [{
                        name: '教学',
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
                            name: '科研',
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
                            name: '总工作量',
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
