<!--<template>-->
<!--  <div style="background: #ededed">-->
<!--    <el-card>-->
<!--      <div style="margin-top: 10px">-->
<!--        <el-col :span="12">-->
<!--          <div>-->
<!--            <el-input v-model="text" @keyup.enter.native="load" placeholder="请输入搜索内容" style="width: 200px"> <i slot="prefix" class="el-input__icon el-icon-search"></i></el-input>-->
<!--            <el-button @click="add" size="small" type="primary" style="margin-left: 10px ">新增</el-button>-->
<!--            <el-button v-show="award.role[0] === 1" type="primary"  @click="calculationAll" style="margin-left: 10px"><i class="el-icon-download"/> 一键核算分数</el-button>-->
<!--          </div>-->
<!--        </el-col>-->
<!--        <el-col :span="12">-->
<!--          <div style="margin-bottom: 20px">-->
<!--            <el-button type="warning"  @click="download" style="margin-left: 435px;margin-right: 0px"><i class="el-icon-download"/> 导 出</el-button>-->
<!--            <el-upload v-show="award.role[0] === 1" action="http://localhost:9999/award/import" :on-success="successUpload" :show-file-list="false" style="display: inline-block">-->
<!--              <el-button  type="warning"  style="margin-left: 10px;margin-right: 0px"><i class="el-icon-upload2"/>导 入</el-button>-->
<!--            </el-upload>-->
<!--          </div>-->
<!--        </el-col>-->
<!--      </div>-->
<!--    </el-card>-->


<!--    <div>-->
<!--      <el-card style="margin-top: 10px">-->
<!--        <div style="margin-top: 10px">-->
<!--          <el-table ref="multipleTable" :data="tableData" border :stripe="true" style="width: 100%" @selection-change="handleSelectionChange">-->
<!--            <el-table-column type="selection" width="55" align="center"></el-table-column>-->
<!--            <el-table-column prop="id" label="序号"  align="center"></el-table-column>-->
<!--            <el-table-column prop="award_year" label="年度"  align="center"></el-table-column>-->
<!--            <el-table-column prop="award_type" label="奖项类型"  align="center"></el-table-column>-->
<!--            <el-table-column prop="award_grade" label="项目级别"  align="center"></el-table-column>-->
<!--            <el-table-column prop="award_name" label="获奖名称"  align="center"></el-table-column>-->
<!--            <el-table-column prop="result_name" label="成果名称"  align="center"></el-table-column>-->
<!--            <el-table-column prop="unit_first" label="第一完成单位"  align="center"></el-table-column>-->
<!--            <el-table-column prop="award_first" label="第一获奖人"  align="center"></el-table-column>-->
<!--            <el-table-column prop="award_all" label="全体获奖人姓名"  align="center"></el-table-column>-->
<!--            &lt;!&ndash;                <el-table-column label="附件">&ndash;&gt;-->
<!--            &lt;!&ndash;                    <template slot-scope="scope">&ndash;&gt;-->
<!--            &lt;!&ndash;                        <el-button icon="el-icon-view" type="primary" @click="preview(scope.row.address)"></el-button>&ndash;&gt;-->
<!--            &lt;!&ndash;                    </template>&ndash;&gt;-->
<!--            &lt;!&ndash;                </el-table-column>&ndash;&gt;-->

