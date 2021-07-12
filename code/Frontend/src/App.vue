<template>
  <span>
    <button @click="home" class="topbutton">
      <img src="./assets/home.png" width=90 height=30>
    </button>
  </span>
  <component
    :is="present"
    :initialdate="today"
    @change-view="present = $event"
    @login-success = "loginSuccess"
  >
  </component>
</template>

<script>
import ForgetPassword from './components/ForgetPassword.vue';
import Login from './components/Login.vue';
import Main from './components/Main.vue';
import Signup from './components/Signup.vue';

export default {
  name: 'App',
  components: {
    ForgetPassword,
    Login,
    Signup,
    Main,
  },
  data() {
    return {
      present: 'Login',
      // for test, this parameter should set to false in production
      loggedIn: false,
      today: new Date(),
    };
  },
  methods: {
    home() {
      if (this.loggedIn === false) {
        this.present = 'Login';
      } else {
        this.present = 'Main';
      }
    },
    loginSuccess() {
      // login operation success
      this.loggedIn = true;
      this.present = 'Main';
    },
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 30px;
}
.calendar{
  display: block;
  margin-left: auto;
  margin-right: auto;
  width: 100%;
  border: 2px solid rgb(0, 0, 0);
  border-radius: 8px;
  border-collapse: collapse;
}
td{
  border-bottom: 1px solid black;
}
.dateRow{
  overflow: hidden;
}
.topbutton{
  background-color: white;
  border: 1px solid black;
  margin: 3px;
}
.bottombutton{
  background-color: white;
  border: 0px;
  margin: 3px;
}
.buttonimage{
  width: 30px;
  height: 30px;
}
.center{
  display: block;
  margin-left: auto;
  margin-right: auto;
  width: 70%;
}
.labelright{
  text-align: right;
  width: 50%;
}
.inputleft{
  text-align: left;
  width: 50%;
}
</style>
