<template>
  <div>
    用户名:<input type="text" v-model="loginForm.username" placeholder="请输入用户名"/>
    <br><br>
    密码： <input type="password" v-model="loginForm.password" placeholder="请输入密码"/>
    <br><br>
    <button v-on:click="login">登录</button>
  </div>
</template>

<script>

import axios from 'axios'

export default {
  name: 'Login',
  data () {
    return {
      loginForm: {
        username: '',
        password: ''
      },
      responseResult: []
    }
  },
  methods: {
    login () {
      axios.post('http://localhost:8443/api/login', {
        username: this.loginForm.username,
        password: this.loginForm.password
      })
        .then(successResponse => {
          if (successResponse.data.code === 200) {
            this.$router.replace({path: '/index'})
          } else {
            this.$router.replace({path: '/shibai'})
          }
        })
        .catch(failResponse => {
        })
    }
  }
}
</script>
