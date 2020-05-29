<template>
  <section>
    七大区
  </section>
</template>

<script>
import {createBMap} from "./js/createBMap"

/* 合并生成大区 */
export default {
  name: "Seven",
  data() {
    return {
      ak:"AOgFFy2kmzFlajQcfEmWGXVLEZGqs6dd"
    };
  },
  mounted() {
     createBMap()

    // const BMap = createBMap(this.ak);
    // console.log(BMap,'BMap')
    // if (!window.BMap) {
    // }
    // const ak = "AOgFFy2kmzFlajQcfEmWGXVLEZGqs6dd";
    // var script = document.createElement("script");
    // script.type = "text/javascript";
    // script.src =
    //   "http://api.map.baidu.com/api?v=3.0&ak=" + ak + "&callback=init";
    // // script.onerror = reject;
    // document.head.appendChild(script);

    //  console.log(window);
    // // window.onload = function(){
    // //     console.log(window.BMap);

    // }
  },
  methods: {

  },
};
</script>

<style></style>
