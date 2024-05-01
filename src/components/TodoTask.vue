<script setup lang="ts">
import { Checkbox } from "@/components/ui/checkbox";
import { Button } from "@/components/ui/button";
import { SquareX } from "lucide-vue-next";

const props = defineProps({
  taskObj: {
    type: Object,
    required: true,
    default: () => {},
  },
  taskId: Number,
});

const emits = defineEmits(["lineThrough", "deleteTask"]);

// changing taskObj.underline and sending emit to TodoContent.vue
const lineThrough = () => {
  props.taskObj.underline = !props.taskObj.underline;

  emits("lineThrough");
};
</script>

<template>
  <article
    class="p-5 border-secondary border-2 rounded-xl text-2xl w-full shadow-md flex items-center justify-between"
  >
    <p :class="{ ckecked: taskObj.underline }">{{ taskObj.text }}</p>

    <div class="flex items-center justify-between max-w-fit gap-3 basis-full">
      <Button
        class="flex items-center justify-between gap-1"
        @click="$emit('deleteTask', taskId, taskObj)"
      >
        <SquareX />

        Delete
      </Button>
      <Checkbox
        class="w-[40px] min-h-[40px]"
        :checked="taskObj.underline"
        v-model="taskObj.underline"
        @click="lineThrough"
      />
    </div>
  </article>
</template>

<style lang="scss" scoped>
.ckecked {
  text-decoration: line-through;
}
</style>
