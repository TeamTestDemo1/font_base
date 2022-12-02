<template>
  <div style="background: #ededed">
    <el-card>
      <div style="margin-top: 10px">
        <el-col :span="12">
          <div>
            <el-input v-model="text" @keyup.enter.native="load" placeholder="请输入搜索内容" style="width: 200px"><i slot="prefix" class="el-input__icon el-icon-search"></i>
            </el-input>
            <el-button @click="add" size="small" type="primary" style="margin-left: 10px ">新增</el-button>
            <el-button  v-show="paper.role[0] === 1" type="primary" style="margin-left: 10px"><i class="el-icon-download"/> 一键核算分数</el-button>
          </div>
        </el-col>
        <el-col :span="12">
          <div style="margin-bottom: 20px">
            <el-button type="warning" @click="out" style="margin-left: 435px;margin-right: 0px"><i class="el-icon-download"/> 导出</el-button>
            <el-upload  v-show="paper.role[0] === 1" action="http://localhost:9999/award/import" :on-success="successUpload" :show-file-list="false" style="display: inline-block">
              <el-button  type="warning"  style="margin-left: 10px;margin-right: 0px"><i class="el-icon-upload2"/>导入</el-button>
            </el-upload>
          </div>
        </el-col>
      </div>
    </el-card>
    <el-card style="margin-top: 10px">
      <div>
        <div style="margin-left: 0px">
          <!--    这是筛选区-->
          <div style="padding: 10px; margin-left: -8px " >
            <el-select v-model="value1" clearable placeholder="请选择发表时间">
              <el-option
                  v-for="item in optionstime"
                  :key="item.value"
                  :label="item.label"
                  :value="item.value">
              </el-option>
            </el-select>
            <el-select
                v-model="value3"
                clearable
                collapse-tags
                style="margin-left: 20px;"
                placeholder="筛选审核状态">
              <el-option
                  v-for="item in optionsstate"
                  :key="item.value"
                  :label="item.label"
                  :value="item.value">
              </el-option>
            </el-select>
            <!--        这是功能区-->
          </div>
          <!--    这是表格区-->
          <el-table
              ref="multipleTable"
              :data="tableData"
              border
              :stripe="true"
              style="width: 100%"
              @selection-change="handleSelectionChange">
            <el-table-column
                type="selection"
                width="55">
            </el-table-column>
            <el-table-column prop="id" label="序号">
            </el-table-column>
            <el-table-column prop="paperId" label="论文编号">
            </el-table-column>
            <el-table-column prop="paperTitle" label="论文名称">
            </el-table-column>
            <el-table-column prop="authorAll" label="所有作者">
            </el-table-column>
            <el-table-column prop="authorFirst" label="第一作者">
            </el-table-column>
            <el-table-column prop="correspondingAuthor" label="通讯作者">
            </el-table-column>
            <el-table-column prop="correspondingAuthorUnit" label="通讯作者单位">
            </el-table-column>
            <el-table-column prop="paperTime" label="发行日期" >
            </el-table-column>
            <el-table-column prop="paperDoi" label="doi">
            </el-table-column>
            <el-table-column prop="journistRank" label="论文类型">
            </el-table-column>
            <el-table-column prop="paperStatus" label="审核状态" show-overflow-tooltip>
              <template slot-scope="{row: {paperStatus}}">
                <span v-if="paperStatus === 1">已审核</span>
                <span v-else>待审核</span>
              </template>
            </el-table-column>
            <el-table-column fixed="right" align="center" label="操作" width="200px">
              <template slot-scope="scope">
                <!--                <el-upload v-show="paper.role[0] === 3" action="http://localhost:9999/files/upload" :on-success="successUploadFiles" :show-file-list="false" style="display: inline-block">-->
                <!--                  <el-button  type="primary"  plain style="margin-left: 10px;margin-right: 0px">上传<i class="el-icon-upload el-icon--right"></i></el-button>-->
                <!--                </el-upload>-->
                <el-button
                    v-if="paper.role[0] === 1"
                    type="primary"
                    size="mini"
                    plain
                    style="margin-left: 20px"
                    :disabled="scope.row.chkState === 1 || scope.row.chkState === 2"
                    @click="hChkState(scope.row)">审核</el-button>
                <el-button  v-if="paper.role[0] === 3" plain type="success" style="margin-left: 10px"   @click="editstate(scope.row)">需求<i class="el-icon-edit"></i></el-button>
                <el-button  v-if="paper.role[0] === 1"  plain type="success" size="mini" @click="edit(scope.row)">编辑</el-button>
                <div style="margin-top: 8px;margin-left: 10px">
                  <el-button
                      v-if="paper.role[0] === 1"
                      size="mini"
                      plain
                      type="warning"
                      style="margin-left: 10px"
                      :disabled="paper.role[0] === 1 || scope.row.bounce !== 0"
                      @click="count(scope.row)">核算</el-button>
                  <el-popconfirm
                      title="这是一段内容确定删除吗？"
                      @confirm="del(scope.row)">
                    <el-button
                        v-if="paper.role[0] === 1"
                        size="mini"
                        plain
                        type="danger"
                        slot="reference"
                        :disabled="paper.role[0] !== 1"
                        style="margin-left: 10px"
                    >删除</el-button>
                  </el-popconfirm>
                </div>
              </template>
            </el-table-column>
          </el-table>
          <!--这是审核区  待修正-->
          <el-dialog title="审核" :visible.sync="showChkStateDialog">
            <el-form>
              <el-form-item>
                <el-input
                    type="textarea"
                    style="width:400px"
                    v-model="entity.journistRemark"
                    placeholder="请输入审核意见">
                </el-input>
              </el-form-item>

              <el-form-item>
                <el-button @click="showChkStateDialog = false">取消</el-button>
                <el-button type="primary" @click="showChkStateDialog = false">确定</el-button>
              </el-form-item>
            </el-form>
          </el-dialog>
          <!--    分页功能-->
          <div style="margin: 15px ">
            <el-pagination
                @size-change="handleSizeChange"
                @current-change="handleCurrentChange"
                :current-page="pageNum"
                :page-sizes="[1,2, 5, 10]"
                :page-size="pageSize"
                layout="total, sizes, prev, pager, next, jumper"
                :total="total">
            </el-pagination>
          </div>
          <!--弹窗-->
          <el-dialog title="论文信息" :visible.sync="dialogFormVisible" width="40%"
                     :close-on-click-modal="false" :close-on-press-escape="false" :show-close="false">
            <el-form :model="entity">
              <el-form-item label="序号" label-width="120px" v-if="!entity.id">
                <el-input v-model="entity.id" autocomplete="off" style="width: 80%"></el-input>
              </el-form-item>
              <el-form-item label="论文编号" label-width="120px">
                <el-input v-model="entity.paperId" autocomplete="off" style="width: 80%"></el-input>
              </el-form-item>
              <el-form-item label="论文名称" label-width="120px">
                <el-input v-model="entity.paperName" autocomplete="off" style="width: 80%"></el-input>
              </el-form-item>
              <el-form-item label="所有作者" label-width="120px">
                <el-input v-model="entity.authorAll" autocomplete="off" style="width: 80%"></el-input>
              </el-form-item>
              <el-form-item label="第一作者" label-width="120px">
                <el-input v-model="entity.authorFirst" autocomplete="off" style="width: 80%"></el-input>
              </el-form-item>
              <el-form-item label="署名单位" label-width="120px">
                <el-input v-model="entity.paperUnit" autocomplete="off" style="width: 80%"></el-input>
              </el-form-item>
              <el-form-item label="通讯作者" label-width="120px">
                <el-input v-model="entity.correspondingAuthor" autocomplete="off" style="width: 80%"></el-input>
              </el-form-item>
              <el-form-item label="通讯作者单位" label-width="120px">
                <el-input v-model="entity.correspondingAuthorUnit" autocomplete="off" style="width: 80%"></el-input>
              </el-form-item>
              <el-form-item label="发行日期" label-width="120px">
                <el-date-picker
                    v-model="entity.paperTime"
                    autocomplete="off"
                    type="date"
                    placeholder="选择日期"
                    format="yyyy-MM-dd">
                </el-date-picker>
              </el-form-item>
              <el-form-item label="doi" label-width="120px">
                <el-input v-model="entity.paperDoi" autocomplete="off" style="width: 80%"></el-input>
              </el-form-item>
              <el-form-item label="论文类型" label-width="120px">
                <el-input v-model="entity. journistRank" autocomplete="off" style="width: 80%"></el-input>
              </el-form-item>
              <el-form-item label="审核状态" label-width="120px">
                <el-input v-model="entity. chkState" autocomplete="off" style="width: 80%"></el-input>
              </el-form-item>
            </el-form>
            <div style="margin-left: 115px">
              <el-upload
                  class="upload-demo"
                  drag
                  action="https://jsonplaceholder.typicode.com/posts/"
                  multiple>
                <i class="el-icon-upload"></i>
                <div class="el-upload__text">将文件拖到此处，或<em>点击上传</em></div>
                <div class="el-upload__tip" slot="tip">只能上传jpg/png文件，且不超过500kb</div>
              </el-upload>
            </div>
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
      </div>
    </el-card>
    <el-progress type="circle" v-show="this.showProgress" :percentage="this.Percentage"  :show-text="this.Percentage === 100"  :stroke-width="16" status="success"></el-progress>
  </div>
