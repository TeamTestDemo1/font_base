<template>
    <div>
        <el-tabs v-model="activeName" type="card" @tab-click="handleClick">
            <el-tab-pane label="上机实验" name="first":key="'first'">
                <child1 v-if="isChildUpdate1"></child1>
            </el-tab-pane>
            <el-tab-pane label="实习" name="second":key="'second'">
                <child2 v-if="isChildUpdate2"></child2>
            </el-tab-pane>
            <el-tab-pane label="实践" name="third":key="'third'">
                <child3 v-if="isChildUpdate3"></child3>
            </el-tab-pane>
            <el-tab-pane label="课程" name="fourth":key="'fourth'">
                <child4 v-if="isChildUpdate4"></child4>
            </el-tab-pane>
            <el-tab-pane label="指导学生" name="fifth":key="'fifth'">
                <child5 v-if="isChildUpdate5"></child5>
            </el-tab-pane>
        </el-tabs>
    </div>
</template>

<script>
import API from '../../utils/request'
import computerPractice from '../../components/course/computerPractice.vue'
import internShip from '../../components/course/internShip.vue'
import practice from '../../components/course/practice.vue'
import tclass from '../../components/course/tclass.vue'
import zhiDao from '../../components/course/zhiDao.vue'


const url = "/api/class/"

export default {
  name: "lilun",
  data() {
    return {
        activeName: 'first',
        isChildUpdate1:true,
        isChildUpdate2:false,
        isChildUpdate3:false,
        isChildUpdate4:false,
        isChildUpdate5:false,
        teacherName:''
    }
  },
    components:{
        child1:computerPractice,
        child2:internShip,
        child3:practice,
        child4:tclass,
        child5:zhiDao,
    },
  created() {
    let str = sessionStorage.getItem("user") || ''
    this.user = JSON.parse(str)
    if (this.user.name === "admin") {
      this.teacherName = ""
    } else {
      this.teacherName = this.user.name
    }
    console.log(this.teacherName)
    // this.getcount()
    // this.getData()
  },
  methods: {
      handleClick(tab) {
          if(tab.name == "first") {
              this.isChildUpdate1 = true;
              this.isChildUpdate2 = false;
              this.isChildUpdate3 = false;
              this.isChildUpdate4 = false;
              this.isChildUpdate5 = false;
          } else if(tab.name == "second") {
              this.isChildUpdate1 = false;
              this.isChildUpdate2 = true;
              this.isChildUpdate3 = false;
              this.isChildUpdate4 = false;
              this.isChildUpdate5 = false;
          } else if(tab.name == "third") {
              this.isChildUpdate1 = false;
              this.isChildUpdate2 = false;
              this.isChildUpdate3 = true;
              this.isChildUpdate4 = false;
              this.isChildUpdate5 = false;
          } else if(tab.name == "fourth") {
              this.isChildUpdate1 = false;
              this.isChildUpdate2 = false;
              this.isChildUpdate3 = false;
              this.isChildUpdate4 = true;
              this.isChildUpdate5 = false;
          } else if(tab.name == "fifth") {
              this.isChildUpdate1 = false;
              this.isChildUpdate2 = false;
              this.isChildUpdate3 = false;
              this.isChildUpdate4 = false;
              this.isChildUpdate5 = true;
          }
      },
  }
}
</script>

<style scoped>
</style>
