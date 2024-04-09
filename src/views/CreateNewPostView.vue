<template>
  <NewPostForm 
  v-model="postData" 
  titlePlaceholder="Title" 
  contentPlaceholder="Write your content here..."
  submitBtn="Submit" 
  resetBtn="Reset" 
  @handleSubmit="createPost"
  @handleReset="reset"
  />
</template>

<script setup>
import { ref } from 'vue';
import NewPostForm from '../components/NewPostForm.vue'

const postData = ref({
  title: '',
  content: ''
});

const createPost = () => {
  fetch('https://jsonplaceholder.typicode.com/posts', {
    method: 'POST',
    body: JSON.stringify({
      title: postData.value.title,
      body: postData.value.content,
      userId: 1
    }),
    headers: {
      'Content-type': 'application/json; charset=UTF-8',
    }
  }).catch((error) => {
    console.error(error);
  });
}

const reset = () => {
  postData.value.title = ''
  postData.value.content = ''
}
</script>