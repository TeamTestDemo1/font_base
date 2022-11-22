<template>
    <div>
      <el-card>
        <div style="padding: 10px 0">
          <el-col :span="12">
            <el-input v-if="user.role[0] ===1"  placeholder="请输入教师姓名" v-model="teacherName" style="width: 150px;margin-left: 10px"></el-input>
            <el-input v-if="user.role[0] ===3" placeholder="请输入课程名称" v-model="className" style="margin-left: 0px;width: 150px"></el-input>
            <el-button type="primary" @click="search()" style="margin-left: 10px"> <i class="el-icon-search"/>搜索</el-button>
            <el-button type="primary" @click="reset()" style="margin-left: 10px">重 置</el-button>
            <el-button v-if="user.role[0] ===1" @click="add" type="primary" style="margin-left: 10px">新增</el-button>
            <el-button v-if="user.role[0] ===1" type="success" @click="All()" style="margin-left: 10px" v-show="user.name !== 'admin'">一 键 核 算</el-button>
          </el-col>
          <!--                                <el-input v-if="user.role[0] ===3" v-model="text" style="width: 200px"> <i slot="prefix" class="el-input__icon el-icon-search"></i></el-input>-->
          <!--                                <el-button v-if="user.role[0] ===3" @click="load" type="primary" style="margin-left: 10px">查询</el-button>-->
          <el-col :span="12">
            <div style="margin-bottom: 20px">
              <el-button type="warning" @click="exp" style="margin-left: 435px;margin-right: 0px"><i class="el-icon-download"/> 导出</el-button>
              <el-upload  v-show="user.role[0] === 1" action="http://localhost:9999/tclass/import" :on-success="successUpload" :show-file-list="false" style="display: inline-block">
                <el-button  type="warning"  style="margin-left: 10px;margin-right: 0px"><i class="el-icon-upload2"/>导入</el-button>
              </el-upload>
            </div>
          </el-col>
        </div>
      </el-card>
        <div>
            <el-card style="margin-top: 10px">
        <el-table :data="tableData" stripe style="width: 100%">
            <el-table-column prop="id" label="ID" width="100" sortable> </el-table-column>
            <el-table-column prop="year" label="年度"></el-table-column>
            <el-table-column prop="courseName" label="课程名称"></el-table-column>
            <el-table-column prop="teacherName" label="指导教师"></el-table-column>
            <el-table-column prop="className" label="指导班级"></el-table-column>
            <el-table-column prop="bounce" label="工作量"></el-table-column>
            <el-table-column prop="note" label="备注"></el-table-column>
            <el-table-column
                    prop="state"
                    label="状态">
            </el-table-column>
          <el-table-column fixed="right" align="center" label="操作" width="200px">
            <template slot-scope="scope">
              <el-button
                  v-if="user.role[0] === 1"
                  type="primary"
                  plain
                  size="mini"
                  style="margin-left: 20px"
                  :disabled="scope.row.chkState === 1 || scope.row.chkState === 2"
                  @click="hChkState(scope.row)">审核</el-button>
              <el-button  v-if="user.role[0] === 3" plain type="success" style="margin-left: 10px"   @click="editstate(scope.row)">需求<i class="el-icon-edit"></i></el-button>
              <el-button  v-if="user.role[0] === 1" plain type="success" size="mini" @click="edit(scope.row)">编辑</el-button>
              <div style="margin-top: 8px;margin-left: 10px">
                <el-button
                    v-if="user.role[0] === 1"
                    size="mini"
                    plain
                    type="warning"
                    style="margin-left: 10px"
                    :disabled="user.role[0] === 1 || scope.row.bounce !== 0"
                    @click="user(scope.row)">核算</el-button>
                <el-popconfirm
                    title="这是一段内容确定删除吗？"
                    @confirm="del(scope.row)">
                  <el-button
                      v-if="user.role[0] === 1"
                      size="mini"
                      plain
                      type="danger"
                      slot="reference"
                      :disabled="user.role[0] !== 1"
                      style="margin-left: 10px">删除</el-button>
                </el-popconfirm>
              </div>
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
            </el-card>
    </div>
        <!-- 弹窗   -->
        <el-dialog title="信息" :visible.sync="dialogFormVisible" width="30%"
                   :close-on-click-modal="false">
            <el-form :model="entity">
                <el-form-item label="年度" label-width="120px">
                    <el-input v-model="entity.year" autocomplete="off" style="width: 80%"></el-input>
                </el-form-item>
                <el-form-item label="课程名称" label-width="120px">
                    <el-input v-model="entity.courseName" autocomplete="off" style="width: 80%"></el-input>
                </el-form-item>
                <el-form-item label="指导教师" label-width="120px">
                    <el-input v-model="entity.teacherName" autocomplete="off" style="width: 80%"></el-input>
                </el-form-item>
                <el-form-item label="指导班级" label-width="120px">
                    <el-input v-model="entity.className" autocomplete="off" style="width: 80%"></el-input>
                </el-form-item>
                <el-form-item label="工作量" label-width="120px">
                    <el-input v-model="entity.bounce" autocomplete="off" style="width: 80%"></el-input>
                </el-form-item>
                <el-form-item label="备注" label-width="120px">
                    <el-input v-model="entity.note" autocomplete="off" style="width: 80%"></el-input>
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

    export default {
        name: "Practice",
        data() {
            return {
                fileList: [],
                practice:{},
                options: [],
                teacherName:'',
                className:'',
                text: '',
                user: {},
                tableData: [],
                pageNum: 1,
                pageSize: 10,
                entity: {},
                total: 0,
                totalwork:{},
                dialogFormVisible: false,
                showChkStateDialog:false,
                remark:"",
                showeditStateDialog:false,
                ratios:'',
                state1:'',
            };
        },
        created() {
            this.user = sessionStorage.getItem("user") ? JSON.parse(sessionStorage.getItem("user")) : {}
            this.load()
        },
        methods: {
            fileSuccessUpload(res) {
                this.entity.file = "http://localhost:9999/files/" + res.data;
                this.entity.img = "http://localhost:9999/files/" + res.data;
                this.fileList = [res.data]
                console.log(res)
            },
            successUpload() {
                this.$message.success("导入成功")
                this.load()
            },
            exp() {   // excel 导出
                window.open("http://localhost:9999/practice/export");
            },
            search(){
                this.getData();
            },
            reset(){
                this.teacherName = ""
                this.className = ""
                this.getData()
            },
            handleSizeChange(pageSize) {
                this.pageSize = pageSize
                this.load()
            },
            getcount(){
                if(this.user.username === 'admin')
                {
                    this.request.get("http://localhost:9999/practice/count").then(res=>{
                        console.log(res)
                        this.totalwork = res.data
                    })
                }
                else
                {
                    this.request.get("http://localhost:9999/practice/"+this.teacherName).then(res=>{
                        console.log(res)
                        this.totalwork = res.data
                    })
                }
            },
            getData(){
                this.request.get("http://localhost:9999/practice/page?pageNum=" + this.pageNum+ "&pageSize=" + this.pageSize + "&teacherName=" + this.teacherName + "&className=" + this.className).then(res=>{
                    this.tableData = res.data.records
                    this.total = res.data.total
                })
                this.getcount()
            },
            handleCurrentChange(pageNum) {
                this.pageNum = pageNum
                this.load()
            },
            changeState(obj, state) {  // 审核
                this.entity = JSON.parse(JSON.stringify(obj));
                this.entity.state = state;
                this.save();
            },
            load() {
                this.request.get("http://localhost:9999/practice/page", {
                    params: {
                        pageNum: this.pageNum,
                        pageSize: this.pageSize,
                        name: this.text
                    }
                }).then(res => {
                    this.tableData = res.data.records || []
                    this.total = res.data.total
                })
            },
            add() {
                this.entity = {}
                this.fileList = []
                this.dialogFormVisible = true
            },
            edit(obj) {
                this.entity = JSON.parse(JSON.stringify(obj))
                this.fileList = []
                this.dialogFormVisible = true
            },
            save() {
                if (!this.entity.id) {
                    this.request.post("http://localhost:9999/practice",this.entity).then(res => {
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
                    this.request.put("http://localhost:9999/practice/",this.entity).then(res => {
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
                this.request.delete("http://localhost:9999/practice/" + id).then(res => {
                    this.$message({
                        type: "success",
                        message: "操作成功"
                    })
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
                this.practice = row
            },
            changeState(e){
                this.state1 = e
            },
            hSubmitChkState(){
                console.log(this.practice)
                if(this.state1 === '通过'){
                    this.practice.state = "审核通过"
                }else if(this.state1 === '拒绝'){
                    this.practice.state = this.remark
                }
                this.request.put("http://localhost:9999/practice",this.practice).then(res=>{
                    console.log(res)
                })
                this.getData()
                this.showChkStateDialog = false
            },
            editstate(row){
                this.showeditStateDialog = true
                this.practice = row
            },
            SubmitEditState(){
                this.practice.state = this.remark
                this.request.put("http://localhost:9999/practice",this.practice).then(res=>{
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
