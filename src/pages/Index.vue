<template>
  <q-page class="flex flex-center">
    <div class="column items-center">
      <h2>{{ name }}</h2>
      <q-img :src="url" :ratio="1" width="250px" />
    </div>
    <div class="row justify-around full-width">
      <q-input filled v-model="search" label="Digite nome pokemon" />
      <q-btn color="primary" label="Pesquisar" @click="getPokemon" />
    </div>
  </q-page>
</template>

<script>
import api from "../services/api";
export default {
  name: 'PageIndex',

  data() {
    return {
      name: "",
      url: "",
      search: "ditto",
    }
  },

  async beforeMount() {
    await this.getPokemon();
  },

  methods: {
    getPokemon() {
      api.get(`/pokemon/${this.search}/`)
      .then((response) => {
        // handle success
        console.log(response);
        this.name = response.data.name;
        this.url = response.data.sprites.other.dream_world.front_default;
      })
      .catch((error) => {
        // handle error
        console.log(error);
      })
      .then(() => {
        // always executed
      });
    }
  }
}
</script>
