<template>
  <div id="LargeScreen">
    <div class="div1" style="font-size: 18px">
      室内场所空气指数排行:
      <el-table
        :data="tableData"
        height="250"
        border
        stripe
        style="width: 100%"
        :default-sort="{ prop: 'value1', order: 'descending' }"
      >
        <el-table-column fixed type="index" label="序号" width="40">
        </el-table-column>
        <el-table-column prop="grid_tid" label="场所号" width="80">
        </el-table-column>
        <el-table-column prop="device_id" label="设备号" width="80">
        </el-table-column>
        <el-table-column
          fixed="right"
          prop="value1"
          label="TVOC"
          sortable
          width="90"
        >
        </el-table-column>
      </el-table>
    </div>
    <div class="div2">
      设备实时数据：
      <el-input
        size="small"
        v-model="deviceId"
        placeholder="1"
        clearable
        style="width: 235px"
      ></el-input>
      <el-button size="small" type="primary" @click="onSubmit">查询</el-button>
      <div>
        <el-table
          :data="tableData1"
          height="275"
          border
          stripe
          style="width: 100%"
        >
          <el-table-column prop="data_type" label="数据类型" width="100">
          </el-table-column>
          <el-table-column prop="data_max" label="阈值" width="100">
          </el-table-column>
          <el-table-column prop="data_time" label="数值" width="100">
          </el-table-column>
        </el-table>
      </div>
    </div>
    <div class="div4">
      <el-button
        type="text"
        @click="centerDialogVisible = true"
        style="font-size: 18px; position: absolute; left: 35%;color: crimson;"
        >报警管理：
      </el-button>
      <el-dialog
        title="提示"
        :visible.sync="centerDialogVisible"
        width="30%"
        center
      >
        <span>请确认是否前往报警管理页面</span>
        <span slot="footer" class="dialog-footer">
          <el-button @click="centerDialogVisible = false">取 消</el-button>
          <el-button type="primary" @click="onClick">确 定</el-button>
        </span>
      </el-dialog>
    </div>
    <div class="div5">
      <el-table
        :data="tableData2"
        height="200"
        border
        stripe
        style="width: 100%"
      >
        <el-table-column prop="device_id" label="设备编号" width="160">
        </el-table-column>
        <el-table-column prop="grid_admin" label="场所负责人" width="160">
        </el-table-column>
        <el-table-column prop="alarm_type" label="报警类型" width="162">
        </el-table-column>
      </el-table>
    </div>
    <div id="echart1" class="echar1"></div>
    <div id="echart2" class="echar2"></div>
    <div id="echart3" class="echar3"></div>
    <div id="echart4" class="echar4"></div>
  </div>
