<template>
  <section class="container">
    <div>
      <h1 class="title">
        Sign up
      </h1>

      <form v-on:submit.prevent="loginSubmitBtn">
        <input type="text" v-model="form.email" placeholder="Email">
        <input type="password" v-model="form.password1" placeholder="Password">
        <input type="password" v-model="form.password2" placeholder="Password">
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
// const API_URL_ADDRESS = 'https://trello-api.smallbee.me';
const API_URL_ADDRESS = 'http://localhost:8000';

export default {
  data() {
    return {
      form: {
        email: '',
        password1: '',
        password2: ''
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
    loginSubmitBtn() {
      const email = this.form.email.trim();
      const password1 = this.form.password1.trim();
      const password2 = this.form.password2.trim();

      // email validation
      if (email === "" ||
          email.indexOf('@') === -1) {
        return this.warningMessage = 'Enter email correctly!';
      }
      // password validation
      else if (password1 === "" ||
               password1 !== password2 ||
               password1.length < 8) {
        return this.warningMessage = 'Enter password correctly!';
      }
       // Success
      else {
        console.log(email, password1, password2);
        this.warningMessage = '';
      }
      console.log('Form submitted!')
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
