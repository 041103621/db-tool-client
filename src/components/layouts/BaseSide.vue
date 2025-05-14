<script lang="ts" setup>
import {
  Document,
  Menu as IconMenu,
  Location,
  Setting,
} from '@element-plus/icons-vue'

// const isCollapse = ref(true)
function handleOpen(key: string, keyPath: string[]) {
  // eslint-disable-next-line no-console
  console.log(key, keyPath)
}
function handleClose(key: string, keyPath: string[]) {
  // eslint-disable-next-line no-console
  console.log(key, keyPath)
}
</script>

<template>
  <el-menu
    router
    default-active="1"
    class="el-menu-vertical-demo"
    @open="handleOpen"
    @close="handleClose"
  >
    <el-sub-menu index="1">
      <template #title>
        <el-icon>
          <i i-ri-user-2-line />
        </el-icon>
        <span>Business</span>
      </template>
      <el-menu-item-group>
        <el-menu-item index="/nav/1/emp">
          Employee
        </el-menu-item>
        <!-- <el-menu-item index="/nav/1/dept">
          Department
        </el-menu-item>
        <el-menu-item index="/nav/1/salgrade">
          Salary Grade
        </el-menu-item> -->
      </el-menu-item-group>
    </el-sub-menu>
    <el-sub-menu index="2">
      <template #title>
        <el-icon>
          <!-- <Location /> -->
          <i i-ri-database-2-line />
        </el-icon>
        <span>Oracle</span>
      </template>
      <el-menu-item-group>
        <el-menu-item index="/nav/2/log-monitor">
          Log Monitor
        </el-menu-item>
        <!-- <el-menu-item index="/nav/2/postgresql">
          PostgreSQL
        </el-menu-item>
        <el-menu-item index="/nav/2/mongodb">
          MongoDB
        </el-menu-item> -->
      </el-menu-item-group>
    </el-sub-menu>
  </el-menu>
</template>
