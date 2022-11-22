<template>
  <div>
    <el-tabs v-model="activeName" type="card" @tab-click="handleClick">
      <el-tab-pane label="中文" name="first":key="'first'">
        <child1 v-if="isChildUpdate1"></child1>
      </el-tab-pane>
      <el-tab-pane label="英文" name="second":key="'second'">
        <child2 v-if="isChildUpdate2"></child2>
      </el-tab-pane>
    </el-tabs>
  </div>
</template>

<script>
import API from '../../utils/request'
import chinese from "@/components/paper/chinese";
import english from "@/components/paper/english";
const url = "/api/paper/"

export default {
  name: "papercheck",
  data() {
    return {
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
      text: '',
      count: 0,
      username: "",
      showChkStateDialog: false,
      remark: "",
      showeditStateDialog: false,
      ratios: '',
      state1: '',
      activeName: 'first',
      isChildUpdate1: true,
      isChildUpdate2: false,

    };
  },
  components: {
    child1: chinese,
    child2: english,
  },
  created() {
    // this.user = sessionStorage.getItem("user") ? JSON.parse(sessionStorage.getItem("user")) : {}
    this.load()
  },
  methods: {
    handleClick(tab) {
      if (tab.name == "first") {
        this.isChildUpdate1 = true;
        this.isChildUpdate2 = false;
        this.isChildUpdate3 = false;
      } else if (tab.name == "second") {
        this.isChildUpdate1 = false;
        this.isChildUpdate2 = true;
        this.isChildUpdate3 = false;
      }
    }
  }
}

</script>

<style scoped>
</style>
