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
        <el-table :data="tableData" border stripe style="width: 100%">
            <el-table-column prop="id" label="ID" width="100" sortable> </el-table-column>
            <el-table-column prop="year" label="年度"></el-table-column>
            <el-table-column prop="num" label="指导人数"></el-table-column>
            <el-table-column prop="studentName" label="学生姓名"></el-table-column>
            <el-table-column prop="studentType" label="学生类型"></el-table-column>
            <el-table-column prop="isSecretor" label="是否答辩秘书"></el-table-column>
            <el-table-column prop="secretorClassName" label="担任答辩秘书班级名称"></el-table-column>
            <el-table-column prop="secretorBounce" label="答辩秘书工作量"></el-table-column>
            <el-table-column prop="zhidaoBounce" label="总工作量"></el-table-column>
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
                <el-form-item label="指导人数" label-width="120px">
                    <el-input v-model="entity.num" autocomplete="off" style="width: 80%"></el-input>
                </el-form-item>
                <el-form-item label="学生姓名" label-width="120px">
                    <el-input v-model="entity.studentName" autocomplete="off" style="width: 80%"></el-input>
                </el-form-item>
                <el-form-item label="学生类型" label-width="120px">
                    <el-input v-model="entity.studentType" autocomplete="off" style="width: 80%"></el-input>
                </el-form-item>
                <el-form-item label="是否答辩秘书" label-width="120px">
                    <el-input v-model="entity.isSecretor" autocomplete="off" style="width: 80%"></el-input>
                </el-form-item>
                <el-form-item label="担任答辩秘书班级名称" label-width="120px">
                    <el-input v-model="entity.secretorClassName" autocomplete="off" style="width: 80%"></el-input>
                </el-form-item>
                <el-form-item label="答辩秘书工作量" label-width="120px">
                    <el-input v-model="entity.secretorBounce" autocomplete="off" style="width: 80%"></el-input>
                </el-form-item>
                <el-form-item label="总工作量" label-width="120px">
                    <el-input v-model="entity.zhidaoBounce" autocomplete="off" style="width: 80%"></el-input>
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

    export default {
        name: "ZhiDao",
        data() {
            return {
                fileList: [],
                options: [],
                text: '',
                user: {},
                tableData: [],
                pageNum: 1,
                pageSize: 10,
                entity: {},
                total: 0,
                dialogFormVisible: false
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
            successUpload(value) {
                this.$message({
                    message: "导入成功",
                    type: "success"
                });
                this.load()
            },
            exp() {   // excel 导出
                window.open("http://localhost:9999/zhidao/export");
            },
            getData(){
                this.request.get("http://localhost:9999/zhidao/page?pageNum=" + this.pageNum+ "&pageSize=" + this.pageSize + "&teacherName=" + this.teacherName + "&className=" + this.className).then(res=>{
                    this.tableData = res.data.records
                    this.total = res.data.total
                })
                this.getcount()
            },
            handleSizeChange(pageSize) {
                this.pageSize = pageSize
                this.load()
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
                this.request.get("http://localhost:9999/zhidao/page", {
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
                    this.request.post("http://localhost:9999/zhidao", this.entity).then(res => {
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
                    this.request.put("http://localhost:9999/zhidao", this.entity).then(res => {
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
                this.request.delete("http://localhost:9999/zhidao/" + id).then(res => {
                    this.$message({
                        type: "success",
                        message: "操作成功"
                    })
                    this.load()
                })
            }
        },
    };
</script>

<style scoped>
</style>
