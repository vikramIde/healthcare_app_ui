<template>
  <div class="app-container">
    <el-tabs v-model="activeName" style="margin-top:15px;" type="border-card">
      <el-tab-pane v-for="item in tabMapOptions" :label="item.label" :key="item.key" :name="item.key">
        <keep-alive>
          <medicineList v-if="item.key=='ML'"/>
        </keep-alive>
      </el-tab-pane>
    </el-tabs>

  </div>
</template>

<script>
// import { fetchList } from '@/api/article'

import medicineList from '../components/tab-panes/medicinelist'

//ddd

export default {
  components: { medicineList},
  name: 'MedicineList',
  filters: {
    statusFilter(status) {
      const statusMap = {
        published: 'success',
        draft: 'info',
        deleted: 'danger'
      }
      return statusMap[status]
    }
  },
  data() {
    return {
      tabMapOptions: [
        { label: 'Medicine List', key: 'ML' },
      ],
      activeName: 'ML',
      list: null,
      total: 0,
      listLoading: true,
      listQuery: {
        page: 1,
        limit: 20
      }
    }
  },
  created() {
    // this.getList()
  },
  methods: {
    getList() {
      this.listLoading = true
      fetchAsset(this.listQuery,'Medicine').then(response => {
        this.list = response.data.items
        this.total = response.data.total
        this.listLoading = false
      })
    },
    handleSizeChange(val) {
      this.listQuery.limit = val
      this.getList()
    },
    handleCurrentChange(val) {
      this.listQuery.page = val
      this.getList()
    }
  }
}
</script>

<style scoped>
.edit-input {
  padding-right: 100px;
}
.cancel-btn {
  position: absolute;
  right: 15px;
  top: 10px;
}
</style>
