<template>
  <div class="mainMap" ref="myEchart"></div>
</template>
<script>
import { geoCoordMap } from "./geo";
import echarts from "echarts";
import "echarts/map/js/china";
export default {
  name: "Map",
  props: {
    msg: String,
  },
  data() {
    return {
      chart: null,
    };
  },
  mounted() {
    this.initChart();
  },
  beforeDestroy() {
    if (!this.chart) {
      return;
    }
    this.chart.dispose();
    this.chart = null;
  },
  methods: {
    convertData(data) {
      var res = [];
      for (var i = 0; i < data.length; i++) {
        var geoCoord = geoCoordMap[data[i].name];
        if (geoCoord) {
          res.push({
            name: data[i].name,
            value: geoCoord.concat(data[i].area),
            area: data[i].area,
            type: data[i].type,
            rippleEffect: {
              color: "#FEAE21",
              period: 2,
              scale: 2.5,
            },
          });
        }
      }
      console.log(res);
      return res;
    },

    initChart() {
      const data = [
        { name: "上海", area: "华东大区", type: "areaCenterCity" },
        { name: "深圳", area: "华南大区", type: "areaCenterCity" },
        { name: "成都", area: "华西大区", type: "areaCenterCity" },
        { name: "北京", area: "华北大区", type: "areaCenterCity" },
        { name: "武汉", area: "华中大区", type: "areaCenterCity" },
        { name: "沈阳", area: "东北大区", type: "areaCenterCity" },
      ];
      this.chart = echarts.init(this.$refs.myEchart);
      this.chart.setOption({
        backgroundColor: "#222733",
        tooltip: {
          show: false, //不显示提示标签
          formatter: "{b}", //提示标签格式
          backgroundColor: "#ff7f50", //提示标签背景颜色
          textStyle: { color: "#fff" }, //提示标签字体颜色
        },
        grid: {
          left: "10%",
          right: "10%",
          top: "10%",
          bottom: "10%",
          containLabel: true,
        },
        geo: {
          map: "china",
          roam: true,
          zoom: 1,
          tooltip: {
            show: false, //不显示提示标签
          },
          // center:  [116.46, 39.92],
          label: {
            normal: {
              show: false, //显示省份标签
              textStyle: { color: "#c71585" }, //省份标签字体颜色
            },
            emphasis: {
              //对应的鼠标悬浮效果
              show: false,
              textStyle: { color: "#800080" },
            },
          },
          itemStyle: {
            normal: {
              areaColor: "#023677",
              borderColor: "#1180c7",
            },
            emphasis: {
              areaColor: "#4499d0",
            },
          },
          // regions: provienceData,
        },
        series: [
          {
            type: "effectScatter",
            coordinateSystem: "geo",
            data: this.convertData(data),
            symbolSize: 20,
            symbolRotate: 35,
            label: {
              normal: {
                formatter: function (params) {
                  return "地点：" + params.data.name + "\n出现问题";
                },
                position: "top",
                backgroundColor: "#e93f42",
                padding: [10, 10],
                borderRadius: 5,
                lineHeight: 25,
                color: "#ffffff",
                show: true,
              },
              emphasis: {
                show: true,
              },
            },
            tooltip: {
              show: true, //不显示提示标签
              formatter: "{c}", //提示标签格式
              backgroundColor: "#fff", //提示标签背景颜色
              borderColor: "#ccc",
              borderWidth: 0.5,
              textStyle: { color: "#000" }, //提示标签字体颜色
            },
            animationDurationUpdate: 100,
            itemStyle: {
              normal: {
                color: "#FEAE21",
              },
            },
          },
        ],
      });
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.mainMap {
  width: 100vw;
  height: 100vh;
}
</style>