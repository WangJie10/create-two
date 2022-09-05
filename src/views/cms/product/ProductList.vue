<template>
  <div>
    <el-table v-loading="listLoading" :data="list" border fit highlight-current-row style="width: 100%">

      <el-table-column align="center" label="图片" width="80">
        <template slot-scope="scope">
          <el-image :src="$target+scope.row.productPicture"/>
        </template>
      </el-table-column>

      <el-table-column align="center" label="产品名称">
        <template slot-scope="scope">
          <router-link :to="'/product/product/edit/'+scope.row.id" class="link-type">
            <span>{{ scope.row.productName }}</span>
          </router-link>
        </template>
      </el-table-column>

      <el-table-column align="center" label="原价">
        <template slot-scope="scope">
          {{ scope.row.productPrice }}
        </template>
      </el-table-column>

      <el-table-column align="center" label="售价">
        <template slot-scope="scope">
          {{ scope.row.productSellingPrice }}
        </template>
      </el-table-column>

      <el-table-column align="center" label="秒杀设置">
        <template slot-scope="scope">
          <el-link type="primary" @click="seckillProduct=scope.row;form={};dialogVisible=true">设置</el-link>
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
            <el-button type="danger" size="small" icon="el-icon-delete" @click="handleDelete(scope.row.productId)">
              删除
            </el-button>
          </div>
        </template>
      </el-table-column>

    </el-table>
    <!-- 分页 -->
    <div style="display: flex;justify-content:center;margin-top: 20px">
      <div>
        <el-pagination
            background
            layout="prev, pager, next"
            :page.sync="listQuery.page"
            :page-size.sync="listQuery.size"
            :total="total"
            @current-change="getList"
        ></el-pagination>
      </div>
    </div>

    <el-dialog
        title="秒杀设置"
        :visible.sync="dialogVisible"
        width="600px">
      <el-form ref="postForm" :model="form" :rules="rules" class="form-container" label-width="100px">
        <el-row>
          <el-col :span="24">
            <el-form-item label="时间:" prop="timeId">
              <seckill-time-option v-model="form.timeId"/>
            </el-form-item>
          </el-col>
        </el-row>
        <el-row>
          <el-col :span="24">
            <el-form-item label="抢购价:" prop="seckillPrice">
              <el-input v-model="form.seckillPrice"/>
            </el-form-item>
          </el-col>
        </el-row>
        <el-row>
          <el-col :span="24">
            <el-form-item label="抢购数量:" prop="seckillStock">
              <el-input v-model="form.seckillStock"/>
            </el-form-item>
          </el-col>
        </el-row>
      </el-form>
      <span slot="footer" class="dialog-footer">
        <el-button @click="dialogVisible = false">取 消</el-button>
        <el-button type="primary" @click="submitSeckillProduct">确 定</el-button>
      </span>
    </el-dialog>
  </div>
</template>

<script>
import SeckillTimeOption from './components/SeckillTimeOption'

export default {
  name: 'ProductList',
  components: {SeckillTimeOption},
  data() {
    return {
      list: null,
      total: 0,
      listLoading: true,
      listQuery: {
        page: 1,
        size: 10
      },
      form: {},
      rules: {
        timeId: [{required: true, message: '必填', trigger: 'change'}],
        seckillPrice: [{required: true, message: '必填', trigger: 'change'}],
        seckillStock: [{required: true, message: '必填', trigger: 'change'}]
      },
      dialogVisible: false,
      seckillProduct: {}
    }
  },
  created() {
    this.getList()
  },
  methods: {
    async getList(currentPage = 1) {
      this.listQuery.page = currentPage
      this.listLoading = true
      let api = "/api/product/page/" + this.listQuery.page + "/" + this.listQuery.size + '/0';
      this.$axios
          .get(api)
          .then(res => {
            this.list = res.data.data;
            this.total = res.data.total;
            this.listLoading = false
          })
          .catch(err => {
            return Promise.reject(err);
          });
    },
    handleDelete(productId) {
      this.$confirm('此操作将永久删除该信息, 是否继续?', '提示', {
        confirmButtonText: '确定',
        cancelButtonText: '取消',
        type: 'warning'
      }).then(async () => {
        this.$axios
            .delete(`/api/product/${productId}`)
            .then(() => {
              this.$message.success('删除成功!')
              this.getList()
            })
            .catch(err => {
              return Promise.reject(err);
            });
      }).catch(() => {
      })
    },
    submitSeckillProduct() {
      this.$refs.postForm.validate(async valid => {
        if (valid) {
          this.loading = true
          const form = JSON.parse(JSON.stringify(this.form))
          form.productId = this.seckillProduct.productId
          this.$axios
              .post("/api/seckill/product", form)
              .then(() => {
                this.$notify({
                  title: '成功',
                  message: '设置成功',
                  type: 'success',
                  duration: 2000
                })
                this.dialogVisible=false
              })
              .catch(err => {
                return Promise.reject(err);
              });
          this.loading = false
        } else {
          console.log('error submit!!')
          return false
        }
      })
    }
  }
}
</script>

<style scoped>

</style>
