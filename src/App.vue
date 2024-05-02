<script setup lang="ts">
import TodoForm from "@/components/TodoForm.vue";
import TodoContent from "@/components/TodoContent.vue";
import { ref, computed } from "vue";
import { Switch } from "@/components/ui/switch";
import {
  Pagination,
  PaginationEllipsis,
  PaginationFirst,
  PaginationLast,
  PaginationList,
  PaginationListItem,
  PaginationNext,
  PaginationPrev,
} from "@/components/ui/pagination";
import { Button } from "@/components/ui/button";
import { watch } from "fs";

// vars
const tasks = ref<object[]>([]);
const darkMode = ref(false);
const tasksCopy = ref<object[]>([]);

// localStorage logic
if (localStorage.getItem("tasks")) {
  tasks.value = JSON.parse(localStorage.getItem("tasks")!);
}

if (localStorage.getItem("darkMode")) {
  darkMode.value = JSON.parse(localStorage.getItem("darkMode")!);
}

// adding new task to "tasks" array
type newTaskType = {
  text: string;
  underline: boolean;
};

const pasteTasks = (task: string) => {
  const newTask: newTaskType = {
    text: task,
    underline: false,
  };

  tasks.value.push(newTask);
  localStorage.setItem("tasks", JSON.stringify(tasks.value));

  tasksCopy.value = tasks.value.slice(); // Оновлюємо копію масиву
  currentPagTasks(); // Оновлюємо сторінку
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

// adding line-through to localStorage
const lineThrough = () => {
  localStorage.setItem("tasks", JSON.stringify(tasks.value));
  doneTasks();
  currentPagTasks();
};

// filtering tasks
const filteredTasks = ref<object[]>([]);

if (localStorage.getItem("filteredTasks")) {
  filteredTasks.value = JSON.parse(localStorage.getItem("filteredTasks")!);
}

const doneTasks = () => {
  tasks.value.forEach((el: object | any) => {
    if (el.underline) {
      filteredTasks.value.push(el);
      tasks.value = tasks.value.filter((el: object | any) => {
        return el.underline === false;
      });
    }
  });

  filteredTasks.value.forEach((el: object | any) => {
    if (el.underline === false) {
      tasks.value.push(el);
      filteredTasks.value = filteredTasks.value.filter((el: object | any) => {
        return el.underline === true;
      });
    }

    localStorage.setItem("tasks", JSON.stringify(tasks.value));
    localStorage.setItem("filteredTasks", JSON.stringify(filteredTasks.value));
  });
};

// deleting the task
const deleteTask = (taskIdToDelete: number, task: newTaskType) => {
  if (task.underline === false) {
    tasks.value = tasks.value.filter((task: object, taskId: number) => {
      return taskId !== taskIdToDelete;
    });
  } else {
    filteredTasks.value = filteredTasks.value.filter(
      (task: object, taskId: number) => {
        return taskId !== taskIdToDelete;
      }
    );
  }

  tasksCopy.value = tasks.value.slice();
  localStorage.setItem("filteredTasks", JSON.stringify(filteredTasks.value));
  localStorage.setItem("tasks", JSON.stringify(tasks.value));

  currentPagTasks();
};

// calculating pagination items
const tasksToShow = ref(3);
const currentPage = ref(1);
let start = ref(0);
let end = ref(tasksToShow.value);

// calculating current page content
function currentPagTasks() {
  start.value = tasksToShow.value * (currentPage.value - 1);
  end.value = start.value + tasksToShow.value;

  tasksCopy.value = tasks.value.slice(start.value, end.value);

  console.log(start.value, end.value);
  console.log(currentPage.value);
}

currentPagTasks();
// calculating the quantity of pagination pages
const pagPages = computed<number>(() => {
  return Math.ceil(tasks.value.length / tasksToShow.value);
});
</script>

<template>
  <div class="wrapper min-w-full min-h-full relative">
    <div class="absolute mr-10 top-0 right-0 flex flex-col items-center gap-3">
      <Switch @click="switchMode()" :checked="darkMode" />
      Dark mode
    </div>
    <TodoForm @addNewTask="pasteTasks" />

    <TodoContent
      :arr="tasksCopy"
      :filtered="filteredTasks"
      @delete="deleteTask"
      @lineThrough="lineThrough()"
    />
  </div>

  <tamplate v-if="tasks.length > tasksToShow">
    <Pagination
      v-slot="{ page }"
      :total="pagPages * 10"
      :sibling-count="1"
      show-edges
      :default-page="1"
      class="w-full flex items-center justify-center mt-4"
    >
      <PaginationList v-slot="{ items }" class="flex items-center gap-1">
        <PaginationFirst
          @click="
            currentPage = 1;
            currentPagTasks();
          "
        />
        <PaginationPrev
          @click="
            currentPage--;
            currentPagTasks();
          "
        />

        <template v-for="(item, index) in items">
          <PaginationListItem
            v-if="item.type === 'page'"
            :key="index"
            :value="item.value"
            @click="
              currentPage = item.value;
              currentPagTasks();
            "
            as-child
          >
            <Button
              class="w-10 h-10 p-0"
              :variant="item.value === page ? 'default' : 'outline'"
            >
              {{ item.value }}
            </Button>
          </PaginationListItem>
          <PaginationEllipsis v-else :key="item.type" :index="index" />
        </template>

        <PaginationNext
          @click="
            currentPage++;
            currentPagTasks();
          "
        />
        <PaginationLast
          @click="
            currentPage = items.length;
            currentPagTasks();
          "
        />
      </PaginationList>
    </Pagination>
  </tamplate>
</template>
