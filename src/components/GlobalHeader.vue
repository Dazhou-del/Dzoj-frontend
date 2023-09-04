<template>
  <a-row
    id="globalHead"
    style="margin-bottom: 16px"
    align="center"
    :wrap="false"
  >
    <a-col flex="auto">
      <!--通用不固定-->
      <a-menu
        mode="horizontal"
        :selected-keys="selectedKeys"
        @menu-item-click="menuItemClick"
      >
        <a-menu-item
          key="0"
          :style="{ padding: 0, marginRight: '38px' }"
          disabled
        >
          <!-- 这是一个logo-->
          <div id="title-bar">
            <img class="logo" src="@/assets/logo.jpg" alt="logo" />
            <div class="title">Dazhou OJ</div>
          </div>
        </a-menu-item>
        <!--菜单栏-->
        <a-menu-item v-for="item in menuRoutes" :key="item.path">
          {{ item.name }}
        </a-menu-item>
      </a-menu>
    </a-col>
    <!--    显示最右侧用户是否登录-->
    <a-col flex="100px">
      <a-dropdown trigger="hover">
        <template
          v-if="loginUserInfo.userRole as string !== ACCESS_ENUM.NOT_LOGIN"
        >
          <template v-if="loginUserInfo.userAvatar">
            <a-avatar shape="circle">
              <img
                alt="avatar"
                :src="loginUserInfo.userAvatar"
                width="24px"
                height="24px"
              />
            </a-avatar>
            {{ loginUserInfo.userName }}
          </template>
          <template v-else>
            <a-avatar>
              <IconUser />
            </a-avatar>
            {{ loginUserInfo.userName }}
          </template>
        </template>
        <template v-else>
          <a-avatar :style="{ backgroundColor: '#168CFF' }"> 未登录</a-avatar>
        </template>
        <template #content>
          <template v-if="loginUserInfo.userRole !== ACCESS_ENUM.NOT_LOGIN">
            <a-doption>
              <!-- <template #icon>
                <icon-idcard />
              </template> -->
              <!-- <template #default>
                <a-anchor-link href="/user/info">个人信息</a-anchor-link>
              </template> -->
            </a-doption>
            <a-doption>
              <template #icon>
                <icon-poweroff />
              </template>
              <template #default>
                <a-anchor-link @click="logout">退出登陆</a-anchor-link>
              </template>
            </a-doption>
          </template>
          <template v-else>
            <a-doption>
              <template #icon>
                <icon-user />
              </template>
              <a-anchor-link href="/user/login">用户登录</a-anchor-link>
            </a-doption>
            <a-doption>
              <template #icon>
                <icon-user-add />
              </template>
              <a-anchor-link href="/user/register">用户注册</a-anchor-link>
            </a-doption>
          </template>
        </template>
      </a-dropdown>
    </a-col>
  </a-row>
</template>

<script setup lang="ts">
import { computed, ref } from "vue";
import { useRouter } from "vue-router";
import { routes } from "@/router/routes";
import { useStore } from "vuex";
import checkAccess from "@/access/checkAccess";
import { Notification } from "@arco-design/web-vue";
import { IconUser } from "@arco-design/web-vue/es/icon";
import { LoginUserVO, UserControllerService } from "../../generated";
import ACCESS_ENUM from "@/access/accessEnum";

const router = useRouter();
const store = useStore();
//获取登陆用户信息
const loginUserInfo: LoginUserVO = computed(
  () => store.state.user?.loginUser
) as LoginUserVO;

// 默认选中的菜单项
const selectedKeys = ref(["/"]);
//退出登录
const logout = () => {
  UserControllerService.userLogoutUsingPost();
  window.localStorage.removeItem("my-Token");
  location.reload();
};

// 从路由中过滤出不需要隐藏的菜单项，使用计算属性，当路由发生变化时，自动更新菜单项
const menuRoutes = computed(() => {
  return routes.filter((item) => {
    // true表示不隐藏，false表示隐藏 因为过滤出来的菜单项是需要显示的，所以默认为true
    if (item.meta?.hideInMenu) {
      return false;
    }
    // 根据权限过滤菜单项,使用全局的权限配置，如果有权限，则显示，没有权限，则不显示
    if (
      !checkAccess(store.state.user.loginUser, item?.meta?.access as string)
    ) {
      return false;
    }
    return true;
  });
});
// 展示在菜单的路由数组
const visibleRoutes = computed(() => {
  return routes.filter((item, index) => {
    if (item.meta?.hideInMenu) {
      return false;
    }
    // 根据权限过滤菜单
    if (
      !checkAccess(store.state.user.loginUser, item?.meta?.access as string)
    ) {
      return false;
    }
    return true;
  });
});

// 路由跳转后，更新选中的菜单项
router.afterEach((to, from, failure) => {
  selectedKeys.value = [to.path];
});

// 菜单项点击事件
const menuItemClick = (key: string) => {
  router.push({
    path: key,
  });
};
</script>

<style scoped>
#title-bar {
  display: flex;
  align-items: center;
}

#title-bar .logo {
  height: 48px;
}

.userName {
  font-size: 16px;
  white-space: nowrap;
  font-weight: bold; /*字体加粗*/
}

#title-bar .title {
  margin-left: 10px;
  color: #18191c;
}
</style>
