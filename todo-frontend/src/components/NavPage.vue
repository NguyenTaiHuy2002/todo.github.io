<template>
  <link
    href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.2/dist/css/bootstrap.min.css"
    rel="stylesheet"
  />

  <div class="nav">
    <router-link to="/" class="nav-logo">TODOS</router-link>
    <div class="nav-button">
    <router-link class="nav-item" to="/">Home</router-link>
    <router-link class="nav-item" v-if="isAuthenticated" to="/to-do">Todo List</router-link>
    <router-link class="nav-item" v-if="!isAuthenticated" to="/register-view">Sign up</router-link>
    <router-link class="nav-item" v-if="!isAuthenticated" to="/login-view">Login</router-link>
    <p class="nav-item nav-auth-name" v-if="isAuthenticated">Hello: {{ user.username }}</p>
    <span class="nav-item" v-if="isAuthenticated" @click="logout">Logout</span>
    </div>
  </div>
</template>

<script>
import { computed } from 'vue';
import { useStore } from 'vuex';
import router from '@/router'
import Swal from 'sweetalert2' // Import SweetAlert2


export default {
  setup() {
    const store = useStore();
    const isAuthenticated = computed(() => store.state.isAuthenticated); // Trạng thái đăng nhập
    const user = computed(() => store.state.user); // Thông tin người dùng

    const logout = () => {
      // Hiển thị cảnh báo xác nhận trước khi đăng xuất
      Swal.fire({
        title: 'Are you sure?',
        text: 'Do you really want to log out?',
        icon: 'question',
        showCancelButton: true,
        confirmButtonText: 'Yes, log out!',
        cancelButtonText: 'Cancel',
      }).then((result) => {
        if (result.isConfirmed) {
          // Nếu người dùng xác nhận, gọi action logout
          store.dispatch('logout')
          Swal.fire(
            'Logged Out!',
            'You have been successfully logged out.',
            'success',
          ).then(() => {
            router.push('/')
          })
        }
      })
    }

    return {
      isAuthenticated,
      user,
      logout,
    };
  },
}
</script>

<style scoped>
.nav {
  justify-content: space-between;
  align-items: center;
  position: fixed;
  top: 0;
  right: 0;
  left: 0;
  z-index: 100;
  background-color: #fff;
  box-shadow: 1px 0px 5px #ccc;
}

.nav-logo,
.nav-button {
  margin: 0 50px;
}

.nav-logo {
    font-size: 30px;
    font-weight: 900;
    font-family: cursive;
    user-select: none;
    cursor: pointer;
    text-decoration: none;
    color: red;
}


.nav-item {
  padding: 16px;
  display: inline-block;
  text-decoration: none;
  margin: 0;
  cursor: pointer;
  color: blue;
}

.nav-item:hover {
    color: #ff0000;
}

.nav-auth-name {
  user-select: none;
  color: #535353;
  cursor: text;
}

.isAuth {
  display: none;
}
</style>