<template lang="html">
  <!-- <head>
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <link rel="stylesheet" href="https://use.fontawesome.com/releases/v5.0.10/css/all.css">
    <link rel="shortcut icon" href="src/assets/favicon.ico" type="image/x-icon">
    <link rel="icon" href="src/assets/favicon.ico" type="image/x-icon">
    <link href="https://fonts.googleapis.com/css?family=Ubuntu" rel="stylesheet">
    <title>To-do app</title>
  </head> -->
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


export default {
  // head tag
  head () {
    return {
        link: [  { rel: "stylesheet", href: "https://use.fontawesome.com/releases/v5.0.10/css/all.css" },
                 { rel: "stylesheet", href: "https://fonts.googleapis.com/css?family=Ubuntu" }
               ],
    }
  },

  // Reactivity(2): TodoList
  data() {
    return {
      todoItems: []
    }
  },
  created() {
    // if (localStorage.length > 0) {
    //   for (var i = 0; i < localStorage.length; i++) {
    //     this.todoItems.push(localStorage.key(i));
    //   }
    // }
    var allCookies = this.getAllCookies();
    if (allCookies.length > 0) {
      for (var i = 0; i < allCookies.length; i++) {
        this.todoItems.push(allCookies[i]);
      }
    }
  },
  methods:{
    // cookie
    setCookie(name,value,days) {
        // var expires = "";
        // if (days) {
        //     var date = new Date();
        //     date.setTime(date.getTime() + (days*24*60*60*1000));
        //     expires = "; expires=" + date.toUTCString();
        // }
        // document.cookie = name + "=" + (value || "")  + expires + "; path=/";

        // 'cookie-universal-nuxt
        this.$cookies.set(name, (value || ""), {
          path: '/',
          maxAge: 60 * 60 * 24 * days
        })
    },
    getCookie(name) {
        // var nameEQ = name + "=";
        // var ca = document.cookie.split(';');
        // for(var i=0;i < ca.length;i++) {
        //     var c = ca[i];
        //     while (c.charAt(0)==' ') c = c.substring(1,c.length);
        //     if (c.indexOf(nameEQ) == 0) return c.substring(nameEQ.length,c.length);
        // }
        // return null;

        // 'cookie-universal-nuxt
        const cookieRes = this.$cookies.get(name);
        if (cookieRes) return cookieRes;
        return null;

    },
    getAllCookies() {
        // var allCookies = [];
        // var ca = document.cookie.split(';');
        // for(var i=0;i < ca.length;i++) {
        //     var c = ca[i];
        //     while (c.charAt(0)==' ') c = c.substring(1,c.length);
        //     if (c.indexOf('csrftoken')=='-1') {
        //       allCookies.unshift(c.substring(0,c.indexOf('=')));
        //     }
        // }
        // return allCookies;

        // 'cookie-universal-nuxt
        var allCookies = [];
        var cookieObjects = this.$cookies.getAll();

        for (var i in cookieObjects) {
          allCookies.unshift(i);
        }
        return allCookies;
    },
    deleteCookie(name) {
      // document.cookie = name+'=; Max-Age=-99999999;';

      // 'cookie-universal-nuxt
      this.$cookies.remove(name);
    },
    deleteAllCookies() {
        // var ca = document.cookie.split(';');
        // for(var i=0;i < ca.length;i++) {
        //     var c = ca[i];
        //     while (c.charAt(0)==' ') c = c.substring(1,c.length);
        //     if (c.indexOf('csrftoken')=='-1') {
        //       var name = c.substring(0,c.indexOf('='))
        //       console.log(name);
        //       document.cookie = name+'=; Max-Age=-99999999;';
        //     }
        // }

        // 'cookie-universal-nuxt
        this.$cookies.removeAll();
    },

    // Reactivity(1): TodoInput - adding new item
    addTodo(value) {
      // This below code is the reason of changing the whole code p.160
      this.todoItems.unshift(value);

      this.setCookie(value, value, 7);
    },
    // Reactivity(4): TodoList(removeTodo)
    removeTodo(item, index) {
      this.deleteCookie(item);

      this.todoItems.splice(index, 1);
    },
    // Reactivity(3): TodoFooter
    clearTodo() {
      // this.todoItems.splice(0, this.todoItems.length);
      this.todoItems = [];

      this.deleteAllCookies();
    },
    editTodo(item, index) {
      var deleteItem = this.todoItems[index];

      this.deleteCookie(deleteItem);
      this.todoItems.splice(index, 1);

      this.setCookie(item, item, 7);
      this.todoItems.push(item);
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