<!--            <el-table-column prop="address" label="附件"  width="180" align="center" header-align="center">-->
<!--              <template slot-scope="scope">-->
<!--                <el-image style="width: 100%; height: 100px" :src="scope.row.address" :preview-src-list="[scope.row.address]" :key="scope.row.id">-->
<!--                  <div slot="error" class="image-slot">-->
<!--                    <i class="el-icon-picture-outline"></i>-->
<!--                  </div>-->
<!--                </el-image>-->
<!--              </template>-->
<!--            </el-table-column>-->
<!--            <el-table-column prop="award_enter" label="录入人"  align="center"></el-table-column>-->
<!--            <el-table-column prop="award_score" label="获奖计分" align="center"></el-table-column>-->
<!--            <el-table-column prop="state" label="状态" align="center"></el-table-column>-->
<!--            <el-table-column fixed="right" align="center" label="操作" width="200px">-->
<!--              <template slot-scope="scope">-->
<!--                <el-upload v-show="award.role[0] === 3" action="http://localhost:9999/files/upload" :on-success="successUploadFiles" :show-file-list="false" style="display: inline-block">-->
<!--                  <el-button  type="primary"  plain style="margin-left: 10px;margin-right: 0px">上传<i class="el-icon-upload el-icon&#45;&#45;right"></i></el-button>-->
<!--                </el-upload>-->
<!--                <el-button-->
<!--                    v-if="award.role[0] === 1"-->
<!--                    type="primary"-->
<!--                    size="mini"-->
<!--                    plain-->
<!--                    style="margin-left: 20px"-->
<!--                    :disabled="scope.row.chkState === 1 || scope.row.chkState === 2"-->
<!--                    @click="hChkState(scope.row)">审核</el-button>-->
<!--                <el-button  v-if="award.role[0] === 3" plain type="success" style="margin-left: 10px"   @click="editstate(scope.row)">需求<i class="el-icon-edit"></i></el-button>-->
<!--                <el-button  v-if="award.role[0] === 1" plain type="success" size="mini" @click="edit(scope.row)">编辑</el-button>-->
<!--                <div style="margin-top: 8px;margin-left: 10px">-->
<!--                  <el-button-->
<!--                      v-if="award.role[0] === 1"-->
<!--                      size="mini"-->
<!--                      plain-->
<!--                      type="warning"-->
<!--                      style="margin-left: 10px"-->
<!--                      :disabled="award.role[0] === 1 || scope.row.bounce !== 0"-->
<!--                      @click="count(scope.row)">核算</el-button>-->
<!--                  <el-popconfirm-->
<!--                      title="这是一段内容确定删除吗？"-->
<!--                      @confirm="del(scope.row)">-->
<!--                    <el-button-->
<!--                        v-if="award.role[0] === 1"-->
<!--                        size="mini"-->
<!--                        plain-->
<!--                        type="danger"-->
<!--                        slot="reference"-->
<!--                        :disabled="award.role[0] !== 1"-->
<!--                        style="margin-left: 10px"-->
<!--                    >删除</el-button>-->
<!--                  </el-popconfirm>-->
<!--                </div>-->
<!--              </template>-->
<!--            </el-table-column>-->
<!--          </el-table>-->
<!--        </div>-->
<!--      </el-card>-->
<!--    </div>-->

<!--    <el-dialog title="科研获奖信息" :visible.sync="dialogFormVisible" width="40%"-->
<!--               :close-on-click-modal="false" :close-on-press-escape="false" :show-close="false">-->
<!--      <el-form :model="entity">-->
<!--        <el-form-item label="序号" label-width="120px" v-if="!entity.id">-->
<!--          <el-input v-model="entity.id" autocomplete="off" style="width: 80%"></el-input>-->
<!--        </el-form-item>-->
<!--        <el-form-item label="年度" label-width="120px">-->
<!--          <el-date-picker-->
<!--              v-model="entity.award_year"-->
<!--              autocomplete="off"-->
<!--              type="year"-->
<!--              placeholder="选择日期"-->
<!--              format="yyyy">-->
<!--          </el-date-picker>-->
<!--        </el-form-item>-->
<!--        <el-form-item label="奖项类型" label-width="120px">-->
<!--          <el-input v-model="entity.award_type" autocomplete="off" style="width: 80%"></el-input>-->
<!--        </el-form-item>-->
<!--        <el-form-item label="项目级别" label-width="120px">-->
<!--          <el-input v-model="entity.award_grade" autocomplete="off" style="width: 80%"></el-input>-->
<!--        </el-form-item>-->
<!--        <el-form-item label="获奖名称" label-width="120px">-->
<!--          <el-input v-model="entity.award_name" autocomplete="off" style="width: 80%"></el-input>-->
<!--        </el-form-item>-->
<!--        <el-form-item label="成果名称" label-width="120px">-->
<!--          <el-input v-model="entity.result_name" autocomplete="off" style="width: 80%"></el-input>-->
<!--        </el-form-item>-->
<!--        <el-form-item label="第一完成单位" label-width="120px">-->
<!--          <el-input v-model="entity.unit_first" autocomplete="off" style="width: 80%"></el-input>-->
<!--        </el-form-item>-->
<!--        <el-form-item label="第一获奖人" label-width="120px">-->
<!--          <el-input v-model="entity.award_first" autocomplete="off" style="width: 80%"></el-input>-->
<!--        </el-form-item>-->
<!--        <el-form-item label="全体获奖人姓名" label-width="120px">-->
<!--          <el-input v-model="entity.award_all" autocomplete="off" style="width: 80%"></el-input>-->
<!--        </el-form-item>-->
<!--        <el-form-item label="录入人" label-width="120px">-->
<!--          <el-input v-model="entity.award_enter" autocomplete="off" style="width: 80%"></el-input>-->
<!--        </el-form-item>-->
<!--      </el-form>-->

