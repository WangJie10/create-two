<template>
  <div class="home" id="home" name="home">
    <!-- 轮播图 -->
    <div class="block">
      <el-carousel height="460px">
        <el-carousel-item v-for="item in carousel" :key="item.carouselId">
          <img style="height:460px;" :src="$target +item.imgPath" :alt="item.describes" />
        </el-carousel-item>
      </el-carousel>
    </div>
    <!-- 轮播图END -->

    <div class="main-box">
      <div class="main">
        <!-- 手机商品展示区域 -->
        <div class="phone">
          <div class="box-hd">
            <div class="title">特卖产品</div>
          </div>
          <div class="box-bd">
            <div class="promo-list">
              <router-link to>
                <img :src="$target +'file/shoujitemai.jpg'" />
              </router-link>
            </div>
            <div class="list">
              <MyList :list="phoneList" :isMore="true"></MyList>
            </div>
          </div>
        </div>
        <!-- 手机商品展示区域END -->

        <!-- 电视商品展示区域 -->
        <div class="phone">
          <div class="box-hd">
            <div class="title">特色产品</div>
          </div>
          <div class="box-bd">
            <div class="promo-list">
              <router-link to>
                <img :src="$target +'/file/zhinengchuandai.jpg'" />
              </router-link>
            </div>
            <div class="list">
              <MyList :list="miChuanDaiList" :isMore="true"></MyList>
            </div>
          </div>
        </div>
        <!-- 电视商品展示区域END -->

        <!-- 配件商品展示区域 -->
        <div class="accessory" id="promo-menu">
          <div class="box-hd">
            <div class="title">热卖</div>
<!--            <div class="more" id="more">-->
<!--              <MyMenu :val="3" @fromChild="getChildMsg2">-->
<!--                <span slot="1">热门</span>-->
<!--                <span slot="2">保护套</span>-->
<!--                <span slot="3">充电器</span>-->
<!--              </MyMenu>-->
<!--            </div>-->
          </div>
          <div class="box-bd">
            <div class="promo-list">
              <ul>
                <li>
                  <img :src="$target +'file/jiadian1.jpg'" alt />
                </li>
                <li>
                  <img :src="$target +'file/jiadian2.jpg'" alt />
                </li>
              </ul>
            </div>
            <div class="list">
              <MyList :list="accessoryList" :isMore="true"></MyList>
            </div>
          </div>
        </div>
        <!-- 配件商品展示区域END -->
      </div>
    </div>
  </div>
</template>
<script>
export default {
  data() {
    return {
      carousel: "", // 轮播图数据
      phoneList: "", // 手机商品列表
      miChuanDaiList: "", // 小米电视商品列表
      accessoryList: "", //配件商品列表
      accessoryHotList: "", //热门配件商品列表
      protectingShellList: "", // 保护套商品列表
      chargerList: "", //充电器商品列表
      applianceActive: 1, // 家电当前选中的商品分类
      accessoryActive: 1 // 配件当前选中的商品分类
    };
  },
  watch: {
    accessoryActive: function(val) {
      // 页面初始化的时候把accessoryHotList(热门配件商品列表)直接赋值给accessoryList(配件商品列表)
      // 所以在切换商品列表时判断accessoryHotList是否为空,为空则是第一次切换,把accessoryList赋值给accessoryHotList
      if (this.accessoryHotList == "") {
        this.accessoryHotList = this.accessoryList;
      }
      if (val == 1) {
        // 1为热门商品
        this.accessoryList = this.accessoryHotList;
        return;
      }
      if (val == 2) {
        // 2为保护套商品
        this.accessoryList = this.protectingShellList;
        return;
      }
      if (val == 3) {
        //3 为充电器商品
        this.accessoryList = this.chargerList;
        return;
      }
    }
  },
  created() {
    // 获取轮播图数据
    this.$axios
      .get("/api/resources/carousel", {})
      .then(res => {
        this.carousel = res.data.data;
      })
      .catch(err => {
        return Promise.reject(err);
      });
    // 获取各类商品数据
    this.getPromo(1, "phoneList");
    this.getPromo(2, "miChuanDaiList");
    this.getPromo(5, "protectingShellList");
    this.getPromo(7, "chargerList");
    this.getHot(
      "accessoryList"
    );
  },
  methods: {
    // 获取配件模块子组件传过来的数据
    getChildMsg2(val) {
      this.accessoryActive = val;
    },
    // 获取各类商品数据方法封装
    getPromo(categoryId, val) {
      let api = "/api/product/category/limit/" + categoryId;
      this.$axios
        .get(api)
        .then(res => {
          this[val] = res.data.data;
        })
        .catch(err => {
          return Promise.reject(err);
        });
    },
    getHot(val) {
      let api = "/api/product/category/hot";
      this.$axios
        .get(api)
        .then(res => {
          this[val] = res.data.data;
        })
        .catch(err => {
          return Promise.reject(err);
        });
    }
  }
};
</script>
<style scoped>
@import "../assets/css/index.css";
</style>