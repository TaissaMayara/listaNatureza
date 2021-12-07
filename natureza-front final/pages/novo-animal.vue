<template>
  <div>
    <Navegacao />
    <div class="mx-auto" style="max-width: 1000px; padding: 30px">
      <h1>Adicionar novo animal</h1>
      <b-form @submit.prevent="criarAnimal">
        <b-form-group
          id="input-group-1"
          label="Nome do novo animal:"
          label-for="nome"
        >
          <b-form-input
            id="nome"
            v-model="form.nome"
            placeholder="Entre com o nome"
            required
          ></b-form-input>
        </b-form-group>

        <b-form-group
          id="input-group-2"
          label="Espécie:"
          label-for="especie"
        >
          <b-form-input
            id="especia"
            v-model="form.especie"
            placeholder="Digite uma espécie..."
            required
          ></b-form-input>
        </b-form-group>

        <b-form-group
          id="input-group-3"
          label="Cor:"
          label-for="cor"
        >
          <b-form-input
            id="cor"
            v-model="form.cor"
            placeholder="Digite uma cor..."
            required
          ></b-form-input>
        </b-form-group>

        <b-form-group
          id="input-group-3"
          label="Expectativa de vida:"
          label-for="expectativaVida"
        >
          <b-form-input
            id="expectativaVida"
            v-model="form.expectativaVida"
            placeholder="Digite a expectativa de vida..."
            required
          ></b-form-input>
        </b-form-group>

        <b-button type="submit" variant="primary">Criar</b-button>
        <b-button type="reset" variant="danger">Limpar</b-button>
      </b-form>
    </div>
  </div>
</template>

<script>
export default {
  data: function () {
    return {
      form: {
        nome: '',
        especie: '',
        cor: '',
        expectativaVida: '',
      },
      /*let data = {"nome":
      "Panda",
      "categoria": "Mamífero",
      "descricao": "O panda é um mamífero"},*/
      
    }
  },
  methods: {
    async criarAnimal() {
      //event.preventDeFault();
      try {
        let dataAnimal = await this.$axios.$post(
          '/listasFauna/criar',
          this.form
        )
        console.log(dataAnimal);
        if (dataAnimal !== null) {
          this.$bvModal.msgBoxOk('Um novo animal foi criado com sucesso.', {
              title: 'Confirmation',
              size: 'sm',
              buttonSize: 'sm',
              okVariant: 'success',
              headerClass: 'p-2 border-bottom-0',
              footerClass: 'p-2 border-top-0',
              centered: true,
            })
            .then(() => {
              this.limparCampos()
            })
        } else{
          throw new Error('Não foi possível criar o animal ${this.form.nome}');
        }
      } catch (error) {
        console.log(error)
      }
    },
    limparCampos() {
      this.form = {
        nome: '',
        especie: '',
        cor: '',
        expectativaVida: '',
      }
    },
  },
}
</script>
