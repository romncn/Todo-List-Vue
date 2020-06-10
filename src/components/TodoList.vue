<template>
  <div>
    <input
      type="text"
      class="todo-input"
      placeholder="What need todo item"
      v-model="newTodo"
      @keyup.enter="addTodo"
    />
    Input Todo Item
    <div v-for="(todo,index) in todos" :key="todo.id">
      <div v-if="todos.length" class="todo-item">
        <div>{{todo.title}}</div>
        <div class="remove-item" @click="removeTodo(index)">&times;</div>
      </div>
    </div>
    <v-btn type="button" @click="deleteTodo" class>delete</v-btn>
  </div>
</template>

<script>
export default {
  name: "todo-list",
  data() {
    return {
      newTodo: "",
      idForTodo: 0,
      todos: []
    };
  },
  methods: {
    addTodo() {
      if (this.newTodo.trim().length == 0) {
        return null;
      }
      this.todos.push({
        id: this.idForTodo,
        title: this.newTodo,
        completed: false
      });
      console.log(this.newTodo);
      this.newTodo = "";
      this.idForTodo++;
    },
    removeTodo() {
    //   this.todos.splice(index, 1);
    },
    deleteTodo() {
      this.todos = [];
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="scss">
.todo-input {
  width: 100%;
  padding: 10px 18px;
  font-size: 18px;
  margin-bottom: 16px;
  &:focus {
    outline: 0;
  }
}
.todo-item {
  margin-bottom: 12px;
  display: flex;
  align-items: center;
  justify-content: space-between;
}
.remove-item {
  margin-bottom: 12px;
  display: flex;
  align-items: center;
  justify-content: space-between;
  color: rgb(194, 194, 194);
  &:hover {
    color: black;
  }
}
v-btn {
  background-color: green;
  color: white;
  border-radius: 10px;
}
</style>