</template>
<script type="text/javascript">
//import "echarts/extension/bmap/bmap";
//import axios from "@/http/axios.js";
var echarts = require("echarts");
export default {
  data() {
    return {
      tableData: [
        {
          grid_tid: "1001",
          device_id: "1",
          value1: "0.21",
        },
        {
          grid_tid: "1001",
          device_id: "2",
          value1: "0.16",
        },
        {
          grid_tid: "1001",
          device_id: "3",
          value1: "0.15",
        },
        {
          grid_tid: "1001",
          device_id: "4",
          value1: "0.12",
        },
         {
          grid_tid: "1001",
          device_id: "5",
          value1: "0.15",
        }
      ],
      tableData1: [
        {
          data_type: "SO2",
          data_max: "0.5",
          data_time: "0.12",
        },
        {
          data_type: "湿度",
          data_max: "70",
          data_time: "45",
        },
        {
          data_type: "CO",
          data_max: "10",
          data_time: "0.13",
        },
        {
          data_type: "温度",
          data_max: "30",
          data_time: "22",
        },
      ],
      tableData2: [
        {
          device_id: "1",
          grid_admin: "张三",
          alarm_type: "设备离线",
        },
        {
          device_id: "1",
          grid_admin: "张三",
          alarm_type: "设备离线",
        },
        {
          device_id: "1",
          grid_admin: "张三",
          alarm_type: "设备离线",
        },
        {
          device_id: "5",
          grid_admin: "张三",
          alarm_type: "设备离线",
        },
      ],
      input1: "",
      centerDialogVisible: false,
      deviceId: "",
      EchartData1: [],
      EchartData2: [],
    };
  },
  created() {
    //向后台请求 地图数据
    this.GetEchart1Data();
    //向后台请求 统计数据
    this.GetEchart2Data();
  },
  mounted() {
    //地图
    //   this.drawEchart1();
    //右侧统计图1~3
    this.drawEchart2();
    this.drawEchart3();
    this.drawEchart4();
  },
  methods: {
    //三张饼图
    GetEchart2Data() {
      let url = "/ls/各项数据统计";
      axios
        .get(url)
        .then((response) => {
          //console.log(response);
          this.EchartData2 = response.data.data;
          //console.log("表格数据:",this.EchartData2.报警网格);
          var value1 = this.EchartData2.报警场所;
          var value2 = this.EchartData2.正常场所;
          var value3 = this.EchartData2.报警设备;
          var value4 = this.EchartData2.正常设备;
          var value5 = this.EchartData2.未处理;
          var value6 = this.EchartData2.已处理;
          //console.log("单条数据:",value1);
          var arr1 = [
            { value: value1, name: "报警场所" },
            { value: value2, name: "正常场所" },
          ];
          var arr2 = [
            { value: value3, name: "报警设备" },
            { value: value4, name: "正常设备" },
          ];
          var arr3 = [
            { value: value5, name: "已处理" },
            { value: value6, name: "未处理" },
          ];
          //console.log("统计图形:",arr1);
          this.edata1 = arr1;
          this.edata2 = arr2;
          this.edata3 = arr3;

          this.drawEchart2();
          this.drawEchart3();
          this.drawEchart4();
        })
        .catch((error) => {
          this.$notify.error({
            title: "失败",
            message: "获取数据失败",
          });
        });
    },
    formatter(row, column) {
      return row.address;
    },
    //点击跳转登录界面
    onClick() {
      //window.location.href="../AlarmManage.vue"
      this.$router.push({ path: "/" });
    },
    onSubmit() {
      console.log("submit!");
      var lab = [
        {
          data_type: "SO2",
          data_max: "0.5",
          data_time: "0.11",
        },
        {
          data_type: "湿度",
          data_max: "70",
          data_time: "44",
        },
        {
          data_type: "CO",
          data_max: "10",
          data_time: "0.1",
        },
        {
          data_type: "温度",
          data_max: "30",
          data_time: "23",
        },
      ]
      this.tableData1 = lab
    },
    drawEchart1() {
      var myChart = echarts.init(document.getElementById("echart1"));
      
      var convertData = function (data) {
        var res = [];
        for (var i = 0; i < data.length; i++) {
          var geoCoord = geoCoordMap[data[i].name];
          if (geoCoord) {
            res.push({
              name: data[i].name,
              value: geoCoord.concat(data[i].value),
            });
          }
        }
        return res;
      };
      // function renderItem(params, api) {
      //   //区域范围
      //   var coords = [];
      //   var points = [];
      //   for (var i = 0; i < coords.length; i++) {
      //     points.push(api.coord(coords[i]));
      //   }
      //   var color = api.visual("color");
      //   return {
      //     type: "polygon",
      //     shape: {
      //       points: echarts.graphic.clipPointsByRect(points, {
      //         x: params.coordSys.x,
      //         y: params.coordSys.y,
      //         width: params.coordSys.width,
      //         height: params.coordSys.height,
      //       }),
      //     },
      //     style: api.style({
      //       fill: color,
      //       stroke: echarts.color.lift(color),
      //     }),
      //   };
      // }
      
      
      myChart.setOption(option);
      window.addEventListener("resize", function () {
        myChart.resize();
      });
    },
    drawEchart2() {
      var myChart = echarts.init(document.getElementById("echart2"));
      let that = this;
      var option = {
        title: {
          left: "center",
          text: "场所数量统计图",
        },
        toolbox: {
          itemSize: 16,
          left: 250,
          feature: {
            dataView: {},
            saveAsImage: {},
          },
        },
        tooltip: {
          trigger: "item",
          formatter: "{a} <br/>{b}: {c} ({d}%)",
        },
        legend: {
          orient: "vertical",
          left: 10,
          data: ["报警场所", "正常场所"],
        },
        series: [
          {
            name: "场所统计",
            type: "pie",
            radius: ["50%", "70%"],
            avoidLabelOverlap: false,
            label: {
              show: false,
              position: "center",
            },
            emphasis: {
              label: {
                show: true,
                fontSize: "30",
                fontWeight: "bold",
              },
            },
            labelLine: {
              show: false,
            },
            data: [
              { value: 1, name: "报警场所" },
              { value: 12, name: "正常场所" },
            ],
          },
        ],
      };
      myChart.setOption(option);
      window.addEventListener("resize", function () {
        myChart.resize();
      });
    },
    drawEchart3() {
      var myChart = echarts.init(document.getElementById("echart3"));
      let that = this;
      var option = {
        title: {
          left: "center",
          text: "设备数量统计图",
        },
        toolbox: {
          itemSize: 16,
          left: 250,
          feature: {
            dataView: {},
            saveAsImage: {},
          },
        },
        tooltip: {
          trigger: "item",
          formatter: "{a} <br/>{b}: {c} ({d}%)",
        },
        legend: {
          orient: "vertical",
          left: 10,
          data: ["报警设备", "正常设备"],
        },
        series: [
          {
            name: "设备统计",
            type: "pie",
            radius: ["50%", "70%"],
            avoidLabelOverlap: false,
            label: {
              show: false,
              position: "center",
            },
            emphasis: {
              label: {
                show: true,
                fontSize: "30",
                fontWeight: "bold",
              },
            },
            labelLine: {
              show: false,
            },
            data: [
              { value: 2, name: "报警设备" },
              { value: 5, name: "正常设备" },
            ],
          },
        ],
      };
      myChart.setOption(option);
      window.addEventListener("resize", function () {
        myChart.resize();
      });
    },
    drawEchart4() {
      var myChart = echarts.init(document.getElementById("echart4"));
      let that = this;
      var option = {
        title: {
          left: "center",
          text: "报警数量统计图",
        },
        toolbox: {
          itemSize: 16,
          left: 250,
          feature: {
            dataView: {},
            saveAsImage: {},
          },
        },
        tooltip: {
          trigger: "item",
          formatter: "{a} <br/>{b}: {c} ({d}%)",
        },
        legend: {
          orient: "vertical",
          left: 10,
          data: ["已处理", "未处理"],
        },
        series: [
          {
            name: "报警统计",
            type: "pie",
            radius: ["50%", "70%"],
            avoidLabelOverlap: false,
            label: {
              show: false,
              position: "center",
            },
            emphasis: {
              label: {
                show: true,
                fontSize: "30",
                fontWeight: "bold",
              },
            },
            labelLine: {
              show: false,
            },
            data: [
              { value: 2, name: "已处理" },
              { value: 2, name: "未处理" },
            ],
          },
        ],
      };
      myChart.setOption(option);
      window.addEventListener("resize", function () {
        myChart.resize();
      });
    },
  },
};
</script>

