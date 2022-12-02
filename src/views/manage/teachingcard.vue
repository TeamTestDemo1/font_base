<template>
  <div style="background: #ededed">
  <el-row>
    <!--    第一张卡片-->
    <el-col :span="8">
      <el-card shadow="hover" class="card" :body-style="{  padding: '0px' }" >
        <el-col :span="10">
          <div  style="margin-top: 30px">
            <span slot="label" class="fontClass">项目完成度</span>
          </div>
          <div>
            <i class="el-icon-document-copy" style="color:#909399"></i>
          </div>
        </el-col>
        <el-col :span="10" :offset="4">
          <div>
            <el-progress class="el-progress" type="dashboard" width="105" :percentage="percentage" :color="customColor" :stroke-width="12" :stroke-linecap="square"></el-progress>
          </div>
          <div style="font-size:10px;margin-left: 1px;margin-top:5px">
            <el-link @click="open" type="primary" :underline="true">查看项目完成情况</el-link>
          </div>
        </el-col>
      </el-card>
    </el-col>

    <!--    第二张卡片-->
    <el-col :span="8">
      <el-card shadow="hover" class="card" :body-style="{  padding: '0px' }" >
        <el-col :span="10">
          <div  style="margin-top: 30px">
            <span slot="label" class="fontClass">待办的事项</span>
          </div>
          <div>
            <i class="el-icon-tickets" style="color:#909399"></i>
          </div>
        </el-col>
        <el-col :span="10" :offset="4">
          <div style="font-size:70px;margin-left: 20px;margin-top: 30px;color: #ff7d1d">
            <span>66</span>
          </div>
          <div style="font-size:10px;margin-right: 10px;margin-top: 12px">
            <el-link @click="open" type="primary" :underline="true">查看待办事项详情</el-link>
          </div>
        </el-col>
      </el-card>
    </el-col>

    <!--    第三张卡片-->
    <el-col :span="8">
      <el-card shadow="hover" class="card" :body-style="{  padding: '0px' }" >
        <el-col :span="11">
          <div  style="margin-top: 30px">
            <span slot="label" class="fontClass">已完成的事项</span>
          </div>
          <div>
            <i class="el-icon-s-claim" style="color:#909399"></i>
          </div>
        </el-col>
        <el-col :span="10" :offset="2">
          <div style="font-size:70px;margin-left: 20px;margin-top: 30px;color: #fc3333">
            <span>34</span>
          </div>
          <div style="font-size:10px;margin-right: 10px;margin-top: 12px">
            <el-link @click="open" type="primary" :underline="true">查看已完成事项详情</el-link>
          </div>
        </el-col>
      </el-card>
    </el-col>
  </el-row>
    <div style="margin-top: 8px;margin-left: 15px">
      <teachingchart></teachingchart>
    </div>
  </div>
</template>


<script>

 import teachingchart from "@/components/charts/teachingchart";

export default {
  components: { teachingchart },
  data() {
    return {
      currentDate: new Date(),
      percentage: 78,
      customColor: '#1e75d2'
    };
  },
  methods: {
    customColorMethod(percentage) {
      if (percentage < 30) {
        return '#909399';
      } else if (percentage < 70) {
        return '#e6a23c';
      } else {
        return '#67c23a';
      }
    },
    open(){
      this.$message('我喜欢吃饭');
    }
  }
}
</script>

<style>

.card{
  width: 405px;
  height: 190px;
  margin-top: 0px;
  margin-bottom: 5px;
  margin-left: 15px;
  border-radius: 10px;
}

.el-progress{
  margin-top: 20px;
}

.fontClass{
  margin-left: 20px;
  font-size:24px;
  color: #909399;
  font-weight: bolder;
  font-family: "微软雅黑";
}


.el-icon-document-copy{
  margin-top: 25px;
  margin-left: 35px;
  font-size: 60px;
}

.el-icon-s-claim{
  margin-top: 25px;
  margin-left: 40px;
  font-size: 60px;
}

.el-icon-tickets{
  margin-top: 25px;
  margin-left: 35px;
  font-size: 60px;
}

</style>