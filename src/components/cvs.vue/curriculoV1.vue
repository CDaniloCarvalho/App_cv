<template>
  <v-row class="cv1">
    <v-col
      cols="2"
      class="pa-0 lista my-0"
      min-heigth="100%">
    </v-col>
    <v-col
      cols="9"
      class="text-left">
      <v-row class="ma-2">
        <v-col
          cols="12"
          class="pa-0 ma-2">
          <h1 class="pa-0 py-0 text-decoration-underline">
            {{ dadosCv.nome.toUpperCase() }}
          </h1>
          <span
            ><strong> {{ dadosCv.areaAtuacao.toUpperCase() }} </strong></span
          >
        </v-col>

        <v-col cols="12">
          <h3 class="pa-0 text-decoration-underline">Contato</h3>
          <span class="pa-0 d-block">
            <v-icon class="icons">mdi-email</v-icon>
            <strong> E-mail: </strong>{{ dadosCv.contato.email }}
          </span>
          <span class="pa-0 d-block">
            <v-icon class="icons">mdi-phone-classic </v-icon>
            <strong> Telefone: </strong> {{ dadosCv.contato.telefone }}
          </span>
          <span class="pa-0">
            <v-icon class="icons">mdi-map-marker</v-icon>
            <strong> Endereço:</strong>
            {{ dadosCv.endereco.rua }}{{ dadosCv.endereco.rua ? "," : "" }}
            {{ dadosCv.endereco.numero }}
            {{ dadosCv.endereco.rua ? "-" : "" }}
            {{ dadosCv.endereco.bairro }}
            {{ dadosCv.endereco.bairro ? "-" : "" }}
            {{ dadosCv.endereco.cidade }}
            {{ dadosCv.endereco.cidade ? "-" : "" }}
            {{ dadosCv.endereco.estado }}
          </span>
        </v-col>

        <v-col cols="12">
          <h3 class="text-decoration-underline">Objetivo</h3>
          <p>{{ dadosCv.objetivo }}</p>
        </v-col>

        <v-col cols="12">
          <h3 class="text-decoration-underline">Experiências</h3>
          <template
            v-for="item in dadosCv.experiencias"
            :key="item">
            <p><strong> Empresa: </strong>{{ item.empresa }}</p>
            <p><strong>Cargo:</strong> {{ item.cargo }}</p>
            <p>
              <strong>Período:</strong> {{ item.entrada }}
              {{ item.entrada ? "até" : "" }} {{ item.saida }}
            </p>
            <p class="mb-3">
              <strong>Responsabilidades:</strong> {{ item.responsabilidades }}
            </p>
          </template>
        </v-col>

        <v-col
          cols="12"
          class="py-0">
          <h3 class="text-decoration-underline">Escolaridade</h3>
          <template
            v-for="item in dadosCv.educacao"
            :key="item">
            <p><strong>Curso:</strong> {{ item.escolaridade }}</p>
            <p><strong>Instituicao</strong> {{ item.instituicao }}</p>
            <p class="mb-3">
              <strong>Período:</strong> {{ item.inicio }}
              {{ item.inicio ? "até" : "" }} {{ item.termino }}
            </p>
          </template>
        </v-col>

        <v-col
          cols="12"
          class="py-0 mt-2">
          <h3 class="text-decoration-underline">Habilidades</h3>
          <p>{{ dadosCv.habilidades }}</p>
        </v-col>
      </v-row>
    </v-col>
  </v-row>
</template>

<script>
  export default {
    props: {
      dadosCv: Object,
    },
  };
</script>

<style scoped lang="scss">
  @import "~vuetify/dist/vuetify.min.css";

  .lista {
    margin: 0 3px;
  }

  .cv1 {
    background-image: url("../../assets/fundo_cv1.png");
    background-size: cover;
    background-repeat: no-repeat;
    background-position: center center;
    width: 100%;
    height: 100%;
    min-height: 100vh;
    padding: 0;
    margin: 0;
    min-height: 1120px;
  }

  .icons {
    width: 20px;
    font-size: 20px;
  }
</style>
