<script setup lang="ts">
definePage({
  name: 'welcome',
  style: {
    navigationBarTitleText: '',
    navigationStyle: 'custom',
  },
})

const router = useRouter()

function handleStart() {
  // TODO: 跳转至注册或引导页
  router.push({ name: 'auth' })
}

// 图标颜色使用 oklch 格式，使用 alpha 通道控制透明度
const features = [
  {
    icon: 'i-carbon:cloud-upload',
    iconColor: '0.6507 0.1729 260.07',
    title: '智能数据上传',
    desc: '引导式上传，轻松管理健康报告',
  },
  {
    icon: 'i-carbon:notification',
    iconColor: '0.7227 0.192 149.58',
    title: '随访智能提醒',
    desc: '一对一贴心随访，不错过每次复查',
  },
  {
    icon: 'i-carbon:warning-alt',
    iconColor: '0.7232 0.1764 49.84',
    title: '异常数据预警',
    desc: '指标异常及时提醒，守护您的健康',
  },
]
</script>

<script lang="ts">
export default {
  options: {
    addGlobalClass: true,
    virtualHost: true,
    styleIsolation: 'shared',
  },
}
</script>

<template>
  <view class="welcome-page box-border min-h-screen flex flex-col items-center bg-white px-6 py-12">
    <!-- Logo -->
    <view class="mb-6 flex items-center justify-center rounded-3xl bg-gradient-to-br from-[#3B82F6] to-[#2867EC] h-20 w-20 shadow-lg color-[#FFF]">
      <i class="i-healthicons:heart-cardiogram-outline-24px w-12 h-12" />
    </view>

    <!-- 标题 -->
    <text class="mb-2 text-xl font-bold wot-text-text-main">
      知了糖眼管家
    </text>
    <text class="mb-10 text-sm wot-text-text-auxiliary">
      90天糖尿病全周期健康管理
    </text>

    <!-- 功能卡片 -->
    <view class="cards mb-10 w-full space-y-4">
      <view
        v-for="(item, index) in features"
        :key="index"
        class="flex items-center gap-4 rounded-2xl bg-white px-5 py-4 shadow-sm border border-gray-100"
      >
        <view
          class="icon flex h-10 w-10 shrink-0 items-center justify-center rounded-xl"
          :style="{ '--icon-color': `${item.iconColor}` }"
        >
          <i :class="item.icon" class="w-6 h-6" />
        </view>
        <view class="flex flex-col">
          <text class="text-base font-medium wot-text-text-main">
            {{ item.title }}
          </text>
          <text class="mt-1 text-xs wot-text-text-auxiliary">
            {{ item.desc }}
          </text>
        </view>
      </view>
    </view>

    <!-- 主按钮 -->
    <view class="mb-4 w-full">
      <wd-button type="primary" size="large" block custom-class="btn-start" @click="handleStart">
        开始健康之旅
      </wd-button>
    </view>

    <!-- 底部文案 -->
    <text class="mt-8 text-xs text-gray-400">
      专业医疗团队 · 全程健康守护
    </text>
  </view>
</template>
<style lang="scss">
.welcome-page {
  background-image: linear-gradient(to bottom, #EFF6FF, #FFF);
  .cards {
    .icon {
      color: oklch(var(--icon-color));
      background: oklch(var(--icon-color)/ .08);
    }
  }
  .wd-button.btn-start {
    font-weight: 700;
    border-radius: 100px;
  }
}
</style>
