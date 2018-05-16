<template>
  <div class="container" id="app">
    <h1>{{title}}</h1>
    <form class="form-inline" action="">
      <input v-validate="'required'" class="form-control" type="text" name="todo" v-model="todoText">
      <button class="btn btn-primary" v-on:click.prevent="createTodo()">新增代辦事項</button> 
      <span v-show="errors.has('todo')">{{ errors.first('todo') }}</span>
    </form>
    <br>
    <ul>
      <li v-for="(todo, index) in todos">
        <input type="checkbox" v-model="todo.isDone" @change="saveToStore()">
        <label v-bind:class="{'checked': todo.isDone}" for="">{{todo.content}} {{todo.created|date}}</label>
        <button class="btn btn-danger" @click="deleteTodo(todo)">刪除</button>
      </li>
    </ul>
  </div>
</template>

<script>
import moment from 'moment';
moment.locale('zh-tw');
export default {
  name: 'app',
  data() {
    return {
      title: 'Vuejs Todo App',
      todoText: '',
      todos: []
    }
  },
  created() {
    if (this.isInit) {
      this.todos = JSON.parse(localStorage.getItem('vues-todos'));
    }
  },
  computed: {
    isInit() {
      return localStorage.getItem('vues-todos') !== null;
    }
  },
  filters: {
    date(value) {
      return moment(value).calendar();
    }
  },
  methods: {
    createTodo(msg) {
      if (this.todoText === '') {
        return;
      }
      this.todos.push({
        content: this.todoText,
        created: Date.now(),
        isDone: false
      });
      this.saveToStore();
      this.todoText = '';
    },
    saveToStore() {
      localStorage.setItem('vues-todos', JSON.stringify(this.todos));
    },
    deleteTodo(todo) {
      this.todos = this.todos.filter((t) => t !== todo);
      this.saveToStore();
    }
  }
}
</script>

<style scoped>
#app {
  padding: 50px;
}
.checked {
  text-decoration: line-through;  
}
</style>
