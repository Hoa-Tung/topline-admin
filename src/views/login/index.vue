<template>
    <div class="login-wrap">
        <el-form class="login-from" ref="form" :model="form">
            <el-form-item>
                <el-input v-model="form.mobile" placeholder="请输入手机号"></el-input>
            </el-form-item>
            <el-form-item>
                <el-input v-model="form.code" placeholder="请输入验证码"></el-input>
            </el-form-item>
            <el-form-item>
                <el-button type="primary" @click="onSubmit">登录</el-button>
            </el-form-item>
        </el-form>
    </div>
</template>

<script type="text/ecmascript-6">
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
    onSubmit() {
      console.log('submit!')
    }
  }
}
</script>

<style scoped lang="less">
    .login-wrap{
        height: 100%;
        background-color: #2b3e4a;
        display: flex;
        justify-content: center;
        align-items: center;

        .login-from {

        }
    }
</style>