<!--      <el-upload-->
<!--          class="upload-demo"-->
<!--          drag-->
<!--          action="https://jsonplaceholder.typicode.com/posts/"-->
<!--          multiple>-->
<!--        <i class="el-icon-upload"></i>-->
<!--        <div class="el-upload__text">将文件拖到此处，或<em>点击上传</em></div>-->
<!--        <div class="el-upload__tip" slot="tip">只能上传jpg/png文件，且不超过500kb</div>-->
<!--      </el-upload>-->

<!--      <div slot="footer" class="dialog-footer">-->
<!--        <el-button @click="dialogFormVisible = false">取消</el-button>-->
<!--        <el-button type="primary" @click="save">确定</el-button>-->
<!--      </div>-->
<!--    </el-dialog>-->

<!--    &lt;!&ndash;        <el-dialog title="审核" :visible.sync="showChkStateDialog">&ndash;&gt;-->
<!--    &lt;!&ndash;            <el-form>&ndash;&gt;-->
<!--    &lt;!&ndash;                <el-form-item>&ndash;&gt;-->
<!--    &lt;!&ndash;                    <el-input&ndash;&gt;-->
<!--    &lt;!&ndash;                            type="textarea"&ndash;&gt;-->
<!--    &lt;!&ndash;                            style="width:400px"&ndash;&gt;-->
<!--    &lt;!&ndash;                            v-model="entity.chkRemarks"&ndash;&gt;-->
<!--    &lt;!&ndash;                            placeholder="请输入审核意见">&ndash;&gt;-->
<!--    &lt;!&ndash;                    </el-input>&ndash;&gt;-->
<!--    &lt;!&ndash;                </el-form-item>&ndash;&gt;-->

<!--    &lt;!&ndash;                <el-form-item>&ndash;&gt;-->
<!--    &lt;!&ndash;                    <el-button @click="showChkStateDialog = false">取消</el-button>&ndash;&gt;-->
<!--    &lt;!&ndash;                    <el-button type="primary" @click="showChkStateDialog = false">确定</el-button>&ndash;&gt;-->
<!--    &lt;!&ndash;                </el-form-item>&ndash;&gt;-->
<!--    &lt;!&ndash;            </el-form>&ndash;&gt;-->
<!--    &lt;!&ndash;        </el-dialog>&ndash;&gt;-->

<!--    &lt;!&ndash;    分页功能&ndash;&gt;-->
<!--    <div style="margin-left: 10px ">-->
<!--      <el-pagination-->
<!--          @size-change="handleSizeChange"-->
<!--          @current-change="handleCurrentChange"-->
<!--          :current-page="pageNum"-->
<!--          :page-sizes="[1,2, 5, 10]"-->
<!--          :page-size="pageSize"-->
<!--          layout="total, sizes, prev, pager, next, jumper"-->
<!--          :total="total">-->
<!--      </el-pagination>-->
<!--    </div>-->

