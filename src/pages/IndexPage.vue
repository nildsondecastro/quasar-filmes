<template>
  <q-page class="flex flex-center">
    <div class="column items-center">
      <h5>{{name}}</h5>
      <q-img :src="url" :ratio="1" width="250px" />
    </div>
    <div class="row justify-around full-width">
      <q-input filled v-model="search" label="Nome do Filme" />
      <q-btn color="primary" label="Pesquisar" @click="getFilme()" />
    </div>
  </q-page>
</template>

<script>
import { defineComponent } from 'vue'
import api from "../services/api"

export default defineComponent({
  name: 'IndexPage',

  data () {
    return {
      name: "",
      url: "",
      search: "",
      i: "",
      t: ""
    }; 
  },

  methods: {
    getFilme() {
      console.log(`i: ${this.i}, t: ${this.t}, t: ${this.search}`);
      api
        .get(`?apikey=e166d688&t=${this.search}`)
        .then((response) => {
          console.log(response);
          console.log(response.data.Title);
          //this.id = response.data.id;
          this.name = response.data.Title;
          this.search = response.data.Title;
          this.url = response.data.Poster;
          if (response.data.Response == "False") {
            this.triggerNegative();
          } else {
            this.triggerPositive();
          }
        })
        .catch((err) => {
          console.log(err);
          this.triggerNegative();
        });
      
    },
    triggerPositive () {
      this.$q.notify({
        type: 'positive',
        position: 'top',
        message: 'Filme Encontrado.'
      })
    },
    triggerNegative () {
      this.$q.notify({
        type: 'negative',
        position: 'top',
        message: 'Filme NÃO Encontrado.'
      })
    },
  },
})
</script>
