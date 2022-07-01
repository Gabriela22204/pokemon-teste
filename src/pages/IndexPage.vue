<template>

<div class="row justify-around full-width items-center margin-top: 90px">
      <q-input outlined filled v-model="search" label="Digite o nome do Pokemon" />
      <q-btn color="purple" label="Pesquisar" @click="getPokemon" />
</div>
  <q-page class="flex flex-center"  :style-fn="myTweak">


    <div class="column items-center">
      <h2>{{name}}</h2>
    <q-img :src="url"  width="250px" />
    </div>



    <div class="row justify-between absolute full-width container-arrows">

      <q-icon
        name="fa-solid fa-caret-left"
        color="primary"
        size="4rem"
        class="q-ml-sm cursor-pointer"
        @click = "getPokemon(this.id - 1)"
      >
        <q-tooltip>
          Anterior
        </q-tooltip>
      </q-icon>
      <q-icon
        name="fa-solid fa-caret-right"
        color="primary"
        size="4rem"
        class="q-mr-sm cursor-pointer"
        @click = "getPokemon(this.id + 1)"
      >
        <q-tooltip>
          Próximo
        </q-tooltip>
      </q-icon>
      </div>
  </q-page>

<div class="column items-center align=center">
  <v-card-content>
    <h2>inserir gráfico aqui</h2>
  </v-card-content>
</div>
</template>

<script>

import { defineComponent } from 'vue'
import api from "../services/api";


    export default defineComponent({
      name: 'IndexPage',

      data(){
        return {
          name: '',
          url: "",
          id: null,
          search: "ditto",
          height: "",
          weight: "",
          HP: "",
          Attack: "",
          Defense: "",
          Speed: "",

        };
      },

      async beforeMount() {
        await this.getPokemon();
      },



      methods: {
        getPokemon(id) {
              api
                .get(id  > 0 ? `/pokemon/${id}/` :  `/pokemon/${this.search}/`)
                .then( (response) => {
                  // handle success
                  this.id = response.data.id;
                  this.name = response.data.name;
                  this.search = response.data.name;
                  this.url = response.data.sprites.other.dream_world.front_default;

                })
          .catch( (error) => {
            // handle error
            this.triggerNegative();
          })
        },
        triggerPositive () {
        this.$q.notify({
          type: "positive",
          position: "top",
          message: `Pokemon encontrado! :D ` ,
        });
      },

      triggerNegative () {
        this.$q.notify({
          type: "negative",
          position: "top",
          message: `Ocorreu um erro, tente novamente. :(` ,
        });
      },
      }


    },)


</script>

<style lang="scss" scoped>
  .container-arrows {

  }


</style>
