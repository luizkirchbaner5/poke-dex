<template>
  <q-page class="flex flex-center">
    <div class="column items-center">
      <h2>{{ name }}</h2>
      <q-img :src="url" width="250px" />
    </div>
    <div class="row justify-around full-width">
      <q-input filled v-model="search" label="Digite nome pokemon" />
      <q-btn color="primary" label="Pesquisar" @click="getPokemon" />
    </div>
    <div class="row justify-between absolute full-width container-arrows">
      <q-icon
        name="far fa-arrow-alt-circle-left"
        color="primary"
        size="50px"
        class="q-ml-sm cursor-pointer"
        @click="getPokemon(id - 1)"
      >
      <q-tooltip>Anterior</q-tooltip>
      </q-icon>
      <q-icon
        name="far fa-arrow-alt-circle-right"
        color="primary"
        size="50px"
        class="q-mr-sm cursor-pointer"
        @click="getPokemon(id + 1)"
      >
      <q-tooltip>Próximo</q-tooltip>
      </q-icon>
    </div>
  </q-page>
</template>

<script>
import api from "../services/api";
export default {
  name: 'PageIndex',

  data() {
    return {
      id: null,
      name: "",
      url: "",
      search: "ditto",
    }
  },

  async beforeMount() {
    await this.getPokemon();
  },

  methods: {
    getPokemon(id) {
      api.get(id > 0 ? `/pokemon/${id}/` : `/pokemon/${this.search}/`)
      .then((response) => {
        // handle success
        console.log(response);
        this.id = response.data.id;
        this.name = response.data.name;
        this.search = response.data.name;
        this.url = response.data.sprites.other.dream_world.front_default;
        this.triggerPositive();
      })
      .catch((error) => {
        // handle error
        this.triggerNegative();
      })
      .then(() => {
        // always executed
      });
    },

    triggerPositive () {
      this.$q.notify({
        type: 'positive',
        position: 'top',
        message: `Pokemon encontrado ;)`
      })
    },

    triggerNegative () {
      this.$q.notify({
        type: 'negative',
        position: 'top',
        message: `Ocorreu um erro, tente novamente.`
      })
    },
  }
}
</script>