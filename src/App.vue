<template>
<h1>TEN RANDOM JOKES</h1>
  <h2>Search among jokes:</h2>
    <input type="text" @input="changeInput" />



    <ol>
    <li v-for="{ id, joke } in jokes" v-bind:id="'list-' + id" v-bind:key="id">
      {{ joke }}
      <button v-bind:key="id" @click="likeThis(id)" v-bind:class="'list-' + id"></button>
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
  updated() {
    this.classObject();
  },
  methods: {

    likeThis(newId) {
      var safe = false;
      for (var key in this.base) {
        if (this.base[key].id==newId) {
          this.base[key].like = !this.base[key].like;
          safe = true;
        }
      };
      if (!safe) {
        this.base.push({id: newId, like : true});

      };
      this.classObject();  
      this.saveLike();
    }
    
    ,
    saveLike() {
      const parsed = JSON.stringify(this.base);
      localStorage.setItem('base', parsed);

    },

    classObject () {
      var checkBase = this.base;
      for (var key in checkBase) {
        if (document.getElementById('list-'+checkBase[key].id)) {
          let elem = document.getElementById('list-'+checkBase[key].id);
          let icon = document.querySelector('button.list-'+checkBase[key].id);
          if (checkBase[key].like) {       
            elem.style.backgroundColor = "#a4d6ed";
            icon.style.opacity = "1.0"

          } else if (!checkBase[key].like) {
            elem.style.backgroundColor = "#ffffff";
            icon.style.opacity = "0.3"
          }
        }
      }  
    },


  
    changeInput: function(event) {
      var searchText = event.target.value;


      for (var key in this.jokes) {
        let elem = document.getElementById('list-'+this.jokes[key].id);
        if (this.jokes[key].joke.toLowerCase().includes(searchText.toLowerCase())) {
          
          elem.style.display = "";

        } else {
          elem.style.display = "none";

        }}

      
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
  }
};
</script>

<style lang="scss">
body {
  text-align: center;
  width: 1000px;
  height: 100vh;
  margin: 0 auto;
  font-family: 'Urbanist', sans-serif;
}


html,
input {
  font-family: 'Urbanist', sans-serif;
}

#app {
button {
  border: 1px solid black;
  background-size: 100% 100%;
  float: right;
  background-image: url("../img/like.png");
  width:50px;
  height: 50px;
  background-repeat : no-repeat;
  opacity: 0.3;
}

  h1 {
    font-size: 3rem;
    margin: 0;
    margin-bottom: 1rem;
  }

  input {
    padding: 0.25rem 0.5rem;

    &:first-of-type {
      margin-left: 0.5rem;
      margin-right: 1rem;
    }
  }

  ol {

    li {
      margin: 20px;
      border: 1px solid black;
      border-radius: 15px;
      overflow: hidden;
      width: 900px;
      text-align: left;
      min-height: 60px;
      list-style-type: none;
      padding: 15px 0.5rem ;
      font-size: 22px;
      box-shadow: 0 0 10px rgba(0,0,0,0.5);
    }
  }

  p {
    font-size: 0.9rem;
    margin-top: 2rem;
  }
}
</style>