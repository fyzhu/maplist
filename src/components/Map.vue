<template>
  <div class="map">
    <!-- <Search :list="list" @hover="handleHover" @mouseOut="handleMouseout" @clickItem="handleClick"/> -->
    <div id="container"></div>
  </div>
</template>

<script>
import AMapLoader from "@amap/amap-jsapi-loader";
import axios from "axios";
import Search from "./Search.vue";
export default {
  name: "Map",
  props: {},
  components: {
    Search,
  },
  data() {
    return {
      map: null,
      list: [
        {
          title: "协和医院",
        },
        {
          title: "同仁医院",
        },
        {
          title: "宣武医院",
        },
        {
          title: "北京友谊医院",
        },
        {
          title: "北京大学第一医院",
        },
        {
          title: "北京大学人民医院",
        },
        {
          title: "北京大学第三医院",
        },
        {
          title: "北京积水潭医院",
        },
        {
          title: "广安门医院",
        },
        {
          title: "北京朝阳医院",
        },
        {
          title: "中日友好医院",
        },
        {
          title: "北京大学首钢医院",
        },
        {
          title: "首都医科大学附属北京中医医院",
        },
        {
          title: "首都医科大学附属北京天坛医院",
        },
        {
          title: "北京世纪坛医院（北京铁路总医院）",
        },
        {
          title: "北京市健宫医院",
        },
        {
          title: "北京市房山区良乡医院",
        },
        {
          title: "北京医院",
          type: "new",
        },
        {
          title: "首都医科大学附属北京安贞医院",
          type: "new",
        },
        {
          title: "首都医科大学附属北京潞河医院",
          type: "new",
        },
        {
          title: "国家电网公司北京电力医院",
          type: "new",
        },
        {
          title: "中国医科大学航空总医院",
          type: "new",
        },
        {
          title: "北京市海淀医院",
          type: "new",
        },
        {
          title: "北京市垂杨柳医院",
          type: "new",
        },
        {
          title: "北京市昌平区医院",
          type: "new",
        },
        {
          title: "北京市顺义区医院",
          type: "new",
        },
        {
          title: "北京市平谷区医院",
          type: "new",
        },
        {
          title: "北京市密云区医院",
          type: "new",
        },
        {
          title: "北京市延庆区医院",
          type: "new",
        },
        {
          title: "北京怀柔医院",
          type: "new",
        },
        {
          title: "北京市大兴区人民医院",
          type: "new",
        },
        {
          title: "北京市石景山医院",
          type: "new",
        },
      ],
    };
  },
  mounted() {
    this.createView();
  },
  methods: {
    handleClick(item) {
      console.log(item.title);
    },
    handleHover(item) {
      console.log(item.title);
      var overlays = this.map.getAllOverlays("marker");
      console.log(overlays);
      var position =
        overlays[
          this.list.findIndex((item2) => item2.title == item.title)
        ].getPosition();
      // console.log(position);
      this.infoWindow.setContent(item.title);
      this.infoWindow.open(this.map, position);
      console.log(this.infoWindow);
    },
    handleMouseout() {
      this.infoWindow.close();
    },
    async createView() {
      AMapLoader.load({
        key: "308c528afc2ff841fbc0cbfe51e925d6", // 申请好的Web端开发者Key，首次调用 load 时必填
        version: "2.0", // 指定要加载的 JSAPI 的版本，缺省时默认为 1.4.15
        plugins: ["AMap.Geocoder", "AMap.Geolocation", "AMap.ToolBar"], // 需要使用的的插件列表，如比例尺'AMap.Scale'等
        AMapUI: {
          // 是否加载 AMapUI，缺省不加载
          version: "1.1", // AMapUI 缺省 1.1
          plugins: [], // 需要加载的 AMapUI ui插件
        },
        // Loca: {
        //   // 是否加载 Loca， 缺省不加载
        //   // version: "1.3.2", // Loca 版本，缺省 1.3.2
        // },
      })
        .then(async (AMap) => {
          this.infoWindow = new AMap.InfoWindow({
            offset: new AMap.Pixel(0, -35),
            content: "", //使用默认信息窗体框样式，显示信息内容
          });
          this.map = new AMap.Map("container", {
            zoom: 13, //级别
            // center: [116.397428, 39.90923],//中心点坐标
            // viewMode:'3D'//使用3D视图
          });
          // 缩放工具条
          var tool = new AMap.ToolBar();
          this.map.addControl(tool);
          // 定位
          var geolocation = new AMap.Geolocation({
            enableHighAccuracy: true, //是否使用高精度定位，默认:true
            timeout: 10000, //超过10秒后停止定位，默认：5s
            position: "LB", //定位按钮停靠位置，默认：'LB'，左下角
            buttonOffset: new AMap.Pixel(10, 20), //定位按钮与设置的停靠位置的偏移量，默认：Pixel(10, 20)
            // zoomToAccuracy: true,   //定位成功后调整地图视野范围使定位位置及精度范围视野内可见，默认：false
          });
          this.map.addControl(geolocation);
          geolocation.getCurrentPosition(function (status, result) {
            if (status == "complete") {
              console.log(result);
            } else {
              console.log(result);
            }
          });
          this.geocoder = new AMap.Geocoder({
            city: "010", //城市设为北京，默认：“全国”
          });

          await this.formdata(AMap);

          // 将点添加到地图
          // this.map.add(this.markers);
          // 创建样式对象
          var styleObject = {
            // url: "//vdata.amap.com/icons/b18/1/2.png", // 图标地址
            url: 'https://webapi.amap.com/theme/v1.3/markers/n/mark_r.png',
            size: new AMap.Size(19, 31), // 图标大小
            anchor: new AMap.Pixel(5, 5), // 图标显示位置偏移量，基准点为图标左上角
          };

          var massMarks = new AMap.MassMarks(this.markers, {
            zIndex: 5, // 海量点图层叠加的顺序
            zooms: [3, 19], // 在指定地图缩放级别范围内展示海量点图层
            style: styleObject, // 设置样式对象
          });
          // massMarks.setData();

          // 将海量点添加至地图实例
          // massMarks.setMap(this.map);
          // 根据地图上添加的覆盖物分布情况，自动缩放地图到合适的视野级别
          // this.map.setFitView();
        })
        .catch((e) => {
          console.log(e);
        });
    },
    geoCode(address) {
      return new Promise((resolve, reject) => {
        this.geocoder.getLocation(address, function (status, result) {
          if (status === "complete" && result.geocodes.length) {
            resolve(
              result.geocodes.map((item) => [
                item.location.lng,
                item.location.lat,
              ])
            );

            // document.getElementById('lnglat').value = lnglat;
            // marker.setPosition(lnglat);
            // map.add(marker);
            // map.setFitView(marker);
          } else {
            reject("根据地址查询位置失败");
          }
        });
      });
    },
    async formdata(AMap) {
      // var lnglats = [];
      // for (i = 0; i < this.list.length / 10; i++) {
      //   var part = await this.geoCode(
      //     this.list.slice(i * 10, (i + 1) * 10).map((item) => item.title)
      //   );
      //   lnglats.push(...part);
      //   console.log(i, this.list.slice(i * 10, (i + 1) * 10));
      // }
      // this.markers = [];

      // for (var i = 0; i < lnglats.length; i++) {
      //   var lnglat = lnglats[i];
      //   // 创建点实例
      //   var marker = new AMap.Marker({
      //     position: new AMap.LngLat(lnglat[0], lnglat[1]),
      //     // icon:
      //     //   "https://webapi.amap.com/theme/v1.3/markers/n/mark_r.png",
      //     extData: {
      //       id: i + 1,
      //     },
      //   });

      //   this.markers.push(marker);
      // }

      // 获取pois
      let { data: res } = await axios.get(
        "http://127.0.0.1:7001/api/v0/pois?source=0&limit=2000"
      );
      this.markers = res.data
        .filter((item) => {
          return (
            item.location &&
            item.location[0] &&
            117 < item.location[0] < 118 &&
            item.location[1] < 42 &&
            item.location[1] > 39
          );
        })
        .map((item, i) => {
          return {
            lnglat: [item.location[0], item.location[1]], //点标记位置
            name: item.name,
            id: i,
          };
          // return new AMap.Marker({
          //   position: new AMap.LngLat(item.location[0], item.location[1]),
          //   // icon:
          //   //   "https://webapi.amap.com/theme/v1.3/markers/n/mark_r.png",
          //   extData: {
          //     id: i + 1,
          //   },
          // });
        });
      console.log(this.markers, res.data);
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.map {
  width: 100%;
  height: 100%;
  position: relative;
}

#container {
  height: 100%;
}
</style>
