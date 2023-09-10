<template>
    <div id="todo">
      <div class="container">
        <div class="row">
          <input class="check" type="checkbox" />
          <h3 class="col strikethrough"
          @dblclick="editTodo(todo)">{{ todo.text }}</h3>
          <div v-show="todo.edit == false">
            <input v-show="todo.edit == true"
            v-model="todo.title"
            v-on:blur="todo.edit=false; $emit('update')"
            @keyup.enter="todo.edit=false; $emit('update')">
          </div>
          <hr>
          <button @click="removeTodo"
          type="button"
          class="btn btn-danger btn-sm">Delete</button>
        </div>
      </div>
    </div>
  </template>
  
  <script>
  export default {
    props: ['todo'],
    methods: {
      removeTodo: function (todo) {
        this.$emit('remove')
      },
      editTodo: function (todo) {
        this.editedTodo = todo
      }
    }
  }
  </script>