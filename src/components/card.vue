<script lang="ts">
import { IconSend } from '@tabler/icons-vue'
import { ref, onMounted } from 'vue'

export default {
  components: { IconSend },
  setup() {
    const todoList = ref([] as string[])
    const handleSubmit = (e: Event) => {
      e.preventDefault()

      const formData = new FormData(e.currentTarget as HTMLFormElement)
      const todoValue = formData.get('todo') as string
      todoList.value.push(todoValue)
      localStorage.setItem('todoList', JSON.stringify(todoList.value))

      const inputEl = document.querySelector('#todo') as HTMLInputElement
      inputEl.value = String()
    }
    onMounted(() => {
      const storage = localStorage.getItem('todoList')
      if (storage) {
        const arrayTodo: [] = JSON.parse(storage)
        arrayTodo.forEach((value) => {
          todoList.value.push(value)
        })
      }
    })
    return {
      todoList,
      handleSubmit,
    }
  },
}
</script>

<template>
  <section class="p-3 bg-[#181818] rounded border border-white grid grid-rows-[auto_1fr] gap-3">
    <header>
      <h1 class="text-2xl text-center">Lista de tareas</h1>
      <p>¿Qué tareas tienes planeadas hoy?</p>
    </header>
    <section class="grid grid-rows-[auto_1fr] gap-3">
      <form class="flex flex-col gap-1" v-on:submit="handleSubmit">
        <label class="text-sm text-gray-300" for="todo">Crea una tarea aquí</label>
        <section class="relative">
          <input class="rounded p-2 focus:placeholder-transparent focus:outline-none w-full" placeholder="Hacer el mercado..." type="text" name="todo" id="todo" autocomplete="off" />
          <button class="absolute right-3 bottom-2"><IconSend /></button>
        </section>
      </form>
      <section>
        <h2>Tareas</h2>
        <section class="min-h[3rem] max-h-72 overflow-y-auto flex flex-col">
          <div v-for="(item, index) in todoList" :key="index">
            <span> {{ `${index + 1}. ${item}` }}</span>
          </div>
        </section>
      </section>
    </section>
  </section>
</template>
