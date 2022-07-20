<template>
  <div id="GridManage">
    <div class="table1">
  <el-table ref="multipleTable"
    :data="tableData.slice((currentPage-1)*size,currentPage*size)"
     tooltip-effect="dark"
    style="width: 100%" height="300"
    @selection-change="handleSelectionChange" border :span="24" :row-style="{height:'25px'}"  :header-row-style="{height:'25px'}"  :cell-style="{padding:'0px'}">
    <el-table-column type="selection" width="55">
    </el-table-column>
    <el-table-column type="expand">
      <template slot-scope="props">
        <el-form label-position="left" inline class="demo-table-expand">
          <el-form-item label="相关单位">
            <span>{{ props.row.company }}</span>
          </el-form-item> 
          <el-form-item label="详细地址">
            <span>{{ props.row.address }}</span>
          </el-form-item>
        </el-form>
      </template>
    </el-table-column>
    <el-table-column label="场所编号">
      <template slot-scope="scope">{{ scope.row.id }}</template>
    </el-table-column>
    <el-table-column label="场所名称" prop="name">
    </el-table-column>
    <el-table-column label="状态" prop="condition">
    </el-table-column>
    <el-table-column label="管理员" prop="administrator">
    </el-table-column>
      <el-table-column label="类型" prop="type">
    </el-table-column>
    <el-table-column label="操作">
      <template slot-scope="scope">
      <el-link icon="el-icon-edit" size="mini" @click="handleEdit(scope.row)">编辑</el-link>
      </template>
    </el-table-column>
  </el-table>
</div>
<div class="block1" style="margin-left:0;">
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
    <div class="delete">
        <el-button type="primary" plain @click="handleDelete">批量删除</el-button>
  </div>
  <div class="addition">
       <el-button type="info" plain @click="handleAddition">添加</el-button>
  </div>
  <div class="form1">
  <el-dialog :title="title" :visible.sync="dialogFormVisible">
  <el-form :model="form">
    <el-form-item label="场所编号" :label-width="formLabelWidth">
      <el-input v-model="form.id" placeholder="请输入场所编号"></el-input>
    </el-form-item>
        <el-form-item label="场所名称" :label-width="formLabelWidth">
      <el-input v-model="form.name" placeholder="请输入场所名称"></el-input>
    </el-form-item>
        <el-form-item label="管理员" :label-width="formLabelWidth">
      <el-input v-model="form.administrator" placeholder="请输入管理员"></el-input>
    </el-form-item>
        <el-form-item label="详细地址" :label-width="formLabelWidth">
      <el-input v-model="form.address" placeholder="请输入详细地址"></el-input>
    </el-form-item>
        <el-form-item label="相关单位" :label-width="formLabelWidth">
      <el-input v-model="form.company" placeholder="请输入相关单位"></el-input>
    </el-form-item>
        <el-form-item label="场所类型" :label-width="formLabelWidth">
      <el-select style="width:400px;"  v-model="form.type" placeholder="请选择类型">
        <el-option label="商场" value="商场"></el-option>
        <el-option label="教室" value="教室"></el-option>
        <el-option label="办公室" value="办公室"></el-option>
        <el-option label="工厂车间" value="工厂车间"></el-option>
        <el-option label="住宅" value="住宅"></el-option>
      </el-select>
    </el-form-item>
        <el-form-item label="网格状态" :label-width="formLabelWidth">
      <el-select style="width:400px;" v-model="form.condition" placeholder="请选择网格状态">
        <el-option label="工作中" value="工作中"></el-option>
        <el-option label="已结束" value="已结束"></el-option>
      </el-select>
    </el-form-item>
  </el-form>
  <div slot="footer" class="dialog-footer">
    <el-button @click="dialogFormVisible = false">取 消</el-button>
    <el-button type="primary" @click="BtnOk">确 定</el-button>
  </div>
</el-dialog>    
  </div> 
  <div class="v1">场所总数：</div>
  <div class="c1">{{totalStation}}</div>
  <div class="v2">运行场所总数：</div>
  <div class="c2">{{runningStation}}</div>
  <div class="v3">停止运行场所总数：</div>
  <div class="c3">{{stoppingStation}}</div>
  </div>
</template>

