<template>
  <div>
    <div style="padding: 10px ">
      <el-input v-model="text" @keyup.enter.native="load" placeholder="请输入姓名" style="width: 200px"> <i slot="prefix" class="el-input__icon el-icon-search"></i></el-input>
      <el-button @click="im" size="small" type="primary" style="margin-left: 10px ">导入</el-button>
      <el-button @click="add" size="small" type="primary" style="margin-left: 10px ">新增</el-button>
    </div>
    <div style="margin-left: 10px">
      <el-table
          ref="multipleTable"
          :data="tableData"
          border
          :stripe="true"
          style="width: 100%"
          @selection-change="handleSelectionChange">
        <el-table-column
            type="selection"
            width="55"
            align="center">
        </el-table-column>
        <el-table-column
            prop="id"
            label="序号"
            width="50"
            align="center">
        </el-table-column>
        <el-table-column
            prop="jobNumber"
            label="工号"
            width="70"
            align="center">
        </el-table-column>
        <el-table-column
            prop="paperName"
            label="论文名称"
            width="350"
            align="center">
        </el-table-column>
        <el-table-column
            prop="journalName"
            label="期刊名称"
            width="120"
            align="center">
        </el-table-column>
        <el-table-column
            prop="DoiNumber"
            label="论文Doi号"
            width="300"
            align="center">
        </el-table-column>
        <el-table-column
            prop="ISSNNumber"
            label="ISSN号"
            width="300"
            align="center">
        </el-table-column>
        <el-table-column
            prop="sort"
            label="期刊院内分级"
            width="50">
        </el-table-column>
        <el-table-column
            prop="allAuthor"
            label="全体作者（中文名，按顺序）">
        </el-table-column>
        <el-table-column
            prop="author"
            label="本院作者（中文名）">
        </el-table-column>
        <el-table-column
            prop="student"
            label="本院学生（中文名）">
        </el-table-column>
        <el-table-column
            prop="chkState"
            label="状态"
            width="100">
        </el-table-column>
        <el-table-column
            label="操作"
            width="120">
          <template slot-scope="scope">
            <el-button
                type="success"
                circle
                :disabled="scope.row.chkState !== '待审核'"
                @click="hChkState(scope.row)">通过</el-button>
            <el-button @click="handleClick(scope.row)" type="danger" size="small" circle>拒绝</el-button>
          </template>
        </el-table-column>
      </el-table>
    </div>

    <el-dialog title="论文信息" :visible.sync="dialogFormVisible" width="40%"
               :close-on-click-modal="false" :close-on-press-escape="false" :show-close="false">
      <el-form :model="entity">
        <el-form-item label="序号" label-width="120px" v-if="!entity.id">
          <el-input v-model="entity.id" autocomplete="off" style="width: 80%"></el-input>
        </el-form-item>
        <el-form-item label="工号" label-width="120px">
          <el-input v-model="entity.jobNumber" autocomplete="off" style="width: 80%"></el-input>
        </el-form-item>
        <el-form-item label="DOI号" label-width="120px">
          <el-input v-model="entity.DoiNumber" autocomplete="off" style="width: 80%"></el-input>
        </el-form-item>
        <el-form-item label="ISSNNumber" label-width="120px">
          <el-input v-model="entity.ISSNNumber" autocomplete="off" style="width: 80%"></el-input>
        </el-form-item>
        <el-form-item label="期刊院内分级" label-width="120px">
          <el-input v-model="entity.sort" autocomplete="off" style="width: 80%"></el-input>
        </el-form-item>
        <el-form-item label="全体作者（中文名，按顺序）" label-width="120px">
          <el-input v-model="entity.allAuthor" autocomplete="off" style="width: 80%"></el-input>
        </el-form-item>
        <el-form-item label="本院作者（中文名）" label-width="120px">
          <el-input v-model="entity.author" autocomplete="off" style="width: 80%"></el-input>
        </el-form-item>
        <el-form-item label="本院学生（中文名）" label-width="120px">
          <el-input v-model="entity.student" autocomplete="off" style="width: 80%"></el-input>
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

const url = "/api/role/"
export default {
  name: "Submitpaper",
  created() {
    this.getPaperList()
  },
  methods: {
    handleClick(row) {
      console.log(row);
      this.entity = {};
      this.showChkStateDialog= true
    },
    add(){
      this.entity = {};
      this.dialogFormVisible = true
    },
    save(){

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
    getPaperList(){

    }
  },

  data() {
    return {
      tableData: [{
        id: '1',
        jobNumber: '123456',
        paperName: '基于深度强化学习的生鲜产品联合库存控制与动态定价研究',
        journalName: '',
        DoiNumber: '10.19734/j.issn.1001-3695.2022.01.0056',
        ISSNNumber: '',
        sort:'A',
        allAuthor:'毕文杰,周玉冰',
        author:'毕文杰',
        student:'',
        chkState:'待审核',
        chkRemarks:''
      }, {
        id: '2',
        jobNumber: '123456',
        paperName: '基于深度强化学习的生鲜产品联合库存控制与动态定价研究',
        journalName: '',
        DoiNumber: '10.19734/j.issn.1001-3695.2022.01.0056',
        ISSNNumber: '',
        sort:'B',
        allAuthor:'毕文杰,周玉冰',
        author:'毕文杰',
        student:'',
        chkState:'审核',
        chkRemarks:''
      }],
      pageNum: 1,
      pageSize: 10,
      total: 2,
      entity: {},
      dialogFormVisible: false,
      showChkStateDialog:false,
      text:''
    }
  }
}
</script>

<style scoped>

</style>
