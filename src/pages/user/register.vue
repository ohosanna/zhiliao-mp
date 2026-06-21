<script setup lang="ts">
definePage({
  name: "register",
  style: {
    navigationBarTitleText: "",
    navigationStyle: "custom",
  },
});

const router = useRouter();
const statusBarHeight = ref(20);

// #ifdef MP-WEIXIN
const sysInfo = uni.getSystemInfoSync();
statusBarHeight.value = sysInfo.statusBarHeight || 20;
// #endif

function goBack() {
  uni.navigateBack();
}

const formData = ref({
  gender: "male", // 'male' | 'female'
  birthday: "",
  height: "",
  weight: "",
  diabetesType: "1", // '1' | '2' | 'other'
});

const showDatePicker = ref(false);
const birthdayDisplay = ref();

function onDateConfirm({ value }: { value: number }) {
  formData.value.birthday = String(value);
  // 将时间戳格式化为日期字符串 yyyy-MM-dd
  const date = new Date(value);
  const year = date.getFullYear();
  const month = String(date.getMonth() + 1).padStart(2, "0");
  const day = String(date.getDate()).padStart(2, "0");
  console.log(year, month, day);
  birthdayDisplay.value = `${year}-${month}-${day}`;
}

function selectGender(gender: "male" | "female") {
  formData.value.gender = gender;
}

function selectDiabetesType(type: "1" | "2" | "other") {
  formData.value.diabetesType = type;
}

function handleSubmit() {
  if (!formData.value.birthday) {
    uni.showToast({ title: "请选择出生日期", icon: "none" });
    return;
  }
  if (!formData.value.height) {
    uni.showToast({ title: "请输入身高", icon: "none" });
    return;
  }
  if (!formData.value.weight) {
    uni.showToast({ title: "请输入体重", icon: "none" });
    return;
  }
  uni.showToast({ title: "注册成功", icon: "success" });
  router.pushTab({
    name: "home",
  });
}
</script>

