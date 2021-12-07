<template>
  <div>
    <Navegacao />
    <div>
      <h1>{{ animal.nomePlanta }}</h1> 
      <h4>{{ animal.especiePlanta }}</h4> 
      <div>
        <b-button @click="showModal = true" pill variant="info">
          <b-icon icon="plus"></b-icon>
        </b-button>
      </div>
      <ul>
        <li
          :key="planta.id" v-for="planta in animal.plantas"
        >
          <b-form-checkbox size="sm" >
            Nome da planta: {{ planta.nomePlanta }}
            Espécie da planta: {{ planta.especiePlanta }} 
            Cor da planta: {{ planta.corPlanta }}
           
           </b-form-checkbox
          >
        </li>
      </ul>
      <b-modal
        id="modal-planta"
        size="lg"
        v-model="showModal"
        hide-footer
      >
        <h1>Nova planta</h1>
        <b-container fluid>
          <b-row class="my-1">
            <b-col sm="1">
              <label for="nomePlanta">Nome planta:</label>
            </b-col>
            <b-col sm="5">
              <b-form-input
                id="nomePlanta"
                size="sm"
                placeholder="Escreva o nome da planta..."
                v-model="novaPlanta.nomePlanta"
              ></b-form-input>
              </b-col>
          </b-row>
        </b-container>
        <b-container fluid>
          <b-row class="my-2">
            <b-col sm="1">
              <label for="especiePlanta">Especie planta:</label>
            </b-col>
            <b-col sm="5">
              <b-form-input
                id="especiePlanta"
                size="sm"
                placeholder="Escreva a especie da planta..."
                v-model="novaPlanta.especiePlanta"
              ></b-form-input>
              </b-col>
          </b-row>
        </b-container>
        <b-container fluid>
          <b-row class="my-3">
            <b-col sm="1">
              <label for="corPlanta">Cor planta:</label>
            </b-col>
            <b-col sm="5">
              <b-form-input
                id="corPlanta"
                size="sm"
                placeholder="Escreva a cor da planta..."
                v-model="novaPlanta.corPlanta"
              ></b-form-input>
              </b-col>
          </b-row>
        </b-container>
        <b-button class="mt-3" block variant="success" @click="criarNovaPlanta">Criar Planta </b-button>
        <b-button class="mt-3" block variant="danger" @click="fecharModal">Fechar</b-button>
      </b-modal>
    </div>
  </div>
</template>

<script>
export default {
  async asyncData({ $axios, params }) {
    let animal
    try {
      //URL Final: http://localhost:8080/listasFauna/{id}
      animal = await $axios.$get(`/listasFauna/${params.caracteristicas}`)
      //data = response
    } catch (e) {
      //trata o erro se houver
      console.log(e)
    }
    console.log(JSON.stringify(animal))
    return { animal }
  },

  data: function () {
    return {
      showModal: false,
      novaPlanta: {
        nomePlanta: '',
        especiePlanta: '',
        corPlanta: '',
      },
      selecionados: [],
    }
  },
  methods: {
    async criarNovaPlanta(event) {
      //Enviar para o servidor a nova caracteristica
      try {
        let dataPlanta = await this.$axios.$post(`/listasFauna/criar/`,this.novaPlanta)
        if (dataPlanta !== null) {
          this.fecharModal(event)
          this.novaPlanta = {
            nomePlanta: '',
            especiePlanta:'',
            corPlanta: '',
          }
          this.atualizarPlantas()
        } else {
          throw new Error(
            `Planta para o animal de id ${this.animal.id} não foi criada!`
          )
        }
      } catch (error) {
        console.log(error)
      }
    },
    async atualizarPlantas() {
      this.animal = await this.$axios.$get(`/listasFauna/${this.animal.id}`)
    },
    fecharModal(event) {
      this.$bvModal.hide('modal-planta')
    },
  },
}
</script>
