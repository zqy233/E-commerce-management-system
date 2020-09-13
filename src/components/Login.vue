<template>
  <div class="login_container">
    <img src="../assets/imgs/1.jpg">

    <div class="login_box">
      <!-- 头像区域 -->
      <div class="avatar_box">
        <img src="../assets/imgs/8.png">
      </div>
      <!-- 登录表单区域 -->
      <el-form ref="loginFormRef" :model="loginForm" :rules="loginFormRules" label-width="0px" class="login_form">
        <!-- 用户名 -->
        <el-form-item prop="username">
          <el-input v-model="loginForm.username" prefix-icon="iconfont icon-user"></el-input>
        </el-form-item>
        <!-- 密码 -->
        <el-form-item prop="password">
          <el-input v-model="loginForm.password" prefix-icon="iconfont icon-3702mima" type="password"></el-input>
        </el-form-item>
        <!-- 按钮 -->
        <el-form-item class="btns">
          <el-button type="primary" @click="login">登录</el-button>
          <el-button type="danger" @click="resetLoginForm">重置</el-button>
        </el-form-item>
      </el-form>
    </div>
  </div>
</template>

<script>
export default {
  data () {
    return {
      // 默认表单值
      loginForm: {
        username: 'admin',
        password: '123456'
      },
      // 表单规则 prop绑定使用
      loginFormRules: {
        username: [
          { required: true, message: '请输入用户名', trigger: 'blur' },
          { min: 3, max: 5, message: '长度应在 3 到 5 个字符', trigger: 'blur' }
        ],
        password: [
          { required: true, message: '请输入密码', trigger: 'blur' },
          { min: 6, max: 12, message: '长度应在 6 到 12 个字符', trigger: 'blur' }
        ]

      }
    }
  },
  methods: {
    resetLoginForm () {
      // 重置回默认表单值
      this.$refs.loginFormRef.resetFields()
    },
    login () {
      // 验证表单合法性，如果不合法直接over
      // 合法后，发送post请求将用户名和密码给接口,接口验证数据返回数据,根据状态码验证登录状态，成功则跳转home组件
      this.$refs.loginFormRef.validate(async valid => {
        if (!valid) return
        const { data: res } = await this.$http.post('login', this.loginForm)
        if (res.meta.status !== 200) return this.$message.error('登录失败')
        this.$message.success('登录成功')
        // token保存在sessionStorage中
        window.sessionStorage.setItem('token', res.data.token)
        this.$router.push('/home')
      })
    }
  }
}
</script>

<!-- less在vue ui里下载依赖 -->
<style lang="less" scoped>
  .login_container {
    background-color: pink;
    height: 100%;
  }

  .login_box {
    width: 450px;
    height: 300px;
    background-color: #fff;
    border-radius: 7px;
    position: absolute;
    left: 50%;
    top: 50%;
    transform: translate(-50%, -50%);
    background: linear-gradient(90deg, #aaffff, #ff007f, #ff557f, #ffaaff);
    background-size: 400%;

    @keyframes color {
      0% {
        background-position: 0%;
      }

      100% {
        background-position: 400%;
      }
    }

    animation: color 15s infinite;

    .avatar_box {
      height: 250px;
      width: 500px;
      border-radius: 50%;
      position: absolute;
      left: 50%;
      transform: translate(-65%, -50%);

      img {
        width: 100%;
        height: 100%;
        clip-path: circle(20% at 68% 34%);

        @keyframes myfirst {

          // 大 右 下
          0% {
            clip-path: circle(1% at 68% 34%);
          }

          25% {
            clip-path: circle(15% at 68% 34%);
          }

          50% {
            clip-path: circle(30% at 68% 34%);
          }

          100% {
            clip-path: circle(100% at 68% 34%);
          }

        }

        animation: myfirst 7s;

        animation-fill-mode: forwards;
      }
    }
  }

  .login_form {
    position: absolute;
    bottom: 0;
    width: 100%;
    padding: 0 10px;
    box-sizing: border-box;
  }

  .btns {
    margin-left: 34%;
    // display: flex;
    // justify-content: flex-end;
  }
</style>
