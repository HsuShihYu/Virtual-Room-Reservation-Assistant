<template>
<div>
  <h1>註冊</h1>
  <table class="center">
    <tr>
      <td><label for="name">姓名</label></td>
      <td><input id="name" type="text" v-model="name"/></td>
    </tr>
    <tr>
      <td><label for="email">e-mail</label></td>
      <td><input id="email" type="email" v-model="email"/></td>
    </tr>
    <tr>
      <td><label for="account">帳號</label></td>
      <td><input id="account" type="text" v-model="account"/></td>
    </tr>
    <tr>
      <td><label for="password">密碼</label></td>
      <td><input id="password" type="password" v-model="password"/></td>
    </tr>
    <tr>
      <td><label for="password2">確認密碼</label></td>
      <td>
        <input
          id="password2"
          type="password"
          placeholder="再輸入一次密碼"
          v-model="password2"
        />
      </td>
    </tr>
    <tr>
      <td></td>
      <td><button type="button" @click="signup">註冊</button></td>
    </tr>
  </table>
</div>
</template>

<script>
export default {
  name: 'Signup',
  methods: {
    signup() {
      if (this.password === this.password2) {
        // collect the data from input
        const formdata = new FormData();
        formdata.append('name', this.name);
        formdata.append('account', this.account);
        formdata.append('mail', this.email);
        formdata.append('password', this.password);
        // send the request
        fetch('http://localhost:7000/test/v1/user_create', {
          method: 'POST',
          body: formdata,
          mode: 'cors',
          credentials: 'same-origin',
        }).then((response) => {
          if (!response.ok) {
            throw new Error('fetch error!!');
          }
          return response.json();
        }).then((jsonResponse) => {
          if (jsonResponse.result === 0) {
            alert('signup success');
            this.$emit('change-view', 'login');
          } else {
            alert(jsonResponse.result);
          }
        }).catch((error) => {
          console.error(error);
        });
      } else {
        alert('Check Your Password again!!');
      }
    },
  },
};
</script>

<style>

</style>
