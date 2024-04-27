<script setup lang="ts">
import { Input } from "@/components/ui/input";
import { Button } from "@/components/ui/button";

import { ref, defineEmits, Ref } from "vue";

const taskText: Ref<string> = ref("");
const emits = defineEmits(["addNewTask"]);

const addTask = () => {
  if (taskText.value.length > 0) {
    emits("addNewTask", taskText.value);

    taskText.value = "";
  }
};
</script>

<template>
  <form
    class="max-w-4xl m-auto mt-10 p-5 border-secondary border-2 rounded-lg shadow-lg"
  >
    <h1 class="text-4xl font-semibold">Add some plans:</h1>
    <label class="flex items-center flex-col gap-2 mt-4">
      <Input placeholder="New task..." v-model="taskText" />
      <Button class="w-full" @click.prevent="addTask" @keypress.enter="addTask"
        >Add Task</Button
      >
    </label>
  </form>
</template>
