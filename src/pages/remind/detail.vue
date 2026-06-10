<script setup lang="ts">
definePage({
  name: 'remind-detail',
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

function goBack() {
  uni.navigateBack()
}

const summary = {
  total: 1,
  title: '1项指标异常',
  subtitle: '请及时关注并咨询医生',
  high: 1,
  mid: 0,
  focus: 2,
}

// 高风险项
const highRisk = {
  type: '高风险',
  typeColor: '#ef4444',
  typeBg: '#fee2e2',
  metric: '空腹血糖',
  title: '空腹血糖持续偏高',
  current: '7.2',
  range: '4.4-6.1',
  trend: '↑ 12%',
  trendColor: '#ef4444',
  suggestion: '建议控制饮食中碳水化合物摄入，增加餐后运动，如持续偏高请及时就医调整用药方案。',
}

// 已关注项
const watched = {
  type: '已关注',
  typeColor: '#22c55e',
  typeBg: '#dcfce7',
  metric: '血压',
  title: '血压临界偏高',
  current: '135/85',
  range: '130/80',
  watchTime: '2025-02-01',
}

// 近7天趋势数据
const trendPoints = [5.2, 5.5, 5.3, 6.0, 6.4, 6.8, 7.2]
const trendLabels = ['02/01', '02/02', '02/03', '02/04', '02/05', '02/06', '02/07', '今天']
const trendMax = 8
const trendMin = 4

// 折线图点坐标计算
const chartWidth = 280
const chartHeight = 100
const chartPoints = computed(() => {
  const range = trendMax - trendMin
  return trendPoints.map((v, i) => {
    const x = (i / (trendPoints.length - 1)) * chartWidth
    const y = chartHeight - ((v - trendMin) / range) * chartHeight
    return { x, y }
  })
})

const polylinePoints = computed(() => {
  return chartPoints.value.map(p => `${p.x},${p.y}`).join(' ')
})

function goToChart() {
  // TODO: 跳转详细图表
}

function consultDoctor() {
  // TODO: 跳转咨询
}

function markFollowed() {
  // TODO: 标记已关注
}
</script>

<template>
  <view class="min-h-screen bg-[#f5f7fa]">
    <!-- 自定义导航栏 -->
    <view class="bg-white pb-3" :style="{ paddingTop: `${statusBarHeight + 12}px` }">
      <view class="flex items-center gap-3 px-4">
        <view
          class="flex h-9 w-9 items-center justify-center rounded-full bg-gray-100"
          @click="goBack"
        >
          <wd-icon name="arrow-left" size="18px" color="#374151" />
        </view>
        <view class="flex flex-col">
          <text class="text-lg font-bold wot-text-text-main">
            异常数据提醒
          </text>
          <text class="mt-0.5 text-xs text-gray-400">
            智能监测 · 及时预警
          </text>
        </view>
      </view>
    </view>

    <!-- 顶部红色统计卡 -->
    <view class="mx-4 mt-3 overflow-hidden rounded-2xl p-4" style="background: linear-gradient(135deg, #ff6b5a 0%, #ff8a3d 100%);">
      <view class="flex items-start gap-3">
        <view class="flex h-10 w-10 shrink-0 items-center justify-center rounded-full" style="background-color: rgba(255,255,255,0.2)">
          <wd-icon name="warning" size="20px" color="#fff" />
        </view>
        <view class="flex flex-col">
          <text class="text-lg font-bold text-white">
            {{ summary.title }}
          </text>
          <text class="mt-0.5 text-xs text-white/80">
            {{ summary.subtitle }}
          </text>
        </view>
      </view>

      <view class="mt-4 flex gap-2.5">
        <view class="flex-1 rounded-xl py-3 text-center" style="background-color: rgba(255,255,255,0.18)">
          <text class="block text-2xl font-bold text-white">
            {{ summary.high }}
          </text>
          <text class="mt-0.5 block text-xs text-white/80">
            高风险
          </text>
        </view>
        <view class="flex-1 rounded-xl py-3 text-center" style="background-color: rgba(255,255,255,0.18)">
          <text class="block text-2xl font-bold text-white">
            {{ summary.mid }}
          </text>
          <text class="mt-0.5 block text-xs text-white/80">
            中风险
          </text>
        </view>
        <view class="flex-1 rounded-xl py-3 text-center" style="background-color: rgba(255,255,255,0.18)">
          <text class="block text-2xl font-bold text-white">
            {{ summary.focus }}
          </text>
          <text class="mt-0.5 block text-xs text-white/80">
            已关注
          </text>
        </view>
      </view>
    </view>

    <!-- 高风险卡片 -->
    <view class="mx-4 mt-4 rounded-2xl bg-white p-4 shadow-sm" style="border: 1.5px solid #ef4444">
      <!-- 顶部 -->
      <view class="flex items-center justify-between">
        <view class="flex items-center gap-3">
          <view
            class="flex h-10 w-10 items-center justify-center rounded-full"
            :style="{ backgroundColor: highRisk.typeBg }"
          >
            <wd-icon name="warning" size="18px" :color="highRisk.typeColor" />
          </view>
          <view class="flex flex-col">
            <view class="flex items-center gap-2">
              <text
                class="rounded px-1.5 py-0.5 text-xs font-medium"
                :style="{ backgroundColor: highRisk.typeBg, color: highRisk.typeColor }"
              >
                {{ highRisk.type }}
              </text>
              <text class="text-xs text-gray-400">
                {{ highRisk.metric }}
              </text>
            </view>
            <text class="mt-1 text-base font-bold wot-text-text-main">
              {{ highRisk.title }}
            </text>
          </view>
        </view>
        <wd-icon name="arrow-right" size="16px" color="#d1d5db" />
      </view>

      <!-- 数据指标 -->
      <view class="mt-4 flex items-start">
        <view class="flex flex-1 flex-col">
          <text class="text-xs text-gray-400">
            当前值
          </text>
          <text class="mt-1 text-2xl font-bold" style="color: #ef4444">
            {{ highRisk.current }}
          </text>
          <text class="mt-0.5 text-xs text-gray-400">
            mmol/L
          </text>
        </view>
        <view class="flex flex-1 flex-col">
          <text class="text-xs text-gray-400">
            目标范围
          </text>
          <text class="mt-1 text-2xl font-bold text-green-500">
            {{ highRisk.range }}
          </text>
          <text class="mt-0.5 text-xs text-gray-400">
            mmol/L
          </text>
        </view>
        <view class="flex flex-1 flex-col">
          <text class="text-xs text-gray-400">
            近3天趋势
          </text>
          <text class="mt-1 text-2xl font-bold" :style="{ color: highRisk.trendColor }">
            {{ highRisk.trend }}
          </text>
          <text class="mt-0.5 text-xs text-gray-400">
            &nbsp;
          </text>
        </view>
      </view>

      <!-- 健康建议 -->
      <view class="mt-4 rounded-xl p-3" style="background-color: #fff7ed">
        <view class="mb-1 flex items-center gap-1.5">
          <wd-icon name="tips" size="14px" color="#f59e0b" />
          <text class="text-sm font-medium" style="color: #ea580c">
            健康建议
          </text>
        </view>
        <text class="block text-xs leading-relaxed text-gray-500">
          {{ highRisk.suggestion }}
        </text>
      </view>

      <!-- 操作按钮 -->
      <view class="mt-4 flex gap-3">
        <view
          class="flex flex-1 items-center justify-center gap-1.5 rounded-full py-2.5"
          style="background-color: #3b82f6"
          @click="consultDoctor"
        >
          <wd-icon name="service" size="14px" color="#fff" />
          <text class="text-sm font-medium text-white">
            咨询医生
          </text>
        </view>
        <view
          class="flex flex-1 items-center justify-center gap-1.5 rounded-full py-2.5"
          style="background-color: #f3f4f6"
          @click="markFollowed"
        >
          <wd-icon name="check" size="14px" color="#374151" />
          <text class="text-sm font-medium text-gray-700">
            标记已关注
          </text>
        </view>
      </view>
    </view>

    <!-- 已关注卡片 -->
    <view class="mx-4 mt-3 rounded-2xl bg-white p-4 shadow-sm">
      <view class="flex items-center justify-between">
        <view class="flex items-center gap-3">
          <view
            class="flex h-10 w-10 items-center justify-center rounded-full"
            :style="{ backgroundColor: watched.typeBg }"
          >
            <wd-icon name="check" size="18px" :color="watched.typeColor" />
          </view>
          <view class="flex flex-col">
            <view class="flex items-center gap-2">
              <text
                class="rounded px-1.5 py-0.5 text-xs font-medium"
                :style="{ backgroundColor: watched.typeBg, color: watched.typeColor }"
              >
                {{ watched.type }}
              </text>
              <text class="text-xs text-gray-400">
                {{ watched.metric }}
              </text>
            </view>
            <text class="mt-1 text-base font-medium text-gray-500">
              {{ watched.title }}
            </text>
          </view>
        </view>
        <wd-icon name="arrow-right" size="16px" color="#d1d5db" />
      </view>

      <view class="mt-4 flex items-start">
        <view class="flex flex-1 flex-col">
          <text class="text-xs text-gray-400">
            当前值
          </text>
          <text class="mt-1 text-2xl font-bold text-gray-500">
            {{ watched.current }}
          </text>
          <text class="mt-0.5 text-xs text-gray-400">
            mmHg
          </text>
        </view>
        <view class="flex flex-1 flex-col">
          <text class="text-xs text-gray-400">
            目标范围
          </text>
          <text class="mt-1 text-2xl font-bold text-gray-500">
            {{ watched.range }}
          </text>
          <text class="mt-0.5 text-xs text-gray-400">
            mmHg
          </text>
        </view>
        <view class="flex flex-1 flex-col">
          <text class="text-xs text-gray-400">
            关注时间
          </text>
          <text class="mt-1 text-2xl font-bold text-gray-500">
            {{ watched.watchTime.substring(5) }}
          </text>
          <text class="mt-0.5 text-xs text-gray-400">
            &nbsp;
          </text>
        </view>
      </view>
    </view>

    <!-- 近7天趋势 -->
    <view class="mx-4 mt-3 rounded-2xl bg-white p-4 shadow-sm">
      <view class="mb-4 flex items-center justify-between">
        <text class="text-base font-bold wot-text-text-main">
          近7天空腹血糖趋势
        </text>
        <text class="text-sm text-blue-500" @click="goToChart">
          详细图表
        </text>
      </view>

      <!-- 折线图 -->
      <view class="relative w-full" :style="{ height: `${chartHeight}px` }">
        <!-- 横向虚线 -->
        <view
          v-for="line in 3"
          :key="line"
          class="absolute left-0 right-0 border-t border-dashed border-gray-200"
          :style="{ top: `${(line - 1) * 33}px` }"
        />
        <!-- 折线 -->
        <svg
          :viewBox="`0 0 ${chartWidth} ${chartHeight}`"
          preserveAspectRatio="none"
          class="absolute inset-0 h-full w-full"
        >
          <polyline
            :points="polylinePoints"
            fill="none"
            stroke="#ef4444"
            stroke-width="2"
            stroke-linejoin="round"
            stroke-linecap="round"
          />
        </svg>
        <!-- 折线点 -->
        <view
          v-for="(p, i) in chartPoints"
          :key="i"
          class="absolute h-2 w-2 -translate-x-1 -translate-y-1 rounded-full"
          :style="{
            left: `${(p.x / chartWidth) * 100}%`,
            top: `${(p.y / chartHeight) * 100}%`,
            backgroundColor: '#ef4444',
          }"
        />
      </view>

      <!-- X轴标签 -->
      <view class="mt-2 flex justify-between">
        <text
          v-for="(label, i) in trendLabels"
          :key="i"
          class="text-xs text-gray-400"
        >
          {{ label }}
        </text>
      </view>
    </view>

    <!-- 底部安全区域 -->
    <wd-gap safe-area-bottom height="20px" />
  </view>
</template>
