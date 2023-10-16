<script>
  import todos from './todos';

  export default {
    data() {
      let todos = [];
      const jsonData = localStorage.getItem('todos') || '[]';

      try {
        todos = JSON.parse(jsonData);
      } catch (error) {}

      return {
        todos,
        title: '',
      }
    },
    computed: {
      activeTodos() {
        return this.todos.filter(todo => !todo.completed);
      },
      completedTodos() {
        return this.todos.filter(todo => todo.completed);
      },
    },
    watch: {
      todos: {
        deep: true,
        handler() {
          localStorage.setItem('todos', JSON.stringify(this.todos));
        }
      }
    },
    methods: {
      handleSubmit() {
        this.todos.push({
          id: this.todos.length + 1,
          title: this.title,
          completed: false,
        });
        this.title = '';
      },
    }
  }
</script>

<template>
  <div class="todoapp">
    <h1 class="todoapp__title">todos</h1>

    <div class="todoapp__content">
      <header class="todoapp__header">
        <button type="button" class="todoapp__toggle-all" :class="{ active: activeTodos.length === 0 }"> </button>

        <form @submit.prevent="handleSubmit">
          <input 
            type="text" 
            class="todoapp__new-todo" 
            placeholder="What needs to be done?" 
            v-model="title"
          />
        </form>
      </header>

      <section class="todoapp__main">
        <div 
          class="todo"
          :class="{completed: todo.completed}" 
          v-for="todo, index of todos" 
          :key="todo.id"
        >
          <label class="todo__status-label">
            <input 
              type="checkbox" 
              class="todo__status" 
              v-model="todo.completed" 
            />
          </label>

          <form v-if="false">
            <input type="text" class="todo__title-field" placeholder="Empty todo will be deleted"
              value="Todo is being edited now" />
          </form>

          <template v-else>
            <span class="todo__title">{{ todo.title }}</span>
            <button 
              type="button" 
              class="todo__remove" 
              @click="todos.splice(index, 1)"
            >
              ×
            </button>
          </template>

          <div class="modal overlay" :class="{'is-active': false}">
            <div class="modal-background has-background-white-ter"></div>
            <div class="loader"></div>
          </div>
        </div>
      </section>

      <footer class="todoapp__footer">
        <span class="todo-count">
          {{ activeTodos.length }} items left
        </span>

        <nav class="filter">
          <a href="#/" class="filter__link selected">
            All
          </a>

          <a href="#/active" class="filter__link">
            Active
          </a>

          <a href="#/completed" class="filter__link">
            Completed
          </a>
        </nav>

        <button 
          type="button" 
          class="todoapp__clear-completed" 
          v-if="completedTodos.length > 0"
        >
          Clear completed
        </button>
      </footer>
    </div>

    <div class="notification is-danger is-light has-text-weight-normal" v-if="false">
      <button type="button" class="delete"> </button>

      Unable to add a todo
      <br />
      Unable to delete a todo
      <br />
      Unable to update a todo
    </div>
  </div>

  <main>
    <TheWelcome />
  </main>
</template>
