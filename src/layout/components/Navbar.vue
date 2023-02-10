<template>
  <div class="navbar">
    <!-- <hamburger id="hamburger-container" :is-active="appStore.sidebar.opened" class="hamburger-container" @toggleClick="toggleSideBar" />
    <breadcrumb id="breadcrumb-container" class="breadcrumb-container" v-if="!settingsStore.topNav" />
    <top-nav id="topmenu-container" class="topmenu-container" v-if="settingsStore.topNav" /> -->
    <div class="center-menu">
      <img class="titleLogo" src="@/assets/logo/titleLogo.png">
      基础管理平台 <span class="line"> | </span>
      <span class="fs-26 m-l-10"> 基础管理平台 </span>
    </div>
    <div class="right-menu m-r-20">
      <el-dropdown @command="handleCommand" class="avatar-container right-menu-item hover-effect" trigger="click">
          <div class="avatar-wrapper">
            <el-icon><UserFilled /></el-icon>
          欢迎，{{ nickName || '用户' }}
          <el-icon><CaretBottom /></el-icon>
          </div>
          <template #dropdown>
            <el-dropdown-menu>
              <router-link to="/user/profile">
                <el-dropdown-item>个人中心</el-dropdown-item>
              </router-link>
              <el-dropdown-item command="setLayout">
                <span>布局设置</span>
              </el-dropdown-item>
              <el-dropdown-item divided command="logout">
                <span>退出登录</span>
              </el-dropdown-item>
            </el-dropdown-menu>
          </template>
        </el-dropdown>
    </div>
  </div>
</template>

<script setup>
import { ElMessageBox } from 'element-plus'
import Breadcrumb from '@/components/Breadcrumb'
import TopNav from '@/components/TopNav'
import Hamburger from '@/components/Hamburger'
import Screenfull from '@/components/Screenfull'
import SizeSelect from '@/components/SizeSelect'
import HeaderSearch from '@/components/HeaderSearch'
import RuoYiGit from '@/components/RuoYi/Git'
import RuoYiDoc from '@/components/RuoYi/Doc'
import useAppStore from '@/store/modules/app'
import useUserStore from '@/store/modules/user'
import useSettingsStore from '@/store/modules/settings'

const appStore = useAppStore()
const userStore = useUserStore()
const settingsStore = useSettingsStore()

function toggleSideBar() {
  appStore.toggleSideBar()
}

function handleCommand(command) {
  switch (command) {
    case "setLayout":
      setLayout();
      break;
    case "logout":
      logout();
      break;
    default:
      break;
  }
}

function logout() {
  ElMessageBox.confirm('确定注销并退出系统吗？', '提示', {
    confirmButtonText: '确定',
    cancelButtonText: '取消',
    type: 'warning'
  }).then(() => {
    userStore.logOut().then(() => {
      location.href = '/index';
    })
  }).catch(() => { });
}

const emits = defineEmits(['setLayout'])
function setLayout() {
  emits('setLayout');
}
</script>

<style lang='scss' scoped>
.navbar {
  height: 80px;
  overflow: hidden;
  background: #1d63cc;
  box-shadow: 0 1px 4px rgba(0, 21, 41, 0.08);
  color: #fff;
  display: flex;
  justify-content: space-between;
  align-items: center;

  .avatar-wrapper {
    display: flex;
    align-items: center;
    margin-right: 20px;
    i {
      margin: 0 5px;
    }
  }
  .center-menu {
    font-size: 30px;
    font-family: Source Han Sans CN;
    font-weight: 500;
    color: #f6f5ff;
    display: flex;
    align-items: flex-end;
    justify-content: center;
    .line {
      font-weight: 200;
      margin-left: 16px;
    }
    .titleLogo {
      width: 34px;
      margin: -10px 20px 0;
      // height: 34px;
    }
    .fs-30 {
      font-weight: normal;
    }
  }

  .hamburger-container {
    line-height: 46px;
    height: 100%;
    float: left;
    cursor: pointer;
    transition: background 0.3s;
    -webkit-tap-highlight-color: transparent;

    &:hover {
      background: rgba(0, 0, 0, 0.025);
    }
  }

  .breadcrumb-container {
    float: left;
  }

  .topmenu-container {
    position: absolute;
    left: 50px;
  }

  .errLog-container {
    display: inline-block;
    vertical-align: top;
  }

  .right-menu {
    height: 100%;
    line-height: 50px;
    display: flex;
    justify-content: right;
    align-items: center;

    &:focus {
      outline: none;
    }
    .right-menu-item {
      font-size: 16px;
      color: #fff;
      cursor: pointer;
    }
  }
}
</style>
