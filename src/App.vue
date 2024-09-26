<template>
  <div id="app">
    <AppHeader />

    <AppFilters :active-filter="activeFilter" @set-filter="setFilter" />

    <main class="app-main">
      <AppToDo :todos="filterdTodos" @toggle-todo="toggleTodo" @remove-todo="removeTodo"  @edit-todo="editTodo"/>

      <AppAddToDo @add-todo="addToDo" />
    </main>

    <AppFooter :stats="stats" />
  </div>
</template>
<script lang="ts">
import { defineComponent } from 'vue';
import AppHeader from './components/AppHeader.vue'
import AppFilters from './components/AppFilters.vue'
import AppToDo from './components/AppToDo.vue'
import AppAddToDo from './components/AppAddToDo.vue'
import AppFooter, { Stats } from './components/AppFooter.vue'
import { Todo } from "@/types/Todo";
import { Filter } from './types/Filter';
interface State {
  todos: Todo[],
  activeFilter: Filter
}
export default defineComponent({
  components: {
    AppHeader,
    AppFilters,
    AppToDo,
    AppAddToDo,
    AppFooter
  },
  data(): State {
    return {
      todos: this.loadTodos(),
      activeFilter: 'All'
    }
  },
  computed: {
    filterdTodos(): Todo[] {
      switch (this.activeFilter) {
        case 'Active':
          return this.todos.filter(todo => !todo.complited)
        case 'Done':
          return this.todos.filter(todo => todo.complited)
        case 'All':
        default:
          return this.todos
      }
    },
    stats(): Stats {
      return {
        active: this.todos.filter(todo => !todo.complited).length,
        done: this.todos.filter(todo => todo.complited).length
      }
    }
  },
  methods: {
    addToDo(todo: Todo) {
      this.todos.push(todo),
      this.saveTodos();
    },
    toggleTodo(id: number) {
      const targetTodo = this.todos.find(todo => todo.id === id)
      if (targetTodo) {
        targetTodo.complited = !targetTodo.complited,
        this.saveTodos();
      }
    },
    removeTodo(id: number) {
      this.todos = this.todos.filter((todo: Todo) => todo.id !== id),
      this.saveTodos();
    },
    editTodo(updatedTodo: Todo) {
      const index = this.todos.findIndex(todo => todo.id === updatedTodo.id);
      if (index !== -1) {
        this.todos[index].text = updatedTodo.text; // Обновляем текст задачи
        this.saveTodos();
      }
    },
    setFilter(filter: Filter) {
      this.activeFilter = filter
    },
    saveTodos() {
      localStorage.setItem('todos', JSON.stringify(this.todos));  // Сохраняем задачи в localStorage
    },
    loadTodos(): Todo[] {
      const savedTodos = localStorage.getItem('todos');
      return savedTodos ? JSON.parse(savedTodos) : [];
    }
  }
});
</script>