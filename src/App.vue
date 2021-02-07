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
          <button
            class="button is-fullwidth is-primary is-rounded"
            @click="buscar"
          >
            Buscar
          </button>
        </div>
      </div>

      <div v-for="bre in filteredBreeds" :key="bre.id">
        <Racadog :name="bre.name" :num="bre.id" :url="bre.url" />
        <!-- <img :src="bre.img" > -->
      </div>

      <hr />

      <nav
        class="pagination is-centered"
        role="navigation"
        aria-label="pagination"
      >
        <a class="pagination-previous" @click="pagePrevious">Anterior</a>
        <a class="pagination-next" @click="pageNext">Próxima</a>

        <ul class="pagination-list">
          <li>
            <a class="pagination-link" aria-label="Ir para página 1">1</a>
          </li>
          <li><span class="pagination-ellipsis">&hellip;</span></li>
          <li>
            <a class="pagination-link" aria-label="Ir para página 6">6</a>
          </li>
          <li>
            <a
              class="pagination-link is-current"
              aria-label="Página 7"
              aria-current="page"
              >7</a
            >
          </li>
          <li>
            <a class="pagination-link" aria-label="Ir para página 8">8</a>
          </li>
          <li><span class="pagination-ellipsis">&hellip;</span></li>
          <li>
            <a class="pagination-link" aria-label="Ir para página 10">10</a>
          </li>
        </ul>
      </nav>
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
      pageData: [],     
    };
  },
  created() {
    axios
      .get("http://localhost/api-racadog/api/v0/dog?offset=0&limit=10")
      .then((res) => {
        this.pageData = res.data;
        this.breeds = res.data.results;
        this.filteredBreeds = res.data.results;
      });
  },
  components: {
    Racadog,
  },
  methods: {
    buscar: function () {
      this.filteredBreeds = this.breeds;
      if (this.busca == "" || this.busca == "") {
        this.filteredBreeds = this.breeds;
      } else {
        this.filteredBreeds = this.breeds.filter((breed) =>
          breed.name.match(this.busca.toLocaleLowerCase())
        );
      }
    },

    pageNext: function () {
      if (this.pageData.next !== null) {
        axios.get(this.pageData.next).then((res) => {
          this.pageData = res.data;
          this.breeds = res.data.results;
          this.filteredBreeds = res.data.results;
        });
      } else {
        alert("Não temos mais raças para mostrar além dessas!");

      }
    },

    pagePrevious: function () {
      if (this.pageData.previous !== null) {
        axios.get(this.pageData.previous).then((res) => {
          this.pageData = res.data;
          this.breeds = res.data.results;
          this.filteredBreeds = res.data.results;
        });
      } else {
        alert("Você já está no início, essas são as primeiras raças.");
      }
    },
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