<template>
  <view class="register-page min-h-screen bg-gradient-to-b from-[#f0f4ff] to-white">
    <!-- 顶部返回 -->
    <view :style="{ paddingTop: `${statusBarHeight + 12}px` }">
      <view class="px-4">
        <view
          class="flex h-9 w-9 items-center justify-center rounded-full bg-white shadow-sm"
          @click="goBack"
        >
          <wd-icon name="arrow-left" size="18px" color="#374151" />
        </view>
      </view>
    </view>

    <!-- 标题区 -->
    <view class="mt-6 px-6 text-center">
      <text class="text-2xl font-bold wot-text-text-main"> 实名信息 </text>
      <text class="mt-2 block text-sm text-gray-500"> 为保障您的健康数据安全，请完成实名认证 </text>
    </view>

    <!-- 步骤指示器 -->
    <view class="mt-6 flex items-center justify-center px-6">
      <view class="flex items-center">
        <!-- 步骤1 已完成 -->
        <view
          class="z-10 flex h-8 w-8 items-center justify-center rounded-full"
          :style="{
            backgroundColor: '#22c55e',
            color: '#fff',
          }"
        >
          <wd-icon name="check" size="14px" color="#fff" />
        </view>
        <view class="h-px w-12 bg-green-500" />
        <!-- 步骤2 进行中 -->
        <view
          class="z-10 flex h-8 w-8 items-center justify-center rounded-full text-sm font-bold"
          :style="{
            backgroundColor: '#3b82f6',
            color: '#fff',
            boxShadow: '0 0 0 4px #dbeafe',
          }"
        >
          2
        </view>
      </view>
    </view>

    <!-- 步骤标题 -->
    <view class="mt-5 px-6 text-center">
      <text class="text-base font-bold wot-text-text-main"> 填写基本信息 </text>
    </view>

    <!-- 表单卡片 -->
    <view class="mx-4 mt-5 rounded-2xl bg-white p-4 shadow-sm">
      <!-- 性别 -->
      <view class="mb-4">
        <view class="mb-2 flex items-center">
          <text class="text-sm font-medium wot-text-text-main"> 性别 </text>
          <text class="ml-0.5 text-sm text-red-500"> * </text>
        </view>
        <view class="flex gap-3">
          <view
            class="flex flex-1 items-center justify-center gap-2 rounded-xl py-3"
            :style="
              formData.gender === 'male'
                ? { backgroundColor: '#eff6ff', border: '1.5px solid #3b82f6' }
                : { backgroundColor: '#f3f4f6', border: '1.5px solid #e5e7eb' }
            "
            @click="selectGender('male')"
          >
            <wd-icon
              name="man"
              size="18px"
              :color="formData.gender === 'male' ? '#3b82f6' : '#9ca3af'"
            />
            <text
              class="text-sm"
              :style="formData.gender === 'male' ? 'color: #3b82f6' : 'color: #6b7280'"
            >
              男
            </text>
          </view>
          <view
            class="flex flex-1 items-center justify-center gap-2 rounded-xl py-3"
            :style="
              formData.gender === 'female'
                ? { backgroundColor: '#eff6ff', border: '1.5px solid #3b82f6' }
                : { backgroundColor: '#f3f4f6', border: '1.5px solid #e5e7eb' }
            "
            @click="selectGender('female')"
          >
            <wd-icon
              name="woman"
              size="18px"
              :color="formData.gender === 'female' ? '#3b82f6' : '#9ca3af'"
            />
            <text
              class="text-sm"
              :style="formData.gender === 'female' ? 'color: #3b82f6' : 'color: #6b7280'"
            >
              女
            </text>
          </view>
        </view>
      </view>

      <!-- 出生日期 -->
      <view class="mb-4">
        <view class="mb-2 flex items-center">
          <text class="text-sm font-medium wot-text-text-main"> 出生日期 </text>
          <text class="ml-0.5 text-sm text-red-500"> * </text>
        </view>
        <wd-form-item custom-class="input-item">
          <view class="value" @tap="showDatePicker = true">{{
            birthdayDisplay || "请选择出生日期"
          }}</view>
          <wd-datetime-picker
            v-model="formData.birthday"
            v-model:visible="showDatePicker"
            type="date"
            @confirm="onDateConfirm"
          />
        </wd-form-item>
      </view>

      <!-- 身高体重 -->
      <view class="mb-4 flex gap-3">
        <view class="flex-1">
          <view class="mb-2 flex items-center">
            <text class="text-sm font-medium wot-text-text-main"> 身高(cm) </text>
            <text class="ml-0.5 text-sm text-red-500"> * </text>
          </view>
          <wd-form-item custom-class="input-item">
            <wd-input
              v-model="formData.height"
              placeholder="如 170"
              type="digit"
              custom-class="form-input"
              clearable
            />
          </wd-form-item>
        </view>
        <view class="flex-1">
          <view class="mb-2 flex items-center">
            <text class="text-sm font-medium wot-text-text-main"> 体重(kg) </text>
            <text class="ml-0.5 text-sm text-red-500"> * </text>
          </view>
          <wd-form-item custom-class="input-item">
            <wd-input
              v-model="formData.weight"
              placeholder="如 65"
              type="digit"
              custom-class="form-input"
              clearable
            />
          </wd-form-item>
        </view>
      </view>

      <!-- 糖尿病类型 -->
      <view>
        <view class="mb-2 flex items-center">
          <text class="text-sm font-medium wot-text-text-main"> 糖尿病类型 </text>
          <text class="ml-0.5 text-sm text-red-500"> * </text>
        </view>
        <view class="flex gap-2.5">
          <view
            class="flex-1 items-center rounded-xl py-2.5 text-center"
            :style="
              formData.diabetesType === '1'
                ? { backgroundColor: '#eff6ff', border: '1.5px solid #3b82f6' }
                : { backgroundColor: '#f3f4f6', border: '1.5px solid #e5e7eb' }
            "
            @click="selectDiabetesType('1')"
          >
            <text
              class="text-sm"
              :style="formData.diabetesType === '1' ? 'color: #3b82f6' : 'color: #6b7280'"
            >
              1型糖尿病
            </text>
          </view>
          <view
            class="flex-1 items-center rounded-xl py-2.5 text-center"
            :style="
              formData.diabetesType === '2'
                ? { backgroundColor: '#eff6ff', border: '1.5px solid #3b82f6' }
                : { backgroundColor: '#f3f4f6', border: '1.5px solid #e5e7eb' }
            "
            @click="selectDiabetesType('2')"
          >
            <text
              class="text-sm"
              :style="formData.diabetesType === '2' ? 'color: #3b82f6' : 'color: #6b7280'"
            >
              2型糖尿病
            </text>
          </view>
          <view
            class="flex-1 items-center rounded-xl py-2.5 text-center"
            :style="
              formData.diabetesType === 'other'
                ? { backgroundColor: '#eff6ff', border: '1.5px solid #3b82f6' }
                : { backgroundColor: '#f3f4f6', border: '1.5px solid #e5e7eb' }
            "
            @click="selectDiabetesType('other')"
          >
            <text
              class="text-sm"
              :style="formData.diabetesType === 'other' ? 'color: #3b82f6' : 'color: #6b7280'"
            >
              其他类型
            </text>
          </view>
        </view>
      </view>
    </view>

    <!-- 信息安全承诺 -->
    <view class="mx-4 mt-3 rounded-xl p-3" style="background-color: #fffbeb">
      <view class="flex items-start gap-2">
        <view
          class="mt-0.5 flex h-5 w-5 shrink-0 items-center justify-center rounded-full"
          style="background-color: #f59e0b"
        >
          <wd-icon name="check" size="12px" color="#fff" />
        </view>
        <view class="flex flex-col">
          <text class="text-sm font-medium" style="color: #d97706"> 信息安全承诺 </text>
          <text class="mt-1 text-xs leading-relaxed text-gray-500">
            您的信息仅用于健康管理服务，我们将严格加密保护，绝不外泄
          </text>
        </view>
      </view>
    </view>

    <!-- 完成按钮 -->
    <view class="px-4 pt-5">
      <wd-button type="primary" block round custom-class="submit-btn" @click="handleSubmit">
        完成
      </wd-button>
    </view>

    <!-- 底部协议提示 -->
    <view class="mt-4 px-6 text-center">
      <text class="text-xs text-gray-400">
        注册即表示同意
        <text class="text-blue-500"> 《用户服务协议》 </text>
        和
        <text class="text-blue-500"> 《隐私政策》 </text>
      </text>
    </view>

    <!-- 底部安全区域 -->
    <wd-gap safe-area-bottom height="30px" />
  </view>
</template>

<style lang="scss">
.register-page {
  .input-item {
    @apply rounded-xl;
    background: rgb(243, 244, 246);
    border: 2rpx solid rgb(229, 231, 235);
  }
  .submit-btn {
    .wd-button {
      border-radius: 999px;
      height: 48px;
      font-size: 16px;
      font-weight: 600;
    }
  }
}
</style>
