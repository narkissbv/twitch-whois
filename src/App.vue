<template>
  <component :is="navigate"></component>
</template>

<script setup>
  import twitch from '@/utils/twitch'
  import { useStore } from 'vuex'
  import { computed, onMounted } from 'vue'
  import MainPage from './views/MainPage';
  import AuthPage from './views/AuthPage';

  const store = useStore()
  const viewer = computed(() => store.state.viewer)
  const navigate = computed(() => {
    if (viewer.value?.id) {
      return MainPage
    }
    return AuthPage
  });

  onMounted( () => {
    twitch.onAuthorized(async function (auth) {
      await store.dispatch('setAuth', auth);
      await store.dispatch('setViewer');
      if (viewer.value?.id) {
        await store.dispatch('fetchUserData');
      }
    })
  })


</script>

<style>
  body {
    background: #1f1f23;
    color: white;
    padding: 0;
    margin: 0;
    font-family: Inter,Roobert,Helvetica Neue,Helvetica,Arial,sans-serif;
    height: 100vh;
    position: relative;
    overflow: hidden;
  }
</style>
