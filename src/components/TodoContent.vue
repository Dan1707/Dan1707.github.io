<script setup lang="ts">
import { Tabs, TabsContent, TabsList, TabsTrigger } from "@/components/ui/tabs";
import TodoTask from "./TodoTask.vue";

defineProps({
  arr: {
    type: Array,
    required: true,
    default: () => [],
  },
  filtered: {
    type: Array,
    required: true,
    default: () => [],
  },
});

const emits = defineEmits(["delete", "lineThrough"]);

// sending task id to App.vue
const deleteTask = (taskId: number, task: object) => {
  emits("delete", taskId, task);
};

// sending emit to App.vue
const lineThrough = () => {
  emits("lineThrough");
};
</script>
<template>
  <Tabs default-value="current" class="m-auto max-w-4xl mt-6">
    <TabsList class="m-auto flex items-center justify-around">
      <TabsTrigger class="w-full" value="current">Current tasks</TabsTrigger>
      <TabsTrigger class="w-full" value="done">Done</TabsTrigger>
    </TabsList>
    <TabsContent value="current" class="basis-full">
      <div
        class="mt-4 flex flex-col basis-full items-center max-w-4xl m-auto gap-3 h-[300px] overflow-hidden"
      >
        <TodoTask
          v-for="(task, id) in arr"
          :key="id"
          @deleteTask="deleteTask"
          @lineThrough="lineThrough"
          :taskObj="task as object"
          :taskId="id"
        />
      </div>
    </TabsContent>
    <TabsContent value="done" class="basis-full">
      <div
        class="mt-4 flex flex-col basis-full items-center max-w-4xl m-auto gap-3 h-[300px] overflow-hidden"
      >
        <TodoTask
          v-for="(task, id) in filtered"
          :key="id"
          @deleteTask="deleteTask"
          @lineThrough="lineThrough"
          :taskObj="task as object"
          :taskId="id"
        />
      </div>
    </TabsContent>
  </Tabs>
</template>
