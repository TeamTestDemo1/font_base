<template>

  <div class="board">
    <el-col :span="8"style="margin-left: 12px">
      <el-card>
      <el-row>
        <div style="height: 150px;margin-left: 120px">
          <el-avatar :size="130" :src="circleUrl"></el-avatar>
        </div>
      </el-row>
        <el-row style="margin-left: 157px">
         #biwenjie
        </el-row>
        <el-divider></el-divider>
        <el-row style="margin-left: 10px">
        <el-descriptions title="用户信息":column="1" :size="size" label-style="font-size: 15px;font-weight:bolder" contentStyle="font-size: 15px">
          <el-descriptions-item label="职工号">000000</el-descriptions-item>
          <el-descriptions-item label="系">信息管理与信息系统</el-descriptions-item>
          <el-descriptions-item label="邮箱">2333@csu.com</el-descriptions-item>
        </el-descriptions>
        <el-divider></el-divider>
        </el-row>
       <el-row>
          <div id="main" style="width: 400px;height: 320px"></div>
       </el-row>
      </el-card>
    </el-col>

    <el-col :span="15"style="margin-left: 25px">
      <el-row>
      <child1></child1>
      </el-row>
      <el-row style="margin-top: 10px">
      <el-card>
        <div>
          <h3>系统公告</h3>
        </div>
        <el-table
                :data="notices"
                height="250"
                border
                style="width: 100%">
          <el-table-column
                  prop="title"
                  label="标题"
                  width="220">
          </el-table-column>
          <el-table-column
                  prop="time"
                  label="发表时间"
                  width="220">
          </el-table-column>
          <el-table-column
                  fixed="right"
                  label="操作"
                  width="150">
            <template slot-scope="scope">
              <el-button @click="opendrawer(scope.row)" type="primary" style="margin-left: 16px;">
                查看详情
              </el-button>
            </template>
          </el-table-column>
        </el-table>
      </el-card>
    </el-row>
    </el-col>
    <el-drawer
            title= ""
            :visible.sync="drawer">
      <span slot="title">{{entity.title}}</span>
      <div style="margin-left:20px">
      <span>{{entity.content}}</span>
      </div>
    </el-drawer>

  </div>
</template>

<script>
import API from '../../utils/request'
import * as echarts from 'echarts';
import mixcharts from '../../components/charts/mixcharts.vue'


const url = "/api/user/"

export default {
  name: "Home",
  data() {
    return {
      notices: [],
      active: 0,
      user: {},
      drawer: false,
      entity: {},
      circleUrl: "https://cube.elemecdn.com/3/7c/3ea6beec64369c2642b92c6726f1epng.png",
    };
  },
  components:{
    child1:mixcharts,
  },
  mounted() {
    this.user = sessionStorage.getItem("user") ? JSON.parse(sessionStorage.getItem("user")) : {}
    this.load()
    var chartDom = document.getElementById('main');
    var myChart = echarts.init(chartDom);
    var option = {
      title: {
        text: '工作量统计',
        left: 'center'
      },
      tooltip: {
        trigger: 'item',
        formatter: '{a} <br/>{b}: {c} ({d}%)'
      },
      series: [
        {
          name: '总工作量',
          type: 'pie',
          radius: ['0%', '25%'],
          label: {
            fontFamily: "sans-serif",
            color: "#fff",

            fontWeight: 'bold',
            normal: {
              show: true,
              position: 'center',
              formatter: '{b}\n  {c}',
              fontSize: 17,
            }
          },
          labelLine: {
            show: false
          },
          data: [
            {value: 5000, name: '总工作量'}
          ],
          color: ["#f5f5f5"],
        },
        {
          name: '分数',
          type: 'pie',
          selectedMode: 'single',
          radius: ['25%', '45%'],
          label: {
            position: 'inner',
            fontSize: 14,
          },
          labelLine: {
            show: false
          },
          data: [
            {value: 1548, name: '教学'},
            {value: 775, name: '科研'},
          ],
          color: [
            "#6be6c1",
            "#066eab",

          ],
        },
        {
          name: '分数',
          type: 'pie',
          radius: ['55%', '70%'],
          labelLine: {
            length: 0
            //show:false
          },
          data: [
            {value: 1048, name: '课程'},
            {value: 335, name: '指导学生'},
            {value: 310, name: '论文'},
            {value: 251, name: '项目'},
            {value: 234, name: '奖项'},
            {value: 147, name: '著作'},
          ],
          color: [
            "#6be6c1",
            "#7bd9a5",
            "#066eab",
            "#0682ab",
            "#0696ab",
            "#06a0ab",
          ],
        }
      ],
    };
    myChart.setOption(option)
  },
  methods: {
    load() {
      API.get("/api/notice/").then((res) => {
        this.notices = res.data;
      })
    },
    drawLine() {
      const pieOption = {
        title: {text: '用户地区分布统计'},
        tooltip: {trigger: 'item'},
        legend: {top: '0', left: 'right'},
        series: [
          {
            name: '访问来源',
            type: 'pie',
            radius: ['40%', '70%'],
            itemStyle: {
              borderRadius: 10,
              borderColor: '#fff',
              borderWidth: 2
            },
            data: [
            ]
          }
        ]
      };
      // 基于准备好的dom，初始化echarts实例
      let myChart = this.$echarts.init(document.getElementById('pie'))
      API.get("/api/user").then(res => {
        const address = Array.from(new Set(res.data.map(v => v.address)))
        address.forEach(item => {
          pieOption.series[0].data.push({name: item, value: res.data.filter(v => v.address === item).length})
        })
        // 绘制图表
        myChart.setOption(pieOption);
      })
    },
    opendrawer(row) {
      console.log(row);
      this.entity = JSON.parse(JSON.stringify(row))
      this.drawer = true
    },
  },
};
</script>

<style scoped>
  .board {
    background-color: #ededed;
  }
</style>
