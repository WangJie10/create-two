<template>
  <div id="cms">
    <div class="main">
      <el-row :gutter="20">
        <el-col :span="4">
          <el-menu
              background-color="#545c64"
              text-color="#fff"
              :router="true"
              :default-active="activeMenu"
              active-text-color="#ffd04b">

            <el-menu-item-group  v-if="this.$store.getters.getUser!=null&&this.$store.getters.getUser.username!=='admin'">
              <span slot="title">地址管理</span>
              <el-menu-item index="/cms/addressList">地址列表</el-menu-item>
              <el-menu-item index="/cms/addressCreate">新增地址</el-menu-item>
            </el-menu-item-group>


            <el-menu-item-group  v-if="this.$store.getters.getUser!=null&&this.$store.getters.getUser.username==='admin'">
              <span slot="title">产品管理</span>
              <el-menu-item index="/cms/productList">
                <span slot="title">产品列表</span>
              </el-menu-item>
              <el-menu-item index="/cms/productCreate">新增产品</el-menu-item>
            </el-menu-item-group>

            <el-menu-item-group  v-if="this.$store.getters.getUser!=null&&this.$store.getters.getUser.username==='admin'">
              <span slot="title">秒杀管理</span>
              <el-menu-item index="/cms/seckillTime">
                <span slot="title">秒杀设置</span>
              </el-menu-item>
            </el-menu-item-group>

          </el-menu>
        </el-col>
        <el-col :span="20" style="background-color: #f9f9f9;padding: 10px">
          <router-view/>
        </el-col>
      </el-row>
    </div>
  </div>
</template>

<script>
export default {
  name: "cms",
  computed: {
    activeMenu() {
      const route = this.$route
      const {meta, path} = route
      // if set path, the sidebar will highlight the path you set
      if (meta.activeMenu) {
        return meta.activeMenu
      }
      return path
    },
  }
}
</script>

<style scoped>
/* 主要内容CSS */
#cms .main {
  width: 1225px;
  height: 560px;
  padding-top: 30px;
  margin: 0 auto;
}
</style>