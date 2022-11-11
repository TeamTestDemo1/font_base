<template>
    <div style="background: #ededed">
        <el-card>
            <div style="margin-top: 10px">
                <div>
            <el-input v-model="text" @keyup.enter.native="load" placeholder="请输入姓名" style="width: 200px"> <i slot="prefix" class="el-input__icon el-icon-search"></i></el-input>
<!--            <el-upload action="http://localhost:9999/award/import" :on-success="successUpload" :show-file-list="false" style="display: inline-block">-->
<!--                <el-button  type="warning"  style="margin-left: 10px"><i class="el-icon-upload2"/>导 入</el-button>-->
<!--            </el-upload>-->
<!--            <el-button type="warning"  @click="download" style="margin-left: 50px"><i class="el-icon-download"/> 导 出</el-button>-->
            <el-button @click="add" size="small" type="primary" style="margin-left: 10px ">新增</el-button>
<!--            <el-button type="primary"  @click="calculationAll" style="margin-left: 50px"><i class="el-icon-download"/> 一键核算分数</el-button>-->
                </div>
            </div>
        </el-card>


        <div>
            <el-card style="margin-top: 10px">
                <div style="margin-top: 10px">
            <el-table ref="multipleTable" :data="tableData" border :stripe="true" style="width: 100%" @selection-change="handleSelectionChange">
                <el-table-column type="selection" width="55" align="center"></el-table-column>
                <el-table-column prop="id" label="序号"  align="center"></el-table-column>
                <el-table-column prop="award_year" label="年度"  align="center"></el-table-column>
                <el-table-column prop="award_type" label="奖项类型"  align="center"></el-table-column>
                <el-table-column prop="award_grade" label="项目级别"  align="center"></el-table-column>
                <el-table-column prop="award_name" label="获奖名称"  align="center"></el-table-column>
                <el-table-column prop="result_name" label="成果名称"  align="center"></el-table-column>
                <el-table-column prop="unit_first" label="第一完成单位"  align="center"></el-table-column>
                <el-table-column prop="award_first" label="第一获奖人"  align="center"></el-table-column>
                <el-table-column prop="award_all" label="全体获奖人姓名"  align="center"></el-table-column>
                <el-table-column prop="award_enter" label="录入人"  align="center"></el-table-column>
                <el-table-column prop="award_score" label="获奖计分"></el-table-column>
                <el-table-column fixed="right" label="操作" width="120">
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
                </div>
            </el-card>
        </div>

        <el-dialog title="科研获奖信息" :visible.sync="dialogFormVisible" width="40%"
                   :close-on-click-modal="false" :close-on-press-escape="false" :show-close="false">
            <el-form :model="entity">
                <el-form-item label="序号" label-width="120px" v-if="!entity.id">
                    <el-input v-model="entity.id" autocomplete="off" style="width: 80%"></el-input>
                </el-form-item>
                <el-form-item label="年度" label-width="120px">
                    <el-date-picker
                            v-model="entity.award_year"
                            autocomplete="off"
                            type="year"
                            placeholder="选择日期"
                            format="yyyy">
                    </el-date-picker>
                </el-form-item>
                <el-form-item label="奖项类型" label-width="120px">
                    <el-input v-model="entity.award_type" autocomplete="off" style="width: 80%"></el-input>
                </el-form-item>
                <el-form-item label="项目级别" label-width="120px">
                    <el-input v-model="entity.award_grade" autocomplete="off" style="width: 80%"></el-input>
                </el-form-item>
                <el-form-item label="获奖名称" label-width="120px">
                    <el-input v-model="entity.award_name" autocomplete="off" style="width: 80%"></el-input>
                </el-form-item>
                <el-form-item label="成果名称" label-width="120px">
                    <el-input v-model="entity.result_name" autocomplete="off" style="width: 80%"></el-input>
                </el-form-item>
                <el-form-item label="第一完成单位" label-width="120px">
                    <el-input v-model="entity.unit_first" autocomplete="off" style="width: 80%"></el-input>
                </el-form-item>
                <el-form-item label="第一获奖人" label-width="120px">
                    <el-input v-model="entity.award_first" autocomplete="off" style="width: 80%"></el-input>
                </el-form-item>
                <el-form-item label="全体获奖人姓名" label-width="120px">
                    <el-input v-model="entity.award_all" autocomplete="off" style="width: 80%"></el-input>
                </el-form-item>
                <el-form-item label="录入人" label-width="120px">
                    <el-input v-model="entity.award_enter" autocomplete="off" style="width: 80%"></el-input>
                </el-form-item>
            </el-form>

            <el-upload
                    class="upload-demo"
                    drag
                    action="https://jsonplaceholder.typicode.com/posts/"
                    multiple>
                <i class="el-icon-upload"></i>
                <div class="el-upload__text">将文件拖到此处，或<em>点击上传</em></div>
                <div class="el-upload__tip" slot="tip">只能上传jpg/png文件，且不超过500kb</div>
            </el-upload>

            <div slot="footer" class="dialog-footer">
                <el-button @click="dialogFormVisible = false">取消</el-button>
                <el-button type="primary" @click="save">确定</el-button>
            </div>
        </el-dialog>

<!--        <el-dialog title="审核" :visible.sync="showChkStateDialog">-->
<!--            <el-form>-->
<!--                <el-form-item>-->
<!--                    <el-input-->
<!--                            type="textarea"-->
<!--                            style="width:400px"-->
<!--                            v-model="entity.chkRemarks"-->
<!--                            placeholder="请输入审核意见">-->
<!--                    </el-input>-->
<!--                </el-form-item>-->

<!--                <el-form-item>-->
<!--                    <el-button @click="showChkStateDialog = false">取消</el-button>-->
<!--                    <el-button type="primary" @click="showChkStateDialog = false">确定</el-button>-->
<!--                </el-form-item>-->
<!--            </el-form>-->
<!--        </el-dialog>-->

        <!--    分页功能-->
        <div style="margin-left: 10px ">
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
    </div>
</template>


<script>

    import API from "../../utils/request";

    const url = "/award/"

    export default {
        name: "award",
        data() {
            return {
                options: [],
                text: '',
                award: {},
                tableData: [],
                pageNum: 1,
                pageSize: 10,
                total: 0,
                entity: {},
                dialogFormVisible: false,
                showChkStateDialog:false,
                username: "",
            };
        },

        created() {
            this.award= sessionStorage.getItem("user") ? JSON.parse(sessionStorage.getItem("user")) : {}
            if(this.award.name==='admin'){
                this.username=""
            }
            else{
                this.username=this.award.name
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
                        leaderName:this.username,
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
                window.open("http://localhost:9999/award/download")
            },
            //action="http://localhost:9999/award/import"
            calculationAll(){
                API.post("http://localhost:9999/award/all",this.tableData).then(res=>{
                    this.$message.success("一键核算成功")
                    this.load()
                })
            },
            handleSelectionChange(val) {
                this.multipleSelection = val;
            }
        },

    }
</script>

<style scoped>

</style>
