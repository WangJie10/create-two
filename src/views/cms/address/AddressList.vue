<template>
  <div>
    <el-table :data="list" border fit highlight-current-row style="width: 100%">

      <el-table-column align="center" label="姓名">
        <template slot-scope="scope">
          {{ scope.row.username }}
        </template>
      </el-table-column>

      <el-table-column align="center" label="电话">
        <template slot-scope="scope">
          {{ scope.row.phone }}
        </template>
      </el-table-column>

      <el-table-column align="center" label="地址">
        <template slot-scope="scope">
          {{ scope.row.address }}
        </template>
      </el-table-column>

      <el-table-column align="center" label="详细地址">
        <template slot-scope="scope">
          {{ scope.row.addressDetail }}
        </template>
      </el-table-column>

      <el-table-column align="center" label="操作" width="180">
        <template slot-scope="scope">
          <div style="display: flex;justify-content: space-between">
            <router-link :to="'/cms/addressEdit/'+scope.row.id">
              <el-button type="primary" size="small" icon="el-icon-edit">
                编辑
              </el-button>
            </router-link>
            <el-button type="danger" size="small" icon="el-icon-delete" @click="handleDelete(scope.row.id)">
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
  name: "Address",
  data() {
    return {
      list: []
    }
  },
  mounted() {
    this.loadData()
  },
  methods: {
    loadData() {
      this.$axios
          .get('api/address/all')
          .then(res => {
            this.list = res.data.data;
            this.listLoading = false
          })
          .catch(err => {
            return Promise.reject(err);
          });
    },
    handleDelete(id) {
      this.$confirm('此操作将永久删除该信息, 是否继续?', '提示', {
        confirmButtonText: '确定',
        cancelButtonText: '取消',
        type: 'warning'
      }).then(async () => {
        this.$axios
            .delete(`/api/address/${id}`)
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