<template>
    <div>
      <el-button type="primary" size="small" @click="toAddHandler">添加</el-button>
      <el-button type="danger" size="small">批量删除</el-button>
      <el-table :data="employees ">
              <el-table-column   fixed="left" prop="id" label="编号"></el-table-column>
               <el-table-column   fixed="left" prop="username" label="姓名"></el-table-column>
              <el-table-column    prop="realname" label="姓名"></el-table-column>
               <el-table-column    prop="gender" label="性别"></el-table-column>
              <el-table-column width="200" prop="idCard" label="身份证号"></el-table-column>
              <el-table-column width="200" prop="bankCard" label="银行卡号"></el-table-column>
               <el-table-column width="120" prop="telephone" label="手机号"></el-table-column>
               <el-table-column fixed="right" prop="操作" label="操作">
                <template v-slot="slot">
                   <a href="" @click.prevent="toDeleteHandler(slot.row.id)">删除</a>
                   <a href="" @click.prevent="toupdateHandler(slot.row)">修改</a>
                 </template>
               </el-table-column>
        </el-table>
        <el-pagination layout="prev, pager, next" :total="50"> </el-pagination>
        <el-dialog :title="title" :visible.sync="visible" width="50%" >
         <el-form  :model="form" label-width="80px">
           测试：{{form}}
            <el-form-item label="用户名">
               <el-input v-model="form.username"></el-input>
           </el-form-item>
           <el-form-item label="密码">
                <el-input type="password" v-model="form.password"></el-input>
           </el-form-item>
           <el-form-item label="性别">
               <el-radio-group v-model="form.gender">
                  <el-radio label="男">男</el-radio>
                 <el-radio  label="女">女</el-radio>
           </el-radio-group>
           </el-form-item>
           <el-form-item label="真实姓名">
                <el-input  v-model="form.realname"></el-input>
           </el-form-item>
           <el-form-item label="联系方式">
                <el-input type="telephone" v-model="form.telephone"></el-input>
           </el-form-item>
           <el-form-item label="省份证号">
                <el-input type="idCard" v-model="form.idCard"></el-input>
           </el-form-item>
           <el-form-item label="银行卡号">
                <el-input type="bankCard" v-model="form.bankCard"></el-input>
           </el-form-item>
         </el-form>
         <span slot="footer" class="dialog-footer">
         <el-button @click="closeModelHandler"  size="small">取 消</el-button>
         <el-button type="primary" @click="submitHandler" size="small">确 定</el-button>
          </span>
         </el-dialog>
    </div>
</template>
<script>
   import request from '@/utils/request'  //自定义需要加路径
   import querystring from 'querystring' //系统库不需要加路径
export default {
  
  methods:{
        loadData(){
          
          let url="http://localhost:6677/waiter/findAll"
          request.get(url).then((response)=>{
            this.employees=response.data;
          })

        },
        submitHandler(){
          // this.visible =true;
           let url="http://localhost:6677/waiter/saveOrUpdate"
           //前端向后台发送请求,完成数据的保存操作
           request({
             url,
             method:"POST",
             //告诉后台我的请求体中放的是查询数据
              headers:{
             "Content-Type":"application/x-www-form-urlencoded"
              },
            data:querystring.stringify(this.form)
           }).then((response)=>{
             //箭头函数的this指向外部函数的this
              this.closeModelHandler();
              this.loadData();
              this.$message({
                type:"success",
                message:response.message
              });
           })
        },
       toDeleteHandler(id){
         this.$confirm('此操作将永久删除该文件, 是否继续?', '提示', {
          confirmButtonText: '确定',
          cancelButtonText: '取消',
          type: 'warning'
        }).then(() => {
          this.$message({
            type: 'success',
            message: '删除成功!'+id
          })
        })
       },
      toupdateHandler(row){
          this.title="修改员工信息";
          this.visible=true;
       },
       closeModelHandler(){
         this.visible=false;
       },
       toAddHandler(){
          this.visible=true;
       },

  },
   created(){
     this.loadData();
   },
  data (){
     return{
            title:"录入员工信息",
            visible:false,
            form:{
              type:"emloyees"
            },
            employees:[]
          }
        }
    
}
</script>
<style scoped>

</style>