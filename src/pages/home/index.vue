<script setup lang="ts">
definePage({
  name: 'home',
  style: {
    navigationBarTitleText: '',
    navigationStyle: 'custom',
  },
})

const statusBarHeight = ref(20)

// #ifdef MP-WEIXIN
const sysInfo = uni.getSystemInfoSync()
statusBarHeight.value = sysInfo.statusBarHeight || 20
// #endif

const user = {
  avatarText: '张',
  greet: '早上好',
  name: '张建国',
}

const cycle = {
  days: 23,
  total: 90,
  progress: 25.5,
}

const shortcuts = [
  { icon: 'upload', iconColor: '#3b82f6', title: '上传数据', sub: '引导式\n上传' },
  { icon: 'calendar', iconColor: '#22c55e', title: '随访提醒', sub: '2条待\n处理' },
  { icon: 'warning', iconColor: '#f59e0b', title: '异常提醒', sub: '1项异\n常' },
]

const indicators = [
  {
    name: '空腹血糖',
    value: '6.8',
    unit: 'mmol/L',
    status: '偏高',
    statusColor: '#ef4444',
    statusBg: '#fee2e2',
  },
  {
    name: '糖化血红蛋白',
    value: '6.2',
    unit: '%',
    status: '正常',
    statusColor: '#22c55e',
    statusBg: '#dcfce7',
  },
  {
    name: '血压',
    value: '135',
    unit: '/85 mmHg',
    status: '临界',
    statusColor: '#f59e0b',
    statusBg: '#fef3c7',
  },
]

const reminds = [
  {
    icon: 'warning',
    iconColor: '#ef4444',
    iconBg: '#fee2e2',
    title: '空腹血糖偏高',
    desc: '最近3天空腹血糖均值7.2mmol/L，超出控制目标',
    time: '今天 08:30',
  },
  {
    icon: 'edit',
    iconColor: '#3b82f6',
    iconBg: '#eff6ff',
    title: '随访提醒：第4周复查',
    desc: '请于3天内完成糖化血红蛋白及血脂四项检查',
    deadline: '截止日期：2025-02-15',
  },
]

const reports = [
  { name: '血常规', status: '已上传', color: '#22c55e' },
  { name: '血脂四项', status: '已上传', color: '#22c55e' },
  { name: '肝功能报告', status: '待上传', color: '#f59e0b' },
  { name: '肾功能报告', status: '待上传', color: '#f59e0b' },
  { name: '超声报告', status: '待上传', color: '#f59e0b' },
  { name: '糖化检查报告', status: '待上传', color: '#f59e0b' },
]

const activities = [
  {
    tag: '免费',
    tagColor: '#22c55e',
    tagBg: '#dcfce7',
    time: '3月15日 08:30-11:30',
    title: '慢病院控糖筛查免费测活动',
    desc: '内分泌科举办糖尿病免费筛查，含空腹血糖、糖化血红蛋白检查及专家咨询',
    location: '|  慢病院门诊一楼大厅',
  },
  {
    tag: '免费',
    tagColor: '#22c55e',
    tagBg: '#dcfce7',
    time: '3月22日 14:00-16:00',
    title: '社区健康讲座：糖尿病饮食管理',
    desc: '营养科专家讲解糖尿病饮食原则，现场提供个性化饮食方案指导及免费体检套餐',
    location: '|  和平区疾控艾防社区活动中心 2楼会议室',
  },
  {
    tag: '免费',
    tagColor: '#22c55e',
    tagBg: '#dcfce7',
    time: '4月5日 07:00-09:00',
    title: '春季糖友健步走活动',
    desc: '组织糖友开展春季户外健步走，专业医护全程陪同，现场免费测血糖及足部检查',
    location: '|  中山公园南门集合',
  },
]

function handleNotify() {}
function goToAll() {}
function goToUpload() {}
function goToMore() {}
</script>

