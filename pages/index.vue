<template lang="html">
  <div id="app">
    <TodoHeader></TodoHeader>
    <TodoInput v-on:add-todo="addTodo"></TodoInput>
    <TodoList v-bind:propsdata="todoItems" v-on:remove-todo="removeTodo" v-on:edit-todo="editTodo"></TodoList>
    <TodoFooter v-on:clear-todo="clearTodo"></TodoFooter>
  </div>
</template>

<script>
import TodoHeader from '~/components/TodoHeader.vue'
import TodoInput from '~/components/TodoInput.vue'
import TodoList from '~/components/TodoList.vue'
import TodoFooter from '~/components/TodoFooter.vue'
import axios from 'axios'


export default {
  // Reactivity(2): TodoList
  data() {
    return {
      // 181223
      // todoItems: {}
      // -> Vue's Reactivity not support 'object' data type

      todoItems: []
    }
  },
  created() {
    // var allCookies = this.getAllCookies();
    // if (allCookies.length > 0) {
    //   for (var i = 0; i < allCookies.length; i++) {
    //     // console.log('Before: ', this.todoItems);
    //     this.todoItems.push(allCookies[i]);
    //     // console.log('After: ', this.todoItems);
    //   }
    // }

    axios({
      url: 'http://trello-eb.ap-northeast-2.elasticbeanstalk.com/card/',
      method: 'get'
    }).then((response) => {
      for (var i of response.data.results) {
        this.todoItems.push(i);
      }
    }).catch((error) => {
      console.log(error);
      console.log(error.response);
    });
    // console.log('todoItems: ', this.todoItems);
  },
  methods:{
    // cookie
    setCookie(name,value,days) {
        // 'cookie-universal-nuxt
        this.$cookies.set(name, (value || ""), {
          path: '/',
          maxAge: 60 * 60 * 24 * days
        })
    },
    getCookie(name) {
        // 'cookie-universal-nuxt
        const cookieRes = this.$cookies.get(name);
        if (cookieRes) return cookieRes;
        return null;


    },
    getAllCookies() {
        // 'cookie-universal-nuxt
        var allCookies = [];
        var cookieObjects = this.$cookies.getAll();

        for (var i in cookieObjects) {
          allCookies.unshift(i);
        }
        return allCookies;
    },
    deleteCookie(name) {
      // 'cookie-universal-nuxt
      this.$cookies.remove(name);
    },
    deleteAllCookies() {
      // 'cookie-universal-nuxt
      this.$cookies.removeAll();
    },


    // Reactivity(1): TodoInput - adding new item
    addTodo(value) {
      // This below code is the reason of changing the whole code p.160
      // this.todoItems.push(value);

      // this.setCookie(value, value, 7);
      axios({
        url: 'http://trello-eb.ap-northeast-2.elasticbeanstalk.com/card/',
        method: 'post',
        data: {
          contents: value
        }
      }).then((response) => {
        this.todoItems.unshift(response.data);
      }).catch((error) => {
        console.log(error);
        console.log(error.response);
      });
    },
    // Reactivity(4): TodoList(removeTodo)
    removeTodo(item, index) {
      // this.deleteCookie(item);
      axios({
        url: 'http://trello-eb.ap-northeast-2.elasticbeanstalk.com/card/' + item.pk + '/',
        method: 'delete'
      }).then((response) => {
        this.todoItems.splice(index, 1);
      }).catch((error) => {
        console.log(error);
        console.log(error.response);
      });
    },
    // Reactivity(3): TodoFooter
    clearTodo() {
      // this.todoItems.splice(0, this.todoItems.length);

      // this.deleteAllCookies();
      for (var index in this.todoItems) {
        axios({
          url: 'http://trello-eb.ap-northeast-2.elasticbeanstalk.com/card/' + this.todoItems[index].pk + '/',
          method: 'delete'
        }).then((response) => {
          this.todoItems.splice(index, 1);
        }).catch((error) => {
          console.log(error);
          console.log(error.response);
        });
      }

      this.todoItems = [];

    },
    editTodo(value, index) {
      // var deleteItem = this.todoItems[index];
      //
      // this.deleteCookie(deleteItem);
      // this.todoItems.splice(index, 1);
      //
      // this.setCookie(item, item, 7);
      // this.todoItems.push(item);

      axios({
        url: 'http://trello-eb.ap-northeast-2.elasticbeanstalk.com/card/' + this.todoItems[index].pk + '/',
        method: 'put',
        data: {
          contents: value
        }
      }).then((response) => {
        this.todoItems[index].contents = value;
      }).catch((error) => {
        console.log(error);
        console.log(error.response);
      });
    }
  },
  components: {
    'TodoHeader': TodoHeader,
    'TodoInput': TodoInput,
    'TodoList': TodoList,
    'TodoFooter': TodoFooter
  }
}
</script>

<style lang="css">
  body {
    text-align: center;
    background-color: #F6F6F8;
  }
  input {
    border-style: groove;
    width: 200px;
  }
  button {
    border-style: groove;
  }
  .shadow {
    box-shadow: 5px 10px 10px rgba(0, 0, 0, 0.03)
  }
</style>


<!-- <template>
  <section class="container">
    <div>
      <app-logo/>
      <h1 class="title">
        trello
      </h1>
      <h2 class="subtitle">
        Nuxt.js project
      </h2>
      <div class="links">
        <a
          href="https://nuxtjs.org/"
          target="_blank"
          class="button--green">Documentation</a>
        <a
          href="https://github.com/nuxt/nuxt.js"
          target="_blank"
          class="button--grey">GitHub</a>
      </div>
    </div>
  </section>
</template>

<script>
import AppLogo from '~/components/AppLogo.vue'

export default {
  components: {
    AppLogo
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
  font-size: 100px;
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
</style>
 -->
