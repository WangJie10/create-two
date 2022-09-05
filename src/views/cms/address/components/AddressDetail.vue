<template>
  <div>
    <el-form ref="postForm" :model="form" :rules="rules" class="form-container" label-width="100px">
      <el-row>
        <el-col :span="12">
          <el-form-item label="用户名:" prop="username">
            <el-input v-model="form.username"/>
          </el-form-item>
        </el-col>
      </el-row>
      <el-row>
        <el-col :span="12">
          <el-form-item label="电话:" prop="phone">
            <el-input v-model="form.phone"/>
          </el-form-item>
        </el-col>
      </el-row>
      <el-row>
        <el-col :span="12">
          <el-form-item label="地址:" prop="adCode">
            <area-select v-model="form.adCode" @addressText="setAddressText"></area-select>
          </el-form-item>
        </el-col>
      </el-row>
      <el-row>
        <el-col :span="12">
          <el-form-item label="详细地址:" prop="addressDetail">
            <el-input v-model="form.addressDetail"/>
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

export default {
  props: {
    isEdit: {
      type: Boolean,
      default: false
    }
  },
  data() {
    return {
      form: {},
      loading: false,
      rules: {
        username: [{required: true, message: '必填', trigger: 'change'}],
        phone: [{required: true, message: '必填', trigger: 'change'}],
        adCode: [{required: true, message: '必填', trigger: 'change'}],
        addressDetail: [{required: true, message: '必填', trigger: 'change'}]
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
    setAddressText(val) {
      this.form.address = val
    },
    // 获取商品详细信息
    fetchData(id) {
      this.$axios
          .get("/api/address/" + id)
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
                .put("/api/address", form)
                .then(() => {
                  this.$notify({
                    title: '成功',
                    message: '更新成功',
                    type: 'success',
                    duration: 2000
                  })
                  this.$router.push({path: '/cms/addressList'})
                })
                .catch(err => {
                  return Promise.reject(err);
                });
          } else {
            this.$axios
                .post("/api/address", form)
                .then(() => {
                  this.$notify({
                    title: '成功',
                    message: '发布成功',
                    type: 'success',
                    duration: 2000
                  })
                  this.$router.push({path: '/cms/addressList'})
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
