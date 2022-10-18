
<template>
  <!-- 表单-->
  <div  style="padding: 10px">
    <!--搜索和导出功能区域-->
    <div style="margin: 10px 0">
      <el-select v-model="value" filterable placeholder="请选择" >
        <el-option
            v-for="item in options"
            :key="item.value"
            :label="item.label"
            :value="item.value">
        </el-option>
      </el-select >
      <el-button type="primary" style="margin-left: 5px">查询</el-button>
      <el-button type="primary" style="margin-left: 10px">导出</el-button>
    </div>

    <el-table
        :data="tableData"
        ref="multipleTable"
        border
        :stripe="true"
        style="width: 100%"
        @selection-change="handleSelectionChange">
      <el-table-column
          type="selection"
          width="55">
      </el-table-column>
      <el-table-column
          prop="id"
          label="序号"
          width="150">
      </el-table-column>
      <el-table-column
          prop="jobNumber"
          label="工号"
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
          prop="DoiNumber"
          label="论文Doi号"
          width="300">
      </el-table-column>
      <el-table-column
          prop="ISSNNumber"
          label="ISSN号"
          width="300">
      </el-table-column>
      <el-table-column
          prop="sort"
          label="期刊院内分级"
          width="120">
      </el-table-column>
      <el-table-column
          prop="allAuthor"
          label="全体作者（中文名，按顺序）"
          width="200">
      </el-table-column>
      <el-table-column
          prop="author"
          label="本院作者（中文名）"
          width="200">
      </el-table-column>
      <el-table-column
          prop="student"
          label="本院学生（中文名）"
          width="100">
      </el-table-column>
      <el-table-column
          prop="chkState"
          label="状态"
          width="100">
      </el-table-column>

      <el-table-column label="操作" width="144">
        <template slot-scope="scope">
          <el-button style="margin-right: 10px" size="mini" @click="dialogFormVisible = true">编辑</el-button>
          <el-popconfirm
              title="这是一段内容确定删除吗？">
            <template #reference>
              <el-button
                  size="mini"
                  type="danger"
                  @click="handleDelete(scope.$index, scope.row)">删除</el-button>
            </template>
          </el-popconfirm>
        </template>
      </el-table-column>
    </el-table>

    <!--              打开嵌入表单的弹窗-->
    <el-dialog title="收货地址" :visible.sync="dialogFormVisible">
      <el-form :model="form">
        <el-form-item label="序号" label-width="120px" v-if="!form.id">
          <el-input v-model="form.id" autocomplete="off" style="width: 80%"></el-input>
        </el-form-item>
        <el-form-item label="工号" label-width="120px">
          <el-input v-model="form.jobNumber" autocomplete="off" style="width: 80%"></el-input>
        </el-form-item>
        <el-form-item label="DOI号" label-width="120px">
          <el-input v-model="form.DoiNumber" autocomplete="off" style="width: 80%"></el-input>
        </el-form-item>
        <el-form-item label="ISSNNumber" label-width="120px">
          <el-input v-model="form.ISSNNumber" autocomplete="off" style="width: 80%"></el-input>
        </el-form-item>
        <el-form-item label="期刊院内分级" label-width="120px">
          <el-input v-model="form.sort" autocomplete="off" style="width: 80%"></el-input>
        </el-form-item>
        <el-form-item label="全体作者（中文名，按顺序）" label-width="120px">
          <el-input v-model="form.allAuthor" autocomplete="off" style="width: 80%"></el-input>
        </el-form-item>
        <el-form-item label="本院作者（中文名）" label-width="120px">
          <el-input v-model="form.author" autocomplete="off" style="width: 80%"></el-input>
        </el-form-item>
        <el-form-item label="本院学生（中文名）" label-width="120px">
          <el-input v-model="form.student" autocomplete="off" style="width: 80%"></el-input>
        </el-form-item>
        <el-form-item label="期刊等级" :label-width="formLabelWidth">
          <el-select v-model="form.level" placeholder="请选择期刊等级">
            <el-option label="第一级-T类" value="firstLevel"></el-option>
            <el-option label="第二级-A类" value="secondLevel"></el-option>
          </el-select>
        </el-form-item>
      </el-form>
      <div slot="footer" class="dialog-footer">
        <el-button @click="dialogFormVisible = false">取消</el-button>
        <el-button type="primary" @click="dialogFormVisible = false">确定</el-button>
      </div>
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

  </div>
</template>

<script>
export default {
  data() {
    return {
      search:'',
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
        chkState:'待审核',
        chkRemarks:''
      }],
      pageNum: 1,
      pageSize: 10,
      total: 2,
      dialogFormVisible: false,
      showChkStateDialog:false,
      form: {
        name: '',
        region: '',
        date1: '',
        date2: '',
        delivery: false,
        type: [],
        resource: '',
        desc: ''
      },
      formLabelWidth: '120px'
    }
  },
  methods: {
    handleEdit(index, row) {
      console.log(index, row);
    },
    handleDelete(index, row) {
      console.log(index, row);
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
    handleClick(row) {
      console.log(row);
      this.form = {};
      this.showChkStateDialog= true
    }
  }
}
</script>