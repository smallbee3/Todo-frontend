<template lang="html">
  <div class="inputBox shadow">
    <!-- <input type="text" v-model="newTodoItem">
    <button v-on:click="addTodo">add</button> -->
    <input type="text" v-model="newTodoItem" placeholder="Type what you want to do"
    v-on:keyup.enter='addTodo'>
    <span class="addContainer" v-on:click="addTodo">
      <i class="addBtn fas fa-plus" aria-hidden="true"></i>
    </span>

    <modal v-if="showModal" v-on:close="showModal = false">
      <h3 slot="header">Alert!</h3>
      <span slot="footer" v-on:click="showModal = false">
        Input what you want to do!
        <i class="closeModalBtn fas fa-times" aria-hidden="true"></i>
      </span>
    </modal>

  </div>
</template>

<script>
import Modal from './modal/Default.vue'

export default {
  data() {
    return {
      newTodoItem: '',
      showModal: false
    }
  },
  methods: {
    addTodo() {

      if (this.newTodoItem.trim() !== "") {
        var value = this.newTodoItem.trim();
        // localStorage.setItem(value, value);

        // Reactivity(1): TodoInput - adding new item
        this.$emit('add-todo', value);
        this.clearInput();
      } else {
        this.showModal = !this.showModal;
      }
    },
    clearInput() {
      this.newTodoItem = '';
    }
  },
  components: {
    'Modal': Modal
  }
}
</script>

<style lang="css" scoped>
  input:focus {
    outline: none;
  }
  .inputBox {
    background: white;
    height: 50px;
    line-height: 50px;
    border-radius: 5px;
  }
  .inputBox input {
    border-style: none;
    font-size: 0.9rem;
  }
  .addContainer {
    float: right;
    background: linear-gradient(to right, #6478FB, #8763FB);
    display: block;
    width: 3rem;
    border-radius: 0 5px 5px 0;
  }
  .addBtn {
    color: white;
    vertical-align: middle;
  }
</style>
