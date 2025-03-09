<template>
  <div class="min-h-screen bg-gray-900 text-white p-4 md:p-8">
    <div class="max-w-7xl mx-auto">
      <!-- 顶部导航 -->
      <header class="flex justify-between items-center mb-8">
        <div class="flex items-center">
          <div class="w-10 h-10 rounded-full bg-gradient-to-r from-purple-500 to-cyan-400 flex items-center justify-center">
            <cube-3d class="w-5 h-5 text-white" />
          </div>
          <h1 class="ml-3 text-xl font-bold">全息数字人控制系统</h1>
        </div>
        <div class="flex items-center space-x-4">
          <button class="bg-gray-800 hover:bg-gray-700 p-2 rounded-full">
            <settings class="w-5 h-5" />
          </button>
          <button class="bg-gray-800 hover:bg-gray-700 p-2 rounded-full">
            <bell class="w-5 h-5" />
          </button>
          <div class="w-8 h-8 rounded-full bg-gray-700 flex items-center justify-center">
            <user class="w-4 h-4" />
          </div>
        </div>
      </header>

      <div class="grid grid-cols-1 lg:grid-cols-12 gap-6">
        <!-- 左侧控制面板 -->
        <div class="lg:col-span-3 bg-gray-800 rounded-2xl p-4">
          <h2 class="text-lg font-medium mb-4">模型控制</h2>
          
          <div class="space-y-4">
            <div>
              <label class="block text-sm text-gray-400 mb-1">选择模型</label>
              <select v-model="selectedModel" class="w-full bg-gray-700 border-0 rounded-lg p-2 text-sm">
                <option v-for="model in models" :key="model.id" :value="model.id">{{ model.name }}</option>
              </select>
            </div>
            
            <div>
              <label class="block text-sm text-gray-400 mb-1">动作类型</label>
              <div class="grid grid-cols-2 gap-2">
                <button 
                  v-for="action in actions" 
                  :key="action.id"
                  :class="[
                    'p-2 rounded-lg text-sm',
                    selectedAction === action.id 
                      ? 'bg-purple-600 text-white' 
                      : 'bg-gray-700 hover:bg-gray-600'
                  ]"
                  @click="selectedAction = action.id"
                >
                  {{ action.name }}
                </button>
              </div>
            </div>
            
            <div>
              <label class="flex justify-between text-sm text-gray-400 mb-1">
                <span>表情强度</span>
                <span>{{ expressionIntensity }}%</span>
              </label>
              <input 
                type="range" 
                v-model="expressionIntensity" 
                min="0" 
                max="100" 
                class="w-full accent-purple-500"
              />
            </div>
            
            <div>
              <label class="flex justify-between text-sm text-gray-400 mb-1">
                <span>动作速度</span>
                <span>{{ motionSpeed }}x</span>
              </label>
              <input 
                type="range" 
                v-model="motionSpeed" 
                min="0.5" 
                max="2" 
                step="0.1" 
                class="w-full accent-purple-500"
              />
            </div>
          </div>
          
          <div class="mt-6">
            <h3 class="text-sm font-medium text-gray-400 mb-2">快速动作</h3>
            <div class="grid grid-cols-3 gap-2">
              <button 
                v-for="quickAction in quickActions" 
                :key="quickAction.id"
                class="p-2 bg-gray-700 hover:bg-gray-600 rounded-lg flex flex-col items-center justify-center text-xs"
              >
                <component :is="quickAction.icon" class="w-4 h-4 mb-1" />
                {{ quickAction.name }}
              </button>
            </div>
          </div>
        </div>
        
        <!-- 中间角色展示区 -->
        <div class="lg:col-span-6 flex flex-col">
          <div class="bg-gray-800 rounded-2xl p-4 flex-grow flex flex-col">
            <div class="flex justify-between items-center mb-4">
              <h2 class="text-lg font-medium">角色预览</h2>
              <div class="flex space-x-2">
                <button class="bg-gray-700 hover:bg-gray-600 p-2 rounded-lg">
                  <maximize-2 class="w-4 h-4" />
                </button>
                <button class="bg-gray-700 hover:bg-gray-600 p-2 rounded-lg">
                  <camera class="w-4 h-4" />
                </button>
                <button class="bg-gray-700 hover:bg-gray-600 p-2 rounded-lg">
                  <refresh-cw class="w-4 h-4" />
                </button>
              </div>
            </div>
            
            <div class="flex-grow relative bg-gradient-to-b from-gray-900 to-gray-800 rounded-xl overflow-hidden">
              <!-- 全息投影效果 -->
              <div class="absolute inset-0 bg-[radial-gradient(circle,rgba(120,119,198,0.1)_0%,rgba(0,0,0,0)_70%)]"></div>
              
              <!-- 底部平台 -->
              <div class="absolute bottom-0 left-1/2 transform -translate-x-1/2 w-3/4 h-1 bg-gradient-to-r from-transparent via-cyan-500 to-transparent blur-sm"></div>
              
              <!-- 角色图像 -->
              <div class="absolute inset-0 flex items-center justify-center">
                <img 
                  src="/fufu.png?height=400&width=200" 
                  alt="数字人角色" 
                  class="h-4/5 object-contain opacity-90"
                />
              </div>
              
              <!-- 全息线框效果 -->
              <div class="absolute inset-0 pointer-events-none">
                <div class="absolute top-1/4 left-1/4 right-1/4 h-px bg-cyan-500/20"></div>
                <div class="absolute top-2/4 left-1/4 right-1/4 h-px bg-cyan-500/20"></div>
                <div class="absolute top-3/4 left-1/4 right-1/4 h-px bg-cyan-500/20"></div>
                <div class="absolute left-1/4 top-1/4 bottom-1/4 w-px bg-cyan-500/20"></div>
                <div class="absolute left-2/4 top-1/4 bottom-1/4 w-px bg-cyan-500/20"></div>
                <div class="absolute left-3/4 top-1/4 bottom-1/4 w-px bg-cyan-500/20"></div>
              </div>
            </div>
            
            <div class="mt-4 bg-gray-700 rounded-lg p-3 flex items-center">
              <mic class="w-5 h-5 text-gray-400 mr-3" />
              <input 
                type="text" 
                v-model="voiceCommand" 
                placeholder="输入语音指令..." 
                class="bg-transparent border-0 flex-grow focus:outline-none text-sm"
              />
              <button class="bg-purple-600 hover:bg-purple-500 rounded-lg p-2 ml-2">
                <send class="w-4 h-4" />
              </button>
            </div>
          </div>
          
          <div class="grid grid-cols-4 gap-4 mt-4">
            <div 
              v-for="stat in stats" 
              :key="stat.id" 
              class="bg-gray-800 rounded-xl p-3"
            >
              <div class="flex justify-between items-start">
                <span class="text-xs text-gray-400">{{ stat.name }}</span>
                <component :is="stat.icon" class="w-4 h-4 text-gray-400" />
              </div>
              <div class="mt-1 text-lg font-medium">{{ stat.value }}</div>
              <div class="text-xs" :class="stat.trend === 'up' ? 'text-green-500' : 'text-red-500'">
                <component :is="stat.trend === 'up' ? 'trend-up' : 'trend-down'" class="w-3 h-3 inline-block mr-1" />
                {{ stat.trendValue }}
              </div>
            </div>
          </div>
        </div>
        
        <!-- 右侧设置面板 -->
        <div class="lg:col-span-3 space-y-4">
          <div class="bg-gray-800 rounded-2xl p-4">
            <h2 class="text-lg font-medium mb-4">投影设置</h2>
            
            <div class="space-y-4">
              <div>
                <label class="flex justify-between text-sm text-gray-400 mb-1">
                  <span>亮度</span>
                  <span>{{ brightness }}%</span>
                </label>
                <input 
                  type="range" 
                  v-model="brightness" 
                  min="0" 
                  max="100" 
                  class="w-full accent-purple-500"
                />
              </div>
              
              <div>
                <label class="flex justify-between text-sm text-gray-400 mb-1">
                  <span>对比度</span>
                  <span>{{ contrast }}%</span>
                </label>
                <input 
                  type="range" 
                  v-model="contrast" 
                  min="0" 
                  max="100" 
                  class="w-full accent-purple-500"
                />
              </div>
              
              <div>
                <label class="flex justify-between text-sm text-gray-400 mb-1">
                  <span>透明度</span>
                  <span>{{ transparency }}%</span>
                </label>
                <input 
                  type="range" 
                  v-model="transparency" 
                  min="0" 
                  max="100" 
                  class="w-full accent-purple-500"
                />
              </div>
              
              <div>
                <label class="block text-sm text-gray-400 mb-1">颜色主题</label>
                <div class="flex space-x-2">
                  <button 
                    v-for="(color, index) in colorThemes" 
                    :key="index"
                    :class="[
                      'w-6 h-6 rounded-full border-2',
                      selectedColorTheme === index ? 'border-white' : 'border-transparent'
                    ]"
                    :style="{ backgroundColor: color }"
                    @click="selectedColorTheme = index"
                  ></button>
                </div>
              </div>
            </div>
          </div>
          
          <div class="bg-gray-800 rounded-2xl p-4">
            <h2 class="text-lg font-medium mb-4">设备状态</h2>
            
            <div class="space-y-3">
              <div class="flex justify-between items-center">
                <div class="flex items-center">
                  <cpu class="w-4 h-4 text-gray-400 mr-2" />
                  <span class="text-sm">CPU 使用率</span>
                </div>
                <div class="text-sm font-medium">32%</div>
              </div>
              
              <div class="flex justify-between items-center">
                <div class="flex items-center">
                  <hard-drive class="w-4 h-4 text-gray-400 mr-2" />
                  <span class="text-sm">内存使用</span>
                </div>
                <div class="text-sm font-medium">2.4GB / 8GB</div>
              </div>
              
              <div class="flex justify-between items-center">
                <div class="flex items-center">
                  <thermometer class="w-4 h-4 text-gray-400 mr-2" />
                  <span class="text-sm">温度</span>
                </div>
                <div class="text-sm font-medium">42°C</div>
              </div>
              
              <div class="flex justify-between items-center">
                <div class="flex items-center">
                  <wifi class="w-4 h-4 text-gray-400 mr-2" />
                  <span class="text-sm">网络状态</span>
                </div>
                <div class="text-sm font-medium text-green-500">已连接</div>
              </div>
            </div>
            
            <div class="mt-4 pt-4 border-t border-gray-700">
              <h3 class="text-sm font-medium mb-2">投影设备</h3>
              <div class="flex items-center justify-between">
                <div class="flex items-center">
                  <div class="w-8 h-8 rounded-full bg-gray-700 flex items-center justify-center mr-2">
                    <projector class="w-4 h-4" />
                  </div>
                  <div>
                    <div class="text-sm font-medium">HoloPro X1</div>
                    <div class="text-xs text-gray-400">已连接</div>
                  </div>
                </div>
                <button class="text-xs bg-gray-700 hover:bg-gray-600 px-2 py-1 rounded">
                  设置
                </button>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue';
