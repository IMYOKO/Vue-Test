<template>
  <div id="login">
    <div class="login-box">
      <img src="../Login/logo@2x.png" width="72" height="72"/>
    </div>
    <div class="login-form-wrapper">
      <ul class="login-form-list">
        <li class="input-row">
          <input type="text" v-model="user" placeholder="用户名/手机号" @keyup="isPhoneNumFn()" />
        </li>
        <li class="input-row">
          {{email}}
          <input type="text" :value='email' placeholder="邮箱号码" @input='updataEmail' />
        </li>
        <li class="input-row">
          {{numbers}}
          <input type="text" :value='number' placeholder="输入数量" @input='updataNumber' />
        </li>
        <li class="input-row">
          <input type="password" v-model="passWord" placeholder="密码" />
        </li>
      </ul>
      {{loginStatus}}
      <div class="sumbit-box">
        <input type="button" class="button login-button" @click="getLogin" value="登录" />
        <input type="button" class="button registed-button mt15" @click="registed()" value="注册" />
      </div>
    </div>
  </div>
</template>

<script>
import store from "@/vuex/store"
import { mapState, mapGetters, mapMutations, mapActions } from 'vuex'
export default {
  data() {
    return {
      isLogin: null,
      user: '',
      passWord: ''
    }
  },
  mounted () {
    this.GET_LOGIN_STATE();
  },
  computed: {
    ...mapState(['loginStatus','email', 'number']),
    ...mapGetters(['numbers'])
  },
  methods: {
    ...mapMutations([
      'GET_LOGIN_STATE'
    ]),
    ...mapActions([
      'getLogin'
    ]),
    updataEmail(e) {
      this.$store.commit('updataEmail', e.target.value)
    },
    updataNumber(e) {
      this.$store.commit('updataNumber', e.target.value)
    },
    login() {
      if(this.user == ''){
        console.log('请输入手机号码');
        return false;
      }
      if(!this.isPhoneNum(this.user)){
        console.log('手机号码不正确');
        return false;
      }
      this.$store.state.isLogin = true;
      sessionStorage.login = true;
      this.$router.push({ name:'Home' })
      console.log('登录')
    },
    isPhoneNumFn() {
      if(!this.isPhoneNum(this.user)){
        console.log('手机号码不正确');
        return false;
      }else{
        console.log('正确');
      }
    },
    // 判断手机号码
    isPhoneNum(num){
      return /^0?1[3|4|5|7|8][0-9]\d{8}$/.test(num);
    },

    // 注册跳转
    registed() {
      this.$router.push({ name:'registed' })
    }
  },
  store
}
</script>

<style lang="scss" scoped>
/* 清除点击虚拟框 */
a, div, p, span, ul, li, i, img, input, select {
	outline: 0;
	text-decoration: none;
	-webkit-tap-highlight-color: transparent;
}
.login-box {
  text-align: center;
  padding: 40px 0 30px;
}
.login-form-wrapper {
  padding: 0 15px;
}
.login-form-list {
  .input-row {
    position: relative;

    input {
      width: 100%;
      font-size: 15px;
      padding: 15px 0;
      border-radius: 0;
      border-bottom: 1px solid #e5e5e5;
    }
  }
}
.sumbit-box {
  padding-top: 15px;
}
.button {
  width: 100%;
  display: block;
  text-align: center;
  font-size: 15px;
  padding: 15px 0;
  border-radius: 5px;
  color: #fff;
  
  &.login-button {
    background: #e22c3a;
  }
  &.registed-button {
    color: #e22c3a;
    background: none;
    padding: 14px 0;
    border: 1px solid #e22c3a;
  }
}
.mt15 {
  margin-top: 15px !important;
}
</style>