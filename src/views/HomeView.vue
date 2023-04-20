<template>
  <div class="home rounded">
    <v-row>
      <v-col
        cols="12"
        :md="6"
        class="py-0 mt-12 mx-auto">
        <v-img
          height="380px"
          width="auto"
          cover
          src="../assets/digitando.jpg"
          class="foto mx-auto rounded"
          alt="foto"></v-img>
      </v-col>
      <v-col
        class="mx-auto mt-10"
        :md="6"
        cols="12">
        <h3 class="title-home">Precisa de um currículo?</h3>
        <p class="text-home mt-10 mx-10">
          Bem-vindo ao nosso sistema de gerador de currículo em PDF! Aqui você
          pode criar um currículo profissional e atraente em questão de minutos.
          Preencha nosso formulário intuitivo com suas informações pessoais,
          formação acadêmica, experiência profissional e habilidades relevantes.
          Em seguida, clique em 'gerar PDF' e obtenha instantaneamente um
          currículo personalizado e pronto para ser impresso ou compartilhado
          digitalmente.
        </p>
      </v-col>

      <v-col
        class="mx-auto mt-5"
        cols="12">
        <router-link
          to="/GeradorCv"
          class="no-color">
          <v-btn
            append-icon="mdi-arrow-right-thin-circle-outline"
            class="btn-comecar"
            height="48"
            variant="outlined">
            <span class="text-btn">COMEÇAR</span>
            <template v-slot:append>
              <v-icon
                class="text-btn"
                size="30"></v-icon>
            </template>
          </v-btn>
        </router-link>
      </v-col>
    </v-row>
  </div>
</template>

<script>
  export default {
    components: {},

    data() {
      return {};
    },

    methods: {},
  };
</script>

<style>
  .home {
    background-color: rgba(125, 130, 145, 0.591);
    padding: 20px;
    margin: 10px;
  }

  .title-home {
    font-size: 3rem;
    line-height: 1;
    font-family: Georgia, "Times New Roman", Times, serif;
    text-shadow: 3px 3px 3px black;
  }

  .text-home {
    width: auto;
    font-size: 18px;
    font-family: Georgia, "Times New Roman", Times, serif;
    text-shadow: 3px 3px 3px black;
    line-height: 2;
  }

  .btn-comecar:hover {
    background-color: rgba(156, 156, 156, 0.463);
  }

  .no-color {
    color: white !important;
  }

  .text-btn {
    text-shadow: 2px 2px 2px black;
  }
</style>
