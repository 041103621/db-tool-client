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
          <!-- <Location /> -->
          <i i-ri-database-2-line />
        </el-icon>
        <span>Oracle</span>
      </template>
      <el-menu-item-group>
        <el-menu-item index="/nav/1/item-1">
          item one
        </el-menu-item>
        <el-menu-item index="/nav/1/item-2">
          item two
        </el-menu-item>
      </el-menu-item-group>
    </el-sub-menu>
    <el-menu-item index="/nav/2">
      <el-icon>
        <i i-ri-database-2-line />
      </el-icon>
      <template #title>
        PostgreSQL
      </template>
    </el-menu-item>
    <el-menu-item index="/nav/4">
      <el-icon>
        <i i-ri-database-2-line />
      </el-icon>
      <template #title>
        MongoDB
      </template>
    </el-menu-item>
  </el-menu>
</template>