<!--    &lt;!&ndash;        弹出框&ndash;&gt;-->
<!--    <el-dialog title="审核" :visible.sync="showChkStateDialog">-->
<!--      <el-form>-->
<!--        <el-form-item>-->
<!--          <el-radio-group v-model="ratios">-->
<!--            <el-radio label="通过" @change="changeState">通过</el-radio>-->
<!--            <el-radio label="拒绝" @change="changeState">拒绝</el-radio>-->
<!--          </el-radio-group>-->
<!--        </el-form-item>-->
<!--        <el-form-item>-->
<!--          <el-input-->
<!--              type="textarea"-->
<!--              v-model="remark"-->
<!--              :rows="5"-->
<!--              style="width:400px"-->
<!--              placeholder="请输入审核意见"-->
<!--          ></el-input>-->
<!--        </el-form-item>-->

<!--        <el-form-item>-->
<!--          <el-button @click="showChkStateDialog = false">取消</el-button>-->
<!--          <el-button type="primary" @click="hSubmitChkState">确定</el-button>-->
<!--        </el-form-item>-->
<!--      </el-form>-->
<!--    </el-dialog>-->
<!--    <el-dialog title="需求" :visible.sync="showeditStateDialog">-->
<!--      <el-form>-->
<!--        <el-form-item>-->
<!--          <el-input-->
<!--              type="textarea"-->
<!--              v-model="remark"-->
<!--              style="width:400px"-->
<!--              placeholder="请输入需求"-->
<!--          ></el-input>-->
<!--        </el-form-item>-->

<!--        <el-form-item>-->
<!--          <el-button @click="showeditStateDialog = false">取消</el-button>-->
<!--          <el-button type="primary" @click="SubmitEditState">确定</el-button>-->
<!--        </el-form-item>-->
<!--      </el-form>-->
<!--    </el-dialog>-->
<!--  </div>-->
<!--</template>-->


<!--<script>-->

<!--import API from "../../utils/request";-->

<!--const url = "/award/"-->

