<template>
  <ul class="todo-list">
    <AppToDoItems v-for="todo in todos" :key="todo.id" :todo="todo" @toggle-todo="toggleTodo"
      @remove-todo="removeTodo" />
  </ul>
</template>
<script lang="ts">
import { defineComponent, PropType } from "vue";
import AppToDoItems from './AppToDoItems.vue'
import { Todo } from "@/types/Todo";

export default defineComponent({
  components: {
    AppToDoItems,
  },
  props: {
    todos: {
      type: Array as PropType<Todo[]>
    }
  },
  methods: {
    toggleTodo(id: number) {
      this.$emit('toggleTodo', id)
    },
    removeTodo(id: number) {
      this.$emit('removeTodo', id)
    }
  },
  emits: {
    toggleTodo: (id: number) => Number.isInteger(id),
    removeTodo: (id: number) => Number.isInteger(id)
  }
})
</script>