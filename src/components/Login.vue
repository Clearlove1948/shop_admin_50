
<template>
  <div class="login">
    <el-form :model="form" class="myForm" label-width="80px" :rules="rules" ref="form" status-icon>
      <el-form-item label="用户名" prop="username">
        <el-input placeholder="请输入用户名" v-model="form.username"></el-input>
      </el-form-item>
      <el-form-item label="密码" prop="password">
        <el-input placeholder="请输入密码" v-model="form.password" show-password></el-input>
      </el-form-item>
      <el-form-item>
        <el-button type="success" class="logBtn" @click="fn2">登录</el-button>
        <el-button type="danger" class="logReset" @click="fn">重置</el-button>
      </el-form-item>
      <img src="../imgs/QQ图片20190921235230.jpg" alt="" class="headImg">
    </el-form>

  </div>
</template>

<script>
import axios from 'axios'
export default {

  data () {
    return {
      form: {
        username: '',
        password: '',
        input: ''
      },
      rules: {
        username: [
          // required ---是否加必填项星号 message:判断是否输入用户名 trigger ---检验方式
          // change 输入事件 blur 焦点事件
          { required: true, message: '请输入用户名', trigger: ['change', 'blur'] },
          { min: 3, max: 12, message: '用户名必须是3-12位', trigger: ['change', 'blur'] }
        ],
        password: [
          { required: true, message: '请输入密码', trigger: ['change', 'blur'] },
          { min: 3, max: 12, message: '用户名必须是3-12位', trigger: ['change', 'blur'] }
        ]
      }
    }
  },
  methods: {
    onSubmit () {
      console.log('submit!')
    },
    fn () {
      this.$refs.form.resetFields()
    },
    fn2 () {
      this.$refs.form.validate((flag) => {
        if (!flag) return
        console.log('发送ajax')
        // eslint-disable-next-line no-undef
        // axios({
        //   data: this.form,
        //   url: 'http://localhost:8888/api/private/v1/login',
        //   method: 'post'
        // }).then((res) => {
        //   // eslint-disable-next-line no-unused-vars
        //   const { meta } = res.data
        //   if (meta.status === 200) {
        //     console.log('登录成功')
        //   } else {
        //     console.log(meta.msg)
        //   }
        // })
        // 优化后
        axios.post('http://localhost:8888/api/private/v1/login', this.form).then((res) => {
          // eslint-disable-next-line no-unused-vars
          const { meta, data } = res.data
          if (meta.status === 200) {
            // 登录时存储token值
            localStorage.setItem('token', data.token)
            this.$message({
              // 设置提示语 颜色 持续时间 (提示框的设置)
              message: meta.msg,
              type: 'success',
              duration: 1000
            })
            // 跳转到首页 传个对象的意思是 找到name值为index的那一条规则
            this.$router.push({ name: 'index' })
          } else {
            console.log(meta.msg)
            this.$message.error({
              message: meta.msg,
              duration: 1000
            })
          }
        })
      })
    }
  }
}
</script>

<style lang="scss">
  .login{
    width:100%;
    height:100%;
    background-color: #2d434c;
    overflow: hidden;
    .el-form{
    width:400px;
    // height: 200px;
    background-color: #fff;
    border-radius: 20px;
    padding: 20px;
    padding-top: 70px;
    margin: 0 auto;
    margin-top: 200px ;
    position: relative;
    .headImg{
      width:120px;
      height:120px;
      position: absolute;
      top:-25%;
      left:50%;
      margin-left: -60px;
      border-radius: 50%;
      border:5px solid #fff;
    }
    .logReset{
      margin-left: 100px;
    }
  }
  }

</style>
