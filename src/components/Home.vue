<template>
  <el-container>
    <el-header>
      <div id="logo-box">
        <img src="../assets/img/heima.png" alt>
        <span>黑马后台管理系统</span>
      </div>
      <el-button typr="info" @click="logout">退出</el-button>
    </el-header>
    <el-container>
      <el-aside :width="menushow ? '65px' : '200px'">
        <div
          style="background-color:rgb(74 ,80, 100);height:25px;line-height:25px;font-size:12px;text-align:center;color:white;letter-spacing:0.1em;user-select:none;cursor:pointer"
          @click="menushow=!menushow"
        >|||</div>
        <el-menu
          background-color="#333744"
          text-color="#fff"
          active-text-color="#409EFF"
          :unique-opened="true"
          :collapse="menushow"
          :collapse-transition="false"
          :router="true"
        >
          <el-submenu :index="item.id+''" v-for="(item,k) in menuList" :key="item.id"
          :style="menushow ? 'width:65px;' : 'width:200px;'">
            <template slot="title">
              <i :class="'iconfont icon-'+menuicon[k]"></i>
              <span>{{item.authName}}</span>
            </template>
            <el-menu-item
              :index="item2.path"
              v-for="item2 in item.children"
              :key="item2.id"
            >
              <i class="el-icon-menu"></i>
              <span>{{item2.authName}}</span>
            </el-menu-item>
          </el-submenu>
        </el-menu>
      </el-aside>
      <el-main>
        <router-view />
      </el-main>
    </el-container>
  </el-container>
</template>

<script>
export default {
  created() {
    this.getMenuList()
  },
  data() {
    return {
      menushow: false,
      menuList: [],
      menuicon: ['user', 'tijikongjian', 'shangpin', 'danju', 'baobiao']
    }
  },
  methods: {
    async getMenuList() {
      const { data: res } = await this.$http.get('/menus')
      if (res.meta.status !== 200) {
        return this.$message.error(res.meta.msg)
      }
      this.menuList = res.data
    },
    logout() {
      this.$confirm('确认要退出系统吗?', '提示', {
        confirmButtonText: '确定',
        cancelButtonText: '取消',
        type: 'warning'
      })
        .then(() => {
          window.sessionStorage.removeItem('token')
          this.$router.push('/login')
        })
        .catch(() => {})
    }
  }
}
</script>

<style lang="less" scoped>
.el-container {
  height: 100%;
  .el-header {
    background-color: rgb(55, 61, 65);
    padding: 0;
    padding-right: 20px;
    display: flex;
    align-items: center;
    justify-content: space-between;
    #logo-box {
      color: #fff;
      font-size: 22px;
      display: flex;
      align-content: center;
      user-select: none;
      line-height: 60px;
      img {
        width: 50px;
        height: 50px;
        margin: 10px 10px 10px 0;
      }
    }
  }
  .el-aside {
    background-color: #333744;
  }
  .el-main {
    background-color: #eaedf1;
  }
}
</style>
