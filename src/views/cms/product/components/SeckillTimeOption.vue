<template>
  <div style="display:inline-block;">
    <el-select v-model="option" placeholder="请选择">
      <el-option
          v-for="item in options"
          :key="item.timeId"
          :label="dataFormat(item.startTime) +'-'+dataFormat(item.endTime)"
          :value="item.timeId">
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
  filter:{

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
          .get('api/seckill/product/time')
          .then(res => {
            this.options = res.data.data;
          })
          .catch(err => {
            return Promise.reject(err);
          });
    },
    dataFormat(dataStr) {
      var time = new Date(dataStr);
      function timeAdd0 (str) {
        if (str < 10) {
          str = '0' + str;
        }
        return str;
      }
      var y = time.getFullYear();
      var m = time.getMonth() + 1;
      var d = time.getDate();
      var h = time.getHours();
      var mm = time.getMinutes();
      var s = time.getSeconds();
      return y + '-' + timeAdd0(m) + '-' + timeAdd0(d) + ' ' + timeAdd0(h) + ':' + timeAdd0(mm) + ':' + timeAdd0(s);
    }
  }
}
</script>
