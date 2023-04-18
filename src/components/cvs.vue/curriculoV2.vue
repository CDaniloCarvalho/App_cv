<template>
  <v-row class="pa-0 cv2">
    <v-col
      class="mt-2"
      cols="5">
      <v-avatar
        size="220"
        class="d-flex justify-start ma-15">
        <v-img
          height="220px"
          cover
          src="../../assets/elon-musk-.jpg"
          alt="John"></v-img>
      </v-avatar>
    </v-col>

    <v-col
      cols="7"
      class="pa-0 mt-12 pt-15 text-end pe-15">
      <h2 class="pa-0 py-0">{{ dadosCv.nome.toUpperCase() }}</h2>
      <span class="pa-0">
        {{ dadosCv.endereco.rua }}{{ dadosCv.endereco.rua ? "," : "" }}
        {{ dadosCv.endereco.numero }}
        {{ dadosCv.endereco.rua ? "-" : "" }}
        {{ dadosCv.endereco.bairro }}
        {{ dadosCv.endereco.bairro ? "-" : "" }}
        {{ dadosCv.endereco.cidade }}
        {{ dadosCv.endereco.cidade ? "-" : "" }}
        {{ dadosCv.endereco.estado }}
        <v-icon class="icons">mdi-map-marker</v-icon>
      </span>
      <span class="pa-0 d-block">
        {{ dadosCv.contato.email }}
        <v-icon class="icons">mdi-email</v-icon>
      </span>
      <span class="pa-0 d-block">
        {{ dadosCv.contato.telefone }}
        <v-icon class="icons">mdi-phone-classic </v-icon>
      </span>
    </v-col>

    <v-col
      cols="10"
      class="pa-0 mx-2 mx-auto">
      <h3>Objetivo</h3>
      <span> {{ dadosCv.areaAtuacao }} </span>
    </v-col>

    <v-col
      cols="10"
      class="pa-0 ma-2 mx-auto">
      <h3>Objetivo</h3>
      <p>{{ dadosCv.objetivo }}</p>
    </v-col>

    <v-col
      cols="10"
      class="pa-0 ma-2 mx-auto">
      <h3>Experiências</h3>
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
      cols="10"
      class="pa-0 ma-2 mx-auto">
      <h3>Escolaridade</h3>
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
      cols="10"
      class="pa-0 ma-2 mx-auto">
      <h3>Habilidades</h3>
      <p>{{ dadosCv.habilidades }}</p>
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
    background-color: rgba(172, 171, 171, 0.521);
    min-height: 1121px;
    margin: 0 3px;
  }

  .icons {
    width: 20px;
    font-size: 20px;
  }

  .cv2 {
    background-image: url("../../assets/fundo_cv.png");
    background-size: cover;
    background-repeat: no-repeat;
    background-position: center center;
    width: 100%;
    height: 100%;
    min-height: 100vh;
    padding: 0;
    margin: 0;
  }
</style>
