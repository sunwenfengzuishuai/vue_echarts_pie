<template>
    <div style="width:100%;height:100%;z-index:10" ref="pieInComparSelf" id="comparSelf_pie"></div>
</template>

<style scoped>
</style>

<script>
    import echarts from "echarts";

    export default {
        data() {
            return {
                timerId: 0
            };
        },
        mounted() {
            this.mycharts()
        },
        watch: {
            Data: {
                handler(newval, old) {
                    this.mycharts();
                },
                deep: true
            }
        },
        props: {
            Data: {
                type: Object,
                default: () => {
                    return {};
                }
            }
        },
        methods: {
            handleResize() {
                var bar_dv = this.$refs.pieInComparSelf;
                echarts.init(bar_dv).resize();
            },
            mycharts() {
                var _this = this;
                var bar_dv = this.$refs.pieInComparSelf;
                var charts = echarts.init(bar_dv, "shine");

                let option = {
                    // title: {
                    //   text: this.Data.titleText,
                    //   textStyle: {
                    //     color: '#fff'
                    //   }
                    // },
                    grid:{
                        containLabel:true
                    },
                    tooltip: {
                        trigger: 'item',
                        formatter: "{a} <br/>{b} : {c} ({d}%)"
                    },
                    toolbox: {
                        right: 10,
                        show: true,
                        feature: {
                            saveAsImage: {
                                title:'Save as a picture',
                                pixelRatio: 2,
                                backgroundColor: 'transparent'
                            },
                            dataView: {
                                lang: ['DataView','Close'],
                                title:"DataView",
                                readOnly: true,
                                textColor: '#fff',
                                backgroundColor: '#09132B',
                                buttonColor: '#fff',
                                buttonTextColor: '#000',
                                iconStyle: {
                                    color: '#409EFF',
                                    borderColor: '#245632'
                                },

                                optionToContent: function (opt) {
                                    console.log(opt);
                                    let tdHeads = '<td  style="padding: 0 10px ;font-Size:30px">区域</td><td  style="padding: 0 10px;font-Size:30px">产值</td>'; //表头
                                    let tdBodys = ''; //数据
                                    let data = opt.series[0].data
                                    let table = `<table width='100%' border="1" style="border-collapse:collapse;text-align:center;color:#ccc; border:solid"><tbody><tr>${tdHeads} </tr>`;
                                    data.forEach((element, index) => {
                                        tdBodys = `<td style="border:solid 1px">${element.name}</td><td style="border:solid 1px">${element.value}</td>`
                                        table += `<tr>${tdBodys}</tr>`
                                    });
                                    table += '</tbody></table>';
                                    return table;
                                }
                            },
                        }
                    },
                    series: [{
                        name: '',
                        type: 'pie',
                        radius: '50%',
                        center: ['50%', '55%'],
                        clockwise: true,
                        data: this.Data.seriesData,
                        startAngle: 0,
                        label: {

                            normal: {
                                //rotate:180,
                                //align: 'center',

                                position:"outside",
                                formatter:"{b}\n{d}%",
                                textStyle: {
                                    color: '#999',
                                    fontSize: 14,
                                }
                            }
                        },
                        labelLine: {
                            normal: {
                                show: true,
                                smooth:false,
                                // length2:10,
                                // length:10,
                            }
                        },
                        itemStyle: {
                            normal: {
                                // borderWidth: 4,
                                // borderColor: '#ffffff',
                            },
                            emphasis: {
                                borderWidth: 0,
                                shadowBlur: 10,
                                shadowOffsetX: 0,
                                shadowColor: 'rgba(0, 0, 0, 0.5)'
                            }
                        }
                    }],
                    color: [
                        '#00acee',
                        '#52cdd5',
                        '#79d9f1',
                        '#a7e7ff',
                        '#c8efff'
                    ],
                    // backgroundColor: '#fff'
                };
                //清空画布
                charts.clear();
                charts.setOption(option);
                // 屏幕自适应
                this.timerId = setTimeout(function () {
                    window.addEventListener("resize", _this.handleResize);
                }, 200);
            }
        },
        beforeDestroy() {
            clearTimeout(this.timerId);
            window.removeEventListener("resize", this.handleResize);
        }
    };
</script>
