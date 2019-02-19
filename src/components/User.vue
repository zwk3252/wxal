<template>
  <div>
    <el-breadcrumb separator-class="el-icon-arrow-right">
      <el-breadcrumb-item :to="{ path: '/' }">首页</el-breadcrumb-item>
      <el-breadcrumb-item>用户管理</el-breadcrumb-item>
      <el-breadcrumb-item>用户列表</el-breadcrumb-item>
    </el-breadcrumb>
    <el-card class="box-card">
      <el-row :gutter="20">
        <el-col :span="8">
          <el-input
            placeholder="请输入搜索内容"
            v-model="querycdt.query"
            :clearable="true"
            @clear="getUserInfos"
            @keyup.enter.native="getUserInfos"
          >
            <el-button slot="append" icon="el-icon-search" @click="getUserInfos"></el-button>
          </el-input>
        </el-col>
        <el-col :span="7">
          <el-button type="primary">添加用户</el-button>
        </el-col>
      </el-row>
      <el-table :data="userInfos" border style="width: 80%">
        <el-table-column type="index" label="序号" width="60"></el-table-column>
        <el-table-column prop="username" label="用户名"></el-table-column>
        <el-table-column prop="mobile" label="手机号码" width="110"></el-table-column>
        <el-table-column prop="role_name" label="角色" width="120"></el-table-column>
        <el-table-column prop="email" label="邮箱" width="160"></el-table-column>
        <el-table-column prop="mg_state" label="状态" width="70">
          <!-- <span slot="info" > {{info.row.username}} </span> -->
          <el-switch
            v-model="info.row.mg_state"
            slot-scope="info"
            @change="changeState(info.row.id, info.row.mg_state)"
          ></el-switch>
        </el-table-column>
        <el-table-column label="操作" width="230">
          <el-button type="primary" icon="el-icon-edit" size="mini"></el-button>
          <el-button type="danger" icon="el-icon-delete" size="mini"></el-button>
          <el-tooltip content="分配角色" placemant="top" :enterable="false">
            <el-button type="warning" icon="el-icon-setting" size="mini"></el-button>
          </el-tooltip>
        </el-table-column>
      </el-table>
      <el-pagination
        @size-change="handleSizeChange"
        @current-change="handleCurrentChange"
        :current-page="querycdt.pagenum"
        :page-sizes="[3 ,5 ,10, 20]"
        :page-size="querycdt.pagesize"
        layout="total, sizes, prev, pager, next, jumper"
        :total="querycdt.tot"
      ></el-pagination>
    </el-card>
  </div>
</template>

<script>
export default {
  created() {
    this.getUserInfos()
  },
  methods: {
    async changeState(uid, state) {
      const { data: res } = await this.$http.put(
        'users/' + uid + '/state/' + state
      )
      if (res.meta.status !== 200) {
        return this.$message.error(res.meta.msg)
      }
      this.$message.success(res.meta.msg)
    },
    handleSizeChange(arg) {
      this.querycdt.pagesize = arg
      this.getUserInfos()
    },
    handleCurrentChange(arg) {
      this.querycdt.pagenum = arg
      this.getUserInfos()
    },
    async getUserInfos() {
      const { data: res } = await this.$http.get('users', {
        params: this.querycdt
      })
      if (res.meta.status !== 200) {
        return this.$message.error(res.meta.msg)
      }
      this.querycdt.tot = res.data.total
      this.userInfos = res.data.users
    }
  },
  data() {
    return {
      userInfos: [],
      querycdt: {
        query: '',
        pagenum: 1,
        pagesize: 3,
        tot: 0
      }
    }
  }
}
</script>

<style lang="less" scoped>
</style>
