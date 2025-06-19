<script setup>
import { ElMessage } from 'element-plus'
import { onMounted, ref } from 'vue'
import { useRouter } from 'vue-router'

// const router = useRouter()
const customerList = ref([])
const loading = ref(false)

async function fetchCustomerData() {
  loading.value = true
  try {
    const response = await fetch('http://localhost:5001/api/customer')

    if (!response.ok) {
      throw new Error(`HTTP ${response.status}: ${response.statusText}`)
    }

    const result = await response.json()

    if (result.code === 200) {
      // API返回的数据在result.data.customers中
      if (result.data && Array.isArray(result.data.customers)) {
        customerList.value = result.data.customers
        ElMessage.success(`Customer data retrieved successfully (${result.data.customers.length} records)`)
      }
      else {
        customerList.value = []
        ElMessage.warning('No customer data available or invalid data format')
      }
    }
    else {
      customerList.value = []
      ElMessage.error('Failed to retrieve customer data')
    }
  }
  catch {
    customerList.value = []
    ElMessage.error('Network error. Please check your connection')
  }
  finally {
    loading.value = false
  }
}

// Load data when page is mounted
onMounted(() => {
  fetchCustomerData()
})
</script>

<template>
  <div class="customer-container">
    <!-- Breadcrumb Navigation -->
    <div class="breadcrumb-container">
      <el-breadcrumb separator="/">
        <el-breadcrumb-item :to="{ path: '/' }">
          homepage
        </el-breadcrumb-item>
        <el-breadcrumb-item>
          <a href="/">Customer Management</a>
        </el-breadcrumb-item>
      </el-breadcrumb>
    </div>

    <div class="query-section">
      <el-button type="primary" @click="fetchCustomerData">
        Query Customers
      </el-button>
    </div>

    <div class="table-container">
      <el-table
        v-loading="loading"
        :data="customerList"
        style="width: 100%"
        border
        :header-cell-style="{ textAlign: 'center' }"
        :cell-style="{ textAlign: 'center' }"
      >
        <el-table-column prop="customer_id" label="ID" align="center" width="80" />
        <el-table-column prop="company_name" label="Company Name" align="center" min-width="150" />
        <el-table-column prop="contact_name" label="Contact" align="center" width="100" />
        <el-table-column prop="contact_title" label="Title" align="center" width="80" />
        <el-table-column prop="email" label="Email" align="center" min-width="160" />
        <el-table-column prop="phone" label="Phone" align="center" width="120" />
        <el-table-column prop="city" label="City" align="center" width="80" />
        <el-table-column prop="country" label="Country" align="center" width="80" />
        <el-table-column prop="industry" label="Industry" align="center" width="100" />
        <el-table-column prop="status" label="Status" align="center" width="80" />
        <el-table-column prop="credit_rating" label="Rating" align="center" width="80" />
        <el-table-column prop="created_date" label="Created Date" align="center" width="140" />
      </el-table>
    </div>
  </div>
</template>

<style scoped>
.customer-container {
  padding: 20px;
}

.query-section {
  margin-bottom: 20px;
  display: flex;
  justify-content: flex-start;
}

.table-container {
  overflow-x: auto;
}

:deep(.el-table) {
  font-size: 14px;
}

:deep(.el-table .cell) {
  text-align: center;
  padding: 8px 4px;
}

:deep(.el-table th) {
  background-color: #f5f7fa;
  text-align: center;
}

:deep(.el-table th.el-table__cell) {
  font-weight: bold;
  text-align: center;
}

:deep(.el-table td.el-table__cell) {
  text-align: center;
}

.breadcrumb-container {
  margin-bottom: 20px;
  padding-bottom: 10px;
  border-bottom: 1px solid #ebeef5;
}

:deep(.el-breadcrumb__item) {
  cursor: pointer;
}
</style>
