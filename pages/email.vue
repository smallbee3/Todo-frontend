<template>
  <section class="container">
    <div>
      <h1 class="title">
        Email Login
      </h1>

      <form v-on:submit.prevent="loginSubmitBtn">
        <input type="text" v-model="form.email" placeholder="Email">
        <input type="password" v-model="form.password" placeholder="Password">
        <button type="button" name="button">CANCEL</button>
        <button type="submit" name="button">SUBMIT</button>
        <div class="">
          <span>{{ warningMessage }}</span>
        </div>
      </form>
    </div>
  </section>
</template>

<script>
import axios from 'axios'

const API_URL_ADDRESS = 'https://todo-api.smallbee.me';
// const API_URL_ADDRESS = 'http://localhost:8000';


export default {
  data() {
    return {
      form: {
        email: '',
        password: ''
      },
      warningMessage: ''
    }
  },
  methods: {
    setCookie(name,value,days) {
        this.$cookies.set(name, (value || ""), {
          path: '/',
          maxAge: 60 * 60 * 24 * days
        })
    },
    loginCancelBtn() {
      console.log('backspace');
    },
    loginSubmitBtn(event) {
      // console.log(this.setCookie);
      // console.log(Cookie.methods.setCookie);
      // Cookie.methods.setCookie();
      const email = this.form.email.trim();
      const password = this.form.password.trim();

      // email validation
      if (email === "" ||
          email.indexOf('@') === -1) {
        return this.warningMessage = 'Enter email correctly!';
      }
      // password validation
      else if (password === "" ||
               password.length < 8) {
        return this.warningMessage = 'Enter password correctly!';
      }
      // http request
      else {
        this.warningMessage = '';
      }
      axios({
        url: `${API_URL_ADDRESS}/user/login/`,
        method: 'post',
        data: {
          username: email,
          password: password
        }
      }).then((response) => {
        this.setCookie('token', response.data.token, 7);
        // Vue.setCookie('token', response.data.token, 7);
        window.location.href = "/main";
      }).catch(function(error) {
        console.log(error);
        console.log(error.response);
      })
    }
  }
}
</script>

<style>
.container {
  min-height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  text-align: center;
}

.title {
  font-family: "Quicksand", "Source Sans Pro", -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, "Helvetica Neue", Arial, sans-serif; /* 1 */
  display: block;
  font-weight: 300;
  font-size: 30px;
  color: #35495e;
  letter-spacing: 1px;
}

.subtitle {
  font-weight: 300;
  font-size: 42px;
  color: #526488;
  word-spacing: 5px;
  padding-bottom: 15px;
}

.links {
  padding-top: 15px;
}

/* Custom */
input {
  font-size: 15px;
}
input:focus {
  /* outline: none; */
}



</style>