import { 
  Settings, 
  Bell, 
  User, 
  Maximize2, 
  Camera, 
  RefreshCw, 
  Mic, 
  Send, 
  Cpu,
  HardDrive,
  Thermometer,
  Wifi,
  Smile,
  Hand,
  MessageSquare,
  Zap,
  RotateCw,
  Music,
} from 'lucide-vue-next';

// 模型数据
const models = [
  { id: 1, name: '女性助手' },
  { id: 2, name: '男性讲解员' },
  { id: 3, name: '卡通形象' },
  { id: 4, name: '定制角色' }
];
const selectedModel = ref(1);

// 动作类型
const actions = [
  { id: 1, name: '站立' },
  { id: 2, name: '走动' },
  { id: 3, name: '手势' },
  { id: 4, name: '表情' }
];
const selectedAction = ref(1);

// 快速动作
const quickActions = [
  { id: 1, name: '微笑', icon: Smile },
  { id: 2, name: '挥手', icon: Hand },
  { id: 3, name: '对话', icon: MessageSquare },
  { id: 4, name: '强调', icon: Zap },
  { id: 5, name: '旋转', icon: RotateCw },
  { id: 6, name: '舞蹈', icon: Music }
];

// 统计数据
const stats = [
  { id: 1, name: '互动次数', value: '1,234', trend: 'up', trendValue: '12%', icon: MessageSquare },
  { id: 2, name: '平均时长', value: '4.5分钟', trend: 'up', trendValue: '8%', icon: MessageSquare },
  { id: 3, name: '识别率', value: '98%', trend: 'up', trendValue: '2%', icon: MessageSquare },
  { id: 4, name: '响应时间', value: '0.3秒', trend: 'down', trendValue: '5%', icon: Zap }
];

// 控制参数
const expressionIntensity = ref(75);
const motionSpeed = ref(1.0);
const brightness = ref(80);
const contrast = ref(65);
const transparency = ref(40);
const voiceCommand = ref('');

// 颜色主题
const colorThemes = [
  '#7878c6', // 紫色
  '#4cc3ff', // 蓝色
  '#ff6b6b', // 红色
  '#4cd964', // 绿色
  '#ffcc00'  // 黄色
];
const selectedColorTheme = ref(0);
</script>

<style scoped>
/* 自定义滚动条 */
::-webkit-scrollbar {
  width: 6px;
  height: 6px;
}

::-webkit-scrollbar-track {
  background: rgba(255, 255, 255, 0.1);
  border-radius: 3px;
}

::-webkit-scrollbar-thumb {
  background: rgba(255, 255, 255, 0.2);
  border-radius: 3px;
}

::-webkit-scrollbar-thumb:hover {
  background: rgba(255, 255, 255, 0.3);
}

/* 全息效果动画 */
@keyframes pulse {
  0% { opacity: 0.7; }
  50% { opacity: 1; }
  100% { opacity: 0.7; }
}

.hologram-effect {
  animation: pulse 3s infinite;
}
</style>