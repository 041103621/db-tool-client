<script setup>
import { ElMessage } from 'element-plus'
import { onMounted, ref } from 'vue'

const employeeList = ref([])
const loading = ref(false)

async function fetchEmployeeData() {
  loading.value = true
  try {
    const response = await fetch('http://localhost:5001/api/emp')
    const result = await response.json()

    if (result.code === 200) {
      employeeList.value = result.data
      ElMessage.success('Employee data retrieved successfully')
    }
    else {
      ElMessage.error('Failed to retrieve employee data')
    }
  }
  catch (error) {
    console.error('Error fetching employee data:', error)
    ElMessage.error('Network error. Please check your connection')
  }
  finally {
    loading.value = false
  }
}

// Load data when page is mounted
onMounted(() => {
  fetchEmployeeData()
})
</script>

<template>
  <div class="emp-container">
    <div class="query-section">
      <el-button type="primary" @click="fetchEmployeeData">
        Query
      </el-button>
    </div>

    <el-table v-loading="loading" :data="employeeList" style="width: 100%" border>
      <el-table-column prop="empno" label="Employee ID" align="center" />
      <el-table-column prop="ename" label="Name" align="center" />
      <el-table-column prop="job" label="Position" align="center" />
      <el-table-column prop="mgr" label="Manager ID" align="center" />
      <el-table-column prop="hiredate" label="Hire Date" align="center" />
      <el-table-column prop="sal" label="Salary" align="center" />
      <el-table-column prop="comm" label="Commission" align="center" />
      <el-table-column prop="deptno" label="Department ID" align="center" />
    </el-table>
  </div>
</template>

<style scoped>
.emp-container {
  padding: 20px;
}

.query-section {
  margin-bottom: 20px;
  display: flex;
  justify-content: flex-start;
}

:deep(.el-table .cell) {
  text-align: center;
}

:deep(.el-table th) {
  background-color: #f5f7fa;
}

:deep(.el-table th.el-table__cell) {
  font-weight: bold;
}
</style>
