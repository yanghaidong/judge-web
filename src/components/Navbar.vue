<style scoped>
  .layout{
    border: 1px solid #d7dde4;
    background: #f5f7f9;
    position: relative;
    border-radius: 4px;
    overflow: hidden;
  }
  .layout-logo{
    height: 30px;
    /*background: #5b6270;*/
    border-radius: 3px;
    color:#fff;
    font-size: 20px;
    line-height: 30px;
    float: left;
    position: relative;
    top: 15px;
    left: 20px;
  }
  .layout-nav{
    width: 600px;
    margin: 0 auto;
  }
  .layout-sign{
    position: relative;
    color: white;
    font-size: 17px;
    float: right;
    margin-right: 50px;
  }
  span:hover{
    cursor: pointer;
  }
  .layout-footer-center{
    text-align: center;
  }
  .findPassword{
    color: dodgerblue;
    float: left;
    margin-left: 20px;
  }
  .signUp{
    color: dodgerblue;
    float: right;
    margin-right: 20px;
  }
  .findPassword:hover{
    cursor: pointer;
  }
  .signUp:hover{
    cursor: pointer;
  }
</style>
<template>
  <div class="layout">
    <Layout>
      <Header :style="{position: 'fixed', width: '100%'}">
        <Menu mode="horizontal" theme="dark" active-name="1">
          <div class="layout-logo">
            <span>NanJing University Online Judge</span>
          </div>
          <div class="layout-nav">
            <MenuItem name="1">
              <Icon type="ios-navigate"></Icon>
              首页
            </MenuItem>
            <MenuItem name="2">
              <Icon type="ios-keypad"></Icon>
              练习
            </MenuItem>
            <router-link to="/contests">
              <MenuItem name="3">
                <Icon type="ios-analytics"></Icon>
                竞赛
              </MenuItem>
            </router-link>
            <MenuItem name="4">
              <Icon type="ios-paper"></Icon>
              试炼场
            </MenuItem>
            <MenuItem name="5">
              <Icon type="ios-paper"></Icon>
              状态
            </MenuItem>
            <MenuItem name="6">
              <Icon type="ios-paper"></Icon>
              讨论区
            </MenuItem>
          </div>
          <div class="layout-sign">
            <div v-if="!loginSuccess">
              <span class="" v-on:click="loginIn">Sign&nbsp;in</span>
              <label style="color: #cbced4">or</label>
              <span v-on:click="signUp">Sign&nbsp;up</span>
            </div>
            <div v-else>
              <span style="font-size: smaller;color: oldlace">{{username}} 登录成功</span>
              <span @click="login_out">|&nbsp;login out</span>
            </div>
          </div>
        </Menu>
      </Header>
      <!--<Content :style="{margin: '88px 20px 0', background: '#fff', minHeight: '500px'}">-->
        <!--Content-->
      <!--</Content>-->
      <!--<Footer class="layout-footer-center">2011-2016 &copy; TalkingData</Footer>-->
    </Layout>
    <!--登录modal-->
    <Modal
      v-model="modal1" :footer-hide="true"
      title="Welcome to Online Judge" style="text-align: center">
      <LoginIn @login_status="get_status"/>
      <footer style="margin-bottom: 40px">
        <span class="findPassword">Forgot password?</span>
        <span class="signUp" @click="changeToSingUp">Sign up</span>
      </footer>
    </Modal>
    <Modal v-model="modal2" :footer-hide="true"
          title="Welcome to Online Judge" style="text-align: center">
      <SignUp />
    </Modal>
  </div>

</template>
<script>
import LoginIn from '@/components/login/LoginIn'
import SignUp from '@/components/login/SignUp'
import axios from 'axios'

export default {
  name: 'Navbar',
  components: { LoginIn, SignUp
  },
  data () {
    return {
      msg: 'Welcome to Your Vue.js App',
      modal1: false,
      modal2: false,
      username: window.localStorage.getItem('username'),
      loginSuccess: window.localStorage.getItem('login_status')
    }
  },
  methods: {
    loginIn: function () {
      this.modal1 = true
    },
    signUp: function () {
      this.modal2 = true
    },
    get_status: function (res) {
      this.loginSuccess = res
      this.modal1 = false
      if (res === true) {
        alert('登录成功')
        window.localStorage.setItem('login_status', 'login_in')
        this.username = window.localStorage.getItem('username')
      } else {
        alert('登录失败，请检查用户名和密码！')
      }
    },
    login_out: function (username) {
      username = 'test' // todo
      let that = this
      axios.get('/apis/loginOut.do', username).then(function (res) {
        if (res.data === true) {
          that.loginSuccess = false
          window.localStorage.clear()
        }
      })
    },
    changeToSingUp: function () {
      this.modal1 = false
      this.modal2 = true
    }
  }
}
</script>
