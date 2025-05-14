<script setup>
// 引入图标
import { Bottom, Delete, Download } from '@element-plus/icons-vue'
import { ElMessage } from 'element-plus'
import { io } from 'socket.io-client'
import { onMounted, onUnmounted, ref } from 'vue'

// 状态变量
const connected = ref(false)
const socket = ref(null)
const autoScroll = ref(true)
const maxLines = ref(500)
const updateInterval = ref(5)
const logContent = ref('Waiting for connection...')
const logInfo = ref({
  filePath: '-',
  fileSize: '-',
  lastModified: '-',
  lastUpdate: '-',
})

// DOM引用
const logContentEl = ref(null)

// 格式化文件大小
function formatFileSize(bytes) {
  if (bytes === 0)
    return '0 B'
  const sizes = ['B', 'KB', 'MB', 'GB', 'TB']
  const i = Math.floor(Math.log(bytes) / Math.log(1024))
  return `${(bytes / 1024 ** i).toFixed(2)} ${sizes[i]}`
}

// 更新日志信息
function updateLogInfo(data) {
  if (!data)
    return

  logInfo.value = {
    filePath: data.file_path || '-',
    fileSize: data.file_size ? formatFileSize(data.file_size) : '-',
    lastModified: data.last_modified || '-',
    lastUpdate: data.timestamp || '-',
  }
}

// 连接WebSocket
function connectSocket() {
  if (socket.value) {
    socket.value.disconnect()
  }

  // 创建Socket.IO连接
  socket.value = io(`http://localhost:5001/logs`, {
    transports: ['websocket'],
  })

  // 连接事件
  socket.value.on('connect', () => {
    connected.value = true
    logContent.value = 'Connected to server, retrieving logs...'

    // 开始监控日志
    startLogMonitor()
  })

  // 断开连接事件
  socket.value.on('disconnect', () => {
    connected.value = false
    logContent.value += '\n\nConnection lost'
  })

  // 日志更新事件
  socket.value.on('log_update', (data) => {
    logContent.value = data.content || 'No log content'
    updateLogInfo(data)

    // 如果启用了自动滚动，则滚动到底部
    if (autoScroll.value) {
      scrollToBottom()
    }
  })
}

// 断开WebSocket连接
function disconnectSocket() {
  if (socket.value) {
    socket.value.emit('stop_log_monitor')
    socket.value.disconnect()
    socket.value = null
  }
  connected.value = false
}

// 开始监控日志
function startLogMonitor() {
  if (!socket.value || !socket.value.connected)
    return

  socket.value.emit('start_log_monitor', {
    max_lines: maxLines.value,
    interval: updateInterval.value,
  })
}

// 刷新日志
async function refreshLog() {
  if (!socket.value || !socket.value.connected) {
    // 如果没有连接，则通过API获取日志
    try {
      const response = await fetch(`http://localhost:5001/api/log?max_lines=${maxLines.value}`)
      const data = await response.json()

      if (data.code === 200 && data.data) {
        logContent.value = data.data.content || 'No log content'
        updateLogInfo(data.data)

        if (autoScroll.value) {
          scrollToBottom()
        }
      }
    }
    catch (error) {
      logContent.value = `Failed to get logs: ${error.message}`
      ElMessage.error('Failed to get logs')
    }
  }
  else {
    // 如果已连接，则通过Socket.IO获取
    socket.value.emit('get_log', {
      max_lines: maxLines.value,
    }, (data) => {
      logContent.value = data.content || 'No log content'
      updateLogInfo(data)

      if (autoScroll.value) {
        scrollToBottom()
      }
    })
  }
}

// 清空日志显示
function clearLog() {
  logContent.value = 'Log cleared'
}

// 滚动到底部
function scrollToBottom() {
  if (logContentEl.value) {
    logContentEl.value.scrollTop = logContentEl.value.scrollHeight
  }
}

// 下载日志
function downloadLog() {
  const content = logContent.value
  const blob = new Blob([content], { type: 'text/plain' })
  const url = URL.createObjectURL(blob)
  const a = document.createElement('a')
  a.href = url
  a.download = `log_${new Date().toISOString().replace(/[:.]/g, '-')}.txt`
  document.body.appendChild(a)
  a.click()
  document.body.removeChild(a)
  URL.revokeObjectURL(url)
}

// 生命周期钩子
onMounted(() => {
  // 初始刷新日志
  refreshLog()
})

onUnmounted(() => {
  // 组件卸载时断开连接
  if (socket.value) {
    disconnectSocket()
  }
})
</script>

