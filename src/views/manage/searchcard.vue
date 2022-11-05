<template>
  <div style="background: #ededed">
  <el-row>
    <!--    第一张卡片-->
    <el-col :span="6">
      <el-card shadow="hover" class="cardone" :body-style="{  padding: '0px' }" >
        <el-col :span="10">
          <div  style="margin-top:20px">
            <span slot="label" class="fontClass">论文</span>
          </div>
          <div>
            <i class="el-icon-tickets"></i>
          </div>
        </el-col>
        <el-col :span="10" :offset="0">
          <div style="font-size:20px;margin-top:40px;margin-left: 0px;font-weight :bold;color: #909399;" >
            <i class="el-icon-error"></i>
            <span>待审论文</span>
            <span class="numbertype">66</span>
          </div>
          <div style="font-size:20px;margin-top:20px;margin-left: auto;font-weight :bold;color: #909399;" >
            <i class="el-icon-success"></i>
            <span>已审论文</span>
            <span class="numbertype">66</span>
          </div>
          <div style="font-size:10px;margin-left: 10px;margin-top: 20px">
            <el-link @click="open" type="primary" :underline="true">查看待办事项详情</el-link>
          </div>
        </el-col>
      </el-card>
    </el-col>

    <!--    第二张卡片-->
    <el-col :span="6">
      <el-card shadow="hover" class="cardone" :body-style="{  padding: '0px' }" >
        <el-col :span="10">
          <div  style="margin-top:20px">
            <span slot="label" class="fontClass">项目</span>
          </div>
          <div>
            <i class="el-icon-s-grid"></i>
          </div>
        </el-col>
        <el-col :span="10" :offset="0">
          <div style="font-size:20px;margin-top:40px;margin-left: 0px;font-weight :bold;color: #909399;" >
            <i class="el-icon-error"></i>
            <span>待审项目</span>
            <span class="numbertype">66</span>
          </div>
          <div style="font-size:20px;margin-top:20px;margin-left: auto;font-weight :bold;color: #909399;" >
            <i class="el-icon-success"></i>
            <span>已审项目</span>
            <span class="numbertype">66</span>
          </div>
          <div style="font-size:10px;margin-left: 10px;margin-top: 20px">
            <el-link @click="open" type="primary" :underline="true">查看待办事项详情</el-link>
          </div>
        </el-col>
      </el-card>
    </el-col>

    <!--    第三张卡片-->
    <el-col :span="6">
      <el-card shadow="hover" class="cardone" :body-style="{  padding: '0px' }" >
        <el-col :span="10">
          <div  style="margin-top:20px">
            <span slot="label" class="fontClass">奖励</span>
          </div>
          <div>
            <i class="el-icon-s-flag"></i>
          </div>
        </el-col>
        <el-col :span="10" :offset="0">
          <div style="font-size:20px;margin-top:40px;margin-left: 0px;font-weight :bold;color: #909399;" >
            <i class="el-icon-error"></i>
            <span>待审奖励</span>
            <span class="numbertype">66</span>
          </div>
          <div style="font-size:20px;margin-top:20px;margin-left: auto;font-weight :bold;color: #909399;" >
            <i class="el-icon-success"></i>
            <span>已审奖励</span>
            <span class="numbertype">66</span>
          </div>
          <div style="font-size:10px;margin-left: 10px;margin-top: 20px">
            <el-link @click="open" type="primary" :underline="true">查看待办事项详情</el-link>
          </div>
        </el-col>
      </el-card>
    </el-col>
    <!--第四张卡片-->
    <el-col :span="6">
      <el-card shadow="hover" class="cardone" :body-style="{  padding: '0px' }" >
        <el-col :span="10">
          <div  style="margin-top:20px">
            <span slot="label" class="fontClass">著作</span>
          </div>
          <div>
            <i class="el-icon-collection"></i>
          </div>
        </el-col>
        <el-col :span="10" :offset="0">
          <div style="font-size:20px;margin-top:40px;margin-left: 0px;font-weight :bold;color: #909399;" >
            <i class="el-icon-error"></i>
            <span>待审著作</span>
            <span class="numbertype">66</span>
          </div>
          <div style="font-size:20px;margin-top:20px;margin-left: auto;font-weight :bold;color: #909399;" >
            <i class="el-icon-success"></i>
            <span>已审著作</span>
            <span class="numbertype">66</span>
          </div>
          <div style="font-size:10px;margin-left: 10px;margin-top: 20px">
            <el-link @click="open" type="primary" :underline="true">查看待办事项详情</el-link>
          </div>
        </el-col>
      </el-card>
    </el-col>
  </el-row>
    <div style="margin-top: 10px;margin-left: 6px">
<searchchart></searchchart>
    </div>
  </div>
</template>


<script>

import searchchart from "@/components/charts/searchchart.vue";

export default {
  components: { searchchart },
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

.cardone{
  margin-left: 4px;
  width: 318px;
  height: 180px;
}

.numbertype{
  margin-left: 3px;
  color: #505255;
  font-weight: bold;
  font-size: 23px;
}

.el-icon-error{
  color: #ea0505;
}

.el-icon-success{
  color: #42b983;
}

.fontClass{
  margin-left: 30px;
  font-size:30px;
  color: black;
  font-weight: bolder;
  font-family: "微软雅黑";
}

.el-icon-tickets{
  margin-top: 20px;
  margin-left: 25px;
  font-size: 75px;
  color: #42b983;
}
.el-icon-s-grid{
  margin-top: 20px;
  margin-left: 25px;
  font-size: 75px;
  color: #2f96fa;
}

.el-icon-s-flag{
  margin-top: 20px;
  margin-left: 25px;
  font-size: 75px;
  color: #fa8b62;
}

.el-icon-collection{
  margin-top: 20px;
  margin-left: 25px;
  font-size: 75px;
  color: #fda132;
}
</style>