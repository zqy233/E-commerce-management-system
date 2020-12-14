<template>
  <el-container class="home-container">
    <!-- 头部区域 -->
    <el-header>
      <div>
        <img src="../assets/imgs/8.png" alt="">
        <span>后台管理系统</span>
      </div>
      <el-button type="info" @click="logout">退出</el-button>
    </el-header>
    <!-- 页面主题区域 -->
    <el-container>
      <!-- 侧边栏 -->
      <el-aside :width="isCollapse?'64px':'200px'">
        <div class="toggle-button" @click="toggleCollapse">|||</div>
        <!-- 侧边栏菜单区域 -->
        <el-menu background-color="#333744" text-color="#fff"
         :unique-opened="true"
          :collapse="isCollapse"
          :collapse-transition="false"
          active-text-color="#409EFF" :router="true"
          :default-active="$route.path">
          <!-- 一级菜单 -->
          <el-submenu :index="item.id+''" v-for="item in menuList" :key="item.id" >
            <!-- 一级菜单的模板区域 -->
            <template slot="title">
              <!-- 图标 -->
              <i :class="iconsObj[item.id]"></i>
              <!-- 文本 -->
              <span>{{item.authName}}</span>
            </template>

            <!-- 二级菜单 -->
            <el-menu-item :index="'/'+subItem.path+''" v-for="subItem in item.children" :key="subItem.id"
            @click="SaveNavState('/'+subItem.path)">
              <!-- 二级菜单的模板区域 -->
              <template slot="title">
                <!-- 图标 -->
                <i class="el-icon-menu"></i>
                <!-- 文本 -->
                <span>{{subItem.authName}}</span>
              </template>
            </el-menu-item>
          </el-submenu>
        </el-menu>
      </el-aside>
      <!-- 右侧内容 -->
      <el-main>
<router-view></router-view>
      </el-main>
    </el-container>
  </el-container>
</template>

<script>
export default {
  data() {
    return {
      menuList: [],
      // 菜单栏icon
      iconsObj: {
        125: 'iconfont icon-user',
        103: 'iconfont icon-tijikongjian',
        101: 'iconfont icon-shangpin',
        102: 'iconfont icon-danju',
        145: 'iconfont icon-baobiao'
      },
      // 是否折叠
      isCollapse: false

    }
  },
  // 获取菜单
  created() {
    this.getMenuList()
  },
  methods: {
    // 清除token
    logout() {
      window.sessionStorage.clear()
      this.$message.info('退出成功')
      this.$router.push('/login')
    },
    // 获取所有菜单名
    async getMenuList() {
      const {
        data: res
      } = await this.$http.get('menus') // eslint-disable-line no-unused-vars
      console.log(res)
      if (res.meta.status !== 200) return this.$message.error(res.meta.msg)
      this.menuList = res.data
    },
    // 点击按钮,切换显示模式
    toggleCollapse() {
      this.isCollapse = !this.isCollapse
    },
    SaveNavState(activePath) {
      window.sessionStorage.setItem('activePath', activePath)
    }

  }
}
</script>

<style lang="less" scoped>
  .home-container {
    height: 100%;
  }

  .el-header {
    background-color: #333744;
    display: flex;
    justify-content: space-between;
    padding-left: 0;
    align-items: center;
    color: #FFFDEF;
    font-size: 20px;

    >div {
      display: flex;
      align-items: center;
    }

    span {
      margin-left: 15px;
    }
  }

  .el-header img {
    width: 120px;
    height: 65px;
  }

  .el-aside {
    background-color: #333744;

    .el-menu {
      border-right: none;
    }
  }

  .el-main {
    background-color:#e5e5e5;
  }

  .iconfont {
    margin-right: 10px;
  }

  .toggle-button {
    background-color: #4A5064;
    font-size: 10px;
    line-height: 24px;
    color: #fff;
    text-align: center;
    letter-spacing: 0.2em;
    cursor: pointer;
  }
</style>
