<template>
  <div id="about">
    <a-form
      :model="form"
      :style="{ width: '400px' }"
      @submit="handleSubmit"
      class="ant-form"
    >
      <span style="font-size: 20px">注 册</span>
      <div class="ant-form-item">
        <a-form-item field="userAccount" label="账号">
          <a-input
            class="ant-input"
            v-model="form.userAccount"
            placeholder="请输入账号..."
          />
        </a-form-item>
        <a-form-item field="userPassword" label="密码">
          <a-input
            class="ant-input"
            v-model="form.userPassword"
            type="password"
            placeholder="请输入密码..."
          />
        </a-form-item>
        <a-form-item field="checkPassword" label="密码">
          <a-input
            class="ant-input"
            v-model="form.checkPassword"
            type="password"
            placeholder="请输入8位密码..."
          />
        </a-form-item>
      </div>
      <a-form-item class="ant-btn">
        <a-button type="primary" status="success" @click="handleReturn"
          >返 回
        </a-button>
        <a-button
          html-type="submit"
          type="dashed"
          status="success"
          style="left: 40px"
          >注 册
        </a-button>
      </a-form-item>
    </a-form>
  </div>
</template>

<script setup lang="ts">
import { reactive } from "vue";
import { UserControllerService, UserRegisterRequest } from "../../../generated";
import store from "@/store";
import { useRouter } from "vue-router";
import { Notification } from "@arco-design/web-vue";
import message from "@arco-design/web-vue/es/message";
const router = useRouter();
// 返回上一级
const handleReturn = async (data: any) => {
  router.go(-1);
};

const form = reactive({
  userAccount: "",
  userPassword: "",
  checkPassword: "",
}) as UserRegisterRequest;
const handleSubmit = async (data: any) => {
  // 校验密码是否一致
  if (form.userPassword !== form.checkPassword) {
    message.error("两次输入的密码不一致");
    return;
  }
  if (Number(form.userPassword?.length) != 8) {
    message.error("密码不等于8位");
    return;
  }
  console.log(data);
  // 使用OpenAPI根据接口文档生成的代码，调用后端接口，将表单数据传递给后端
  const res = await UserControllerService.userRegisterUsingPost(form);
  if (res.code === 0) {
    // 注册成功 ,跳转到登录页面
    console.log(res);
    // 获取登录用户信息存到全局状态管理,因为user.ts中获取登录用户的方法使用的async ，所以使用await使异步变同步
    await store.dispatch("user/getLoginUser");
    message.success("注册成功啦");
    // 路由到主页
    router.push({
      path: "/user/login",
      replace: true,
    });
  } else {
    // 登录失败
    console.log(res);
    message.error("账号或密码错误");
  }
};
</script>
<style scoped>
#about {
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
}

#about .ant-form {
  background: rgba(255, 255, 255, 0.5);
  border-radius: 15px;
  box-shadow: 0 0 20px rgba(0, 0, 0, 0.1);
  backdrop-filter: blur(10px);
  background-clip: padding-box;
  padding: 30px;
  margin-top: 30px;
}

#about .ant-form-item {
  margin-top: 30px;
  margin-right: 30px;
}

#about .ant-input {
  border-radius: 15px;
  background: rgba(255, 255, 255, 0.5);
  box-shadow: 0 0 1px rgba(0, 0, 0, 0.5);
  backdrop-filter: blur(10px);
  background-clip: padding-box;
}
</style>
