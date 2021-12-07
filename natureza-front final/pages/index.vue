<template>
  <div>
    <Navegacao />
    <div class="container">
      <h1>Lista de Animais</h1>

      <b-table striped responsive hover :items="animais" :fields="campos">
        <template v-slot:cell(nomePlanta)="data">
          <b-icon class="h2 mb-2" :icon="iconsCategoryMap[data.value]"></b-icon>
        </template>
        <template v-slot:cell(nome)="data">
          <NuxtLink :to="`/animal/${data.item.id}`">
            <span>{{ data.value }}</span>
          </NuxtLink>
        </template>
      </b-table>
    </div>
  </div>
</template>

<script>
export default {

  //GET
  async asyncData({ $axios }) {
    let animais
    try {
      // URL Final: http://localhost:8080/listasFauna
      animais = await $axios.$get('/listasFauna')
      // data = response
    } catch (e) {
      // trata o erro se houver
      console.log(e)
    }
    console.log(JSON.stringify(animais));
    return { animais }
  },
 

  data:  ()=> {
    return {
      campos: [
        {
          key: 'nome',
          label: 'Nome',
        },
        {
          key: 'especie',
          label: 'Espécie',
        },
        {
          key: 'cor',
          label: 'Cor',
        },
        {
          key: 'expectativaVida',
          label: 'Expectativa de Vida'
        },

      ],
      iconsCategoryMap:{
        Animal: 'star-fill',
        /* Trabalho: 'house-door-fill',
        Saúde: 'suit-heart-fill',
        Compras: 'cart3', */

      },
      animais: [
        {
          categoria: 'star-fill',
          animal: 'Coala',
          descricao: '',
          dataCriacao: '22/11/2021',
          id: 1,
        },
        {
          categoria: 'star-fill',
          animal: 'Elefante asiático',
          descricao: '',
          dataCriacao: '22/11/2021',
          id: 2,
        },
        {
          categoria: 'star-fill',
          animal: 'Panda',
          descricao: '',
          dataCriacao: '22/11/2021',
          id: 3,
        },
      ],
    }
  },
}
</script>


