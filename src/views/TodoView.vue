<script setup lang="ts">
import TodoCreator from '@/components/TodoCreator.vue';
import { ref, type Ref } from 'vue';
import { uid } from 'uid';

type Todo = {
   id: string,
   todo: string,
   isCompleted: boolean,
   isEditing: boolean,
}

const todoList: Ref<Todo[]> = ref([]);

const createTodo = (todo: string) => {
   todoList.value.push({
      id: uid(),
      todo,
      isCompleted: false,
      isEditing: false,
   })
}

</script>

<template>
   <main>
      <h1>Create ToDo</h1>
      <TodoCreator @create-todo="createTodo" />
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
   }
</style>
