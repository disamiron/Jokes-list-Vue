<template>
  <h1>10 jokes for fun</h1>

    <ol>
    <li v-for="{ id, joke } in jokes" v-bind:key="id">
      {{ joke }}
      <button v-bind:key="id" @click="likeThis(id)">lol</button>
    </li>
  </ol>
</template>


<script>
import { ref } from "vue";



export default {

  data () {
    return {
      base : []
    }
  },
  mounted() {
    if (localStorage.getItem('base')) {
     
        this.base = JSON.parse(localStorage.getItem('base'));
   
    }
  },
  methods: {
    likeThis(newId) {
      var checkBase = [];
      for (var key in this.base) {
        checkBase.push(this.base[key].id);
      }

      if (checkBase.includes(newId)) {
        this.base[key].like = !this.base[key].like;
        console.log(this.base[key].like);
      } else {
        this.base.push({id: newId, like : true});
      }
      this.saveLike();
    },
    saveLike() {
      const parsed = JSON.stringify(this.base);
      localStorage.setItem('base', parsed);
    }
  },
    

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