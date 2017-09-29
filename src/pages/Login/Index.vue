<template>
  <light-layout>
    <h1>{{ $t('loginPage.title') }}</h1>
    <el-row type="flex" justify="center">
      <el-col :span="5">
        <el-card class="box-card">
          <el-form :model="loginForm">
            <el-form-item label="Email" prop="email">
              <el-input type="email" v-model="loginForm.email" auto-complete="on"></el-input>
            </el-form-item>
            <el-form-item label="Password" prop="password">
              <el-input type="password" v-model="loginForm.password" auto-complete="off"></el-input>
            </el-form-item>
            <el-form-item>
              <el-button type="primary" @click="onLogin">{{ $t('loginPage.submit') }}</el-button>
            </el-form-item>
          </el-form>
        </el-card>
      </el-col>
    </el-row>
  </light-layout>
</template>

<script>
import LightLayout from '@/layouts/Light';

export default {
  name: 'login-page',
  components: { LightLayout },
  data() {
    return {
      loginForm: {
        email: null,
        password: null,
      },
    };
  },
  methods: {
    onLogin() {
      const email = this.loginForm.email;
      const password = this.loginForm.password;

      if (!email || !password) {
        return;
      }
      this.$firebase.auth().signInWithEmailAndPassword(email,
      password).then((user) => {
        this.$message({
          message: 'You are logged in',
          type: 'success',
        });
        this.$store.dispatch('auth/login', user);
        this.$router.push({ name: 'Private' });
      }).catch((err) => {
        if (err) {
          this.$message({
            message: err.message,
            type: 'warning',
          });
        }
      });
    },
  },
};
</script>

<style scoped>
</style>