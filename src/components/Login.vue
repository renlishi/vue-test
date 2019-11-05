<style>
  .login-container {
    border-radius: 15px;  /**设置圆角边框*/
    background-clip: padding-box;
    margin: 90px auto;/**与页面最上方距离*/
    width: 350px;/**长度*/
    padding: 35px 35px 15px 35px;/**与内边框四面距离*/
    background: #fff;/**内边框 背景颜色*/
    border: 1px solid #eaeaea;/**外边框线的粗细与线颜色*/
    box-shadow: 0 0 25px #cac6c6; /**外边框环绕阴影颜色*/
  }

  .login_title {
    margin: 0px auto 40px auto;
    text-align: center;
    color: #505458;
  }

  #poster {
    background:url("../assets/login.jpg") no-repeat;
    background-position: center;
    height: 100%;
    width: 100%;
    background-size: cover;
    position: fixed;
  }
  body{
    margin: 0px;
  }


</style>


<template>
  <body id="poster">
    <el-form class="login-container" label-position="left"
             label-width="0px">
      <h3 class="login_title">系统登录</h3>
      <el-form-item>
        <el-input type="text" v-model="loginForm.username"
                  auto-complete="off" placeholder="账号"></el-input>
      </el-form-item>
      <el-form-item>
        <el-input type="password" v-model="loginForm.password"
                  auto-complete="off" placeholder="密码"></el-input>
      </el-form-item>
      <el-form-item style="width: 100%">
        <el-button type="primary" style="width: 100%;background: #505458;border: none" v-on:click="login">登录</el-button>
      </el-form-item>
    </el-form>
  </body>
</template>



<script>

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
        var _this = this
        console.log(this.$store.state)
        this.$axios
          .post('/login/login', {
            signInName: this.loginForm.username,
            passWord: this.loginForm.password
          })
          .then(successResponse => {
            if (successResponse.data.code === 200) {
              // var data = this.loginForm
              _this.$store.commit('login', _this.loginForm)
              var path = this.$route.query.redirect
              this.$router.replace({path: path === '/' || path === undefined ? '/index' : path})
            }
          })
          .catch(failResponse => {
          })
      }

    }
  }
</script>
