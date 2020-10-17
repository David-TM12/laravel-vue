<template>
  <div>
    <h1>Tabela</h1>
    <b-table striped hover :items="contatos" :fields="cabecalhos">
      <template v-slot:cell(acoes)="data">
        <b-button
          variant="danger"
          size="sm"
          @click="alertaExclusao(data.item.id)"
          >Excluir</b-button
        >
      </template>
    </b-table>
  </div>
</template>

<script>
export default {
  data() {
    return {
      // Note `isActive` is left out and will not appear in the rendered table
      cabecalhos: [
        { key: "nome", label: "Nome" },
        { key: "telefone", label: "Telefone" },
        { key: "acoes", label: "Ações" },
      ],
    };
  },

  mounted() {
    axios
      .get("/contatos")
      .then((res) => {
        // this.contatos = res.data
        this.$store.commit("setContatos", res.data);
      })
      .catch((err) => {
        alert("Erro ao listar os contatos");
      });
  },
  computed: {
    contatos() {
      return this.$store.state.contatos;
    },
  },

  methods: {
    alertaExclusao(id) {
      this.$swal
        .fire({
          // title: "Deseja realmente excluir este contato?",
          text: "Deseja realmente excluir este contato?",
          icon: "warning",
          showCancelButton: true,
          confirmButtonColor: "#3085d6",
          cancelButtonColor: "#d33",
          confirmButtonText: "Sim",
          cancelButtonText: "Não",
        })
        .then((result) => {
          if (result.isConfirmed) {
            //   Swal.fire("Deleted!", "Your file has been deleted.", "success");
            this.excluir(id);
          }
        });
    },
    excluir(id) {
      axios.delete("/contatos/" + id)
        .then((res) => {
          this.$store.commit(
            "setContatos",
            this.$store.state.contatos.filter((contato) => contato.id !== id)
          );
          this.$swal({
            icon: "success",
            title: "Perfeito",
            text: "Excluindo com sucesso",
          });
        })
        .catch((err) => {});
    },
  },
};
</script>