<template>

  <div>
    #  这是增删查改系统用户的页面,超级管理员可查看
    <div style="padding: 5px 0">
      <el-input v-model="text" @keyup.enter.native="load" style="width: 200px"> <i slot="prefix" class="el-input__icon el-icon-search"></i></el-input>
      <el-button @click="add" size="small" style="margin-left: 10px ;background-color: #ffd04b">新增</el-button>
    </div>
    <el-table :data="tableData" border stripe style="width: 100%">
      <el-table-column prop="id" label="ID"></el-table-column>
      <el-table-column prop="username" label="用户名（账号）"></el-table-column>
      <el-table-column prop="name" label="姓名"></el-table-column>
      <el-table-column prop="uid" label="学号"></el-table-column>
      <el-table-column prop="phone" label="电话"></el-table-column>
      <el-table-column
          label="角色">
        <template slot-scope="scope">
          <el-select v-model="scope.row.role" value-key="id" multiple placeholder="请选择" @change="changeRole(scope.row)">
            <el-option
                v-for="item in options"
                :key="item.id"
                :label="item.name"
                :value="item.id">
            </el-option>
          </el-select>
        </template>
      </el-table-column>
      <el-table-column
          fixed="right"
          label="操作"
          width="200">
        <template slot-scope="scope">
          <el-button type="primary" icon="el-icon-edit" circle @click="edit(scope.row)"></el-button>
          <el-popconfirm
              @confirm="del(scope.row.id)"
              title="确定删除？"
          >
            <el-button type="danger" icon="el-icon-delete" circle slot="reference"
                       style="margin-left: 10px"></el-button>
          </el-popconfirm>
        </template>
      </el-table-column>
    </el-table>
    <div style="margin-top: 10px">
      <el-pagination
          @size-change="handleSizeChange"
          @current-change="handleCurrentChange"
          :current-page="pageNum"
          :page-size="pageSize"
          :page-sizes="[2, 5, 10]"
          layout="total, sizes, prev, pager, next, jumper"
          :total="total"
      >
      </el-pagination>
    </div>

    <!-- 弹窗   -->
    <el-dialog title="用户信息" :visible.sync="dialogFormVisible" width="30%"
               :close-on-click-modal="false" :close-on-press-escape="false" :show-close="false">
      <el-form :model="entity">
        <el-form-item label="工号" label-width="120px" v-if="!entity.id">
          <el-input v-model="entity.username" autocomplete="off" style="width: 80%"></el-input>
        </el-form-item>
<!--        <el-form-item label="昵称" label-width="120px">-->
<!--          <el-input v-model="entity.nickName" autocomplete="off" style="width: 80%"></el-input>-->
<!--        </el-form-item>-->
<!--        <el-form-item label="学号" label-width="120px">-->
<!--          <el-input v-model="entity.uid" autocomplete="off" style="width: 80%"></el-input>-->
<!--        </el-form-item>-->
        <el-form-item label="电话" label-width="120px">
          <el-input v-model="entity.phone" autocomplete="off" style="width: 80%"></el-input>
        </el-form-item>
<!--        <el-form-item label="地址" label-width="120px">-->
<!--          <el-input v-model="entity.address" autocomplete="off" style="width: 80%"></el-input>-->
<!--        </el-form-item>-->
        <el-form-item label="role" label-width="120px">
          <el-input v-model="entity.role" autocomplete="off" style="width: 80%"></el-input>
        </el-form-item>
      </el-form>
      <div slot="footer" class="dialog-footer">
        <el-button @click="dialogFormVisible = false">取 消</el-button>
        <el-button type="primary" @click="save">确 定</el-button>
      </div>
    </el-dialog>
  </div>
</template>

<script>
import API from '../../utils/request'

const url = "/api/user/"

export default {
  name: "User",
  data() {
    return {
      options: [],
      text: '',
      user: {},
      tableData: [],
      pageNum: 1,
      pageSize: 10,
      total: 0,
      entity: {},
      dialogFormVisible: false
    };
  },
  created() {
     this.user = sessionStorage.getItem("user") ? JSON.parse(sessionStorage.getItem("user")) : {}
    this.load()
  },
  methods: {
    changeRole(row) {
      this.entity = JSON.parse(JSON.stringify(row));
      this.save();
    },
    handleSizeChange(pageSize) {
      this.pageSize = pageSize
      this.load()
    },
    handleCurrentChange(pageNum) {
      this.pageNum = pageNum
      this.load()
    },
    load() {
      API.get(url + "/page", {
        params: {
          pageNum: this.pageNum,
          pageSize: this.pageSize,
          name: this.text
        }
      }).then(res => {
        this.tableData = res.data.records || []
        this.total = res.data.total
      })

      API.get("/api/role").then(res => {
        this.options = res.data
      })
    },
    add() {
      this.entity = {}
      this.dialogFormVisible = true
    },
    edit(obj) {
      this.entity = JSON.parse(JSON.stringify(obj))
      this.dialogFormVisible = true
    },
    save() {
      if (!this.entity.id) {
        API.post(url, this.entity).then(res => {
          if (res.code === '0') {
            this.$message({
              type: "success",
              message: "操作成功"
            })
          } else {
            this.$message({
              type: "error",
              message: res.msg
            })
          }
          this.load()
          this.dialogFormVisible = false
        })
      } else {
        API.put(url, this.entity).then(res => {
          if (res.code === '0') {
            this.$message({
              type: "success",
              message: "操作成功"
            })
          } else {
            this.$message({
              type: "error",
              message: res.msg
            })
          }
          this.load()
          this.dialogFormVisible = false
        })
      }
    },
    del(id) {
      API.delete(url + id).then(res => {
        if (res.code === '0') {
          this.$message({
            type: "success",
            message: "操作成功"
          })
        } else {
          this.$message({
            type: "error",
            message: res.msg
          })
        }
        this.load()
      })
    }
  },
};
</script>

<style scoped>
</style>
