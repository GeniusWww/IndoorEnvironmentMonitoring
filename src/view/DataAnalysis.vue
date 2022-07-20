<template>
  <div id="DataAnalysis">
    <div class="table1">
  <el-table ref="multipleTable"
    :data="tableData.slice((currentPage-1)*size,currentPage*size)"
     tooltip-effect="dark"
    style="width: 100%" height="300" border :span="24" :row-style="{height:'25px'}"  :header-row-style="{height:'25px'}"  :cell-style="{padding:'0px'}">
    <el-table-column label="设备编号" prop="device_id"></el-table-column>
    <el-table-column label="场所编号" prop="grid_id"></el-table-column>
    <el-table-column label="TVOC" prop="devdata_AQI"></el-table-column>
   
    <el-table-column label="SO2" prop="devdata_SO2"></el-table-column>
    <el-table-column label="CO" prop="devdata_NO2"></el-table-column>
    <el-table-column label="温度" prop="devdata_temper"></el-table-column>
    <el-table-column label="湿度" prop="devdata_damp"></el-table-column>
    
  </el-table>
</div>
<div class="block1" style="margin-left:0;">
    <!-- <span class="demonstration">完整功能</span> -->
    <el-pagination
      @size-change="handleSizeChange"
      @current-change="handleCurrentChange"
      :current-page="currentPage"
      :page-sizes="[10, 20, 30, 40]"
      :page-size="size"
      layout="total, sizes, prev, pager, next, jumper"
      :total="tableData.length" >
    </el-pagination>
  </div>
  <div class="v1">场所总数：</div>
  <div class="c1">13</div>
  <div class="v2">正常运行：</div>
  <div class="c2">1</div>
  <div class="v3">停止运行：</div>
  <div class="c3">12</div>
  </div>
</template>

<script type="text/javascript" scoped>
    //import axios from '@/http/axios.js'
export default{
  data(){
  return{
        formInline:{},
        currentPage:1,
        size:10,
        tableData: [{
            device_id:'1',
            grid_id:'1001',
            devdata_AQI:'0.2',
            devdata_PM25:'test',
            devdata_SO2:'0.3', //标准<0.5
            devdata_NO2:'2.1',  //标准<10
            devdata_temper:'21',
            devdata_damp:'40',
            devdata_wspeed:'test',
            devdata_light:'test'},
           {
            device_id:'2',
            grid_id:'1001',
            devdata_AQI:'0.15',
            devdata_PM25:'test',
            devdata_SO2:'0.25', //标准<0.5
            devdata_NO2:'4.1',  //标准<10
            devdata_temper:'22',
            devdata_damp:'42',
            devdata_wspeed:'test',
            devdata_light:'test'}
            ,{
            device_id:'3',
            grid_id:'1001',
            devdata_AQI:'0.23',
            devdata_PM25:'test',
            devdata_SO2:'0.12', //标准<0.5
            devdata_NO2:'3.6',  //标准<10
            devdata_temper:'21',
            devdata_damp:'41',
            devdata_wspeed:'test',
            devdata_light:'test'},{
            device_id:'4',
            grid_id:'1001',
            devdata_AQI:'0.31',
            devdata_PM25:'test',
            devdata_SO2:'0.14', //标准<0.5
            devdata_NO2:'2.3',  //标准<10
            devdata_temper:'22',
            devdata_damp:'41',
            devdata_wspeed:'test',
            devdata_light:'test'},{
            device_id:'5',
            grid_id:'1001',
            devdata_AQI:'0.24',
            devdata_PM25:'test',
            devdata_SO2:'0.22', //标准<0.5
            devdata_NO2:'3.2',  //标准<10
            devdata_temper:'21',
            devdata_damp:'43',
            devdata_wspeed:'test',
            devdata_light:'test'}],
  }
  },
      created(){
    //this.getAllData();
  },
  methods:{
    // getAllData(){
    //   let url='/devicehis/环境数据显示';
    //   axios.get(url)
    //   .then((response)=>{
    //     //console.log(response);
    //     this.tableData=response.data;
    //   }).catch((error)=>{
    //       this.$notify.error({
    //       title: '失败',
    //       message: '获取数据失败'
    //     });
    //   })

    //   let url1='/grid/Grids';
    //   axios.get(url1)
    //   .then((response)=>{
    //     //console.log(response);
    //     this.formInline=response.data.data;
    //   }).catch((error)=>{
    //       this.$notify.error({
    //       title: '失败',
    //       message: '获取数据失败'
    //     });
    //   })
    // },
    //   handleSizeChange(val) {
    //     //console.log(`每页 ${val} 条`);
    //     this.size = val
    //   },
    //   handleCurrentChange(val) {
    //     //console.log(`当前页: ${val}`);
    //     this.currentPage = val
    //   },s
  }
}
</script>

<style type="text/css" scoped>
.table1{
  top:100px;
  position: relative;
}
.el-table{
  font-size: 12px;
  position: absolute;
}
.block1{
  top:430px;
  left:500px;
  position: absolute;
} 
.v1{
font-size: 14px;
text-align: center;
line-height:50px;
height: 50px;
width: 100px;
top: 10px;
left:80px;
position: absolute;
}
.c1{
font-size: 14px;
text-align: center;
line-height:30px;
height: 30px;
width: 100px;
top: 20px;
left:170px;
position: absolute;
box-shadow: 0 2px 4px rgba(0, 0, 0, .12), 0 0 6px rgba(0, 0, 0, .04)
}
.v2{
font-size: 14px;
text-align: center;
line-height:50px;
height: 50px;
width: 100px;
top: 10px;
left:280px;
position: absolute;
}
.c2{
font-size: 14px;
text-align: center;
line-height:30px;
height: 30px;
width: 100px;
top: 20px;
left:370px;
position: absolute;
box-shadow: 0 2px 4px rgba(0, 0, 0, .12), 0 0 6px rgba(0, 0, 0, .04)
}
.v3{
font-size: 14px;
text-align: center;
line-height:50px;
height: 50px;
width: 100px;
top: 10px;
left:480px;
position: absolute;
}
.c3{
font-size: 14px;
text-align: center;
line-height:30px;
height: 30px;
width: 100px;
top: 20px;
left:570px;
position: absolute;
box-shadow: 0 2px 4px rgba(0, 0, 0, .12), 0 0 6px rgba(0, 0, 0, .04)
}
</style>