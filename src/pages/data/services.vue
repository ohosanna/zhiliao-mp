<script setup lang="ts">
definePage({
  name: 'data-services',
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

const searchKeyword = ref('')

const filterTabs = ref([
  { name: 'all', label: '全部' },
  { name: 'community', label: '社区卫生中心' },
  { name: 'secondary', label: '二级医院' },
  { name: 'tertiary', label: '三级医院' },
])

const activeFilter = ref('all')

function selectFilter(name: string) {
  activeFilter.value = name
}

interface ServiceItem {
  id: number
  icon: string
  iconColor: string
  iconBg: string
  name: string
  type: string
  rating: string
  distance: string
  tags: { text: string, color: string, bg: string }[]
}

const services = ref<ServiceItem[]>([
  {
    id: 1,
    icon: 'home',
    iconColor: '#3b82f6',
    iconBg: '#dbeafe',
    name: '朝阳区劲松社区卫生服务中心',
    type: '社区卫生中心',
    rating: '4.8',
    distance: '1.2km',
    tags: [
      { text: '糖尿病专科', color: '#22c55e', bg: '#dcfce7' },
      { text: '医保定点', color: '#3b82f6', bg: '#dbeafe' },
    ],
  },
  {
    id: 2,
    icon: 'home',
    iconColor: '#22c55e',
    iconBg: '#dcfce7',
    name: '朝阳区双井社区卫生服务中心',
    type: '社区卫生中心',
    rating: '4.6',
    distance: '2.5km',
    tags: [
      { text: '糖尿病专科', color: '#22c55e', bg: '#dcfce7' },
      { text: '医保定点', color: '#3b82f6', bg: '#dbeafe' },
    ],
  },
  {
    id: 3,
    icon: 'hospital',
    iconColor: '#f97316',
    iconBg: '#ffedd5',
    name: '北京朝阳医院（内分泌科）',
    type: '三级甲等',
    rating: '4.9',
    distance: '3.8km',
    tags: [
      { text: '内分泌重点', color: '#22c55e', bg: '#dcfce7' },
      { text: '医保定点', color: '#3b82f6', bg: '#dbeafe' },
      { text: '三甲', color: '#3b82f6', bg: '#dbeafe' },
    ],
  },
])

let selectedId = ref(1)

function selectService(id: number) {
  selectedId.value = id
}

function confirmSelection() {
  const selected = services.value.find(s => s.id === selectedId.value)
  if (!selected) {
    uni.showToast({ title: '请先选择机构', icon: 'none' })
    return
  }
  // TODO: 提交选择
  uni.showToast({ title: `已选择：${selected.name}`, icon: 'success' })
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
            选择卫生服务机构
          </text>
          <text class="mt-0.5 text-xs text-gray-400">
            北京市 · 朝阳区
          </text>
        </view>
      </view>
    </view>

    <!-- 步骤指示器 -->
    <view class="mt-4 flex items-center justify-center px-6">
      <view class="flex items-center">
        <!-- 步骤1 已完成 -->
        <view
          class="z-10 flex h-7 w-7 items-center justify-center rounded-full"
          :style="{
            backgroundColor: '#22c55e',
            color: '#fff',
          }"
        >
          <wd-icon name="check" size="12px" color="#fff" />
        </view>
        <text class="ml-1.5 mr-2 text-xs text-gray-400">
          选择区域
        </text>
        <view class="h-px w-8 bg-green-500" />
        <!-- 步骤2 当前 -->
        <view
          class="z-10 ml-2 flex h-7 w-7 items-center justify-center rounded-full text-xs font-bold"
          :style="{
            backgroundColor: '#3b82f6',
            color: '#fff',
            boxShadow: '0 0 0 4px #dbeafe',
          }"
        >
          2
        </view>
        <text class="ml-1.5 mr-2 text-xs font-medium text-blue-500">
          选择机构
        </text>
        <view class="h-px w-8 bg-blue-300" />
        <!-- 步骤3 -->
        <view
          class="z-10 ml-2 flex h-7 w-7 items-center justify-center rounded-full border bg-white text-xs font-medium"
          :style="{
            color: '#9ca3af',
            borderColor: '#e5e7eb',
          }"
        >
          3
        </view>
        <text class="ml-1.5 text-xs text-gray-400">
          完成
        </text>
      </view>
    </view>

    <!-- 搜索框 -->
    <view class="mx-4 mt-4">
      <wd-search
        v-model="searchKeyword"
        placeholder="搜索机构名称"
        hide-cancel
        custom-class="search-bar"
      />
    </view>

    <!-- 筛选标签 -->
    <view class="mt-3 px-4">
      <view class="flex gap-2.5 overflow-x-auto">
        <view
          v-for="tab in filterTabs"
          :key="tab.name"
          class="shrink-0 rounded-full px-4 py-1.5"
          :style="activeFilter === tab.name
            ? { backgroundColor: '#3b82f6', color: '#fff' }
            : { backgroundColor: '#f3f4f6', color: '#6b7280' }"
          @click="selectFilter(tab.name)"
        >
          <text
            class="text-sm"
            :style="activeFilter === tab.name ? 'color: #fff' : 'color: #6b7280'"
          >
            {{ tab.label }}
          </text>
        </view>
      </view>
    </view>

    <!-- 机构列表 -->
    <view class="mx-4 mt-3 space-y-3">
      <view
        v-for="item in services"
        :key="item.id"
        class="rounded-2xl bg-white p-4 shadow-sm"
        :style="selectedId === item.id
          ? { border: '1.5px solid #3b82f6' }
          : { border: '1.5px solid transparent' }"
        @click="selectService(item.id)"
      >
        <view class="flex items-start gap-3">
          <!-- 图标 -->
          <view
            class="flex h-11 w-11 shrink-0 items-center justify-center rounded-lg"
            :style="{ backgroundColor: item.iconBg }"
          >
            <wd-icon :name="item.icon" size="22px" :color="item.iconColor" />
          </view>

          <!-- 内容 -->
          <view class="flex flex-1 flex-col">
            <text class="text-sm font-bold wot-text-text-main">
              {{ item.name }}
            </text>
            <text class="mt-0.5 text-xs text-gray-400">
              {{ item.type }}
            </text>

            <view class="mt-2 flex items-center gap-3 text-xs text-gray-500">
              <view class="flex items-center gap-1">
                <wd-icon name="star" size="12px" color="#f59e0b" />
                <text class="text-xs font-medium text-gray-700">
                  {{ item.rating }}
                </text>
              </view>
              <view class="flex items-center gap-1">
                <wd-icon name="location" size="12px" color="#9ca3af" />
                <text>
                  {{ item.distance }}
                </text>
              </view>
            </view>

            <view class="mt-2 flex gap-1.5">
              <text
                v-for="(tag, tIdx) in item.tags"
                :key="tIdx"
                class="rounded px-1.5 py-0.5 text-xs"
                :style="{ backgroundColor: tag.bg, color: tag.color }"
              >
                {{ tag.text }}
              </text>
            </view>
          </view>

          <!-- 选中状态 -->
          <view
            class="flex h-5 w-5 shrink-0 items-center justify-center rounded-full"
            :style="selectedId === item.id
              ? { backgroundColor: '#3b82f6' }
              : { backgroundColor: 'transparent', border: '1.5px solid #d1d5db' }"
          >
            <wd-icon v-if="selectedId === item.id" name="check" size="12px" color="#fff" />
          </view>
        </view>
      </view>
    </view>

    <!-- 底部按钮 -->
    <view class="px-4 pt-6 pb-6">
      <wd-button
        type="primary"
        block
        custom-class="confirm-btn"
        @click="confirmSelection"
      >
        确认选择，开始健康管理
      </wd-button>
    </view>

    <wd-gap safe-area-bottom height="20px" />
  </view>
</template>

<style lang="scss" scoped>
.search-bar {
  :deep(.wd-search) {
    background-color: #f3f4f6;
    border-radius: 999px;
  }
  :deep(.wd-search__inner) {
    border-radius: 999px;
  }
  :deep(.wd-search__block) {
    background-color: #f3f4f6;
  }
  :deep(.wd-search__input) {
    background-color: transparent;
  }
}

.confirm-btn {
  :deep(.wd-button) {
    border-radius: 999px;
    height: 48px;
    font-size: 15px;
    font-weight: 600;
  }
}
</style>
