<template>
  <div>
    <DetailedPostContent v-model="editedText" :current-post="currentPost" :edit="edit">

      <template #body>
        <GreyButtons
        button-name="Edit" 
        @eventHandler="edit = false" 
        />
        <GreyButtons
        v-if="edit === false" 
        button-name="Done" 
        @eventHandler="updatePost(currentPost.id)" 
        />
        <GreyButtons
        button-name="Delete" 
        @eventHandler="deletePost(currentPost.id), $router.push({ name: 'home' })" 
        />
      </template>

    </DetailedPostContent>

    <DetailedPostComments
    header="Comments"
    :comments="comments"
     />
  </div>

</template>

<script setup>
import DetailedPostContent from '../components/DetailedPostContent.vue'
import DetailedPostComments from '../components/DetailedPostComments.vue'
import GreyButtons from '@/components/GreyButtons.vue';
import { useStore } from "vuex";
import { ref, computed, watch } from "vue";
import { useRoute } from "vue-router";

const store = useStore();
const router = useRoute();

store.commit(
  "setCurrentPostFromServer",
  store
    .dispatch("getCurrentPostFromServer", router.params.id)
    .then((res) => res)
);

store.commit(
    "setCurrentPostComments",
    store.dispatch("getComments").then((res) => res)
  );

const currentPost = computed(() => store.getters.currentPostFromServer);
const comments = computed(() => store.getters.currentPostComments);

const edit = ref(true);
const editedText = ref();

const updatePost = (id) => {
  edit.value = true;
  fetch(`https://jsonplaceholder.typicode.com/posts/${id}`, {
    method: "PATCH",
    body: JSON.stringify({
      body: editedText.value,
    }),
    headers: {
      "Content-type": "application/json; charset=UTF-8",
    },
  }).catch((error) => {
    console.error(error);
  });
};

const deletePost = (id) => {
  fetch(`https://jsonplaceholder.typicode.com/posts/${id}`, {
    method: "DELETE",
  }).catch((error) => {
    console.error(error);
  });
};
</script>