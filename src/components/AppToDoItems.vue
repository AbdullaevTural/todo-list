<template>
  <li class="todo-item" :class=" {'todo-item--done':todo.complited}" @click="toggleTodo">
      <div class="todo-item__status">
        <i class="bi bi-check2"></i>
      </div>
      <span v-if="!isEditing" class="todo-item__text">{{ todo.text }}</span>
    <input v-if="isEditing" v-model="editText" @keyup.enter="saveEdit" @blur="saveEdit" />
    <div class="todo-item__actions">
    <button class="todo-item__remove-button" @click.stop="removeTodo">
        <i class="bi bi-trash3"></i>
      </button>
      <button class="todo-item__edit-button" @click.stop="enableEditing" v-if="!isEditing">
        <i class="bi bi-pencil"></i>
      </button>
    </div>
    </li>
</template>

<script lang="ts">

import { Todo } from "@/types/Todo";
import { defineComponent, PropType } from "vue"

export default  defineComponent({
 props:{
  todo:{
      type:Object as PropType<Todo>,
      required:true,
  }
 },
 data() {
    return {
      isEditing: false, 
      editText: this.todo.text,
    };
  },
 methods:{
  toggleTodo(){
    this.$emit('toggleTodo', this.todo.id)
  },
  removeTodo(){
    this.$emit('removeTodo', this.todo.id)
  },
  enableEditing() {
      this.isEditing = true; // Включаем режим редактирования
    },
    saveEdit() {
      if (this.editText.trim() !== "") {
        this.$emit("editTodo", { ...this.todo, text: this.editText }); // Эмитим событие редактирования
      }
      this.isEditing = false; // Выключаем режим редактирования
    },
 },
 emits:{
  toggleTodo: (id:number)=> Number.isInteger(id),
  removeTodo: (id:number)=> Number.isInteger(id),
  editTodo: (todo: Todo) => typeof todo === "object",
}
})
</script>