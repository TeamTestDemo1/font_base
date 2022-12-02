<template>
  <div style="background: #ededed">
    <!--        奖励-->
    <el-col :span="6">
      <el-card shadow="hover" class="cardone" :body-style="{  padding: '0px' }"  >
        <div  class="board-column-header" style=" background: #0696ab;">
          <span >奖励</span>
        </div>
        <div v-for="o in 4" :key="o" class="board-item">
          {{'列表内容 ' + o }}
        </div>

        <!--                数据连接后展示-->
        <!--                <div v-for="(item) in tableData" :key="item.id" class="board-item">-->

        <!--                    {{item.award_type}}}-->
        <!--                </div>-->

        <div style="text-align: center;margin: auto;margin-bottom: 10px;margin-top: 10px">
          <el-button style="background-color: #0696ab;color: white" round @click="turnaward()"  >查看详情</el-button>
        </div>
      </el-card>
    </el-col>
    <!--著作-->
    <el-col :span="6">
      <el-card shadow="hover" class="cardone" :body-style="{  padding: '0px' }"  >
        <div  class="board-column-header" style=" background: #06a0ab;">
          <span >著作</span>
        </div>
        <div v-for="o in 4" :key="o" class="board-item">
          {{'列表内容 ' + o }}
        </div>
        <div style="text-align: center;margin: auto;margin-bottom: 10px;margin-top: 10px">
          <el-button style="background-color:#06a0ab;color: white" round @click="turnaward()"  >查看详情</el-button>
        </div>
      </el-card>
    </el-col>
    <!--党建-->
    <el-col :span="6">
      <el-card shadow="hover" class="cardone" :body-style="{  padding: '0px' }"  >
        <div  class="board-column-header" style=" background: #ab2029;">
          <span >党建</span>
        </div>
        <div v-for="o in 4" :key="o" class="board-item">
          {{'列表内容 ' + o }}
        </div>
        <div style="text-align: center;margin: auto;margin-bottom: 10px;margin-top: 10px">
          <el-button style="background-color: #ab2029;color: white" round @click="turnaward()"  >查看详情</el-button>
        </div>
      </el-card>
    </el-col>

    <el-col :span="6">
      <el-card shadow="hover" class="cardone" :body-style="{  padding: '0px' }"  >
        <div  class="board-column-header" style=" background: #ab2029;">
          <span >党建</span>
        </div>
        <div v-for="o in 4" :key="o" class="board-item">
          {{'列表内容 ' + o }}
        </div>
        <div style="text-align: center;margin: auto;margin-bottom: 10px;margin-top: 10px">
          <el-button style="background-color: #ab2029;color: white" round @click="turnaward()"  >查看详情</el-button>
        </div>
      </el-card>
    </el-col>
  </div>




</template>

<script>
export default {
  name: 'award',
  data() {
    return {
      tableData: null,
      listdata: {}
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
    load() {
      API.get(url + "/page", {
        params: {
          pageNum: this.pageNum,
          pageSize: this.pageSize,
          name: this.text,
          leaderName: this.username,
          // leaderName:this.user.username 用于教师查看
        }
      }).then(res => {
        this.tableData = res.data.records || []
        this.total = res.data.total
      })
    },
  }
}
</script>

<style>

/*卡片设置*/
.cardone{
  min-width: 270px;
  min-height: 100px;
  height: auto;
  overflow: hidden;
  background: white;
  border-radius: 25px;
  margin-left: 15px;
}

/*设置卡片标题*/
.board-column-header {
  height: 50px;
  line-height: 50px;
  overflow: hidden;
  padding: 0 20px;
  text-align: center;
  border-radius: 3px 3px 0 0;
  font-family: "微软雅黑";
  font-size:20px;
  color: white;
  font-weight: bolder;
}
/*内容设置*/
/*.textitem{*/
/*    height: auto;*/
/*    overflow: hidden;*/
/*    border: 10px solid transparent;*/
/*    min-height: 60px;*/
/*    display: flex;*/
/*    justify-content: flex-start;*/
/*    flex-direction: column;*/
/*    align-items: flex-start;*/
/*}*/
.board-item {
  width: 100%;
  height: 64px;
  margin: 5px 0;
  background-color: #ffffff;
  text-align: left;
  line-height: 54px;
  padding: 5px 10px;
  box-sizing: border-box;
  /*box-shadow: 0px 1px 3px 0 rgba(0, 0, 0, 0.2);*/
}



</style>