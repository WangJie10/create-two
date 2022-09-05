<template>
  <div>
    <el-form ref="postForm" :model="form" :rules="rules" class="form-container" label-width="80px">
      <el-row>
        <el-col :span="12">
          <el-form-item label="名称:" prop="productName">
            <el-input v-model="form.productName"/>
          </el-form-item>
        </el-col>
      </el-row>
      <el-row>
        <el-col :span="12">
          <el-form-item label="标题:" prop="productTitle">
            <el-input v-model="form.productTitle"/>
          </el-form-item>
        </el-col>
        <el-col :span="6">
          <el-form-item label="分类:" prop="categoryId">
            <category-option v-model="form.categoryId"/>
          </el-form-item>
        </el-col>
      </el-row>
      <el-row>
        <el-col :span="6">
          <el-form-item label="标价:" prop="productPrice">
            <el-input v-model="form.productPrice"/>
          </el-form-item>
        </el-col>
        <el-col :span="6">
          <el-form-item label="售价:" prop="productSellingPrice">
            <el-input v-model="form.productSellingPrice"/>
          </el-form-item>
        </el-col>
        <el-col :span="6">
          <el-form-item label="数量:" prop="productNum">
            <el-input v-model="form.productNum"/>
          </el-form-item>
        </el-col>
      </el-row>
      <el-row>
        <el-col :span="24">
          <el-form-item label="主图:" prop="productName">
            <upload-img v-model="form.productPicture"/>
          </el-form-item>
        </el-col>
      </el-row>
      <el-row>
        <el-col :span="24">
          <el-form-item label="介绍:" prop="productIntro">
            <el-input type="textarea" :rows="8" v-model="form.productIntro"/>
          </el-form-item>
        </el-col>
      </el-row>
      <el-row>
        <el-col :span="24">
          <el-form-item label="">
            <el-button type="primary" @click="submitForm" v-loading="loading">保存</el-button>
          </el-form-item>
        </el-col>
      </el-row>
    </el-form>
  </div>
</template>

<script>
import CategoryOption from './CategoryOption'
import UploadImg from '../../../../components/SingleImage'

export default {
  props: {
    isEdit: {
      type: Boolean,
      default: false
    }
  },
  components: {CategoryOption, UploadImg},
  data() {
    return {
      form: {},
      loading: false,
      rules: {
        productName: [{required: true, message: '必填', trigger: 'change'}],
        categoryId: [{required: true, message: '必填', trigger: 'change'}],
        productPicture: [{required: true, message: '必填', trigger: 'change'}],
        productPrice: [{required: true, message: '必填', trigger: 'change'}],
        productSellingPrice: [{required: true, message: '必填', trigger: 'change'}],
        productNum: [{required: true, message: '必填', trigger: 'change'}],
        productIntro: [{required: true, message: '必填', trigger: 'change'}]
      }
    }
  },
  created() {
    if (this.isEdit) {
      const id = this.$route.params && this.$route.params.id
      this.fetchData(id)
    }
  },
  methods: {
    // 获取商品详细信息
    fetchData(id) {
      this.$axios
          .get("/api/product/" + id)
          .then(res => {
            this.form = res.data.data;
          })
          .catch(err => {
            return Promise.reject(err);
          });
    },
    submitForm() {
      this.$refs.postForm.validate(async valid => {
        if (valid) {
          this.loading = true
          const form = JSON.parse(JSON.stringify(this.form))
          // 编辑
          if (this.isEdit) {
            this.$axios
                .put("/api/product", form)
                .then(res => {
                  this.$notify({
                    title: '成功',
                    message: '更新成功',
                    type: 'success',
                    duration: 2000
                  })
                  console.log(res)
                  this.$router.push({path: '/cms/productList'})
                })
                .catch(err => {
                  return Promise.reject(err);
                });
          } else {
            this.$axios
                .post("/api/product", form)
                .then(res => {
                  this.$notify({
                    title: '成功',
                    message: '发布成功',
                    type: 'success',
                    duration: 2000
                  })
                  console.log(res)
                  this.$router.push({path: '/cms/productList'})
                })
                .catch(err => {
                  return Promise.reject(err);
                });
          }
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
