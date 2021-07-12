<template>
<div>
  <h1>登入</h1>
  <table class="center">
    <tr>
      <td><label for="account">帳號</label></td>
      <td><input id="account" type="text" v-model="account"></td>
    </tr>
    <tr>
      <td><label for="password">密碼</label></td>
      <td><input id="password" type="password" v-model="password"></td>
    </tr>
    <tr>
      <td></td>
      <td>
        <button @click="$emit('change-view','Signup')">註冊</button>
        <button @click="$emit('change-view','ForgetPassword')">忘記密碼</button>
        <button @click="login">登入</button>
      </td>
    </tr>
  </table>
</div>
</template>

<script>
export default {
  name: 'Login',
  methods: {
    login() {
      const formdata = new FormData();
      formdata.append('account', this.account);
      formdata.append('password', this.password);
      fetch('http://localhost:7000/test/v1/user_login', {
        method: 'POST',
        body: formdata,
        mode: 'cors',
        credentials: 'same-origin',
      }).then((response) => {
        if (!response.ok) {
          throw new Error('Fetch Error!!');
        }
        return response.json();
      }).then((jsonResponse) => {
        // clone the response for second use
        if (jsonResponse.result === 0) {
          this.$emit('login-success');
        } else {
          alert('Password or Account is wrong!!');
        }
      }).catch((error) => {
        console.error(error);
      });
    },
  },
};
</script>
