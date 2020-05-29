<template>
  <div class="china-wrapper">
    <header class="data-header">
      <Row>
        <Col
          :xs="{ span: 6, offset: 2 }"
          :lg="{ span: 6, offset: 2 }"
          :style="{ background: 'red' }"
        >
          <h3>1110</h3>
          <h4>年检报警</h4>
        </Col>
        <Col
          :xs="{ span: 6, offset: 2 }"
          :lg="{ span: 6, offset: 2 }"
          :style="{ background: 'red' }"
        >
          <h3>1110</h3>
          <h4>合同报警</h4>
        </Col>
        <Col
          :xs="{ span: 6, offset: 2 }"
          :lg="{ span: 6, offset: 2 }"
          :style="{ background: 'red' }"
        >
          <h3>1110</h3>
          <h4>付款报警</h4>
        </Col>
      </Row>
    </header>
    <div class="baidu-warpper">
      <div class="map" id="map"></div>
    </div>
  </div>
</template>
<script>
import { Row, Col } from "view-design";
import { MP } from "./js/map";
export default {
  name: "BaiduMap",
  props: {
    mapDivId: {
      default: "mapcontainer",
    },
    centerMapPoint: {
      type: Object,
      default: function() {
        return {
          longitude: 106.530635013,
          latitude: 29.5446061089,
        };
      },
    },
  },
  data() {
    return {
      ak: "AOgFFy2kmzFlajQcfEmWGXVLEZGqs6dd", // @yuanzhuang 账号申请
      chongqingPoint: {
        //重庆经纬度
        longitude: 106.530635013,
        latitude: 29.5446061089,
      },
      points: [],
      markers: [],
      searchValue: "",
    };
  },
  computed: {},

  created() {},
  mounted() {
    this.$nextTick(function() {
      const _this = this;
      MP(_this.ak).then((BMap) => {
        // console.log(bmp, "bmp");
        var mp = new BMap.Map("map");
        mp.centerAndZoom(
          new BMap.Point(
            _this.centerMapPoint.longitude,
            _this.centerMapPoint.latitude
          ),
          1
        );
        //禁止滚轮放大缩小
        mp.disableScrollWheelZoom();
        //禁用双击放大
        mp.disableDoubleClickZoom();
        function getBoundary() {
          var bdary = new BMap.Boundary();
          bdary.get("重庆", function(rs) {
            //获取行政区域
             mp.clearOverlays(); //清除地图覆盖物
            var count = rs.boundaries.length; //行政区域的点有多少个
            if (count === 0) {
              alert("未能获取当前输入行政区域");
              return;
            }
            var pointArray = [];
            for (var i = 0; i < count; i++) {
              var ply = new BMap.Polygon(rs.boundaries[i], {
                strokeWeight:3,
                strokeColor: "#ff0000",
              }); //建立多边形覆盖物
              mp.addOverlay(ply); //添加覆盖物
              pointArray = pointArray.concat(ply.getPath());
            }
            mp.setViewport(pointArray); //调整视野
            // addlabel();
          });
        }

        setTimeout(function() {
          getBoundary();
        }, 2000);
      });
    });
  },
  methods: {},
};
</script>
<style lang="less" scoped>
.china-wrapper {
  display: flex;
  height: calc(100% - 80px);
  flex-direction: column;
  width: 100%;
  .data-header {
    overflow: hidden;
  }
  .baidu-warpper {
    flex: 1;
    .map {
      width: 100%;
      height: 100%;
    }
  }
}
</style>
