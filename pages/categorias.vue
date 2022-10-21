<template>
  <v-form ref="form" v-model="valid" lazy-validation>
    <h2>Cadastrar uma categoria</h2>
    <v-text-field
      v-model="categoria.nome"
      :counter="20"
      :rules="nomeRules"
      label="Categoria"
      required
    ></v-text-field>

    <v-text-field
      v-model="categoria.descricao"
      :rules="descricaoRules"
      label="Descrição"
      required
    ></v-text-field>

    <v-btn :disabled="!valid" color="success" class="mr-4" @click="submit">
      Cadastrar
    </v-btn>

    <v-btn color="error" class="mr-4" @click="reset"> Resetar </v-btn>
    <v-data-table
      :headers="headers"
      :items="categorias"
      :items-per-page="5"
      class="elevation-1"
    ></v-data-table>
  </v-form>

</template>

<script>
export default {
  data: () => ({
    valid: true,
    categoria: {
      nome: "",
      descricao: "",
    },
    nomeRules: [
      (v) => !!v || "Nome da categoria é necessário",
      (v) => (v && v.length <= 20) || "O nome deve ter menos 20 caracteres",
    ],
    descricaoRules: [(v) => !!v || "É necessário inserir a descrição"],

    headers: [
      {
        text: "Categoria",
        align: "start",
        sortable: false,
        value: "nome",
      },
      { text: "Descrição", value: "descricao" },
    ],
    categorias: [
      {
        nome: "Categoria teste",
        descricao: "descrição da categoria",
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
          url: "https://localhost:44333/categoria",
          data: this.categoria,
        });

        const result = await this.$axios({
          method: "GET",
          url: "https://localhost:44333/categoria",
        });

        this.categoria = result.data;
      } catch {}
    },
  },
};
</script>