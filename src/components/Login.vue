<template>
  <div class="login_container">
    <div class="login_box">
      <div class="avatar_box">
        <img src="../assets/logo.png" alt="" />
      </div>
      <!-- 登录表单 -->
      <el-form ref="loginFormRef" :rules="loginFormRules" :model="loginForm" label-width="0px" class="login_form">
        <!-- 账号 -->
        <el-form-item prop="username">
          <el-input placeholder="请输入账号" v-model="loginForm.username" prefix-icon="el-icon-user"></el-input>
        </el-form-item>
        <!-- 密码 -->
        <el-form-item prop="password">
          <el-input placeholder="请输入密码" v-model="loginForm.password" prefix-icon="el-icon-lock" type="password">
          </el-input>
        </el-form-item>
        <el-form-item>
          <el-radio-group v-model="loginForm.resource">
            <el-radio label="学生登录"></el-radio>
            <el-radio label="管理员登录"></el-radio>
          </el-radio-group>
        </el-form-item>
        <!-- 按钮 -->
        <el-form-item class="btns">
          <el-button type="primary" @click="login">登录</el-button>
          <el-button type="info" @click="resetLoginForm">重置</el-button>
        </el-form-item>
      </el-form>
    </div>
  </div>
</template>
<script>
export default {
  data() {
    return {
      loginForm: {
        username: "",
        password: "",
        resource: "",
        flag: 0,
      },
      loginFormRules: {
        username: [{ required: true, message: "请输入账号", trigger: "blur" }],
        password: [{ required: true, message: "请输入密码", trigger: "blur" }],
      },
    };
  },
  methods: {
    // 重置表单
    resetLoginForm() {
      //   console.log(this);
      this.$refs.loginFormRef.resetFields();
    },
    login() {
      //   this.$refs.loginFormRef.validate(async (valid) => {
      //     if (!valid) return;
      //     const { data: res } = await this.$http.post("login", this.loginForm);
      //     if (res.meta.status !== 200) return this.$message.error("登录失败");
      //     this.$message.success('登陆成功');

      console.log(this.loginForm.resource)
      if (this.loginForm.resource == "学生登录") {
        this.loginForm.flag = 1
        this.$http.post("/user/login", {
          params: {
            userId: this.loginForm.username,
            password: this.loginForm.password,
            usertype: this.loginForm.flag
          },
        }).then((result) => {
          console.log(result);
          if(result.data.status == 200){
          window.sessionStorage.setItem("cookies", result.data.data),
          this.$router.push("/home");}
        })
      }
      else {
        console.log(this.loginForm.username)
        console.log(this.loginForm.password)
        console.log(this.loginForm.flag)
       this.$http.post("/user/login", {
          params: {
            userId: this.loginForm.username,
            password: this.loginForm.password,
            usertype: this.loginForm.flag
          },
        }).then((result) => {
          console.log(result);
          console.log(result.data.status)
          if(result.data.status == 200){
          window.sessionStorage.setItem("cookies", result.data.data),
          this.$router.push("/manage");}
        })
      }


      //   });
    },
  },
};
</script>
<style lang="less" scoped>
.login_container {
  background-color: #2b4b6b;
  height: 100%;
}

.login_box {
  width: 450px;
  height: 300px;
  background-color: #fff;
  border-radius: 3px;
  position: absolute;
  left: 50%;
  top: 50%;
  transform: translate(-50%, -50%);
}

.avatar_box {
  height: 100px;
  width: 100px;
  border: 1px solid #eee;
  border-radius: 50%;
  padding: 10px;
  box-shadow: 0 0 10px #eee;
  position: absolute;
  left: 50%;
  transform: translate(-50%, -70%);
  background-color: #fff;

  img {
    width: 100%;
    height: 100%;
    border-radius: 50%;
    background-color: #eee;
  }
}

.btns {
  display: flex;
  justify-content: right;
}

.login_form {
  position: absolute;
  bottom: 0;
  width: 100%;
  padding: 0 20px;
  box-sizing: border-box;
}
</style>