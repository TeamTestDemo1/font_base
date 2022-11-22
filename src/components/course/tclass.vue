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
            <div style="margin-top: 10px">
                <el-table
                        :data="tableData"
                        stripe
                        style="width: 100%">
                    <el-table-column
                            prop="id"
                            label="ID"
                            width="50px">
                    </el-table-column>
                    <el-table-column
                            prop="courseName"
                            label="课程名称">
                    </el-table-column>
                    <el-table-column
                            prop="teacherName"
                            label="教师">
                    </el-table-column>
                    <el-table-column
                            prop="time"
                            label="总学时">
                    </el-table-column>
                    <el-table-column
                            prop="className"
                            label="选课班级">
                    </el-table-column>
                    <el-table-column
                            prop="num"
                            label="选课人数">
                    </el-table-column>
                    <el-table-column
                            prop="bounce"
                            label="工作量">
                    </el-table-column>
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
                              type="danger"
                              plain
                              slot="reference"
                              :disabled="user.role[0] !== 1"
                              style="margin-left: 10px">删除</el-button>
                        </el-popconfirm>
                      </div>
                    </template>
                  </el-table-column>
                </el-table>
            </div>
            <div style="display: flex;flex-direction: row">
                <el-pagination
                        @size-change="handleSizeChange"
                        @current-change="handleCurrentChange"
                        :current-page="pageNum"
                        :page-sizes="[10,20,50]"
                        :page-size=pageSize
                        layout="total, sizes, prev, pager, next, jumper"
                        :total=total>
                </el-pagination>
                <span style="margin-left: 400px;font-size: 10pt;color: gray">总工作量：{{totalwork}}</span>
            </div>
            </el-card>
        </div>
        <el-dialog title="信息" :visible.sync="dialogFormVisible" width="30%"
                   :close-on-click-modal="false">
            <el-form :model="entity">
                <el-form-item label="课程名称" label-width="120px">
                    <el-input v-model="entity.courseName" autocomplete="off" style="width: 80%"></el-input>
                </el-form-item>
                <el-form-item label="指导教师" label-width="120px">
                    <el-input v-model="entity.teacherName" autocomplete="off" style="width: 80%"></el-input>
                </el-form-item>
                <el-form-item label="学时" label-width="120px">
                    <el-input v-model="entity.time" autocomplete="off" style="width: 80%"></el-input>
                </el-form-item>
                <el-form-item label="指导班级" label-width="120px">
                    <el-input v-model="entity.className" autocomplete="off" style="width: 80%"></el-input>
                </el-form-item>
                <el-form-item label="人数" label-width="120px">
                    <el-input v-model="entity.num" autocomplete="off" style="width: 80%"></el-input>
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
    import API from "../../utils/request";

    export default {
        name: "tclass",
        data(){
            return{
                teacherName:'',
                class:{},
                ratios:'',
                state1:'',
                className:'',
                tableData: [],
                pageNum: 1,
                pageSize: 10,
                total:0,
                user:"",
                totalwork:'',
                dialogFormVisible: false,
                entity: {},
                showChkStateDialog:false,
                remark:"",
                showeditStateDialog:false
            }
        },
        created() {
            let str = sessionStorage.getItem("user") || ''
            this.user = JSON.parse(str)
            if(this.user.username === "admin")
            {
                this.teacherName = ""
            }
            else
            {
                this.teacherName = this.user.name
            }
            console.log(this.teacherName)
            this.getcount()
            this.getData()
        },
        methods: {
            download() {
                window.open("http://localhost:9999/tclass/download")
            },
            add() {
                this.entity = {}
                this.fileList = []
                this.dialogFormVisible = true
            },
            successUpload(){
                this.$message.success("导入成功")
                this.getData()
            },
            loginout()
            {
                sessionStorage.removeItem("user")
                this.$router.push('/')
            },
            search(){
                this.getData();
            },
            count(row){
                this.request.post("http://localhost:9999/tclass/calculate",row).then(res=>{
                    this.$message.success(res.msg)
                    this.getcount()
                    this.getData()
                })
            },
            getcount(){
                if(this.user.username === 'admin')
                {
                    this.request.get("http://localhost:9999/tclass/count").then(res=>{
                        console.log(res)
                        this.totalwork = res.data
                    })
                }
                else
                {
                    this.request.get("http://localhost:9999/tclass/"+this.teacherName).then(res=>{
                        console.log(res)
                        this.totalwork = res.data
                    })
                }
            },
            reset(){
                this.teacherName = ""
                this.className = ""
                this.getData()
            },
            handleDelete(row){
                this.request.delete("http://localhost:9999/tclass/"+row.id).then(res=>{
                    this.$message.success(res.msg)
                    this.getData()
                })
            },
            All(){
                console.log(this.tableData)
                this.request.post("http://localhost:9999/tclass/calculate/all",this.tableData).then(res=>{
                    this.$message.success(res.msg)
                    this.getcount()
                    this.getData()
                })
            },
            getData(){
                this.request.get("http://localhost:9999/tclass/page?pageNum=" + this.pageNum+ "&pageSize=" + this.pageSize + "&teacherName=" + this.teacherName + "&className=" + this.className).then(res=>{
                    this.tableData = res.data.records
                    this.total = res.data.total
                })
                this.getcount()
            },
            handleSizeChange(pageSize){
                this.pageSize = pageSize
                this.getData()
            },
            handleCurrentChange(pageNum){
                this.pageNum = pageNum
                this.getData()
            },
            edit(obj) {
                this.entity = JSON.parse(JSON.stringify(obj))
                this.fileList = []
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
                this.request.post("http://localhost:9999/tclass",this.class).then(res=>{
                    console.log(res)
                })
                this.getData()
                this.showChkStateDialog = false
            },
            editstate(row){
                this.showeditStateDialog = true
                this.class = row
            },
            SubmitEditState(){
                this.class.state = this.remark
                this.request.post("http://localhost:9999/tclass",this.class).then(res=>{
                    console.log(res)
                })
                this.getData()
                this.showeditStateDialog = false
            }
        }
    }
</script>

<style scoped>

</style>
