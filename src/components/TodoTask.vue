<script setup lang="ts">
import { Checkbox } from "@/components/ui/checkbox";
import { Button } from "@/components/ui/button";
import { Ref, ref } from "vue";

const props = defineProps({
  taskText: String,
  taskId: Number,
});

const checkbox: Ref<Boolean> = ref(false);
const showBtns: Ref<Boolean> = ref(false);
</script>

<template>
  <article
    class="p-5 border-secondary border-2 rounded-xl text-2xl w-full shadow-md flex items-center justify-between"
  >
    <p :class="{ ckecked: checkbox }">{{ taskText }}</p>

    <div class="flex items-center justify-between max-w-fit gap-3 basis-full">
      <Button
        class="flex items-center justify-between gap-1"
        @click="$emit('deleteTask', taskId)"
      >
        <svg
          fill="#ffffff"
          height="800px"
          width="800px"
          version="1.1"
          id="Layer_1"
          class="w-full h-full fill-background"
          xmlns="http://www.w3.org/2000/svg"
          xmlns:xlink="http://www.w3.org/1999/xlink"
          viewBox="0 0 330 330"
          xml:space="preserve"
        >
          <g id="XMLID_6_">
            <g id="XMLID_11_">
              <path
                d="M240,121.076H30V275c0,8.284,6.716,15,15,15h60h37.596c19.246,24.348,49.031,40,82.404,40c57.897,0,105-47.103,105-105
			C330,172.195,290.816,128.377,240,121.076z M225,300c-41.355,0-75-33.645-75-75s33.645-75,75-75s75,33.645,75,75
			S266.355,300,225,300z"
              />
            </g>
            <g id="XMLID_18_">
              <path
                d="M240,90h15c8.284,0,15-6.716,15-15s-6.716-15-15-15h-30h-15V15c0-8.284-6.716-15-15-15H75c-8.284,0-15,6.716-15,15v45H45
			H15C6.716,60,0,66.716,0,75s6.716,15,15,15h15H240z M90,30h90v30h-15h-60H90V30z"
              />
            </g>
            <g id="XMLID_23_">
              <path
                d="M256.819,193.181c-5.857-5.858-15.355-5.858-21.213,0L225,203.787l-10.606-10.606c-5.857-5.858-15.355-5.858-21.213,0
			c-5.858,5.858-5.858,15.355,0,21.213L203.787,225l-10.606,10.606c-5.858,5.858-5.858,15.355,0,21.213
			c2.929,2.929,6.768,4.394,10.606,4.394c3.839,0,7.678-1.465,10.607-4.394L225,246.213l10.606,10.606
			c2.929,2.929,6.768,4.394,10.607,4.394c3.839,0,7.678-1.465,10.606-4.394c5.858-5.858,5.858-15.355,0-21.213L246.213,225
			l10.606-10.606C262.678,208.535,262.678,199.039,256.819,193.181z"
              />
            </g>
          </g>
        </svg>

        <p>Delete</p>
      </Button>
      <Checkbox
        class="w-[40px] min-h-[40px]"
        v-model="checkbox"
        @click="checkbox = !checkbox"
      />
    </div>
  </article>
</template>

<style lang="scss" scoped>
.ckecked {
  text-decoration: line-through;
}
</style>
