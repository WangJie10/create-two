<template>
  <div>
    <el-cascader
      v-if="isShowInfo"
      v-model="adCode"
      style="width: 100%"
      placeholder="请选择"
      @change="handleChange"
      ref="addr"
      :props="props"
    />
  </div>
</template>

<script>
export default {
  name: 'AreaSelect',
  props: {
    value: {
      type: String,
      default: null
    }
  },
  data() {
    const that=this
    return {
      isShowInfo: true,
      props: {
        value: 'adCode',
        label: 'name',
        lazy: true,
        lazyLoad(node, resolve) {
          const { level } = node
          if (level === 0) {
            that.$axios
                .get('/api/area/province')
                .then(res => {
                  resolve(res.data.data)
                })
                .catch(err => {
                  return Promise.reject(err);
                });
          } else if (level === 1) {
            that.$axios
                .get(`/api/area/city?province=${node.value}`)
                .then(res => {
                  resolve(res.data.data)
                })
                .catch(err => {
                  return Promise.reject(err);
                });
          } else if (level === 2) {
            that.$axios
                .get(`/api/area/district?city=${node.value}`)
                .then(res => {
                  res.data.data.forEach(x => { x.leaf = true })
                  resolve(res.data.data)
                })
                .catch(err => {
                  return Promise.reject(err);
                });
          }
        }
      }
    }
  },
  computed: {
    adCode: {
      get() {
        if (this.value) {
          const province = this.value.substring(0, 2) + '0000'
          const city = this.value.substring(0, 4) + '00'
          return [province, city, this.value]
        }
        return null
      },
      set(val) {
        this.$emit('input', val[2])
      }
    }
  },
  watch: {
    value: {
      handler() {
        this.isShowInfo = false
        this.$nextTick(() => {
          this.isShowInfo = true
        })
      }
    }
  },
  methods: {
    handleChange() {
      const district = this.$refs.addr.getCheckedNodes()[0];
      const city = district.parent;
      const province = city.parent;
      this.$emit('addressText',`${province.label}${city.label}${district.label}`)
    }
  }
}
</script>

<style scoped>

</style>
