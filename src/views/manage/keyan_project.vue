<template>
  <div style="background: #ededed">
    <el-card>
      <div style="margin-top: 10px">
        <el-col :span="12">
          <div>
            <el-input v-model="text" @keyup.enter.native="load" placeholder="请输入搜索内容" style="width: 200px"> <i slot="prefix" class="el-input__icon el-icon-search"></i></el-input>
            <el-button @click="add" size="small" type="primary" style="margin-left: 10px ">新增</el-button>
            <el-button v-show="project.role[0] === 1" type="primary"  @click="calculationAll" style="margin-left: 10px"><i class="el-icon-download"/> 一键核算分数</el-button>
          </div>
        </el-col>
        <el-col :span="12">
          <div style="margin-bottom: 20px">
            <el-button type="warning"  @click="download" style="margin-left: 435px;margin-right: 0px"><i class="el-icon-download"/> 导出</el-button>
            <el-upload v-show="project.role[0] === 1" action="http://localhost:9999/project/import" :on-success="successUpload" :show-file-list="false" style="display: inline-block">
              <el-button  type="warning"  style="margin-left: 10px;margin-right: 0px"><i class="el-icon-upload2"/>导入</el-button>
            </el-upload>

          </div>
        </el-col>
      </div>
    </el-card>
    <el-card style="margin-top: 10px">
      <div style="margin-top: 10px">
    <el-table
        :data="tableData"
        ref="multipleTable"
        border
        :stripe="true"
        style="width: 100%">
      <el-table-column type="selection"></el-table-column>
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
      <el-table-column prop="state" label="状态"></el-table-column>
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
      <el-table-column fixed="right" align="center" label="操作" width="200px">
        <template slot-scope="scope">
          <el-upload v-show="project.role[0] === 3" action="http://localhost:9999/files/upload" :on-success="successUploadFiles" :show-file-list="false" style="display: inline-block">
            <el-button  type="primary"  plain style="margin-left: 10px;margin-right: 0px">上传<i class="el-icon-upload el-icon--right"></i></el-button>
          </el-upload>
          <el-button
              v-if="project.role[0] === 1"
              type="primary"
              size="mini"
              plain
              style="margin-left: 20px"
              :disabled="scope.row.chkState === 1 || scope.row.chkState === 2"
              @click="hChkState(scope.row)">审核</el-button>
          <el-button  v-if="project.role[0] === 3" plain type="success" style="margin-left: 10px"   @click="editstate(scope.row)">需求<i class="el-icon-edit"></i></el-button>
          <el-button  v-if="project.role[0] === 1" plain type="success" size="mini" @click="edit(scope.row)">编辑</el-button>
          <div style="margin-top: 8px;margin-left: 10px">
            <el-button
                v-if="project.role[0] === 1"
                size="mini"
                plain
                type="warning"
                style="margin-left: 10px"
                :disabled="project.role[0] === 1 || scope.row.bounce !== 0"
                @click="count(scope.row)">核算</el-button>
            <el-popconfirm
                title="这是一段内容确定删除吗？"
                @confirm="del(scope.row)">
              <el-button
                  v-if="project.role[0] === 1"
                  size="mini"
                  plain
                  type="danger"
                  slot="reference"
                  :disabled="project.role[0] !== 1"
                  style="margin-left: 10px"
              >删除</el-button>
            </el-popconfirm>
          </div>
        </template>
      </el-table-column>
