<template>

  <div>
    <div style="padding: 5px 0">
      <el-input v-model="text" @keyup.enter.native="load" style="width: 200px"> <i slot="prefix" class="el-input__icon el-icon-search"></i></el-input>
<!--      <el-button @click="successUpload" size="small" style="margin-left: 10px ;background-color: #ffd04b">导入</el-button>-->
      <el-upload action="http://localhost:9999/api/project/import" :on-success="successUpload" :show-file-list="false" style="display: inline-block">
        <el-button  type="warning"  style="margin-left: 10px"><i class="el-icon-upload2"/>导 入</el-button>
      </el-upload>
      <el-button type="warning"  @click="download" style="margin-left: 50px;margin-top: 50px"><i class="el-icon-download"/> 导 出</el-button>
      <el-button @click="add" size="small" style="margin-left: 10px ;background-color: #ffd04b">新增</el-button>
      <el-button type="warning"  @click="calculationAll" style="margin-left: 50px;margin-top: 50px"><i class="el-icon-download"/> 一键核算分数</el-button>
    </div>
    <el-table :data="tableData" border stripe style="width: 100%">
      <el-table-column prop="id" label="ID"></el-table-column>
      <el-table-column prop="projectName" label="项目名称"></el-table-column>
      <el-table-column prop="projectType" label="项目类型"></el-table-column>
      <el-table-column prop="projectLeader" label="项目牵头人"></el-table-column>
      <el-table-column prop="projectJoin" label="项目参与人"></el-table-column>
      <el-table-column prop="projectGrade" label="项目等级"></el-table-column>
      <el-table-column prop="concludeTime" label="结题时间"></el-table-column>
      <el-table-column prop="projectId" label="项目编号"></el-table-column>
      <el-table-column prop="startTime" label="开始时间"></el-table-column>
      <el-table-column prop="endTime" label="结束时间"></el-table-column>
      <el-table-column prop="fundingDirect" label="直接经费"></el-table-column>
      <el-table-column prop="fundingIndirect" label="间接经费"></el-table-column>
      <el-table-column prop="projectEnter" label="录入人"></el-table-column>
      <el-table-column prop="projectScore" label="项目分数"></el-table-column>
<!--    如果项目类型项做选择框，可以用这一段  <el-table-column-->
<!--          label="项目">-->
<!--        <template slot-scope="scope">-->
<!--          <el-select v-model="scope.row.role" value-key="id" multiple placeholder="请选择" @change="changeRole(scope.row)">-->
<!--            <el-option-->
<!--                v-for="item in options"-->
<!--                :key="item.id"-->
<!--                :label="item.name"-->
<!--                :value="item.id">-->
<!--            </el-option>-->
<!--          </el-select>-->
<!--        </template>-->
<!--      </el-table-column>-->
      <el-table-column
          fixed="right"
          label="操作"
          width="200">
        <template slot-scope="scope">
          <el-button type="primary" icon="el-icon-edit" circle @click="edit(scope.row)"></el-button>
          <el-popconfirm
              @confirm="del(scope.row.id)"
              title="确定删除？"
          >
            <el-button type="danger" icon="el-icon-delete" circle slot="reference"
                       style="margin-left: 10px"></el-button>
          </el-popconfirm>
        </template>
      </el-table-column>
    </el-table>
    <div style="margin-top: 10px">
      <el-pagination
          @size-change="handleSizeChange"
          @current-change="handleCurrentChange"
          :current-page="pageNum"
          :page-size="pageSize"
          :page-sizes="[2, 5, 10]"
          layout="total, sizes, prev, pager, next, jumper"
          :total="total"
      >
      </el-pagination>
    </div>

    <!-- 弹窗   -->
    <el-dialog title="项目信息" :visible.sync="dialogFormVisible" width="30%"
               :close-on-click-modal="false" :close-on-press-escape="false" :show-close="false">
      <el-form :model="entity">
