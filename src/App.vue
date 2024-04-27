<script setup lang="ts">
import TodoForm from "@/components/TodoForm.vue";
import TodoContent from "@/components/TodoContent.vue";
import { ref, Ref, watch } from "vue";
import { Switch } from "@/components/ui/switch";

const tasks: Ref<Array<string>> = ref([]);
const darkMode = ref(false);

if (localStorage.getItem("tasks")) {
  tasks.value = JSON.parse(localStorage.getItem("tasks")!);
}

if (localStorage.getItem("darkMode")) {
  darkMode.value = JSON.parse(localStorage.getItem("darkMode")!);
}

const pasteTasks = (task: string) => {
  tasks.value.push(task);
  localStorage.setItem("tasks", JSON.stringify(tasks.value));
};

const dark = () => {
  if (darkMode.value) {
    document.body.classList.add("dark");
  } else {
    document.body.classList.remove("dark");
  }
  localStorage.setItem("darkMode", JSON.stringify(darkMode.value));
};

dark();

const switchMode = () => {
  darkMode.value = !darkMode.value;
  dark();
};

const deleteTask = (idToRemove: number) => {
  tasks.value = tasks.value.filter((task, taskId) => {
    return taskId !== idToRemove;
  });

  localStorage.setItem("tasks", JSON.stringify(tasks.value));
};
</script>

<template>
  <div class="wrapper min-w-full min-h-full relative">
    <div class="absolute mr-10 top-0 right-0 flex flex-col items-center gap-3">
      <Switch @click="switchMode()" v-model="darkMode" />
      Dark mode
    </div>
    <TodoForm @addNewTask="pasteTasks" />
    <TodoContent :arr="tasks" @delete="deleteTask" />
  </div>
</template>
