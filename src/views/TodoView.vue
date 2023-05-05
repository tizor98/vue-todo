<script setup lang="ts">
import TodoCreator from '@/components/TodoCreator.vue';
import TodoView from '@/components/TodoView.vue'
import { ref, type Ref, watch, computed } from 'vue';
import { uid } from 'uid';
import type { Todo } from '../interface/Todo';
import { Icon } from "@iconify/vue";

let todoList: Ref<Todo[]> = ref([]);

const setTodoLocalStorage = () => {
   localStorage.setItem("todo-list", JSON.stringify(todoList.value))
}
const fetchTodoList = () => {
   const savedTodoList: Todo[] = JSON.parse(localStorage.getItem("todo-list") || "{}")
   if(savedTodoList.length > 0) {
      todoList.value = savedTodoList
   }
}

watch(todoList, setTodoLocalStorage, { deep: true })
fetchTodoList()

const todosCompleted = computed(() => todoList.value.every(todo => todo.isCompleted))

const createTodo = (todo: string): void => {
   todoList.value.push({
      id: uid(),
      todo,
      isCompleted: false,
      isEditing: false,
   })
}

const toggleTodoComplete = (todoIndex: number): void => {
   todoList.value[todoIndex].isCompleted = !todoList.value[todoIndex].isCompleted
}
const toggleEditTodo = (todoIndex: number): void => {
   todoList.value[todoIndex].isEditing = !todoList.value[todoIndex].isEditing
}
const updateTodo = (val: string, todoIndex: number): void => {
   todoList.value[todoIndex].todo = val
}
const deleteTodo = (todoId: string): void => {
   todoList.value = todoList.value.filter( todo => todo.id !== todoId)
}

</script>

<template>
   <main>
      <h1>Create ToDo</h1>
      <TodoCreator @create-todo="createTodo" />
      <ul class="todo-list" v-if="todoList.length > 0">
         <TodoView v-for="(todo, index) in todoList" :todo="todo" :index="index" @toggle-complete="toggleTodoComplete" @edit-todo="toggleEditTodo" @update-todo="updateTodo" @delete-todo="deleteTodo" />
      </ul>
      <p class="todos-msg" v-else>
         <Icon icon="noto-v1:sad-but-relieved-face" width="22" />
         <span>You have no ToDo's to complete! Add one</span>
      </p>
      <p v-if="todosCompleted && todoList.length > 0" class="todos-msg">
         <Icon icon="noto-v1:party-popper" />
         <span>You have completed all your ToDos!</span>
    </p>
   </main>
</template>

<style lang="scss" scoped>
   main {
      display: flex;
      flex-direction: column;
      max-width: 500px;
      width: 100%;
      margin: 0 auto;
      padding: 40px 16px;

      h1 {
         margin-bottom: 16px;
         text-align: center;
      }

      .todo-list {
         display: flex;
         flex-direction: column;
         list-style: none;
         margin-top: 24px;
         gap: 20px;
      } 
   
      .todos-msg {
         display: flex;
         align-items: center;
         justify-content: center;
         gap: 8px;
         margin-top: 24px;
      }
   }

</style>
