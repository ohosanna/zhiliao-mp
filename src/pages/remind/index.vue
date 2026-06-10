<script setup lang="ts">
definePage({
  name: 'remind',
  layout: 'tabbar',
  style: {
    navigationBarTitleText: '随访提醒',
    navigationStyle: 'custom',
  },
})

const statusBarHeight = ref(20)

// #ifdef MP-WEIXIN
// 微信小程序通过 getSystemInfoSync 获取状态栏高度
const sysInfo = uni.getSystemInfoSync()
statusBarHeight.value = sysInfo.statusBarHeight || 20
// #endif

const tabs = ref([
  { name: 'pending', title: '待处理' },
  { name: 'completed', title: '已完成' },
  { name: 'all', title: '全部' },
])

const activeTab = ref('pending')

function handleTabChange({ index }: { index: number }) {
  activeTab.value = tabs.value[index].name
}

function goBack() {
  uni.navigateBack()
}

const pendingList = ref([
  {
    id: 1,
    type: 'emergency',
    badgeText: '紧急',
    badgeColor: '#ef4444',
    cycle: '第4周随访',
    title: '糖化血红蛋白复查',
    desc: '需要完成糖化血红蛋白及血脂四项检查，点击按钮查看复查指引',
    deadline: '2025-02-15',
    actionText: '查看指引',
    actionType: 'primary',
  },
  {
    id: 2,
    type: 'normal',
    badgeText: '常规',
    badgeColor: '#3b82f6',
    cycle: '第8周随访',
    title: '肾功能及肝功能复查',
    desc: '请完成肾功能（肌酐、尿素氮）及肝功能全套检查，评估药物影响',
    deadline: '2025-03-10',
    actionText: '确认完成',
    actionType: 'primary',
  },
])

const completedList = ref([
  {
    id: 3,
    type: 'completed',
    badgeText: '已完成',
    badgeColor: '#22c55e',
    cycle: '第1周随访',
    title: '初始血管检查',
    desc: '已完成血管常规及基础指标采集',
    finishedAt: '2025-01-20',
    actionText: '查看详情',
    actionType: 'link',
  },
])

const currentList = computed(() => {
  if (activeTab.value === 'completed') {
    return completedList.value
  }
  return pendingList.value
})

function handleAction(item: typeof pendingList.value[0]) {
  if (item.actionType === 'link') {
    // TODO: 跳转详情
  }
  else {
    // TODO: 弹窗确认
  }
}
</script>

<template>
  <view class="min-h-screen bg-[#f5f7fa]">
    <!-- 自定义导航栏 -->
    <view
      class="relative bg-gradient-to-br from-[#4a90e2] to-[#357abd] px-4 pb-6"
      :style="{ paddingTop: `${statusBarHeight + 20}px` }"
    >
      <view class="flex items-center gap-3">
        <view
          class="flex h-9 w-9 items-center justify-center rounded-full"
          style="background-color: rgba(255,255,255,0.2)"
          @click="goBack"
        >
          <wd-icon name="arrow-left" size="18px" color="#fff" />
        </view>
        <view class="flex flex-col">
          <text class="text-lg font-bold text-white">
            随访提醒
          </text>
          <text class="mt-0.5 text-xs text-white/70">
            一对一智能随访管理
          </text>
        </view>
      </view>
    </view>

    <!-- Tabs 区域 -->
    <view class="-mt-3 rounded-t-3xl bg-[#f5f7fa]">
      <view class="px-4 pt-3">
        <wd-tabs
          v-model="activeTab"
          custom-class="remind-tabs"
          @change="handleTabChange"
        >
          <wd-tab
            v-for="(tab, idx) in tabs"
            :key="tab.name"
            :name="tab.name"
            :title="tab.title"
            :badge-props="idx === 0 ? { value: 2, type: 'danger', max: 99 } : undefined"
          />
        </wd-tabs>
      </view>

      <!-- 列表 -->
      <view class="space-y-3 px-4 pt-3 pb-4">
        <view
          v-for="item in currentList"
          :key="item.id"
          class="overflow-hidden rounded-2xl bg-white shadow-sm"
          :style="{
            border: item.type === 'emergency' ? '1.5px solid #ef4444' :
                    item.type === 'completed' ? '1.5px solid #22c55e' : '1.5px solid #3b82f6',
          }"
        >
          <view class="p-4">
            <!-- 顶部标签 + 周期 -->
            <view class="mb-2 flex items-center gap-2">
              <text
                class="rounded px-1.5 py-0.5 text-xs font-medium"
                :style="{ backgroundColor: `${item.badgeColor}14`, color: item.badgeColor }"
              >
                {{ item.badgeText }}
              </text>
              <text class="text-xs text-gray-400">
                {{ item.cycle }}
              </text>
            </view>

            <!-- 标题 + 图标 -->
            <view class="mb-2 flex items-center gap-3">
              <view
                class="flex h-10 w-10 shrink-0 items-center justify-center rounded-lg"
                :style="{ backgroundColor: `${item.badgeColor}14` }"
              >
                <wd-icon
                  :name="item.type === 'completed' ? 'check' : item.type === 'emergency' ? 'warning' : 'edit'"
                  size="18px"
                  :color="item.badgeColor"
                />
              </view>
              <text class="text-base font-bold wot-text-text-main">
                {{ item.title }}
              </text>
            </view>

            <!-- 描述 -->
            <text class="mb-4 block text-xs leading-relaxed text-gray-500">
              {{ item.desc }}
            </text>

            <!-- 底部行 -->
            <view class="flex items-center justify-between">
              <view class="flex items-center gap-1">
                <wd-icon
                  :name="item.type === 'completed' ? 'check' : 'calendar'"
                  size="12px"
                  :color="item.type === 'completed' ? '#22c55e' : '#9ca3af'"
                />
                <text
                  class="text-xs"
                  :class="item.type === 'completed' ? '' : 'text-gray-500'"
                  :style="item.type === 'completed' ? 'color: #22c55e' : ''"
                >
                  {{ item.type === 'completed' ? '完成于' : '截止' }}：
                  <text :class="item.type === 'emergency' ? 'text-red-500' : ''" :style="item.type === 'emergency' ? 'color: #ef4444' : ''">
                    {{ item.type === 'completed' ? item.finishedAt : item.deadline }}
                  </text>
                </text>
              </view>
              <view v-if="item.actionType === 'link'" class="flex items-center gap-1" @click="handleAction(item)">
                <text class="text-sm text-blue-500">
                  {{ item.actionText }}
                </text>
                <wd-icon name="arrow-right" size="12px" color="#3b82f6" />
              </view>
              <wd-button
                v-else
                size="small"
                type="primary"
                custom-class="remind-action-btn"
                @click="handleAction(item)"
              >
                {{ item.actionText }}
              </wd-button>
            </view>
          </view>
        </view>
      </view>
    </view>

    <!-- 底部安全区域 -->
    <wd-gap safe-area-bottom height="20px" />
  </view>
</template>

<style lang="scss" scoped>
.remind-tabs {
  :deep(.wd-tabs__nav-item) {
    font-size: 16px;
    font-weight: 500;
  }
  :deep(.wd-tabs__nav-item.is-active) {
    color: #4a90e2;
  }
  :deep(.wd-tabs__line.is-primary) {
    background-color: #4a90e2;
  }
}

.remind-action-btn {
  :deep(.wd-button) {
    background-color: #4a90e2;
    border-radius: 999px;
    padding: 0 18px;
    height: 30px;
    line-height: 30px;
    font-size: 12px;
  }
}
</style>
