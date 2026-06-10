<script setup lang="ts">
definePage({
  name: 'user',
  layout: 'tabbar',
  style: {
    navigationBarTitleText: '我的',
  },
})

const router = useRouter()

const userInfo = {
  avatarText: '张',
  name: '张建国',
  id: '20250108001',
  org: '朝阳区劲松社区卫生服务中心',
  points: 1280,
  cycle: {
    current: 23,
    total: 90,
    startDate: '2025-01-15',
    endDate: '2025-04-14',
    manager: '李医生',
  },
}

const menuList = [
  { title: '我的健康档案', icon: 'edit', iconColor: '#3b82f6', bgColor: '#eff6ff', path: '' },
  { title: '历史报告记录', icon: 'clock', iconColor: '#22c55e', bgColor: '#f0fdf4', path: '' },
  { title: '联系健康管理师', icon: 'service', iconColor: '#f59e0b', bgColor: '#fffbeb', path: '' },
  { title: '糖尿病知识库', icon: 'book', iconColor: '#a855f7', bgColor: '#faf5ff', path: '' },
  { title: '关于我们', icon: 'info-circle', iconColor: '#9ca3af', bgColor: '#f9fafb', path: '' },
]

function handleSettings() {
  // TODO: 跳转设置页
}

function handleMenuClick(item: typeof menuList[0]) {
  if (item.path) {
    router.push({ name: item.path })
  }
}

function handleLogout() {
  uni.showModal({
    title: '提示',
    content: '确定要退出登录吗？',
    success: (res) => {
      if (res.confirm) {
        // TODO: 清除登录态
        router.replace({ name: 'welcome' })
      }
    },
  })
}
</script>

<template>
  <view class="min-h-screen bg-[#f5f7fa]">
    <!-- 顶部蓝色区域 -->
    <view class="relative bg-gradient-to-br from-[#4a90e2] to-[#357abd] px-5 pt-6 pb-20">
      <!-- 设置 -->
      <view class="absolute right-5 top-6">
        <view
          class="flex h-8 w-8 items-center justify-center rounded-full"
          style="background-color: rgba(255,255,255,0.15)"
          @click="handleSettings"
        >
          <wd-icon name="setting" size="16px" color="#fff" />
        </view>
      </view>

      <!-- 用户信息 -->
      <view class="flex items-center gap-4 pt-4">
        <view
          class="flex h-15 w-15 items-center justify-center rounded-full text-xl font-bold text-white"
          style="background-color: rgba(255,255,255,0.2)"
        >
          {{ userInfo.avatarText }}
        </view>
        <view class="flex flex-col">
          <text class="text-lg font-bold text-white">
            {{ userInfo.name }}
          </text>
          <text class="mt-1 text-xs text-white/80">
            ID: {{ userInfo.id }}
          </text>
          <view class="mt-1 flex items-center gap-1">
            <wd-icon name="home" size="10px" color="#ffffffaa" />
            <text class="text-xs text-white/70">
              {{ userInfo.org }}
            </text>
          </view>
        </view>
      </view>

      <!-- 积分 -->
      <view
        class="mt-5 flex items-center justify-between rounded-xl px-4 py-3"
        style="background-color: rgba(255,255,255,0.12)"
      >
        <view class="flex items-center gap-2">
          <wd-icon name="star" size="16px" color="#fbbf24" />
          <text class="text-sm text-white">
            我的积分
          </text>
        </view>
        <text class="text-lg font-bold text-amber-300">
          {{ userInfo.points.toLocaleString() }}分
        </text>
      </view>
    </view>

    <!-- 管理周期卡片 -->
    <view class="mx-4 -mt-12 rounded-2xl bg-white p-5 shadow-sm">
      <view class="mb-5 flex items-center justify-between">
        <text class="text-base font-bold wot-text-text-main">
          90天管理周期
        </text>
        <view class="rounded-full bg-blue-50 px-3 py-1">
          <text class="text-xs text-blue-500">
            进行中
          </text>
        </view>
      </view>

      <view class="flex items-center gap-5">
        <!-- 环形进度 -->
        <view class="relative flex h-18 w-18 shrink-0 items-center justify-center">
          <svg class="absolute h-full w-full -rotate-90" viewBox="0 0 100 100">
            <circle cx="50" cy="50" r="42" stroke="#e5e7eb" stroke-width="8" fill="none" />
            <circle
              cx="50" cy="50" r="42" stroke="#3b82f6" stroke-width="8" fill="none"
              stroke-dasharray="263.89" stroke-dashoffset="196.43" stroke-linecap="round"
            />
          </svg>
          <view class="flex flex-col items-center">
            <text class="text-xl font-bold text-blue-500">
              {{ userInfo.cycle.current }}
            </text>
            <text class="text-xs text-gray-400">
              /{{ userInfo.cycle.total }}天
            </text>
          </view>
        </view>

        <view class="flex-1 space-y-2.5">
          <view class="flex justify-between">
            <text class="text-xs text-gray-400">
              开始日期
            </text>
            <text class="text-xs wot-text-text-main">
              {{ userInfo.cycle.startDate }}
            </text>
          </view>
          <view class="flex justify-between">
            <text class="text-xs text-gray-400">
              预计结束
            </text>
            <text class="text-xs wot-text-text-main">
              {{ userInfo.cycle.endDate }}
            </text>
          </view>
          <view class="flex justify-between">
            <text class="text-xs text-gray-400">
              健康管理师
            </text>
            <text class="text-xs wot-text-text-main">
              {{ userInfo.cycle.manager }}
            </text>
          </view>
        </view>
      </view>
    </view>

    <!-- 菜单列表 -->
    <view class="mx-4 mt-4 overflow-hidden rounded-2xl bg-white shadow-sm">
      <view
        v-for="(item, index) in menuList"
        :key="index"
        class="flex items-center justify-between px-4 py-3.5"
        :class="{ 'border-b border-gray-100': index !== menuList.length - 1 }"
        @click="handleMenuClick(item)"
      >
        <view class="flex items-center gap-3">
          <view
            class="flex h-9 w-9 items-center justify-center rounded-lg"
            :style="{ backgroundColor: item.bgColor }"
          >
            <wd-icon :name="item.icon" size="18px" :color="item.iconColor" />
          </view>
          <text class="text-sm wot-text-text-main">
            {{ item.title }}
          </text>
        </view>
        <wd-icon name="arrow-right" size="16px" color="#d1d5db" />
      </view>
    </view>

    <!-- 退出登录 -->
    <view class="mx-4 mt-4 rounded-2xl bg-white py-3.5 text-center shadow-sm" @click="handleLogout">
      <text class="text-sm text-red-500">
        退出登录
      </text>
    </view>

    <!-- 底部安全区域 -->
    <wd-gap safe-area-bottom height="20px" />
  </view>
</template>
