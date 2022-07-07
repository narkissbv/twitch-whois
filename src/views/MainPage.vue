<template>
  <h2>Welcome, {{ username }}</h2>
  <div><input type="text" ref="input"/></div>
  <div><button @click="fetchData">Submit</button></div>
  <div class="results">
    <span>{{ results.display_name }}</span>
    <a :href="url">{{ url }}</a>
  </div>
</template>

<script setup>
  import { useStore } from 'vuex'
  import { computed, ref } from 'vue'
  import axios from 'axios'

  const store = useStore();
  const username = computed(() => store.state.username);
  const input = ref(null);
  const url = ref(null);
  let results = ref('');
  const fetchData = async () => {
    const userData = await axios.get(`https://api.twitch.tv/helix/users?id=${input.value.value}`, {
      headers: {
        'Authorization': `Extension ${store.state.auth.helixToken}`,
        'Client-Id': 'dyui2ak59yhvkwm7oll2hlpg3jw6j2'
      }
    })
    results.value = userData.data.data[0]
    url.value = `https://twitch.tv/${userData.data.data[0].display_name}`
  }
</script>

<style scoped>

</style>