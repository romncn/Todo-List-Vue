<template>
  <div>
    <input
      type="text"
      class="todo-input"
      placeholder="What need todo item"
      v-model="newTodo"
      @keyup.enter="addTodo"
    />
    <transition-group
      name="fade"
      enter-active-class="animated fadeInUp"
      leave-active-class="animated fadeOutDown"
    >
      <div v-for="(todo,index) in todosFiltered" :key="todo.id">
        <div v-if="todos.length" class="todo-item">
          <div class="todo-item-left">
            <input type="checkbox" v-model="todo.completed" />
            <div
              v-if="!todo.editing"
              class="todo-item-label"
              :class="{ completed : todo.completed }"
              @dblclick="editTodo(todo)"
            >{{todo.title}}</div>
            <input
              v-else-if="todo.editing"
              class="todo-item-edit"
              type="text"
              v-model="todo.title"
              @keyup.enter="doneEdit(todo,index)"
              @keyup.esc="cancelEdit(todo)"
              @blur="doneEdit(todo,index)"
              v-focus
            />
          </div>
          <div class="todo-item-right">
            <div class="remove-item" @click="removeTodo(index)">&times;</div>
          </div>
        </div>
      </div>
    </transition-group>

    <div class="extra-container">
      <div>
        <label>
          <input type="checkbox" :checked="!anyRemaining" @change="checkAllTodos" />Check All
        </label>
      </div>
      <div>
        <span>{{ remaining }} items left</span>
      </div>
    </div>
    <div class="extra-container">
      <div>
        <button :class="{ active: filter == 'all' }" @click="filter = 'all'">All</button>
        <button :class="{ active: filter == 'active' }" @click="filter = 'active'">Active</button>
        <button :class="{ active: filter == 'completed' }" @click="filter = 'completed'">Completed</button>
      </div>
      <div>
        <transition name="fade">
          <button v-if="showClearCompletedButton" @click="clearCompleted">Clear Completed</button>
        </transition>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "todo-list",
  data() {
    return {
      newTodo: "",
      idForTodo: 0,
      beforeEditCache: "",
      filter: "all",
      todos: []
    };
  },
  computed: {
    remaining() {
      return this.todos.filter(todo => !todo.completed).length;
    },
    anyRemaining() {
      return this.remaining != 0;
    },
    todosFiltered() {
      if (this.filter == "all") {
        return this.todos;
      } else if (this.filter == "active") {
        return this.todos.filter(todo => !todo.completed);
      } else if (this.filter == "completed") {
        return this.todos.filter(todo => todo.completed);
      }
      return this.todos;
    },
    showClearCompletedButton() {
      return this.todos.filter(todo => todo.completed).length > 0;
    }
  },
  directives: {
    focus: {
      inserted: function(el) {
        el.focus();
      }
    }
  },
  methods: {
    addTodo() {
      if (this.newTodo.trim().length == 0) {
        return null;
      }
      this.todos.push({
        id: this.idForTodo,
        title: this.newTodo,
        titleEdited: this.newTodo,
        completed: false,
        editing: false
      });
      console.log(this.newTodo);
      this.newTodo = "";
      this.idForTodo++;
    },
    editTodo(todo) {
      this.beforeEditCache = todo.title;
      todo.editing = true;
    },
    doneEdit(todo, index) {
      if (todo.title.trim() == "") {
        this.todos.splice(index, 1);
      }
      todo.editing = false;
      todo.completed = false;
    },
    cancelEdit(todo) {
      todo.title = this.beforeEditCache;
      todo.editing = false;
    },
    removeTodo(index) {
      this.todos.splice(index, 1);
      console.log("remove", index);
    },
    checkAllTodos() {
      this.todos.forEach(todo => (todo.completed = event.target.checked));
    },
    clearCompleted() {
      this.todos = this.todos.filter(todo => !todo.completed);
    },
    log(index) {
      console.log("show", index);
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="scss">
@import url("https://cdnjs.cloudflare.com/ajax/libs/animate.css/3.7.2/animate.min.css");

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
  animation-duration: 0.2s;
}
.todo-item-left {
  display: flex;
  align-items: center;
}
.todo-item-label {
  padding: flex;
  border: 1px solid white;
  margin-left: 12px;
}
.todo-item-edit {
  font-size: 24px;
  color: #1a6aa0;
  margin-left: 12px;
  widows: 100%;
  padding: 10px;
  border: 1px solid #ccc;
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  &:focus {
    outline: none;
  }
}
.completed {
  text-decoration: line-through;
  color: grey;
}
.remove-item {
  margin-bottom: 12px;
  display: flex;
  align-items: center;
  justify-content: space-between;
  color: rgb(194, 194, 194);
  &:hover {
    color: black;
    cursor: pointer;
  }
}
.extra-container {
  display: flex;
  align-items: center;
  justify-content: space-between;
  font-size: 16px;
  border-top: 1px solid lightgrey;
  padding-top: 14px;
  margin-bottom: 14px;
}
button {
  font-size: 14px;
  background-color: white;
  appearance: none;
  border: 0.5px solid black;
  margin: 2px;
  &:hover {
    background: lightgreen;
  }
  &:focus {
    outline: none;
  }
}
.active {
  background: lightgreen;
}

//CSS Transition
.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.3s;
}
.fade-enter,
.fade-leave-to {
  opacity: 0;
}
// .delete-btn {
//   margin-top: 20px !important;
// }
// v-btn {
//   background-color: green;
//   color: white;
//   border-radius: 10px;
// }
</style>
