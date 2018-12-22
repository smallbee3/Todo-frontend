<template lang="html">
  <div class="clearAllContainer">
    <span class="clearAllBtn" v-on:click="clearTodo">Clear All</span>
  </div>
</template>

<script>
export default {
  methods: {
    clearTodo() {
      // localStorage.clear();

      // Reactivity(3): TodoFooter
      this.$emit('clear-todo');
    }
  }
}
</script>

<style lang="css">
  .clearAllContainer {
    width: 8.5rem;
    height: 50px;
    line-height: 50px;
    background-color: white;
    border-radius: 5px;
    margin: 0 auto;
  }
  .clearAllBtn{
    color: #e20303;
    display: black;
  }
</style>
