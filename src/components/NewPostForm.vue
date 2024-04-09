<template>
    <v-sheet class="mx-auto ma-10" max-width="700">
      <h1 class="my-10">Create New Post</h1>
      <v-form ref="form">
      
        <v-textarea  
        :rules="[rules.required(modelValue.title)]" 
        rows="2" 
        v-model="modelValue.title" 
        :placeholder="titlePlaceholder" 
        />
        <v-textarea 
        :rules="[rules.required(modelValue.content)]" 
        maxlength="auto" rows="10" 
        v-model="modelValue.content" 
        :placeholder="contentPlaceholder" 
        />
  
        <v-btn class="mt-2" @click="handleSubmit" block>{{submitBtn}}</v-btn>
        <v-btn class="mt-4" color="error" block @click="handleReset">
          {{resetBtn}}
        </v-btn>
      </v-form>
  
    </v-sheet>
  </template>
  
  <script setup>
  import { defineProps, defineEmits, ref, watch, computed } from "vue";
  
  const props = defineProps({
    titlePlaceholder: String,
    contentPlaceholder: String,
    submitBtn: String,
    resetBtn: String,
    modelValue: {
      type: Object,
      required: true
    }
  });
  
  const emits = defineEmits(['update:modelValue','handleSubmit', 'handleReset']);
  const title = ref(props.modelValue.title || '');
  const content = ref(props.modelValue.content || '');
  
  const handleSubmit = (val) => {
      emits('handleSubmit', val)
  }
  
  const handleReset = (val) => {
      emits('handleReset', val)
  }

  const rules = ref({
    required(value) {
      if(value) return true
      return 'You must enter text here...'
    }
  })

  watch(() => props.modelValue, (newValue) => {
    title.value = newValue.title || '';
    content.value = newValue.content || '';
  });
  
  watch([title, content], () => {
    emits('update:modelValue', { title: title.value, content: content.value });
  });
  </script>