<!--export default {-->
<!--  name: "award",-->
<!--  data() {-->
<!--    return {-->
<!--      options: [],-->
<!--      text: '',-->
<!--      ratios:'',-->
<!--      state1:'',-->
<!--      award: {},-->
<!--      tableData: [-->
<!--        {   id:10,-->
<!--          address:require('D:\\front\\font_base\\OIP-C.jpg')},-->
<!--      ],-->
<!--      pageNum: 1,-->
<!--      pageSize: 10,-->
<!--      total: 0,-->
<!--      entity: {},-->
<!--      dialogFormVisible: false,-->
<!--      showChkStateDialog:false,-->
<!--      username: "",-->
<!--      remark: "",-->
<!--      showeditStateDialog:false-->
<!--    };-->
<!--  },-->

<!--  created() {-->
<!--    this.award= sessionStorage.getItem("user") ? JSON.parse(sessionStorage.getItem("user")) : {}-->
<!--    if(this.award.name==='admin'){-->
<!--      this.username=""-->
<!--    }-->
<!--    else{-->
<!--      this.username=this.award.name-->
<!--    }-->
<!--    this.load()-->
<!--  },-->
<!--  methods: {-->

<!--    handleSizeChange(pageSize) {-->
<!--      this.pageSize = pageSize-->
<!--      this.load()-->
<!--    },-->
<!--    handleCurrentChange(pageNum) {-->
<!--      this.pageNum = pageNum-->
<!--      this.load()-->
<!--    },-->

<!--    load() {-->
<!--      API.get(url + "/page", {-->
<!--        params: {-->
<!--          pageNum: this.pageNum,-->
<!--          pageSize: this.pageSize,-->
<!--          name: this.text,-->
<!--          leaderName:this.username,-->
<!--          // leaderName:this.user.username 用于教师查看-->
<!--        }-->
<!--      }).then(res => {-->
<!--        this.tableData = res.data.records || []-->
<!--        this.total = res.data.total-->
<!--      })-->
<!--    },-->
<!--    add() {-->
<!--      this.entity = {}-->
<!--      this.dialogFormVisible = true-->
<!--    },-->
<!--    edit(obj) {-->
<!--      this.entity = JSON.parse(JSON.stringify(obj))-->
<!--      this.dialogFormVisible = true-->
<!--    },-->
<!--    save() {-->
<!--      if (!this.entity.id) {-->
<!--        API.post(url, this.entity).then(res => {-->
<!--          if (res.code === '0') {-->
<!--            this.$message({-->
<!--              type: "success",-->
<!--              message: "操作成功"-->
<!--            })-->
<!--          } else {-->
<!--            this.$message({-->
<!--              type: "error",-->
<!--              message: res.msg-->
<!--            })-->
<!--          }-->
<!--          this.load()-->
<!--          this.dialogFormVisible = false-->
<!--        })-->
<!--      } else {-->
<!--        API.put(url, this.entity).then(res => {-->
<!--          if (res.code === '0') {-->
<!--            this.$message({-->
<!--              type: "success",-->
<!--              message: "操作成功"-->
<!--            })-->
<!--          } else {-->
<!--            this.$message({-->
<!--              type: "error",-->
<!--              message: res.msg-->
<!--            })-->
<!--          }-->
<!--          this.load()-->
<!--          this.dialogFormVisible = false-->
<!--        })-->
<!--      }-->
<!--    },-->
<!--    del(id) {-->
<!--      API.delete(url + id).then(res => {-->
<!--        if (res.code === '0') {-->
<!--          this.$message({-->
<!--            type: "success",-->
<!--            message: "操作成功"-->
<!--          })-->
<!--        } else {-->
<!--          this.$message({-->
<!--            type: "error",-->
<!--            message: res.msg-->
<!--          })-->
<!--        }-->
<!--        this.load()-->
<!--      })-->
<!--    },-->
<!--    successUpload(){-->
<!--      this.$message.success("导入成功")-->
<!--      this.load()-->
<!--    },-->
<!--    successUploadFiles(){-->
<!--      this.$message.success("上传成功")-->
<!--      this.load()-->
<!--    },-->
<!--    download() {-->
<!--      window.open("http://localhost:9999/award/download")-->
<!--    },-->
<!--    //action="http://localhost:9999/award/import"-->
<!--    calculationAll(){-->
<!--      API.post("http://localhost:9999/award/all",this.tableData).then(res=>{-->
<!--        this.$message.success("一键核算成功")-->
<!--        this.load()-->
<!--      })-->
<!--    },-->
<!--    handleSelectionChange(val) {-->
<!--      this.multipleSelection = val;-->
<!--    },-->
<!--    chkState(data) {-->
<!--      if (data === 0) {-->
<!--        return '待审核'-->
<!--      } else if (data === 1) {-->
<!--        return '已审核'-->
<!--      } else {-->
<!--        return '已拒绝'-->
<!--      }-->
<!--      console.log(data)-->
<!--    },-->
<!--    hChkState(row) {-->
<!--      this.showChkStateDialog = true-->
<!--      this.class = row-->
<!--    },-->
<!--    changeState(e){-->
<!--      this.state1 = e-->
<!--    },-->
<!--    hSubmitChkState(){-->
<!--      console.log(this.class)-->
<!--      if(this.state1 === '通过'){-->
<!--        this.class.state = "审核通过"-->
<!--      }else if(this.state1 === '拒绝'){-->
<!--        this.class.state = this.remark-->
<!--      }-->
<!--      this.request.post("http://localhost:9999/award",this.award).then(res=>{-->
<!--        console.log(res)-->
<!--      })-->
<!--      this.getData()-->
<!--      this.showChkStateDialog = false-->
<!--    },-->
<!--    editstate(row){-->
<!--      this.showeditStateDialog = true-->
<!--      this.class = row-->
<!--    },-->
<!--    SubmitEditState(){-->
<!--      this.class.state = this.remark-->
<!--      this.request.post("http://localhost:9999/award",this.award).then(res=>{-->
<!--        console.log(res)-->
<!--      })-->
<!--      this.getData()-->
<!--      this.showeditStateDialog = false-->
<!--    },-->
<!--    // preview(url) {-->
<!--    //     window.open('https://file.keking.cn/onlinePreview?url=' + encodeURIComponent(window.btoa((url))))-->
<!--    // },-->
<!--  },-->

<!--}-->
<!--</script>-->

<!--<style scoped>-->

<!--</style>-->
