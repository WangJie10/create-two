<template>
  <div style="display:inline-block;">
    <el-select v-model="option" placeholder="请选择">
      <el-option
          v-for="item in options"
          :key="item.categoryId"
          :label="item.categoryName"
          :value="item.categoryId">
      </el-option>
    </el-select>
  </div>
</template>

<script>
export default {
  props: {
    value: {
      type: Number
    }
  },
  data() {
    return {
      options: []
    }
  },
  computed: {
    option: {
      get() {
        return this.value
      },
      set(val) {
        this.$emit('input', val)
      }
    }
  },
  created() {
    this.loadData()
  },
  methods: {
    loadData() {
      this.$axios
          .get("/api/category")
          .then(res => {
            this.options = res.data.data;
          })
          .catch(err => {
            return Promise.reject(err);
          });
    }
  }
}
</script>
