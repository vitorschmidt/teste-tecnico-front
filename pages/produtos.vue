<template>
  <v-form ref="form" v-model="valid" lazy-validation>
    <h2>Cadastrar um produto</h2>
    <v-text-field
      v-model="produto.nome"
      :counter="20"
      :rules="nomeRules"
      label="Produto"
      required
    ></v-text-field>

    <v-text-field
      v-model="produto.descricao"
      :rules="descricaoRules"
      label="Descrição"
      required
    ></v-text-field>

        <v-text-field
      v-model="produto.categoria_id"
      :rules="idRules"
      label="Id categoria"
      required
    ></v-text-field>

    <v-btn :disabled="!valid" color="success" class="mr-4" @click="submit">
      Cadastrar
    </v-btn>

    <v-btn color="error" class="mr-4" @click="reset"> Resetar </v-btn>
    <v-data-table
      :headers="headers"
      :items="produto"
      :items-per-page="5"
      class="elevation-1"
    ></v-data-table>
  </v-form>

</template>

<script>
export default {
  data: () => ({
    valid: true,
    produto: {
      nome: "",
      descricao: "",
      categoria_id: "",
    },
    nomeRules: [
      (v) => !!v || "Nome do produto é necessário",
      (v) => (v && v.length <= 20) || "O nome deve ter menos 20 caracteres",
    ],
    descricaoRules: [(v) => !!v || "É necessário inserir a descrição"],
    idRules:[(v) => !!v || "É necessário inserir o id"],

    headers: [
      {
        text: "Produto",
        align: "start",
        sortable: false,
        value: "nome",
      },
      { text: "Descrição", value: "descricao" },
      { text: "Id da categoria", value: "categoria_id" },

    ],
    produto: [
      {
        nome: "Produto teste",
        descricao: "descrição do produto",
        categoria_id: 1,
      },
    ],
  }),

  methods: {
    reset() {
      this.$refs.form.reset();
    },
    async submit() {
      try {
        const response = await this.$axios({
          method: "POST",
          url: "https://localhost:44333/produto",
          data: this.produto,
        });

        const result = await this.$axios({
          method: "GET",
          url: "https://localhost:44333/produto",
        });

        this.produto = result.data;
      } catch {}
    },
  },
};
</script>