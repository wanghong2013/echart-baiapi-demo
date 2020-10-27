<template>
    <section class="mapWrapper">
        <article id="map" class="map"></article>
    </section>
</template>

<script>
    /* 合并生成大区 */
    import echarts from "echarts"
    import chinaJson from 'echarts/map/json/china.json'
    export default {
        name: "Seven",
        data() {
            return {
                ak: "AOgFFy2kmzFlajQcfEmWGXVLEZGqs6dd"
            };
        },
        mounted() {
            const This = this;
            // console.log(chinaJson,'china');
            // console.log(typeof chinaJson )
            // this.mergeProvinces(chinaJson, params.names, params.properties)
            window.onload = function () {
                This.repRegionStrategy()
            }
        },
        methods: {
            mergeProvinces(chinaJson, names, properties) {
                var features = chinaJson.features;
                var polygons = [];
                var polygons2 = [];

                for (var i = 0; i < names.length; i++) {
                    var polygonsCoordinates = [];
                    var polygonsEncodeOffsets = [];
                    for (var z = 0; z < names[i].length; z++) {
                        for (var j = 0; j < features.length; j++) {
                            if (features[j].properties.name == names[i][z]) {
                                if (features[j].geometry.coordinates[0].constructor == String) {//合并coordinates
                                    for (var l = 0; l < features[j].geometry.coordinates.length; l++) {
                                        polygonsCoordinates.push(features[j].geometry.coordinates[l]);
                                    }

                                } else if (features[j].geometry.coordinates[0].constructor == Array) {
                                    for (var k = 0; k < features[j].geometry.coordinates.length; k++) {
                                        for (var d = 0; d < features[j].geometry.coordinates[k].length; d++) {
                                            polygonsCoordinates.push(features[j].geometry.coordinates[k][d]);
                                        }
                                    }
                                }


                                if (features[j].geometry.encodeOffsets[0].constructor == String) {//合并encodeOffsets
                                    polygonsEncodeOffsets.push(features[j].geometry.encodeOffsets);
                                } else if (features[j].geometry.encodeOffsets[0].constructor == Array) {
                                    for (var k = 0; k < features[j].geometry.encodeOffsets.length; k++) {
                                        if (features[j].geometry.encodeOffsets[k][0].constructor == Array) {
                                            for (var e = 0; e < features[j].geometry.encodeOffsets[k].length; e++) {
                                                polygonsEncodeOffsets.push(features[j].geometry.encodeOffsets[k][e]);
                                            }
                                        } else {
                                            polygonsEncodeOffsets.push(features[j].geometry.encodeOffsets[k]);
                                        }
                                    }
                                }

                                break;
                            }
                        }
                    }
                    polygons.push(polygonsCoordinates);
                    polygons2.push(polygonsEncodeOffsets);

                }

                // 构建新的合并区域
                var features = [];

                for (var a = 0; a < names.length; a++) {
                    var feature = {
                        id: features.length.toString(),
                        type: "FeatureCollection",
                        geometry: {
                            type: "Polygon",
                            coordinates: polygons[a],
                            encodeOffsets: polygons2[a]
                        },
                        properties: {
                            name: properties.name[a] || "",
                            childNum: polygons[a].length
                        }
                    };
                    if (properties.cp[a]) {
                        feature.properties.cp = properties.cp[a];
                    }

                    // 将新的合并区域添加到地图中
                    features.push(feature);
                }
                chinaJson.features = features;
            },

            repRegionStrategy() {
                var params = {
                    names: [//把各个大区的省份用二维数组分开
                        ['北京', '天津', '河北', '山西', '内蒙古'],
                        ["黑龙江", "吉林", "辽宁"],
                        ['山东', '江苏', '安徽', '江西', '浙江', '福建', '上海', '台湾'],
                        ['河南', '湖北', '湖南'],
                        ['广东', '广西', '海南', '香港', '澳门'],
                        ['重庆', '四川', '云南', '西藏', '贵州'],
                        ['陕西', '甘肃', '青海', '宁夏', '新疆']
                    ],
                    properties: {//自定义大区的名字，要和上面的大区省份一一对应
                        name: ['华北', '东北', '华东', '华中', '华南', '西南', '西北'],
                        cp: [//经纬度可以自己随意定义
                            [116.24, 39.54],
                            [126.32, 43.50],
                            [121.28, 31.13],
                            [114.20, 30.32],
                            [113.15, 23.08],
                            [104.04, 30.39],
                            [103.49, 36.03]
                        ]
                    }
                };

                this.mergeProvinces(chinaJson, params.names, params.properties);

                echarts.registerMap('china', chinaJson); // 注册地图
                var pRChart = echarts.init(document.getElementById('map'));
                var data = [//地图数据
                    {
                        "name": "东北",
                        "value": 3685
                    },
                    {
                        "name": "华北",
                        "value": 7342
                    },
                    {
                        "name": "华南",
                        "value": 21416
                    },
                    {
                        "name": "华东",
                        "value": 25314
                    },
                    {
                        "name": "华中",
                        "value": 2500
                    },
                    {
                        "name": "西南",
                        "value": 10427
                    },
                    {
                        "name": "西北",
                        "value": 2440
                    }
                ];

                var option = {

                    backgroundColor: '#fff',//地图背景色
                    title: {
                        text: '营销策略',
                        left: 'center',
                        top: 'top',
                        textStyle: {
                            color: '#333'
                        }
                    },
                    grid: {    //echarts地图距离容器位置
                        left: 20,
                        top: 20,
                        containLabel: false,
                    },
                    tooltip: {//鼠标放上去提示
                        trigger: 'item'
                    },
                    //左侧小导航图标
                    visualMap: {
                        show: true,
                        type: 'continuous',                     // 定义为连续型 viusalMap
                        min: 10,                                  //指定 visualMapContinuous 组件的允许的最小值
                        max: 100000,
                        minOpen: true,                       //界面上会额外多出一个『< min』的选块
                        maxOpen: true,
                        realtime: true,
                        itemWidth: 20,
                        align: "auto",
                        left: "50px",                              //组件离容器左侧的距离,'left', 'center', 'right','20%'
                        top: "auto",
                        orient: "vertical",
                        hoverLink: true,
                        inverse: false,
                        text: ['高', '低'],
                        inRange: {                               //定义 在选中范围中 的视觉元素
                            color: ['#f6efa6', '#ffb61e', '#ee9a00'],
                            symbolSize: [30, 100]
                        },
                        outOfRange: {  //定义 在选中范围外 的视觉元素。
                            color: ['#666', '#333', '#000'],
                            symbolSize: [30, 100]
                        }
                    },
                    series: [
                        {
                            name: '大区',//城市
                            type: 'map',
                            map: 'china',
                            geoIndex: 1,

                            aspectScale: 0.75, //长宽比
                            symbolSize: 20,
                            data: data,
                            showLegendSymbol: true, // 存在legend时显示
                            label: {
                                normal: {
                                    show: true,
                                    textStyle: {
                                        color: '#333'
                                    }
                                },
                                emphasis: {//鼠标放上去时的状态
                                    show: true,
                                    textStyle: {
                                        color: '#333'
                                    }
                                }
                            },
                            roam: true,//禁止拖拽和伸缩
                            itemStyle: {
                                normal: {
                                    areaColor: '#f6efa6',
                                    borderColor: '#a5a279',
                                    borderWidth: 1
                                },
                                emphasis: {
                                    areaColor: '#ffdf33'
                                }
                            }
                        }
                    ]
                };

                pRChart.setOption(option, true);
            }

        },
    };
</script>

<style lang="less" scoped>
    .mapWrapper {
        width: 100%;
        height: calc(100% - 80px);
    }
    .map{
        width: 100%;
        height: 100%;
    }

</style>
