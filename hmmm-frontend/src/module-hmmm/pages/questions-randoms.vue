<template>
  <el-table
    class="zuti-list"
    :data="dataList"
    border
    style="width: 100%"
    slot-scope="scope">
    <el-table-column
      prop="id"
      label="序号"
      width="60">
    </el-table-column>
    <el-table-column
      prop="addTime"
      label="组题时间"
      width="120">
    </el-table-column>
    <el-table-column
      prop="userName"
      label="用户名"
      width="120">
    </el-table-column>
    <el-table-column
      prop="questionIDs"
      label="试题ID"
      width="120">
    </el-table-column>
    <el-table-column
      prop="progressOfAnswer"
      label="答题进度"
      width="120">
    </el-table-column>
    <el-table-column
      prop="accuracyRate"
      label="正确率"
      width="120">
    </el-table-column>
    <el-table-column
      prop="totalSeconds"
      label="答题总耗时（秒）"
      width="160">
    </el-table-column>
    <el-table-column
      prop="questionType"
      label="组题类型"
      width="120">
    </el-table-column>
    <el-table-column
      prop="address"
      label="操作"
      width="120">
      <template slot-scope="scope">
        <el-button
          v-if="scope.row.status!='deleted'"
          size="mini"
          type="danger"
          @click="removeUser(scope.$index, scope.row.id)"
        >删除
        </el-button>
      </template>
    </el-table-column>
  </el-table>
</template>

<script>
import {randoms, removeRandoms} from '@/api/hmmm/questions'
export default {
  name: 'QuestionsRandoms',
  data() {
    return {
      dataList: []
    }
  },
  created () {
    this.loadZuti()
  },
  methods: {
    loadZuti () {
      randoms()
        .then(res => {
          this.dataList = res.data.items
        })
        .catch(e => {
          this.$message.e('出错了')
        })
    },
    // removeUser(user) {
    //   removeRandoms(user)
    //   this.loadZuti()
    // }
    removeUser(index, user) {
      console.log(index, user)
      this.$confirm('此操作将永久删除 ' + ', 是否继续?', '提示', {
        type: 'warning'
      })
        .then(() => {
          removeRandoms({ id: user })
            .then(response => {
              this.$message.success('成功删除了用户' + '!')
              console.log(user)
              console.log(response)
              console.log(this.dataList)
              this.dataList.splice(index, 1)
              this.loadZuti(this.requestParameters)
            })
            .catch(response => {
              this.$message.error('删除失败!')
            })
        })
        .catch(() => {
          this.$message.info('已取消操作!')
        })
    }
  }
}
</script>

<style scoped>
.zuti-list {
  margin: 20px;
}
</style>