<template>
  <view class="min-h-screen bg-[#f5f7fa]">
    <!-- 顶部蓝色背景区域 -->
    <view
      class="bg-gradient-to-br from-[#4a90e2] to-[#357abd] px-4 pb-24"
      :style="{ paddingTop: `${statusBarHeight + 12}px` }"
    >
      <!-- 头部 -->
      <view class="flex items-center justify-between">
        <view class="flex items-center gap-3">
          <view
            class="flex h-10 w-10 items-center justify-center rounded-full text-base font-bold text-white"
            style="background-color: rgba(255,255,255,0.2)"
          >
            {{ user.avatarText }}
          </view>
          <view class="flex flex-col">
            <text class="text-xs text-white/80">
              {{ user.greet }}
            </text>
            <text class="text-base font-bold text-white">
              {{ user.name }}
            </text>
          </view>
        </view>
        <view
          class="flex h-9 w-9 items-center justify-center rounded-full"
          style="background-color: rgba(255,255,255,0.15)"
          @click="handleNotify"
        >
          <wd-icon name="bell" size="18px" color="#fff" />
          <view class="absolute mt-3 ml-4 h-2 w-2 rounded-full bg-red-500" />
        </view>
      </view>

      <!-- 90天管理周期卡 -->
      <view class="mt-4 rounded-2xl p-4" style="background-color: rgba(255,255,255,0.15)">
        <view class="flex items-center justify-between">
          <view class="flex items-center gap-2">
            <wd-icon name="calendar" size="16px" color="#fff" />
            <text class="text-sm text-white">
              90天健康周期
            </text>
          </view>
          <view class="rounded-full bg-white px-3 py-0.5">
            <text class="text-xs font-medium text-blue-500">
              第 {{ cycle.days }} 天
            </text>
          </view>
        </view>

        <view class="mt-3 flex items-center justify-between">
          <text class="text-xs text-white/80">
            开始
          </text>
          <text class="text-xs text-white/80">
            已完成 {{ cycle.progress }}%
          </text>
          <text class="text-xs text-white/80">
            90天
          </text>
        </view>

        <view class="mt-2 h-1.5 w-full overflow-hidden rounded-full" style="background-color: rgba(255,255,255,0.2)">
          <view
            class="h-full rounded-full bg-white"
            :style="{ width: `${cycle.progress}%` }"
          />
        </view>
      </view>
    </view>

    <!-- 三个快捷入口 -->
    <view class="mx-4 -mt-16 rounded-2xl bg-white p-4 shadow-sm">
      <view class="flex justify-between">
        <view
          v-for="(item, idx) in shortcuts"
          :key="idx"
          class="flex flex-1 flex-col items-center"
          @click="goToUpload"
        >
          <view
            class="flex h-10 w-10 items-center justify-center rounded-lg"
            :style="{ backgroundColor: `${item.iconColor}14` }"
          >
            <wd-icon :name="item.icon" size="20px" :color="item.iconColor" />
          </view>
          <text class="mt-2 text-sm font-medium wot-text-text-main">
            {{ item.title }}
          </text>
          <text class="mt-1 text-center text-xs leading-tight text-gray-400 whitespace-pre-line">
            {{ item.sub }}
          </text>
        </view>
      </view>
    </view>

    <!-- 关键健康指标 -->
    <view class="mx-4 mt-3 rounded-2xl bg-white p-4 shadow-sm">
      <view class="mb-3 flex items-center justify-between">
        <text class="text-base font-bold wot-text-text-main">
          关键健康指标
        </text>
        <view class="flex items-center gap-1" @click="goToAll">
          <text class="text-sm text-blue-500">
            查看全部
          </text>
          <wd-icon name="arrow-right" size="12px" color="#3b82f6" />
        </view>
      </view>

      <view class="flex gap-2.5">
        <view
          v-for="(item, idx) in indicators"
          :key="idx"
          class="flex-1 rounded-xl p-3"
          :style="{ backgroundColor: item.statusBg }"
        >
          <text class="text-xs text-gray-500">
            {{ item.name }}
          </text>
          <text class="mt-1 block text-2xl font-bold wot-text-text-main">
            {{ item.value }}
          </text>
          <text class="text-xs text-gray-400">
            {{ item.unit }}
          </text>
          <view
            class="mt-2 inline-block rounded px-1.5 py-0.5"
            :style="{ backgroundColor: `${item.statusColor}29`, color: item.statusColor }"
          >
            <text class="text-xs">
              {{ item.status }}
            </text>
          </view>
        </view>
      </view>
    </view>

    <!-- 最新提醒 -->
    <view class="mx-4 mt-3 rounded-2xl bg-white p-4 shadow-sm">
      <view class="mb-3 flex items-center justify-between">
        <text class="text-base font-bold wot-text-text-main">
          最新提醒
        </text>
        <view class="flex items-center gap-1" @click="goToAll">
          <text class="text-sm text-blue-500">
            全部
          </text>
          <wd-icon name="arrow-right" size="12px" color="#3b82f6" />
        </view>
      </view>

      <view class="space-y-3">
        <view
          v-for="(item, idx) in reminds"
          :key="idx"
          class="flex items-start gap-3 rounded-xl p-3"
          :style="{ backgroundColor: idx === 0 ? '#fef2f2' : '#eff6ff' }"
        >
          <view
            class="flex h-9 w-9 shrink-0 items-center justify-center rounded-lg"
            :style="{ backgroundColor: item.iconBg }"
          >
            <wd-icon :name="item.icon" size="16px" :color="item.iconColor" />
          </view>
          <view class="flex flex-1 flex-col">
            <text class="text-sm font-medium wot-text-text-main">
              {{ item.title }}
            </text>
            <text class="mt-1 text-xs leading-relaxed text-gray-500">
              {{ item.desc }}
            </text>
            <text class="mt-1.5 text-xs text-gray-400">
              {{ item.time || item.deadline }}
            </text>
          </view>
        </view>
      </view>
    </view>

    <!-- 数据上传进度 -->
    <view class="mx-4 mt-3 rounded-2xl bg-white p-4 shadow-sm">
      <view class="mb-3 flex items-center justify-between">
        <text class="text-base font-bold wot-text-text-main">
          数据上传进度
        </text>
        <text class="text-sm text-blue-500" @click="goToUpload">
          去上传
        </text>
      </view>

      <view class="space-y-2.5">
        <view
          v-for="(item, idx) in reports"
          :key="idx"
          class="flex items-center justify-between"
        >
          <view class="flex items-center gap-2">
            <view
              class="flex h-5 w-5 items-center justify-center rounded-full"
              :style="{ backgroundColor: item.color }"
            >
              <wd-icon name="check" size="12px" color="#fff" />
            </view>
            <text class="text-sm text-gray-700">
              {{ item.name }}
            </text>
          </view>
          <text class="text-sm" :style="{ color: item.color }">
            {{ item.status }}
          </text>
        </view>
      </view>
    </view>

    <!-- 健康活动通知 -->
    <view class="mx-4 mt-3 mb-4 rounded-2xl bg-white p-4 shadow-sm">
      <view class="mb-3 flex items-center justify-between">
        <text class="text-base font-bold wot-text-text-main">
          健康活动通知
        </text>
        <view class="flex items-center gap-1" @click="goToMore">
          <text class="text-sm text-blue-500">
            更多活动
          </text>
          <wd-icon name="arrow-right" size="12px" color="#3b82f6" />
        </view>
      </view>

      <view class="space-y-3">
        <view
          v-for="(item, idx) in activities"
          :key="idx"
          class="rounded-xl p-3"
          :style="{ backgroundColor: item.tagBg }"
        >
          <view class="mb-1.5 flex items-center justify-between">
            <view class="flex items-center gap-2">
              <text
                class="rounded px-1.5 py-0.5 text-xs font-medium"
                :style="{ backgroundColor: item.tagColor, color: '#fff' }"
              >
                {{ item.tag }}
              </text>
            </view>
            <text class="text-xs text-gray-500">
              {{ item.time }}
            </text>
          </view>
          <text class="block text-sm font-medium wot-text-text-main">
            {{ item.title }}
          </text>
          <text class="mt-1 block text-xs leading-relaxed text-gray-500">
            {{ item.desc }}
          </text>
          <view class="mt-1.5 flex items-center gap-1">
            <wd-icon name="location" size="12px" color="#9ca3af" />
            <text class="text-xs text-gray-500">
              {{ item.location }}
            </text>
          </view>
        </view>
      </view>
    </view>

    <!-- 底部安全区域 -->
    <wd-gap safe-area-bottom height="20px" />
  </view>
</template>
