<template>
    <div class="china-wrapper">
        <heder class="data-header">
                顶部数据
        </heder>
        <div class="chinaContent" id="china-content">
            中国地图
        </div>
    </div>
</template>

<script>
import echarts from 'echarts'
export default {    
    name:"china",
    data(){
        return {

        }
    }
}
</script>

<style>
.china-wrapper{
    /* height: calc(100%-80px);
    width: 100%; */
    background: red;
}
</style>