<!--      另一种形式的按钮-->
<!--      <el-table-column-->
<!--          fixed="right"-->
<!--          label="操作"-->
<!--          width="180">-->
<!--        <template slot-scope="scope">-->
<!--          <el-button v-show="project.role[0] === 3" type="primary" icon="el-icon-edit" round @click="edit(scope.row)">编辑</el-button>-->
<!--          <el-popconfirm-->
<!--              @confirm="del(scope.row.id)"-->
<!--              title="确定删除？">-->
<!--            <el-button  v-show="project.role[0] === 3" type="danger" icon="el-icon-delete" round slot="reference"-->
<!--                       style="margin-left: 10px">删除</el-button>-->
<!--          </el-popconfirm>-->
<!--          <el-button v-show="project.role[0] === 1" type="success" icon="el-icon-check" round >通过</el-button>-->
<!--          <el-button v-show="project.role[0] === 1" type="danger" icon="el-icon-close" round >拒绝</el-button>-->
<!--        </template>-->
<!--      </el-table-column>-->
    </el-table>
      </div>
    </el-card>
    <div style="margin-top: 10px">
      <el-pagination
          @size-change="handleSizeChange"
          @current-change="handleCurrentChange"
          :current-page="pageNum"
          :page-size="pageSize"
          :page-sizes="[2, 5, 10]"
          layout="total, sizes, prev, pager, next, jumper"
          :total="total">
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
            <el-date-picker
                v-model="entity.startTime"
                autocomplete="off"
                type="datetime"
                placeholder="选择日期"
                format="yyyy-MM-dd hh:mm:ss">
            </el-date-picker>
        </el-form-item>
        <el-form-item label="结束时间" label-width="120px">
          <el-date-picker
              v-model="entity.endTime"
              autocomplete="off"
              type="datetime"
              placeholder="选择日期"
              format="yyyy-MM-dd hh:mm:ss">
          </el-date-picker>
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

    <el-dialog title="审核" :visible.sync="showChkStateDialog">
      <el-form>
        <el-form-item>
          <el-radio-group v-model="ratios">
            <el-radio label="通过" @change="changeState">通过</el-radio>
            <el-radio label="拒绝" @change="changeState">拒绝</el-radio>
          </el-radio-group>
        </el-form-item>
        <el-form-item>
          <el-input
              type="textarea"
              v-model="remark"
              :rows="5"
              style="width:400px"
              placeholder="请输入审核意见"
          ></el-input>
        </el-form-item>

        <el-form-item>
          <el-button @click="showChkStateDialog = false">取消</el-button>
          <el-button type="primary" @click="hSubmitChkState">确定</el-button>
        </el-form-item>
      </el-form>
    </el-dialog>
    <el-dialog title="需求" :visible.sync="showeditStateDialog">
      <el-form>
        <el-form-item>
          <el-input
              type="textarea"
              v-model="remark"
              style="width:400px"
              placeholder="请输入需求"
          ></el-input>
        </el-form-item>

        <el-form-item>
          <el-button @click="showeditStateDialog = false">取消</el-button>
          <el-button type="primary" @click="SubmitEditState">确定</el-button>
        </el-form-item>
      </el-form>
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
      showChkStateDialog:false,
      remark:"",
      showeditStateDialog:false,
      ratios:'',
      state1:'',
      count:0,
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
    console.log(this.username)
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
        console.log(res)
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
    },
    chkState(data) {
      if (data === 0) {
        return '待审核'
      } else if (data === 1) {
        return '已审核'
      } else {
        return '已拒绝'
      }
      console.log(data)
    },
    hChkState(row) {
      this.showChkStateDialog = true
      this.class = row
    },
    changeState(e){
      this.state1 = e
    },
    hSubmitChkState(){
      console.log(this.class)
      if(this.state1 === '通过'){
        this.class.state = "审核通过"
      }else if(this.state1 === '拒绝'){
        this.class.state = this.remark
      }
      this.request.post("http://localhost:9999/project",this.class).then(res=>{
        console.log(res)
      })
      this.getData()
      this.showChkStateDialog = false
    },
    editstate(row){
      this.showeditStateDialog = true
      this. class = row
    },
    SubmitEditState(){
      this.class.state = this.remark
      this.request.post("http://localhost:9999/project",this.class).then(res=>{
        console.log(res)
      })
      this.getData()
      this.showeditStateDialog = false
    }
  },

};
</script>

<style scoped>
</style>
