<template>
    <div>
    <h1>Form contato</h1>
    <b-form @submit="onSubmit" @reset="onReset">
      <b-form-group
        label="Nome:"
        label-for="nome"
      >
        <b-form-input
          id="nome"
          v-model="form.nome"
          type="text"
          required
        ></b-form-input>
      </b-form-group>

      <b-form-group label="Telefone:" label-for="telefone">
        <b-form-input
          id="telefone"
          v-model="form.telefone"
          type="text"
          required
        ></b-form-input>
      </b-form-group>

      <b-form-group label="Email:" label-for="email">
        <b-form-input
          id="email"
          v-model="form.email"
          type="email"
        ></b-form-input>
      </b-form-group>

      <b-button type="submit" variant="primary">Submit</b-button>
      <b-button type="reset" variant="danger">Reset</b-button>
    </b-form>
    <!-- <b-card class="mt-3" header="Form Data Result">
      <pre class="m-0">{{ form }}</pre>
    </b-card> -->
  </div>
</template>

<script>
export default {
    data() {
        return {
            form: {
                nome: '',
                telefone: '',
                email: '',
            },
        }
    },
    methods: {
      onSubmit(evt) {
        evt.preventDefault()
        
        axios.post('/contatos', this.form)
        .then((res) =>{
            this.$store.commit('addContato', res.data)
            
            this.$swal({
                icon: 'success',
                title: 'Salvo',
                text: 'Salvo com sucesso'
            })
            
            this.form.nome = ''
            this.form.telefone = ''
            this.form.email = ''
        })
        .catch((err) => {
            alert('Erro ao salvar')
        })
      },
      onReset(evt) {
        evt.preventDefault()
        // Reset our form values
        this.form.nome = ''
        this.form.telefone = ''
        this.form.email = ''
      }
    }
}
</script>