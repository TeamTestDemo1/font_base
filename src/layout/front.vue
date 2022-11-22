<template>
  <div>
    <!--    头部-->
    <div style="width: 100%; height: 60px; line-height: 60px; background-color: white">



      <el-row>
        <el-col :span="4">
          <div style="font-size: 20px; font-weight: bold; color: #ffd04b; text-align: center">开放平台</div>
        </el-col>
        <el-col :span="14">
          <el-menu
              class="side-menu"
              :default-active="$route.path"
              @select="handleMenuSelect"
              mode="horizontal"
              active-text-color="#ffd04b"
              background-color="#545c64"
          >
            <el-menu-item index="/front/home">
              首页
            </el-menu-item>
            <el-menu-item index="/front/notice">
              公告
            </el-menu-item>
            <el-menu-item index="/front/message">
              聊天
            </el-menu-item>
            <el-menu-item index="/front/person">
              个人中心
            </el-menu-item>
          </el-menu>
        </el-col>
        <el-col :span="6">
          <div style="text-align: right; padding-right: 10px" v-if="user.id">
            <el-dropdown style="float: right;" @command="handleCommand">
              <span class="el-dropdown-link" style="cursor: pointer">
                用户{{ user.username }}<i class="el-icon-arrow-down el-icon--right"></i>
              </span>
              <el-dropdown-menu slot="dropdown">
                <el-dropdown-item command="person">个人信息</el-dropdown-item>
                <el-dropdown-item command="person">个人信息</el-dropdown-item>
                <el-dropdown-item command="loginOut">退出登录</el-dropdown-item>
              </el-dropdown-menu>
            </el-dropdown>
          </div>
          <div style="text-align: right; padding-right: 10px" v-else>
            <el-button @click="$router.replace('/login')">登录</el-button>
            <el-button @click="$router.replace('/register')">注册</el-button>
          </div>

        </el-col>
      </el-row>
    </div>

    <!-- 主体 -->
    <el-row>
      <el-col :span="16" :offset="4">
        <router-view />
      </el-col>
    </el-row>

    <div style="height: 60px; width: 100%; background-color:#2f65ef; padding: 10px 0; position: fixed; bottom: 0">
      <div style="text-align: center">
        Copyright © 2022 中南大学绩效考核
      </div>
    </div>

  </div>
</template>

<script>
import router from '@/router';

export default {
  name: "Front",
  data() {
    return {
      user: {}
    }
  },
  created() {
    this.user = sessionStorage.getItem("user") ? JSON.parse(sessionStorage.getItem("user")) : {}
  },
  methods: {
    handleCommand(command) {
      if (command === 'person') {
        this.$router.push('/front/person');
      }
      if (command === 'loginOut') {
        sessionStorage.removeItem("user")
        this.$router.replace('/login');

      }
    },
    handleMenuSelect(index) {
      console.log(index)
      this.$router.push(index);
    },
  }
}
</script>

<style scoped>

</style>
