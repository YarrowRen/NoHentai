<template>
  <div class="settings-wrapper">
    <div class="settings-container">
      <h1 class="settings-title">Settings</h1>
      
      <!-- EX配置区域 -->
      <div class="config-section">
        <h2 class="section-title">
          ExHentai Configuration
        </h2>
        
        <div class="form-grid">
          <div class="form-group">
            <label for="exBase">Base URL</label>
            <select v-model="config.EXHENTAI_BASE_URL" id="exBase" class="form-select">
              <option value="https://exhentai.org/favorites.php">ExHentai</option>
              <option value="https://e-hentai.org/favorites.php">E-Hentai</option>
            </select>
          </div>

          <div class="form-group">
            <label for="igneous">Igneous</label>
            <input 
              v-model="config.EXHENTAI_COOKIE_IGNEOUS"
              id="igneous"
              type="text"
              class="form-input"
              placeholder="输入你的Igneous (可选)"
            />
          </div>

          <div class="form-group">
            <label for="memberId">Member ID</label>
            <input 
              v-model="config.EXHENTAI_COOKIE_MEMBER_ID"
              id="memberId"
              type="text"
              class="form-input"
              placeholder="输入你的Member ID"
            />
          </div>

          <div class="form-group">
            <label for="passHash">Pass Hash</label>
            <input 
              v-model="config.EXHENTAI_COOKIE_PASS_HASH"
              id="passHash"
              type="password"
              class="form-input"
              placeholder="输入你的Pass Hash"
            />
          </div>
        </div>
      </div>



      <!-- 操作按钮 -->
      <div class="actions">
        <button 
          @click="loadConfig" 
          class="btn btn-secondary"
          :disabled="loading"
        >
          <span class="btn-icon">🔄</span>
          重新加载
        </button>
        
        <button 
          @click="saveConfig" 
          class="btn btn-primary"
          :disabled="loading || !hasChanges"
        >
          <span class="btn-icon">💾</span>
          {{ loading ? '保存中...' : '保存配置' }}
        </button>

        <button 
          @click="testConnection" 
          class="btn btn-test"
          :disabled="loading"
        >
          <span class="btn-icon">🔗</span>
          测试连接
        </button>

        <button 
          @click="triggerSync" 
          class="btn btn-sync"
          :disabled="loading"
        >
          <span class="btn-icon">🔄</span>
          同步数据
        </button>
      </div>

      <!-- 状态消息 -->
      <div v-if="message" :class="['message', messageType]">
        {{ message }}
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, computed, onMounted, watch } from 'vue'

const API = import.meta.env.VITE_API_BASE || ''

// 响应式数据
const config = ref({
  EXHENTAI_BASE_URL: 'https://exhentai.org/favorites.php',
  EXHENTAI_COOKIE_MEMBER_ID: '',
  EXHENTAI_COOKIE_PASS_HASH: '',
  EXHENTAI_COOKIE_IGNEOUS: ''
})

const originalConfig = ref({})
const loading = ref(false)
const message = ref('')
const messageType = ref('info') // info, success, error


// 检测是否有变更
const hasChanges = computed(() => {
  return JSON.stringify(config.value) !== JSON.stringify(originalConfig.value)
})

// 显示消息
const showMessage = (msg, type = 'info', duration = 3000) => {
  message.value = msg
  messageType.value = type
  if (duration > 0) {
    setTimeout(() => {
      message.value = ''
    }, duration)
  }
}

// 加载配置
const loadConfig = async () => {
  loading.value = true
  try {
    const response = await fetch(`${API}/api/settings/config`)
    if (!response.ok) throw new Error('获取配置失败')
    
    const data = await response.json()
    config.value = { ...config.value, ...data }
    originalConfig.value = JSON.parse(JSON.stringify(config.value))
    showMessage('配置加载成功', 'success')
  } catch (error) {
    showMessage('加载配置失败: ' + error.message, 'error')
  } finally {
    loading.value = false
  }
}

// 保存配置
const saveConfig = async () => {
  loading.value = true
  try {
    const response = await fetch(`${API}/api/settings/config`, {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json'
      },
      body: JSON.stringify(config.value)
    })
    
    if (!response.ok) throw new Error('保存配置失败')
    
    originalConfig.value = JSON.parse(JSON.stringify(config.value))
    showMessage('配置保存成功！请重启应用以生效', 'success', 5000)
  } catch (error) {
    showMessage('保存配置失败: ' + error.message, 'error')
  } finally {
    loading.value = false
  }
}

// 测试连接
const testConnection = async () => {
  loading.value = true
  try {
    const response = await fetch(`${API}/api/settings/test-connection`, {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json'
      },
      body: JSON.stringify(config.value)
    })
    
    const result = await response.json()
    
    if (response.ok) {
      showMessage(result.message || '连接测试成功', 'success')
    } else {
      showMessage(result.error || '连接测试失败', 'error')
    }
  } catch (error) {
    showMessage('测试连接失败: ' + error.message, 'error')
  } finally {
    loading.value = false
  }
}

// 同步数据
const triggerSync = async () => {
  try {
    // 只发送请求，不等待响应
    fetch(`${API}/api/gallery/sync`, {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json'
      }
    }).catch(() => {}); // 忽略错误
    
    showMessage('同步请求已发送', 'info')
  } catch (error) {
    showMessage('发送同步请求失败: ' + error.message, 'error')
  }
}

// 页面加载时获取配置
onMounted(() => {
  loadConfig()
})
</script>

<style src="../assets/Settings.css"></style>