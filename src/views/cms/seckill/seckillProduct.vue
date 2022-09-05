<template>
  <div>
    <el-alert
        title="产品列表中设置秒杀产品"
        type="warning">
    </el-alert>
    <el-table :data="productList" border fit highlight-current-row style="width: 100%">

      <el-table-column align="center" label="图片" width="80">
        <template slot-scope="scope">
          <el-image :src="$target+scope.row.productPicture"/>
        </template>
      </el-table-column>

      <el-table-column align="center" label="产品名称">
        <template slot-scope="scope">
          <span>{{ scope.row.productName }}</span>
        </template>
      </el-table-column>

      <el-table-column align="center" label="原价">
        <template slot-scope="scope">
          {{ scope.row.productPrice }}
        </template>
      </el-table-column>

      <el-table-column align="center" label="秒杀价格">
        <template slot-scope="scope">
          {{ scope.row.seckillPrice }}
        </template>
      </el-table-column>

      <el-table-column align="center" label="数量">
        <template slot-scope="scope">
          {{ scope.row.seckillStock }}
        </template>
      </el-table-column>

      <el-table-column align="center" label="操作" width="180">
        <template slot-scope="scope">
          <div style="display: flex;justify-content: space-between">
            <router-link :to="'/cms/productEdit/'+scope.row.productId">
              <el-button type="primary" size="small" icon="el-icon-edit">
                编辑
              </el-button>
            </router-link>
            <el-button type="danger" size="small" icon="el-icon-delete" @click="handleDelete(scope.row.seckillId)">
              删除
            </el-button>
          </div>
        </template>
      </el-table-column>

    </el-table>
  </div>
</template>

<script>
export default {
  name: "seckillProduct",
  data() {
    return {
      timeId: null,
      productList: []
    }
  },
  mounted() {
    if (this.$route.params.id) {
      this.timeId = this.$route.params.id;
      this.loadData()
    } else {
      alert('错误')
    }
  },
  methods: {
    loadData() {
      this.$axios
          .get("/api/seckill/product/time/" + this.timeId)
          .then(res => {
            this.productList = res.data.data;
          })
          .catch(err => {
            return Promise.reject(err);
          });
    },
    handleDelete(seckillId) {
      this.$confirm('此操作将永久删除该信息, 是否继续?', '提示', {
        confirmButtonText: '确定',
        cancelButtonText: '取消',
        type: 'warning'
      }).then(async () => {
        this.$axios
            .delete(`/api/seckill/product/${seckillId}`)
            .then(() => {
              this.$message.success('删除成功!')
              this.loadData()
            })
            .catch(err => {
              return Promise.reject(err);
            });
      }).catch(() => {
      })
    }
  }
}
</script>

<style scoped>

</style>