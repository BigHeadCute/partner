<template>
  <div style="height: 100vh;overflow: hidden; position: relative;background-image: linear-gradient(to top, #fdcbf1 0%, #fdcbf1 1%, #e6dee9 100%);" >
    <div class="form-box">
      <el-form ref="ruleFormRef" :model="form" :rules="rules" status-icon>
        <h2 style="text-align: center;">登录</h2>
        <el-form-item prop="username">
          <el-input v-model="form.username" :prefix-icon="User" placeholder="请输入账号"></el-input>
        </el-form-item>
        <el-form-item prop="password">
          <el-input v-model="form.password" show-password :prefix-icon="Lock" placeholder="请输入密码"
                    autocomplete="new-password"></el-input>
        </el-form-item>
        <div style="margin-bottom: 0.83em">
          <el-button style="width: 100%" type="primary" @click="login">登录</el-button>
        </div>
        <div>
          <el-button type="primary" link @click="router.push('/register')" style="float: right">没有账号？请注册</el-button>
        </div>
      </el-form>
    </div>
  </div>
</template>

<script setup>
import {reactive, ref} from "vue";
import { User, Lock } from '@element-plus/icons-vue'
import request from "../utils/request";
import {ElMessage} from "element-plus";
import router from "../router";

const ruleFormRef = ref()
const form = reactive({})
const rules = reactive({
  username: [
    { required: true, message: '请输入账号', trigger: 'blur'},
  ],
  password: [
    { required: true, message: '请输入密码', trigger: 'blur'},
  ],
})
const login = () => {
  ruleFormRef.value.validate(valid => {
    //当返回true 就可以调用登录接口
    if(valid){
      request.post("/login",form).then(res => {
        if(res.code === '200'){
          ElMessage.success("登陆成功")
          router.push("/")
        }else{
          ElMessage.error(res.msg)
        }
      })
    }
  })
}
</script>

<style scoped>
.form-box {
  width: 400px;
  border-radius: 10px;
  margin: 0 auto;
  box-shadow: 0 0 5px -2px rgba(0, 0, 0, .5);
  background-image: linear-gradient(to top, #cfd9df 0%, #e2ebf0 100%);;
  padding: 50px;
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translateX(-50%) translateY(-50%);
}
</style>