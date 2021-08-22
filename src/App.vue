<template>
  <h1>10 jokes for fun</h1>

    <ol>
    <li v-for="{ id, joke } in jokes" v-bind:key="id">
      {{ joke }}
    </li>
  </ol>
</template>


<script>
import { ref } from "vue";

export default {
  setup() {
    const jokes = ref(null);

    async function updateJokes() {
      jokes.value = null;

      const res = await fetch(
        `https://v2.jokeapi.dev/joke/Any?type=single&amount=10`
      );

      const data = await res.json();
      jokes.value = data.jokes;
    }

    updateJokes();

    return { jokes };
  },
};
</script>