</template>

<script>
import API from '../../utils/request'
const url = "/api/paper/english"

export default {
  name: "english",
  data() {
    return {
      showProgress:false,//控制进度条是否显示
      Percentage: 10,  //进度条的进度
      options: [],
      value1: [],
      value2: [],
      value3: [],
      tableData: [],
      user: {},
      pageNum: 1,
      pageSize: 10,
      total: 1,
      entity: {},
      dialogFormVisible: false,
      multipleSelection: [],
      text:'',
      count:0,
      username:"",
      showChkStateDialog:false,
      remark:"",
      showeditStateDialog:false,
      ratios:'',
      state1:'',
      paper:{},
      optionstime: [{
        value: '选项1',
        label: '2022年'
      }, {
        value: '选项2',
        label: '2021年'
      }, {
        value: '选项3',
        label: '2020年'
      }, {
        value: '选项4',
        label: '2019年'
      }],
      optionsstate: [{
        value: '选项1',
        label: '待审核'
      }, {
        value: '选项2',
        label: '已审核'
      }],
    };
  },
  created() {
    this.paper = sessionStorage.getItem("user") ? JSON.parse(sessionStorage.getItem("user")) : {}
    if(this.paper.name==='admin'){
      this.username=""
    }
    else{
      this.username=this.paper.name
    }
    console.log(this.username)
    this.load()
  },
  methods: {
    changeRole(row) {
      this.entity = JSON.parse(JSON.stringify(row));
      this.save();
    },
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
          name: this.text
        }
      }).then(res => {
        this.tableData = res.data.records || []
        this.total = res.data.total
        console.log(this.tableData)
      })

      API.get("/api/role").then(res => {
        this.options = res.data
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
    out(){
      window.open('http://localhost:9999/api/paper//export/chinese')
    },
    changeState(row,state){

    },
    successUpload(){
      this.$message.success("导入成功")
      this.load()
    },
    handleSelectionChange(val) {
      this.multipleSelection = val;
    },
    hChkState(row) {
      this.showChkStateDialog = true
      this.class = row
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
