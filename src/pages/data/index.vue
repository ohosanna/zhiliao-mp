<script setup lang="ts">
definePage({
  name: 'data-form',
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

const formData = ref({
  reportType: '', // 报告类型
  testDate: '', // 检测日期
  fastingGlucose: '', // 空腹血糖
  postprandial2h: '', // 餐后2h血糖
  hba1c: '', // 糖化血红蛋白
  systolicBP: '', // 收缩压（高压）
  diastolicBP: '', // 舒张压（低压）
  weight: '', // 体重
  notes: '', // 备注
})

const reportTypeOptions = [
  { label: '血常规', value: 'blood' },
  { label: '血脂四项', value: 'lipid' },
  { label: '肝功能', value: 'liver' },
  { label: '肾功能', value: 'kidney' },
  { label: '糖化血红蛋白', value: 'hba1c' },
  { label: '其他', value: 'other' },
]

let showReportPicker = ref(false)
let selectedReportLabel = ref('请选择报告类型')

function openReportPicker() {
  showReportPicker.value = true
}

function onReportSelect({ item }: any) {
  formData.value.reportType = item.value
  selectedReportLabel.value = item.label
}

let showDatePicker = ref(false)

function openDatePicker() {
  showDatePicker.value = true
}

function onDateConfirm({ value }: { value: string }) {
  formData.value.testDate = value
}

let showHbA1cPicker = ref(false)
let showWeightPicker = ref(false)

const fileList = ref<string[]>([])

function onChooseFile(e: any) {
  if (e.tempFilePaths) {
    fileList.value.push(...e.tempFilePaths)
  }
}

function removeFile(index: number) {
  fileList.value.splice(index, 1)
}

function handleSubmit() {
  if (!formData.value.reportType) {
    uni.showToast({ title: '请选择报告类型', icon: 'none' })
    return
  }
  if (!formData.value.testDate) {
    uni.showToast({ title: '请选择检测日期', icon: 'none' })
    return
  }
  uni.showToast({ title: '上传成功', icon: 'success' })
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
        <text class="text-lg font-bold wot-text-text-main">
          数据上传
        </text>
      </view>
    </view>

    <!-- 表单区域 -->
    <view class="px-4 pt-3 pb-6">
      <!-- 报告类型 -->
      <view class="mb-4 rounded-2xl bg-white p-4 shadow-sm">
        <view class="mb-3 flex items-center gap-2">
          <view class="h-1 w-1 rounded-full bg-blue-500" />
          <text class="text-sm font-bold wot-text-text-main">
            基本信息
          </text>
        </view>

        <!-- 报告类型选择 -->
        <view class="mb-3 flex items-center justify-between rounded-xl border border-gray-200 px-4 py-3" @click="openReportPicker">
          <text class="text-sm" :class="selectedReportLabel === '请选择报告类型' ? 'text-gray-400' : 'wot-text-text-main'">
            {{ selectedReportLabel }}
          </text>
          <wd-icon name="arrow-right" size="14px" color="#d1d5db" />
        </view>

        <!-- 检测日期 -->
        <view
          class="flex items-center justify-between rounded-xl border border-gray-200 px-4 py-3"
          @click="openDatePicker"
        >
          <text class="text-sm" :class="!formData.testDate ? 'text-gray-400' : 'wot-text-text-main'">
            {{ formData.testDate || '请选择检测日期' }}
          </text>
          <view class="flex items-center gap-1">
            <text v-if="formData.testDate" class="text-xs text-gray-400">已选</text>
            <wd-icon name="calendar" size="14px" color="#9ca3af" />
          </view>
        </view>
      </view>

      <!-- 血糖指标 -->
      <view class="mb-4 rounded-2xl bg-white p-4 shadow-sm">
        <view class="mb-3 flex items-center gap-2">
          <view class="h-1 w-1 rounded-full bg-red-500" />
          <text class="text-sm font-bold wot-text-text-main">
            血糖指标
          </text>
        </view>

        <view class="space-y-3">
          <!-- 空腹血糖 -->
          <view class="flex items-center gap-3">
            <text class="w-20 text-sm text-gray-500 shrink-0">
              空腹血糖
            </text>
            <view class="flex flex-1 items-center rounded-lg border border-gray-200 px-3 py-2.5">
              <wd-input
                v-model="formData.fastingGlucose"
                placeholder="请输入空腹血糖值"
                type="digit"
                custom-class="form-input"
                clearable
              />
            </view>
            <text class="w-12 text-right text-xs text-gray-400">
              mmol/L
            </text>
          </view>

          <!-- 餐后2h血糖 -->
          <view class="flex items-center gap-3">
            <text class="w-20 text-sm text-gray-500 shrink-0">
              餐后2h血糖
            </text>
            <view class="flex flex-1 items-center rounded-lg border border-gray-200 px-3 py-2.5">
              <wd-input
                v-model="formData.postprandial2h"
                placeholder="请输入餐后2h血糖值"
                type="digit"
                custom-class="form-input"
                clearable
              />
            </view>
            <text class="w-12 text-right text-xs text-gray-400">
              mmol/L
            </text>
          </view>

          <!-- 糖化血红蛋白 -->
          <view class="flex items-center gap-3">
            <text class="w-20 text-sm text-gray-500 shrink-0">
              糖化血红蛋白
            </text>
            <view class="flex flex-1 items-center rounded-lg border border-gray-200 px-3 py-2.5">
              <wd-input
                v-model="formData.hba1c"
                placeholder="请输入糖化血红蛋白值"
                type="digit"
                custom-class="form-input"
                clearable
              />
            </view>
            <text class="w-12 text-right text-xs text-gray-400">
              %
            </text>
          </view>
        </view>
      </view>

      <!-- 其他指标 -->
      <view class="mb-4 rounded-2xl bg-white p-4 shadow-sm">
        <view class="mb-3 flex items-center gap-2">
          <view class="h-1 w-1 rounded-full bg-green-500" />
          <text class="text-sm font-bold wot-text-text-main">
            其他指标
          </text>
        </view>

        <view class="space-y-3">
          <!-- 血压 -->
          <view class="flex items-center gap-3">
            <text class="w-20 text-sm text-gray-500 shrink-0">
              血压
            </text>
            <view class="flex flex-1 items-center gap-2 rounded-lg border border-gray-200 px-3 py-2.5">
              <wd-input
                v-model="formData.systolicBP"
                placeholder="收缩压"
                type="digit"
                custom-class="form-input-mini"
                clearable
              />
              <text class="text-xs text-gray-400">
                /
              </text>
              <wd-input
                v-model="formData.diastolicBP"
                placeholder="舒张压"
                type="digit"
                custom-class="form-input-mini"
                clearable
              />
            </view>
            <text class="w-12 text-right text-xs text-gray-400">
              mmHg
            </text>
          </view>

          <!-- 体重 -->
          <view class="flex items-center gap-3">
            <text class="w-20 text-sm text-gray-500 shrink-0">
              体重
            </text>
            <view class="flex flex-1 items-center rounded-lg border border-gray-200 px-3 py-2.5">
              <wd-input
                v-model="formData.weight"
                placeholder="请输入体重"
                type="digit"
                custom-class="form-input"
                clearable
              />
            </view>
            <text class="w-12 text-right text-xs text-gray-400">
              kg
            </text>
          </view>
        </view>
      </view>

      <!-- 上传报告图片 -->
      <view class="mb-4 rounded-2xl bg-white p-4 shadow-sm">
        <view class="mb-3 flex items-center gap-2">
          <view class="h-1 w-1 rounded-full bg-orange-500" />
          <text class="text-sm font-bold wot-text-text-main">
            上传报告图片
          </text>
        </view>

        <view class="grid grid-cols-4 gap-3">
          <view
            v-for="(file, idx) in fileList"
            :key="idx"
            class="relative h-20 w-full overflow-hidden rounded-lg bg-gray-100"
          >
            <image :src="file" mode="aspectFill" class="h-full w-full" />
            <view
              class="absolute right-1 top-1 flex h-5 w-5 items-center justify-center rounded-full bg-black/50"
              @click="removeFile(idx)"
            >
              <text class="text-xs text-white">×</text>
            </view>
          </view>
          <!-- 上传按钮 -->
          <view
            class="flex h-20 w-full cursor-pointer flex-col items-center justify-center rounded-lg border border-dashed border-gray-300 bg-gray-50"
            @click="$refs.uploadRef?.chooseFile()"
          >
            <wd-icon name="add" size="22px" color="#9ca3af" />
            <text class="mt-1 text-xs text-gray-400">
              上传
            </text>
          </view>
        </view>

        <!-- 隐藏的上传组件 -->
        <wd-upload
          ref="uploadRef"
          accept="image"
          :limit="9"
          :file-list="fileList"
          @change="onChooseFile"
          style="position:absolute;opacity:0;height:0;width:0;overflow:hidden;"
        />
      </view>

      <!-- 备注 -->
      <view class="mb-6 rounded-2xl bg-white p-4 shadow-sm">
        <view class="mb-3 flex items-center gap-2">
          <view class="h-1 w-1 rounded-full bg-purple-500" />
          <text class="text-sm font-bold wot-text-text-main">
            备注
          </text>
        </view>
        <view class="rounded-xl border border-gray-200 p-3">
          <wd-textarea
            v-model="formData.notes"
            placeholder="请填写备注信息，如特殊情况说明等..."
            :maxlength="200"
            show-word-limit
            custom-class="form-textarea"
          />
        </view>
      </view>

      <!-- 提交按钮 -->
      <wd-button
        type="primary"
        block
        custom-class="submit-btn"
        @click="handleSubmit"
      >
        提交上传
      </wd-button>
    </view>

    <!-- 底部安全区域 -->
    <wd-gap safe-area-bottom height="30px" />

    <!-- 选择器弹窗：报告类型 -->
    <wd-picker
      v-model="showReportPicker"
      :columns="[reportTypeOptions]"
      label-key="label"
      value-key="value"
      title="选择报告类型"
      @confirm="onReportSelect"
    />

    <!-- 日期选择器 -->
    <wd-datetime-picker
      v-model="showDatePicker"
      type="date"
      title="选择检测日期"
      :max-date="new Date().getTime()"
      @confirm="onDateConfirm"
    />
  </view>
</template>

<style lang="scss" scoped>
.form-input {
  :deep(.wd-input) {
    padding: 0;
    background-color: transparent;
    font-size: 13px;
  }
  :deep(.wd-input__inner) {
    min-height: auto;
    height: 24px;
    line-height: 24px;
  }
  :deep(.wd-input__clear) {
    top: 50%;
    transform: translateY(-50%);
  }
}

.form-input-mini {
  :deep(.wd-input) {
    padding: 0;
    background-color: transparent;
    font-size: 13px;
  }
  :deep(.wd-input__inner) {
    min-height: auto;
    height: 24px;
    line-height: 24px;
    text-align: center;
  }
  :deep(.wd-input__clear) {
    top: 50%;
    transform: translateY(-50%);
  }
}

.form-textarea {
  :deep(.wd-textarea__inner) {
    font-size: 13px;
    min-height: 80px;
  }
}

.submit-btn {
  :deep(.wd-button) {
    border-radius: 999px;
    height: 46px;
    font-size: 16px;
    font-weight: 600;
  }
}
</style>
