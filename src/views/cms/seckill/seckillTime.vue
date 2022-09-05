<template>
  <div>
    <el-table :data="list" border fit highlight-current-row style="width: 100%">

      <el-table-column align="center" label="id">
        <template slot-scope="scope">
          {{ scope.row.timeId }}
        </template>
      </el-table-column>

      <el-table-column align="center" label="开始时间">
        <template slot-scope="scope">
          {{ scope.row.startTime | dateFormat }}
        </template>
      </el-table-column>

      <el-table-column align="center" label="结束时间">
        <template slot-scope="scope">
          {{ scope.row.endTime | dateFormat }}
        </template>
      </el-table-column>

      <el-table-column align="center" label="操作" width="180">
        <template slot-scope="scope">
          <div style="display: flex;justify-content: space-between">
            <router-link :to="`seckillProduct/${scope.row.timeId}`">
              <el-button type="primary" size="small">
                管理产品
              </el-button>
            </router-link>
            <el-button type="danger" size="small" icon="el-icon-delete" @click="handleDelete(scope.row.timeId)">
              删除
            </el-button>
          </div>
        </template>
      </el-table-column>

    </el-table>
    <el-button type="primary" size="small" style="margin-top: 10px" @click="dialogVisible = true">新增</el-button>


    <el-dialog
        title="新增"
        :visible.sync="dialogVisible"
        width="500px">
      <el-date-picker
          v-model="date"
          type="datetimerange"
          range-separator="至"
          start-placeholder="开始日期"
          end-placeholder="结束日期"
          :picker-options="pickerOptions">
      </el-date-picker>
      <span slot="footer" class="dialog-footer">
    <el-button @click="dialogVisible = false">取 消</el-button>
    <el-button type="primary" @click="submitTime">确 定</el-button>
  </span>
    </el-dialog>
  </div>
</template>

<script>
export default {
  name: "seckillTime",
  data() {
    return {
      dialogVisible: false,
      date: [new Date(), new Date()],
      pickerOptions: {
        disabledDate: (date) => {
          return date < (new Date() - 86400000)
        }
      },
      list: []
    }
  },
  mounted() {
    this.loadData()
  },
  methods: {
    loadData() {
      this.$axios
          .get('api/seckill/product/time')
          .then(res => {
            this.list = res.data.data;
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
            .delete(`/api/seckill/product/time/${id}`)
            .then(() => {
              this.$message.success('删除成功!')
              this.loadData()
            })
            .catch(err => {
              return Promise.reject(err);
            });
      }).catch(() => {
      })
    },
    submitTime() {
      const startTime = Date.parse(this.date[0])
      const endTime = Date.parse(this.date[1])
      this.$axios
          .post('api/seckill/product/time', {
            startTime, endTime
          })
          .then(() => {
            this.loadData()
            this.dialogVisible = false
          })
          .catch(err => {
            return Promise.reject(err);
          });
    }
  }
}
</script>

<style scoped>

</style>