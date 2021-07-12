<template>
<div>
  <h1>重新設定密碼</h1>
  <table class="center">
    <tr>
      <td><label for="account">帳號</label></td>
      <td><input type="text" v-model="account"/></td>
    </tr>
    <tr>
      <td><label for="password">新的密碼</label></td>
      <td><input type="password" v-model="password"/></td>
    </tr>
    <tr>
      <td></td>
      <td><button @click="sendPassword">送出</button></td>
    </tr>
  </table>
</div>
</template>

<script>
export default {
  name: 'ForgetPassword',
  methods: {
    sendPassword() {
      const formdata = new FormData();
      formdata.append('account', this.account);
      formdata.append('password', this.password);
      fetch('http://localhost:7000/test/v1/change_password', {
        method: 'POST',
        mode: 'cors',
        body: formdata,
        credentials: 'same-origin',
      }).then((response) => {
        if (!response.ok) {
          throw new Error('Fetch Error!!');
        }
        return response.json();
      }).then((jsonResponse) => {
        if (jsonResponse.result === 0) {
          alert('change password success');
          this.$emit('change-view', 'login');
        } else {
          alert('change password fail');
        }
      }).catch((error) => {
        console.error(error);
      });
    },
  },
};
</script>

<style>

</style>
