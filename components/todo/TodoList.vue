<template lang="html">
  <section>
    <transition-group name="list" tag="ul">

      <!-- Reactivity(2): TodoList -->
      <li v-for="(item, index) in propsdata" v-bind:key="item.pk" class="shadow">
        <i class="checkBtn fas fa-check" aria-hidden="true"></i>
        {{ item.contents }}
        <span class="editBtn" type="button" v-on:click="editTodo(item, index)">
          <i class="fas fa-pencil-alt" aria-hidden="true"></i>
        </span>

        <span class="removeBtn" type="button" v-on:click="removeTodo(item, index)">
          <i class="far fa-trash-alt" aria-hidden="true"></i>
        </span>
      </li>
    </transition-group>

    <!-- Edit function using Modal -->
    <modal v-if="showEditModal" v-on:close="showEditModal = false">
      <h3 slot="header">Edit</h3>
      <!-- <span slot="footer" v-on:click="showEditModal = false"> -->
      <span slot="footer">
        <input class="inputBox" type="text" v-model="editTodoItem" placeholder=this.editTodoItem
        v-on:keyup.enter="doEditTodo();showEditModal = false">
        <i class="writeModalBtn fas fa-pencil-alt" aria-hidden="true" v-on:click="doEditTodo();showEditModal = false"></i>
        <i class="closeModalBtn fas fa-times" aria-hidden="true" v-on:click="showEditModal = false"></i>
      </span>
    </modal>

  </section>
</template>

<script>
import Modal from '../modal/Edit.vue'

export default {
  // data() {
  //   return {
  //     todoItems: []
  //   }
  // },
  // created() {
  //   if (localStorage.length > 0) {
  //     for (var i = 0; i < localStorage.length; i++) {
  //       this.todoItems.push(localStorage.key(i));
  //     }
  //   }
  // },

  // Reactivity(2): TodoList
  props: ['propsdata'],

  data() {
    return {
      showEditModal: false,
      editTodoItem: '',
      editIndex: ''
    }
  },

  methods: {
    removeTodo(item, index) {
      // Reactivity(4): TodoList(removeTodo)
      this.$emit('remove-todo', item, index)
    },
    editTodo(item, index) {
      this.showEditModal = !this.showEditModal;
      this.editTodoItem = item.contents;
      this.editIndex = index;
    },
    doEditTodo() {
      // console.log(this.editTodoItem, this.editIndex);
      this.$emit('edit-todo', this.editTodoItem, this.editIndex);
    }
  },
  components: {
    'Modal': Modal
  }
}
</script>

<style lang="css" scoped>
  ul {
    list-style-type: none;
    padding-left: 0px;
    margin-top: 0;
    text-align: left;
  }
  li {
    display: flex;
    min-height: 50px;
    height: 50px;
    line-height: 50px;
    margin: 0.5rem 0;
    padding: 0 0.9rem;
    background: white;
    border-radius: 5px;
  }
  .checkBtn {
    line-height: 45px;
    color: #62acde;
    margin-right: 5px;
  }
  .editBtn {
    margin-left: auto;
    color: #de4343;
  }
  .removeBtn {
    margin-left: 15px;
    color: #de4343;
  }
  /* Vue Animation */
  .list-enter-active, .list-leave-active {
    transition: all 1s;
  }
  .list-enter, .list-leave-to {
    opacity: 0;
    transform: translateX(-30px);
  }

</style>
