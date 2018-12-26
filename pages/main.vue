<template lang="html">
  <div id="app">
    <TodoHeader></TodoHeader>
    <TodoInput v-on:add-todo="addTodo"></TodoInput>
    <TodoList v-bind:propsdata="todoItems" v-on:remove-todo="removeTodo" v-on:edit-todo="editTodo"></TodoList>
    <TodoFooter v-on:clear-todo="clearTodo"></TodoFooter>
  </div>
</template>

<script>
import TodoHeader from '~/components/todo/TodoHeader.vue'
import TodoInput from '~/components/todo/TodoInput.vue'
import TodoList from '~/components/todo/TodoList.vue'
import TodoFooter from '~/components/todo/TodoFooter.vue'
import axios from 'axios'

const API_URL_ADDRESS = 'https://trello-api.smallbee.me';
// const API_URL_ADDRESS = 'http://localhost:8000';


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
    const token = this.getCookie('token')
    axios({
      url: `${API_URL_ADDRESS}/card/`,
      method: 'get',
      headers: {
        Authorization: `Token ${token}`
      }
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
    getCookie(name) {
        const cookieRes = this.$cookies.get(name);
        if (cookieRes) return cookieRes;
        return null;
    },
    // Reactivity(1): TodoInput - adding new item
    addTodo(value) {
      // This below code is the reason of changing the whole code p.160
      // this.todoItems.push(value);

      // this.setCookie(value, value, 7);
      const token = this.getCookie('token')
      axios({
        url: `${API_URL_ADDRESS}/card/`,
        method: 'post',
        data: {
          contents: value
        },
        headers: {
          Authorization: `Token ${token}`
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
      const token = this.getCookie('token')
      axios({
        url: `${API_URL_ADDRESS}/card/${item.pk}/`,
        method: 'delete',
        headers: {
          Authorization: `Token ${token}`
        }
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
      const token = this.getCookie('token')
      for (var index in this.todoItems) {
        axios({
          url: `${API_URL_ADDRESS}/card/${this.todoItems[index].pk}/`,
          method: 'delete',
          headers: {
            Authorization: `Token ${token}`
          }
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
      const token = this.getCookie('token')
      axios({
        url: `${API_URL_ADDRESS}/card/${this.todoItems[index].pk}/`,
        method: 'put',
        data: {
          contents: value
        },
        headers: {
          Authorization: `Token ${token}`
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
