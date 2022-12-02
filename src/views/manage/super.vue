<template>
<div>
  <el-table
      :data="tableData"
      stripe
      style="width: 100%">
    <el-table-column
        prop="date"
        label="日期"
        width="180">
    </el-table-column>
    <el-table-column
        prop="name"
        label="姓名"
        width="180">
    </el-table-column>
    <el-table-column
        prop="address"
        label="地址">
    </el-table-column>
    <el-table-column
        fixed="right"
        label="操作"
        width="180">
      <template slot-scope="scope">
        <el-button  type="success" icon="el-icon-check" round @click="paperChineseScoreStandard(data)">通过</el-button>
        <el-button  type="danger" icon="el-icon-close" round >拒绝</el-button>
      </template>
    </el-table-column>
  </el-table>
</div>
</template>

<script>

export default {
  data() {
    return {
      chart: null,
      tableData: [{
        date: '2016-05-02',
        name: '王小虎',
        address: '上海市普陀区金沙江路 1518 弄'
      }, {
        date: '2016-05-04',
        name: '王小虎',
        address: '上海市普陀区金沙江路 1517 弄'
      }, {
        date: '2016-05-01',
        name: '王小虎',
        address: '上海市普陀区金沙江路 1519 弄'
      }, {
        date: '2016-05-03',
        name: '王小虎',
        address: '上海市普陀区金沙江路 1516 弄'
      }],
    }
  },
  created() {
    let coef = this.tableData.map(item => ({name: item.name, address: item.address}))
    console.log(coef)
    this.data = coef;
  },
  methods: {
    paperChineseScoreStandard(){
      this.data = coef
    }
  }
}

</script>
