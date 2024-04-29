<script setup lang="ts">
import TodoForm from "@/components/TodoForm.vue";
import TodoContent from "@/components/TodoContent.vue";
import { ref } from "vue";
import { Switch } from "@/components/ui/switch";

// vars
const tasks = ref<object[]>([]);
const darkMode = ref(false);

// localStorage logic
if (localStorage.getItem("tasks")) {
  tasks.value = JSON.parse(localStorage.getItem("tasks")!);
}

if (localStorage.getItem("darkMode")) {
  darkMode.value = JSON.parse(localStorage.getItem("darkMode")!);
}

// adding new task to "tasks" array
interface newTaskType {
  text: string;
  underline: boolean;
}

const pasteTasks = (task: string) => {
  const newTask: newTaskType = {
    text: task,
    underline: false,
  };

  tasks.value.push(newTask);
  localStorage.setItem("tasks", JSON.stringify(tasks.value));
};

// switching color theme
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

// deleting the task
const deleteTask = (idToRemove: number) => {
  tasks.value = tasks.value.filter((task: object, taskId: number) => {
    console.log(task);

    return taskId !== idToRemove;
  });

  localStorage.setItem("tasks", JSON.stringify(tasks.value));
};

// adding line-through to localStorage
const lineThrough = () => {
  localStorage.setItem("tasks", JSON.stringify(tasks.value));
};
</script>

<template>
  <div class="wrapper min-w-full min-h-full relative">
    <div class="absolute mr-10 top-0 right-0 flex flex-col items-center gap-3">
      <Switch @click="switchMode()" :checked="darkMode" />
      Dark mode
    </div>
    <TodoForm @addNewTask="pasteTasks" />

    <TodoContent :arr="tasks" @delete="deleteTask" @lineThrough="lineThrough" />
  </div>
</template>
