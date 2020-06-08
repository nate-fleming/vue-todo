<template>
  <div>
    <h1 class="header">ToDos</h1>
    <input
      class="todo-input"
      type="text"
      v-model="newTodo"
      placeholder="Add a new Todo"
      @keyup.enter="addTodo"
    />
    <div v-for="todo in todos" :key="todo.id" class="todo-item">
      <input type="checkbox" v-model="todo.isComplete" />
      <div class="todo-title" :class="{ completed: todo.isComplete }">
        {{ todo.title }}
      </div>
      <div class="delete-todo" @click="removeTodo(todo.id)">x</div>
    </div>
  </div>
</template>

<script>
export default {
  name: "todo-list",
  data() {
    return {
      newId: 3,
      newTodo: "",
      todos: [
        {
          id: 1,
          title: "Learn Vue",
          isComplete: false,
        },
        {
          id: 2,
          title: "Learn Python",
          isComplete: false,
        },
      ],
    };
  },
  methods: {
    addTodo() {
      if (this.newTodo.trim().length === 0) {
        return;
      }

      this.todos.push({
        id: this.newId,
        title: this.newTodo,
        isComplete: false,
      });

      this.newTodo = "";
      this.newId++;
    },
    removeTodo(id) {
      const newTodos = this.todos.filter((todo) => todo.id !== id);
      this.todos = newTodos;
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="scss" scoped>
.header {
  text-align: center;
}

.todo-input {
  display: block;
  margin: 8px auto;
  width: 200px;
  height: 40px;
}

.todo-item {
  display: flex;
  align-items: center;
  margin-bottom: 8px;
}

.todo-title {
  margin-left: 8px;
}

.delete-todo {
  cursor: pointer;
  margin-left: auto;
  margin-right: 8px;
  &:hover {
    color: red;
  }
}

.completed {
  color: grey;
  text-decoration: line-through;
}
</style>
