<template>
  <v-sheet class="mx-auto ma-10 text-center" height="auto" max-width="700">
    <h1 class="text-capitalize font-weight-black">
      {{ currentPost.title }}
    </h1>

    <h5 class="ma-7">Post Number {{ currentPost.id }}</h5>

    <v-img
      height="400px"
      src="https://cdn.vuetifyjs.com/images/cards/sunshine.jpg"
      cover
    ></v-img>

    <v-textarea
      v-if="edit === false"
      class="ma-8"
      maxlength="auto"
      v-model="editedText"
      :value="editedText === undefined ? currentPost.body : editedText"
    >
    </v-textarea>

    <p v-if="edit === true" class="ma-8">
      {{ editedText === undefined ? currentPost.body : editedText }}
    </p>

    <div class="d-flex flex-end">
      <slot name="body"></slot>
    </div>
  </v-sheet>
</template>

<script setup>
import { defineProps, defineEmits, ref, watch } from "vue";

const props = defineProps({
  modelValue: [String ] ,
  currentPost: Object,
  edit: {
    type: Boolean,
    default: true,
  },
});

const emits = defineEmits(['update:modelValue'])

const editedText = ref(props.modelValue)

watch(() => props.modelValue, (newVal) => {
editedText.value = newVal
})

watch(editedText, (newVal) => {
  emits('update:modelValue', newVal)
})
</script>