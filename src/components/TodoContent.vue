<script setup lang="ts">
import { ref, computed } from "vue";

import TodoTask from "./TodoTask.vue";
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

const props = defineProps({
  arr: {
    type: Array,
    required: true,
    default: () => [],
  },
});

const emits = defineEmits(["delete", "lineThrough"]);

// vars
const tasksToShow = ref(3);
const currentPage = ref(1);

// calculating the quantity of pagination pages
const pagPages = computed(() => {
  return Math.ceil(props.arr.length / tasksToShow.value);
});

// sending task id to App.vue
const deleteTask = (taskId: number) => {
  emits("delete", taskId);
};

// sending emit to App.vue
const lineThrough = () => {
  emits("lineThrough");
};
</script>
<template>
  <div
    class="mt-10 flex flex-col items-center max-w-4xl m-auto gap-5 h-[300px] overflow-hidden"
  >
    <TodoTask
      v-for="(task, id) in arr"
      :key="id"
      @deleteTask="deleteTask(id)"
      @lineThrough="lineThrough"
      :taskObj="task as object"
      :taskId="id"
    />
  </div>

  <template v-if="arr.length > tasksToShow">
    <Pagination
      v-slot="{ page }"
      :total="pagPages * 10"
      :sibling-count="1"
      show-edges
      :default-page="1"
      class="w-full flex items-center justify-center mt-10"
    >
      <PaginationList v-slot="{ items }" class="flex items-center gap-1">
        <PaginationFirst @click="currentPage = 1" />
        <PaginationPrev @click="currentPage--" />

        <template v-for="(item, index) in items">
          <PaginationListItem
            v-if="item.type === 'page'"
            :key="index"
            :value="item.value"
            @click="currentPage = item.value"
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

        <PaginationNext @click="currentPage++" />
        <PaginationLast @click="currentPage = items.length" />
      </PaginationList>
    </Pagination>
  </template>
</template>
