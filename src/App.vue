<template>
  <div class="w-full h-full flex justify-center items-center">
    <div v-if="!loading" class="max-w-xs w-full flex flex-col">
      <div class="w-3/4 p-4 rounded-2xl bg-teal-800 text-white self-start">
        {{data.setup}}
      </div>
      <div v-if="isClickTellMe" class="w-3/4 mt-2 p-4 rounded-2xl bg-red-800 text-white self-end">
        {{data.delivery}}
      </div>
      <button v-if="!isClickTellMe" class="bg-green col-span-1 rounded-lg py-2 hover:opacity-90 w-full mx-auto mt-4" @click="onTellMe">
        Tell me
      </button>
      <button v-else class="bg-green col-span-1 rounded-lg py-2 hover:opacity-90 w-full mx-auto mt-4" @click="onAnother">
        Another
      </button>
    </div>
    <div v-else>
      Loading ...
    </div>
  </div>
</template>

<script setup>
import { onMounted, ref } from 'vue'

const loading = ref(false);
const data = ref({});
const isClickTellMe = ref(false);

onMounted(() => {
  fetchData();
})

const fetchData = () => {
  loading.value = true;
   return fetch('https://v2.jokeapi.dev/joke/christmas')
    .then(res => res.json())
    .then(res => {
      data.value = res;
    })
     .finally(() => {
       loading.value = false;
     });
}
const onTellMe = () => {
  isClickTellMe.value = true;
}
const onAnother = () => {
  fetchData().then(() => {
    isClickTellMe.value = false;
  });

}
</script>