<!--        <el-form-item label="id" label-width="120px">-->
<!--          <el-input v-model="entity.id" autocomplete="off" style="width: 80%"></el-input>-->
<!--        </el-form-item>-->
        <el-form-item label="项目名称" label-width="120px" >
          <el-input v-model="entity.projectName" autocomplete="off" style="width: 80%"></el-input>
        </el-form-item>

        <el-form-item label="项目类型" label-width="120px">
          <el-input v-model="entity.projectType" autocomplete="off" style="width: 80%"></el-input>
        </el-form-item>

        <el-form-item label="项目牵头人" label-width="120px">
          <el-input v-model="entity.projectLeader" autocomplete="off" style="width: 80%"></el-input>
        </el-form-item>
        <el-form-item label="项目参与人" label-width="120px">
          <el-input v-model="entity.projectJoin" autocomplete="off" style="width: 80%"></el-input>
        </el-form-item>
        <el-form-item label="项目等级" label-width="120px">
          <el-input v-model="entity.projectGrade" autocomplete="off" style="width: 80%"></el-input>
        </el-form-item>
        <el-form-item label="结题时间" label-width="120px">
          <el-input v-model="entity.concludeTime" autocomplete="off" style="width: 80%"></el-input>
        </el-form-item>
        <el-form-item label="项目编号" label-width="120px">
          <el-input v-model="entity.projectId" autocomplete="off" style="width: 80%"></el-input>
        </el-form-item>
        <el-form-item label="开始时间" label-width="120px">
          <el-input v-model="entity.startTime" autocomplete="off" style="width: 80%"></el-input>
        </el-form-item>
        <el-form-item label="结束时间" label-width="120px">
          <el-input v-model="entity.endTime" autocomplete="off" style="width: 80%"></el-input>
        </el-form-item>
        <el-form-item label="直接经费" label-width="120px">
          <el-input v-model="entity.fundingDirect" autocomplete="off" style="width: 80%"></el-input>
        </el-form-item>
        <el-form-item label="间接经费" label-width="120px">
          <el-input v-model="entity.fundingIndirect" autocomplete="off" style="width: 80%"></el-input>
        </el-form-item>
        <el-form-item label="录入人" label-width="120px">
          <el-input v-model="entity.projectEnter" autocomplete="off" style="width: 80%"></el-input>
        </el-form-item>


      </el-form>
      <div slot="footer" class="dialog-footer">
        <el-button @click="dialogFormVisible = false">取 消</el-button>
        <el-button type="primary" @click="save">确 定</el-button>
      </div>
    </el-dialog>
  </div>
</template>

<script>
import API from '../../utils/request'

const url = "/api/project/"

export default {
  name: "project",
  data() {
    return {
      options: [],
      text: '',
      project: {},
      tableData: [],
      pageNum: 1,
      pageSize: 10,
      total: 0,
      entity: {},
      dialogFormVisible: false,
      username:"",
    };
  },
  created() {
     this.project = sessionStorage.getItem("user") ? JSON.parse(sessionStorage.getItem("user")) : {}
    if(this.project.name==='admin'){
      this.username=""
    }
    else{
      this.username=this.project.name
    }
    this.load()
  },
  methods: {

    handleSizeChange(pageSize) {
      this.pageSize = pageSize
      this.load()
    },
    handleCurrentChange(pageNum) {
      this.pageNum = pageNum
      this.load()
    },
    load() {
      API.get(url + "/page", {
        params: {
          pageNum: this.pageNum,
          pageSize: this.pageSize,
          name: this.text,
          leaderName:this.username
          // leaderName:this.user.username 用于教师查看
        }
      }).then(res => {
        this.tableData = res.data.records || []
        this.total = res.data.total
      })
    },
    add() {
      this.entity = {}
      this.dialogFormVisible = true
    },
    edit(obj) {
      this.entity = JSON.parse(JSON.stringify(obj))
      this.dialogFormVisible = true
    },
    save() {
      if (!this.entity.id) {
        API.post(url, this.entity).then(res => {
          if (res.code === '0') {
            this.$message({
              type: "success",
              message: "操作成功"
            })
          } else {
            this.$message({
              type: "error",
              message: res.msg
            })
          }
          this.load()
          this.dialogFormVisible = false
        })
      } else {
        API.put(url, this.entity).then(res => {
          if (res.code === '0') {
            this.$message({
              type: "success",
              message: "操作成功"
            })
          } else {
            this.$message({
              type: "error",
              message: res.msg
            })
          }
          this.load()
          this.dialogFormVisible = false
        })
      }
    },
    del(id) {
      API.delete(url + id).then(res => {
        if (res.code === '0') {
          this.$message({
            type: "success",
            message: "操作成功"
          })
        } else {
          this.$message({
            type: "error",
            message: res.msg
          })
        }
        this.load()
      })
    },


    successUpload(){
      this.$message.success("导入成功")
      this.load()
    },
    download() {
      window.open("http://localhost:9999/api/project/download")
    },
    //action="http://localhost:9999/project/import"
    calculationAll(){
      API.post("http://localhost:9999/api/project/all",this.tableData).then(res=>{
        this.$message.success("一键核算成功")
        this.load()
      })
    }
  },

};
</script>

<style scoped>
</style>