<style type="text/css" scoped>
.LargeScreen {
  height: 100%;
  width: 100%;
  position: relative;
}
.div1 {
  left: 0%;
  top: 0%;
  width: 20%;
  font-size: 10px;
  margin-right: 10px;
  position: absolute;
  color: #4395bc;
  /* background-color: #4395BC; */
}
.div2 {
  left: 0%;
  top: 50%;
  width: 300px;
  position: absolute;
  color: #4395bc;
  /* background-color: #4395BC; */
}
.div4 {
  left: 30%;
  top: 0px;
  width: 150px;
  position: absolute;
}
.div5 {
  left: 40%;
  top: 0px;
  width: 500px;
  position: absolute;
}
.echar1 {
  /* width: 650px; */
  height: 60%;
  top: 25%;
  right: 25%;
  left: 25%;
  position: absolute;
  background-image: url('../assets/data.jpg');
  background-size: 900px 550px;
}
.echar2 {
  width: 20%;
  height: 200px;
  right: 0%;
  top: 10px;
  position: absolute;
}
.echar3 {
  width: 20%;
  height: 200px;
  top: 30%;
  right: 0%;
  position: absolute;
}
.echar4 {
  width: 20%;
  height: 200px;
  top: 60%;
  right: 0%;
  position: absolute;
}
</style>