<template>
  <div class="container">
    <!-- <div class="card"> -->
    <!-- 添加面包屑导航 -->
    <div class="breadcrumb-container">
      <el-breadcrumb separator="/">
        <el-breadcrumb-item :to="{ path: '/' }">
          homepage
        </el-breadcrumb-item>
        <el-breadcrumb-item>Log Monitor</el-breadcrumb-item>
      </el-breadcrumb>
    </div>

    <div class="controls">
      <el-button type="primary" :disabled="connected" @click="connectSocket">
        Connect
      </el-button>
      <el-button type="danger" :disabled="!connected" @click="disconnectSocket">
        Disconnect
      </el-button>
      <el-button type="success" @click="refreshLog">
        Refresh
      </el-button>
      <div>
        <label for="connection-status">Status:</label>
        <span class="status" :class="[connected ? 'connected' : 'disconnected']">
          {{ connected ? 'Connected' : 'Disconnected' }}
        </span>
      </div>
      <div class="no-wrap">
        <label for="max-lines">Display Lines:</label>
        <el-select v-model="maxLines" @change="startLogMonitor">
          <el-option :value="100" label="100" />
          <el-option :value="500" label="500" />
          <el-option :value="1000" label="1000" />
          <el-option :value="2000" label="2000" />
        </el-select>
      </div>
      <div>
        <label for="update-interval">Interval(s):</label>
        <el-input-number v-model="updateInterval" :min="1" :max="60" @change="startLogMonitor" />
      </div>
      <div>
        <strong>File Path:</strong> <span>{{ logInfo.filePath || '-' }}</span>
      </div>
      <div>
        <strong>File Size:</strong> <span>{{ logInfo.fileSize || '-' }}</span>
      </div>
    </div>

    <div class="log-container">
      <pre ref="logContentEl" class="log-content">{{ logContent }}</pre>

      <!-- 右下角操作按钮 -->
      <div class="action-buttons">
        <div>
          <strong style="color: #fff;">Last Modified:</strong> <span style="color: #fff;">{{ logInfo.lastModified || '-' }}</span>
        </div>
        <el-button circle type="primary" :icon="Bottom" title="Scroll to Bottom" @click="scrollToBottom" />
        <el-button circle type="success" :icon="Download" title="Download Log" @click="downloadLog" />
        <el-button circle type="danger" :icon="Delete" title="Clear Screen" @click="clearLog" />
      </div>
    </div>
    <!-- </div> -->
  </div>
</template>

<style scoped>
.container {
  padding: 20px;
  position: relative;
}

.breadcrumb-container {
  margin-bottom: 20px;
  padding-bottom: 10px;
  border-bottom: 1px solid #ebeef5;
}

.card {
  background-color: #fff;
  border-radius: 5px;
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
  padding: 20px;
  margin-bottom: 20px;
}

h1 {
  color: #333;
  margin-top: 0;
}

.log-info {
  display: flex;
  justify-content: start;
  align-items: center;
  margin-bottom: 10px;
  color: #555;
  flex-wrap: wrap;
}

.log-info div {
  margin-right: 20px;
  margin-bottom: 10px;
}

.log-info strong {
  color: #333;
}

.log-container {
  position: relative;
  margin-bottom: 20px;
}

.log-content {
  background-color: #282c34;
  color: #abb2bf;
  padding: 15px;
  border-radius: 5px;
  font-family: Monaco, Menlo, 'Courier New', monospace;
  font-size: 13px;
  line-height: 1.5;
  white-space: pre-wrap;
  overflow-y: auto;
  max-height: 600px;
  min-height: 400px;
  text-align: left;
}

.action-buttons {
  position: absolute;
  bottom: 15px;
  right: 15px;
  display: flex;
  align-items: center;
  gap: 10px;
  background-color: rgba(40, 44, 52, 0.6);
  padding: 8px;
  border-radius: 25px;
}

.controls {
  margin-bottom: 15px;
  display: flex;
  flex-wrap: wrap;
  align-items: center;
  gap: 10px;
}

.controls > div {
  display: flex;
  align-items: center;
  margin-right: 10px;
}

.no-wrap {
  display: flex;
  align-items: center;
  margin-right: 10px;
  white-space: nowrap;
  flex-shrink: 0;
}

.controls label {
  margin-right: 5px;
  color: #333;
}

.status {
  padding: 5px 10px;
  border-radius: 3px;
  display: inline-block;
  font-size: 14px;
  margin-left: 10px;
}

.status.connected {
  color: #4caf50;
  /* color: white; */
}

.status.disconnected {
  color: #f44336;
  /* color: white; */
}

@media (max-width: 768px) {
  .controls {
    flex-direction: column;
    align-items: flex-start;
  }

  .controls > div,
  .controls .el-button {
    width: 100%;
    margin-right: 0;
  }

  .no-wrap {
    width: 100%;
  }
}
</style>
