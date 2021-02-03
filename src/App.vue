<template>
  <div id="app">
    <div class="column is-half is-offset-one-quarter">
      <div class="has-text-centered">
        <img src="./assets/img_home.png" />
        <hr />
        <h4 class="is-size-4">Racadog</h4>
        <div class="field">
          <input
            type="text"
            class="input is-rounded"
            placeholder="Buscar raça pelo nome"
            v-model="busca"
          />
        </div>
        <div class="field is-grouped">
          <button class="button is-fullwidth is-primary is-rounded" @click="buscar">
           Buscar
          </button>
        </div>
      </div>

      <div v-for="(bre, index) in filteredBreeds" :key="bre.url">
        <Racadog :name="bre.name" :num="index + 1" :url="bre.url" />
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import Racadog from "./components/Racadog.vue";
export default {
  name: "App",
  data() {
    return {
      breeds: [],
      filteredBreeds: [],
      busca: "",
    };
  },
  created() {
    axios
      .get("http://192.168.0.13/api-racadog/api/v0/dog?offset=0&limit=277")
      .then((res) => {
        this.breeds = res.data.results;
        this.filteredBreeds = res.data.results
      });
  },
  components: {
    Racadog,
  },
  methods: {
    buscar: function() {
      this.filteredBreeds = this.breeds;
       if (this.busca == "" || this.busca == "") {
         this.filteredBreeds = this.breeds;
      } else {      
        this.filteredBreeds = this.breeds.filter(breed => breed.name.match(this.busca.toLocaleLowerCase()));       
      }
    }
  },
  computed: {
    /*
    resultadoBusca: function () {
      if (this.busca == "" || this.busca == "") {
        return this.pokemons;
      } else {
        return this.pokemons.filter(pokemon => pokemon.name == this.busca);
      }
    },
    */
  },
};
</script>

<style>
</style>
