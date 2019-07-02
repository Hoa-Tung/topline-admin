<template>
  <div class="login-wrap">
    <div class="from-wrap">
      <div class="from-head">
        <img src="./logo_index.png" alt="黑马头条号">
      </div>
      <el-form class="login-from" ref="form" :model="form">
        <el-form-item>
          <el-input v-model="form.mobile" placeholder="请输入您的手机号"></el-input>
        </el-form-item>
        <el-form-item>
          <el-col :span="14">
            <el-input v-model="form.code" placeholder="请输入验证码"></el-input>
          </el-col>
          <el-col :offset="1" :span="9">
            <el-button @click="handleSendCode">获取验证码</el-button>
          </el-col>
        </el-form-item>
        <el-form-item>
          <el-button class="btn-login" type="primary" @click="onSubmit">登录</el-button>
        </el-form-item>
      </el-form>
    </div>
  </div>
</template>

<script>
import axios from 'axios'
// 引入极验
import '@/vendor/gt'
export default {
  name: 'AppLogin',
  data() {
    return {
      form: {
        mobile: '',
        code: ''
      }
    }
  },
  methods: {
    onSubmit () {
      console.log('submit!')
    },
    handleSendCode () {
      const { mobile } = this.form
      axios({
        method: 'GET',
        url: `http://ttapi.research.itcast.cn/mp/v1_0/captchas/${mobile}`

      }).then(res => {
        // console.log(res.data)
        // const data = res.data.data
        const { data } = res.data
        window.initGeetest({
          // 以下配置参数来自服务端 SDK
          gt: data.gt,
          challenge: data.challenge,
          offline: !data.success,
          new_captcha: true,
          product: 'bind' // 隐藏式 直接弹出
        }, function (captchaObj) {
          captchaObj.onReady(function() {
            // 验证码ready之后才能调用verify方法显示验证码
            captchaObj.verify() // 弹出验证码内容框
          }).onSuccess(function() {
            // console.log(captchaObj.getValidate())
            const {
              geetest_challenge: challenge,
              geetest_validate: validate,
              geetest_seccode: seccode
            } = captchaObj.getValidate()
            axios({
              method: 'GET',
              url: `http://ttapi.research.itcast.cn/mp/v1_0/sms/codes/${mobile}`,
              params: {
                challenge,
                validate,
                seccode
              }
            }).then(res => {
              console.log(res.data)
            })
          }).onError(function() {
            // your code
          })

          // 在这里注册“发送验证码”按钮点击事件，验证用户是否输入手机号以及手机号格式是否正确
        })
      })
    }
  }
}
</script>

<style scoped lang="less">
.login-wrap {
  height: 100%;
  background-color: #203542;
  display: flex;
  justify-content: center;
  align-items: center;
  .from-wrap {
    width: 400px;
    background-color: #fff;
    padding: 20px;
    border-radius: 10px;
    .btn-login {
      width: 100%;
    }
    .from-head {
      display: flex;
      justify-content: center;
      align-items: center;
      margin-bottom: 10px;
      img {
        width: 200px;
      }
    }
  }
}
</style>
