<template>
    <div>
    <div class="child1"style="padding: 5px 0">
        <el-input v-model="text" @keyup.enter.native="load" placeholder="请输入" style="width: 200px"> <i slot="prefix" class="el-input__icon el-icon-search"></i></el-input>
        <el-button @click="paperImport" size="small" style="margin-left: 10px ;background-color: #ffd04b">导入</el-button>
        <el-button @click="add" size="small" style="margin-left: 10px ;background-color: #ffd04b">新增</el-button>

    </div>
    <el-table
            ref="multipleTable"
            :data="tableData"
            border
            style="width: 100%">
        <el-table-column
                fixed
                prop="id"
                label="序号"
                width="150">
        </el-table-column>
        <el-table-column
                prop="userId"
                label="工号"
                width="120">
        </el-table-column>
        <el-table-column
                prop="paperId"
                label="论文编号"
                width="120">
        </el-table-column>
        <el-table-column
                prop="paperName"
                label="论文名称"
                width="350">
        </el-table-column>
        <el-table-column
                prop="journalName"
                label="期刊名称"
                width="120">
        </el-table-column>
        <el-table-column
                prop="authorAll"
                label="全体作者（中文名，按顺序）"
                width="200">
        </el-table-column>
        <el-table-column
                prop="authorFirst"
                label="本院作者（中文名）"
                width="200">
        </el-table-column>
        <el-table-column
                prop="paperUnit"
                label="单位"
                width="300">
        </el-table-column>
        <el-table-column
                prop="paperTime"
                label="发表时间"
                width="150">
        </el-table-column>
        <el-table-column
                prop="paperDoi"
                label="论文Doi号"
                width="300">
        </el-table-column>
        <el-table-column
                prop="ISSN"
                label="ISSN号"
                width="300">
        </el-table-column>
        <el-table-column
                prop="journistName"
                label="期刊名称"
                width="120">
        </el-table-column>
        <el-table-column
                prop="journistRank"
                label="期刊院内分级"
                width="120">
        </el-table-column>
        <el-table-column
                prop="chkState"
                label="状态"
                width="100">
        </el-table-column>
        <el-table-column
                fixed="right"
                label="操作"
                width="150">
            <template slot-scope="scope">
                <el-button style="background-color: #ffd04b ;margin-top: 3px ;margin-left: 1px"  round @click="edit(scope.row)"  size='mini'>编辑</el-button>
                <el-popconfirm
                        @confirm="del(scope.row.id)"
                        title="确定删除？"><el-button  style="background-color: #ffd04b ;margin-top: 3px;margin-left: 1px" round size='mini'  slot="reference">删除</el-button>
                </el-popconfirm>
            </template>
        </el-table-column>
    </el-table>
        <el-dialog title="论文信息" :visible.sync="dialogFormVisible" width="40%"
                   :close-on-click-modal="false" :close-on-press-escape="false" :show-close="false">
            <el-form :model="entity">
                <el-form-item label="序号" label-width="120px" v-if="!entity.id">
                    <el-input v-model="entity.id" autocomplete="off" style="width: 80%"></el-input>
                </el-form-item>
                <el-form-item label="工号" label-width="120px">
                    <el-input v-model="entity.userId" autocomplete="off" style="width: 80%"></el-input>
                </el-form-item>
                <el-form-item label="论文编号" label-width="120px">
                    <el-input v-model="entity.paperId" autocomplete="off" style="width: 80%"></el-input>
                </el-form-item>
                <el-form-item label="论文名称" label-width="120px">
                    <el-input v-model="entity.paperName" autocomplete="off" style="width: 80%"></el-input>
                </el-form-item>
                <el-form-item label="期刊名称" label-width="120px">
                    <el-input v-model="entity.journalName" autocomplete="off" style="width: 80%"></el-input>
                </el-form-item>
                <el-form-item label="全体作者（中文名，按顺序）" label-width="120px">
                    <el-input v-model="entity.authorAll" autocomplete="off" style="width: 80%"></el-input>
                </el-form-item>
                <el-form-item label="本院作者（中文名）" label-width="120px">
                    <el-input v-model="entity.authorFirst" autocomplete="off" style="width: 80%"></el-input>
                </el-form-item>
                <el-form-item label="单位" label-width="120px">
                    <el-input v-model="entity.paperUnit" autocomplete="off" style="width: 80%"></el-input>
                </el-form-item>
                <el-form-item label="发表时间" label-width="120px">
                    <el-input v-model="entity.paperTime" autocomplete="off" style="width: 80%"></el-input>
                </el-form-item>
                <el-form-item label="Doi号" label-width="120px">
                    <el-input v-model="entity.paperDoi" autocomplete="off" style="width: 80%"></el-input>
                </el-form-item>
                <el-form-item label="ISSN号" label-width="120px">
                    <el-input v-model="entity.ISSN" autocomplete="off" style="width: 80%"></el-input>
                </el-form-item>
                <el-form-item label="期刊名称" label-width="120px">
                    <el-input v-model="entity.journistName" autocomplete="off" style="width: 80%"></el-input>
                </el-form-item>
                <el-form-item label="期刊院内分级" label-width="120px">
                    <el-input v-model="entity. journistRank" autocomplete="off" style="width: 80%"></el-input>
                </el-form-item>
                <el-form-item label="状态" label-width="120px">
                    <el-input v-model="entity. chkState" autocomplete="off" style="width: 80%"></el-input>
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
                <el-button @click="dialogFormVisible = false">取 消</el-button>
                <el-button type="primary" @click="save">确定</el-button>
            </div>
        </el-dialog>
        <el-dialog title="审核" :visible.sync="showChkStateDialog">
            <el-form>
                <el-form-item>
                    <el-input
                            type="textarea"
                            style="width:400px"
                            v-model="entity.chkRemarks"
                            placeholder="请输入审核意见">
                    </el-input>
                </el-form-item>

                <el-form-item>
                    <el-button @click="showChkStateDialog = false">取消</el-button>
                    <el-button type="primary" @click="showChkStateDialog = false">确定</el-button>
                </el-form-item>
            </el-form>
        </el-dialog>
    </div>
</template>

<script>
    export default {
        name: "chinese",
        data() {
            return {
                tableData: [{
                    id: '1',
                    userId:'123456',
                    paperId: '1234',
                    paperName: '基于深度强化学习的生鲜产品联合库存控制与动态定价研究',
                    journalName: '',
                    authorAll:'毕文杰,周玉冰',
                    authorFirst:'毕文杰',
                    paperUnit:'中南大学',
                    paperTime:'2021',
                    paperDoi: '10.19734/j.issn.1001-3695.2022.01.0056',
                    ISSN: '',
                    journistName:'',
                    journistRank:'A',
                    chkState:'待审核',
                }],
                pageNum: 1,
                pageSize: 10,
                total: 2,
                entity: {},
                dialogFormVisible: false,
                showChkStateDialog:false
            }
        },

        methods:{
            paperImport(row) {
                console.log(row);
                this.entity = {};
                this.showChkStateDialog= true
            },
            add(){
                this.entity = {};
                this.dialogFormVisible = true
            },
            //监听pagesize的改变事件
            handleSizeChange(newSize){
                console.log(newSize);
                this.pageSize = newSize
            },
            //监听页码值改变的事件
            handleCurrentChange(newPage){
                console.log(newPage);
                this.pageNum = newPage
            },
            edit(row) {
                console.log(row);
                this.entity = JSON.parse(JSON.stringify(row))
                this.dialogFormVisible = true
            },
        }
    }
</script>

<style scoped>

</style>
