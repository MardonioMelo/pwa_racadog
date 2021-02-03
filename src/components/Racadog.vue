<template>
  <div id="margin">
    <div class="card">
      <div class="card-image">
        <figure class="level-item has-text-centered">
          <img :src="currentImg" alt="Imagem da Raça" />
        </figure>
      </div>
      <div class="card-content">
        <div class="media">
          <div class="media-content  has-text-centered">
            <p class="title is-4">{{ num }} - {{ upper }}</p>
            <p class="subtitle is-6">{{ breed.type }}</p>
          </div>
        </div>
        <div class="content">
          <button class="button is-medium is-fullwidth" @click="mudarSprite">
            OUTRA FOTO
          </button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  created: function () {
    axios.get(this.url).then((res) => {     
      this.breed.type = res.data.breed.group_akc_pt;
      this.breed.front = res.data.album[0];
      this.breed.back = res.data.album[1];
      this.currentImg = this.breed.front;     
    });
  },
  data() {
    return {
      isFront: true,
      currentImg: "",
      breed: {
        type: "",
        front: "",
        back: "",
      },
    };
  },

  props: {
    num: Number,
    name: String,
    url: String,
  },
  computed: {
    upper() {
      return this.name[0].toUpperCase() + this.name.slice(1);
    },
  },
  methods: {
    mudarSprite: function () {
      if (this.isFront) {
        this.isFront = false;
        this.currentImg = this.breed.back;
      } else {
        this.isFront = true;
        this.currentImg = this.breed.front;
      }
    },
  },
};
</script>

<style>
#margin {
  margin-top: 2%;
}
</style>