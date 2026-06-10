<script setup lang="ts">
definePage({
  name: 'data-areas',
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

const currentLocation = ref('北京市 · 朝阳区')

function refreshLocation() {
  uni.showLoading({ title: '定位中...' })
  setTimeout(() => {
    uni.hideLoading()
    uni.showToast({ title: '已更新当前位置', icon: 'success' })
  }, 800)
}

const provinces = ref([
  '北京市',
  '上海市',
  '广东省',
  '浙江省',
  '江苏省',
  '四川省',
  '湖北省',
])

let selectedProvince = ref('')

function selectProvince(province: string) {
  selectedProvince.value = province
  uni.showToast({ title: `已选择：${province}`, icon: 'none' })
}

function nextStep() {
  if (!selectedProvince.value) {
    uni.showToast({ title: '请先选择省份/直辖市', icon: 'none' })
    return
  }
  // TODO: 跳转至选择机构
  uni.showToast({ title: `进入下一步：${selectedProvince.value}`, icon: 'success' })
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
            选择所属区域
          </text>
          <text class="mt-0.5 text-xs text-gray-400">
            请选择您所在的省市区
          </text>
        </view>
      </view>
    </view>

    <!-- 步骤指示器 -->
    <view class="mt-4 flex items-center justify-center px-6">
      <view class="flex items-center">
        <!-- 步骤1 当前 -->
        <view
          class="z-10 flex h-7 w-7 items-center justify-center rounded-full text-xs font-bold"
          :style="{
            backgroundColor: '#3b82f6',
            color: '#fff',
            boxShadow: '0 0 0 4px #dbeafe',
          }"
        >
          1
        </view>
        <text class="ml-1.5 mr-2 text-xs font-medium text-blue-500">
          选择区域
        </text>
        <view class="h-px w-8 bg-blue-300" />
        <!-- 步骤2 -->
        <view
          class="z-10 ml-2 flex h-7 w-7 items-center justify-center rounded-full border bg-white text-xs font-medium"
          :style="{
            color: '#9ca3af',
            borderColor: '#e5e7eb',
          }"
        >
          2
        </view>
        <text class="ml-1.5 mr-2 text-xs text-gray-400">
          选择机构
        </text>
        <view class="h-px w-8 bg-gray-200" />
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

    <!-- 当前位置 -->
    <view class="mx-4 mt-4 flex items-center gap-3 rounded-2xl bg-white p-3 shadow-sm">
      <view
        class="flex h-9 w-9 shrink-0 items-center justify-center rounded-full"
        style="background-color: #eff6ff"
      >
        <wd-icon name="location" size="18px" color="#3b82f6" />
      </view>
      <view class="flex flex-1 flex-col">
        <text class="text-xs text-gray-400">
          当前定位
        </text>
        <text class="text-sm font-medium wot-text-text-main">
          {{ currentLocation }}
        </text>
      </view>
      <view
        class="flex h-8 w-8 items-center justify-center rounded-full"
        style="background-color: #f3f4f6"
        @click="refreshLocation"
      >
        <wd-icon name="refresh" size="14px" color="#6b7280" />
      </view>
    </view>

    <!-- 省份列表 -->
    <view class="mx-4 mt-3 rounded-2xl bg-white py-2 shadow-sm">
      <view class="px-4 pt-2 pb-2">
        <text class="text-sm font-medium wot-text-text-main">
          选择省份/直辖市
        </text>
      </view>

      <view class="px-4 pb-2">
        <view
          v-for="(item, idx) in provinces"
          :key="idx"
          class="flex items-center justify-between border-b border-gray-100 py-3 last:border-b-0"
          @click="selectProvince(item)"
        >
          <text class="text-sm" :class="selectedProvince === item ? 'text-blue-500 font-medium' : 'wot-text-text-main'">
            {{ item }}
          </text>
          <wd-icon name="arrow-right" size="14px" color="#d1d5db" />
        </view>
      </view>
    </view>

    <!-- 底部安全区域 + 下一步按钮 -->
    <view class="px-4 pt-6 pb-6">
      <wd-button
        type="primary"
        block
        custom-class="next-btn"
        @click="nextStep"
      >
        下一步：选择服务机构
      </wd-button>
    </view>

    <wd-gap safe-area-bottom height="20px" />
  </view>
</template>

<style lang="scss" scoped>
.next-btn {
  :deep(.wd-button) {
    border-radius: 999px;
    height: 48px;
    font-size: 15px;
    font-weight: 600;
  }
}
</style>
