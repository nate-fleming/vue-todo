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
    <transition-group
      name="fade"
      enter-active-class="animated fadeInUp"
      leave-active-class="animated fadeOutDown"
    >
      <div v-for="todo in filteredTodos" :key="todo.id" class="todo-item">
        <input type="checkbox" v-model="todo.isComplete" />
        <div>
          <input
            type="text"
            class="todo-edit"
            v-model="todo.title"
            v-if="todo.isEditable"
            @blur="toggleEdit(todo)"
            @keyup.enter="(e) => e.target.blur()"
            @keyup.escape="restoreTitle(todo)"
            v-focus
          />
          <div
            class="todo-title"
            :class="{ completed: todo.isComplete }"
            v-else
            @dblclick="toggleEdit(todo)"
          >
            {{ todo.title }}
          </div>
        </div>
        <div class="delete-todo" @click="removeTodo(todo.id)">x</div>
      </div>
    </transition-group>
    <div class="footer">
      <div>
        <label for="check-all"
          ><input type="checkbox" :checked="!anyRemaining" @change="checkAll" />
          Check All</label
        >
      </div>
      <div>{{ remaining }} items left</div>
    </div>
    <div class="filter-section">
      <button
        class="button"
        :class="{ active: filter === 'all' }"
        @click="filter = 'all'"
      >
        All
      </button>
      <button
        class="button"
        :class="{ active: filter === 'active' }"
        @click="filter = 'active'"
      >
        Active
      </button>
      <button
        class="button"
        :class="{ active: filter === 'complete' }"
        @click="filter = 'complete'"
      >
        Complete
      </button>
      <transition
        enter-active-class="animated fadeIn"
        leave-active-class="animated fadeOut"
      >
        <button
          v-if="showClearCompleted"
          class="button"
          :style="{ marginLeft: 'auto', marginRight: 0 }"
          @click="clearCompleted"
        >
          Clear Completed
        </button>
      </transition>
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
      titleCache: "",
      filter: "all",
      todos: [
        {
          id: 1,
          title: "Learn Vue",
          isComplete: false,
          isEditable: false,
        },
        {
          id: 2,
          title: "Learn Python",
          isComplete: false,
          isEditable: false,
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
        id: this.newId++,
        title: this.newTodo,
        isComplete: false,
      });

      this.newTodo = "";
    },
    removeTodo(id) {
      this.todos = this.todos.filter((todo) => todo.id !== id);
    },
    toggleEdit(todo) {
      todo.isEditable = !todo.isEditable;
    },
    restoreTitle(todo) {
      if (todo.title.trim().length === 0) {
        return;
      }

      todo.title = this.titleCache;
      todo.isEditable = false;
    },
    checkAll() {
      this.todos.forEach((todo) => (todo.isComplete = event.target.checked));
    },
    clearCompleted() {
      this.todos = this.todos.filter((todo) => !todo.isComplete);
    },
  },
  computed: {
    remaining() {
      return this.todos.filter((todo) => !todo.isComplete).length;
    },
    anyRemaining() {
      return this.remaining !== 0;
    },
    filteredTodos() {
      if (this.filter === "all") {
        return this.todos;
      } else if (this.filter === "active") {
        return this.todos.filter((todo) => !todo.isComplete);
      } else if (this.filter === "complete") {
        return this.todos.filter((todo) => todo.isComplete);
      } else {
        return this.todos;
      }
    },
    showClearCompleted() {
      return this.todos.filter((todo) => todo.isComplete).length;
    },
  },
  directives: {
    focus: {
      // directive definition
      inserted: function(el) {
        el.focus();
      },
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
  animation-duration: 0.5s;
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

.footer {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 8px 0;
  border-top: 1px solid black;
}

.filter-section {
  display: flex;
  align-items: center;
  padding: 8px 0;
}

.button {
  margin-right: 4px;
  height: auto;
  width: auto;
  padding: 4px 8px;
  vertical-align: middle;
  text-align: center;
}

.active {
  background-color: green;
}
</style>