<script type="text/javascript">
export default{
props:['count'],
  data(){
  return{
        totalStation:13,
        runningStation:1,
        stoppingStation:12,
        chooseID:'',
        title:'',
        currentPage:1,
        size:10,
        tableData: [{
          id: '1001',
          name: '住宅A',
          condition: '工作中',
          administrator: '张三',
          type: '住宅',
          company: '恒大',
          address: '苏州'
        }, {
          id: '1002',
          name: '住宅B',
          condition: '已结束',
          administrator: '李四',
          type: '住宅',
          company: '恒大',
          address: '苏州'
        }, {
          id: '1003',
          name: '教室A',
          condition: '已结束',
          administrator: '张四',
          type: '教室',
          company: '附属小学',
          address: '苏州'
        }, {
          id: '1004',
          name: '教室B',
          condition: '已结束',
          administrator: '李五',
          type: '教室',
          company: '附属小学',
          address: '苏州'
        }, {
          id: '1005',
          name: '办公室A',
          condition: '已结束',
          administrator: '周五',
          type: '办公室',
          company: '百度',
          address: '苏州'
        }, {
          id: '1006',
          name: '办公室B',
          condition: '已结束',
          administrator: '周六',
          type: '办公室',
          company: '阿里巴巴',
          address: '苏州'
        }, {
          id: '1007',
          name: '商场A',
          condition: '已结束',
          administrator: '王三',
          type: '商场',
          company: '万达',
          address: '苏州'
        }, {
          id: '1008',
          name: '商场B',
          condition: '已结束',
          administrator: '王五',
          type: '商场',
          company: '万达',
          address: '苏州'
        }, {
          id: '1009',
          name: '车间A',
          condition: '已结束',
          administrator: '周周',
          type: '工厂车间',
          company: '恒力',
          address: '苏州'
        }, {
          id: '1010',
          name: '车间B',
          condition: '已结束',
          administrator: '州州',
          type: '工厂车间',
          company: '恒力',
          address: '苏州'
        }, {
          id: '1011',
          name: '商场C',
          condition: '已结束',
          administrator: '多多',
          type: '商场',
          company: '吾悦',
          address: '苏州'
        }, {
          id: '1012',
          name: '商场D',
          condition: '已结束',
          administrator: '滴滴',
          type: '商场',
          company: '吾悦',
          address: '苏州'
        }, {
          id: '1013',
          name: '住宅C',
          condition: '已结束',
          administrator: '张六',
          type: '住宅',
          company: '金科',
          address: '苏州'
        }],
        //模态框数据
        dialogFormVisible:false,
        form:{
          id: '',
          name: '',
          administrator: '',
          address: '',
          company: '',
          type: '',
          condition: '',
        },
        formLabelWidth:'80px',
      }
  },
  methods:{
      handleSizeChange(val) {
        // console.log(`每页 ${val} 条`);
        this.size = val
      },
      handleCurrentChange(val) {
        // console.log(`当前页: ${val}`);
        this.currentPage = val
      },
      handleSelectionChange(val) {
        this.multipleSelection = val;
        console.log(this.multipleSelection[1]);
      },
       handleEdit(row) {
        console.log(row);
        this.title="编辑";
        this.dialogFormVisible=true;
        this.form=row;
        this.chooseID=row.id;
      },
      handleDelete(){
        this.$confirm('此操作将删除所有选中的网格, 是否继续?', '提示', {
          confirmButtonText: '确定',
          cancelButtonText: '取消',
          type: 'warning'
        }).then(() => {
                  for(var j=0;j<this.multipleSelection.length;j++){
          for(var i=0;i<this.tableData.length;i++){
            if(this.tableData[i].id==this.multipleSelection[j].id){
              this.tableData.splice(i,1);
            }
          }
        }
          this.$message({
            type: 'success',
            message: '删除成功!'
          });
        }).catch(() => {
          this.$message({
            type: 'info',
            message: '已取消删除'
          });          
        });
      },
      handleAddition(){
        this.title="添加";
        this.dialogFormVisible=true;
        this.form={};
      },
      BtnOk(){
        if(this.title=="添加"){
        console.log(this.form);
        this.tableData.push(this.form);
        console.log(this.tableData);
        }
        if(this.title=="编辑"){
          console.log(this.chooseID);
          for(var i=0;i<this.tableData.length;i++){
            if(this.chooseID==this.tableData[i].id){
              this.tableData.splice(i,1,this.form);
            }
          }
        }
        this.dialogFormVisible=false;
      },
  }
}
</script>

<style type="text/css" scoped>
.demo-table-expand {
  font-size: 0;
}
.demo-table-expand label {
  width: 90px;
  color: #99a9bf;
}
.demo-table-expand .el-form-item {
  margin-right: 0;
  margin-bottom: 0;
  width: 50%;
}
.table1{
  top:100px;
  position: relative;
}
.el-table{
  font-size: 12px;
  position: absolute;
}
.el-form-item__label{
  font-size: 12px;
  line-height: 20px;
  vertical-align: right;
}
.el-form-item__content{
  font-size: 12px;
  line-height: 20px;
    vertical-align: right;
}
.block1{
  top:430px;
  left:500px;
  position: absolute;
} 
.delete{
  top:430px;
  left:10px;
  position: absolute;
}
.addition{
  top:60px;
    left:10px;
  position: absolute;
}
.el-dialog__title{
  font-size: 16px;
  margin: 10px;
}
.el-dialog__header{
  padding: 15px 15px 0px;
}
.el-form-item{
  margin: 10px;
}
.el-dialog__body{
    font-size: 12px;
}
.el-form-item__content{
  font-size: 12px;
/*  line-height: 10px;*/
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
width: 140px;
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
left:408px;
position: absolute;
box-shadow: 0 2px 4px rgba(0, 0, 0, .12), 0 0 6px rgba(0, 0, 0, .04)
}
.v3{
font-size: 14px;
text-align: center;
line-height:50px;
height: 50px;
width: 140px;
top: 10px;
left:575px;
position: absolute;
}
.c3{
font-size: 14px;
text-align: center;
line-height:30px;
height: 30px;
width: 100px;
top: 20px;
left:700px;
position: absolute;
box-shadow: 0 2px 4px rgba(0, 0, 0, .12), 0 0 6px rgba(0, 0, 0, .04)
}
</style>