<template>
  <div class="home">
    <v-col
      class="mx-auto mt-5"
      cols="8">
      <h2 class="mt-10 mx-8">
        Bem-vindo ao nosso sistema de gerador de currículo em PDF! Aqui você
        pode criar um currículo profissional e atraente em questão de minutos.
        Preencha nosso formulário intuitivo com suas informações pessoais,
        formação acadêmica, experiência profissional e habilidades relevantes.
        Em seguida, clique em 'gerar PDF' e obtenha instantaneamente um
        currículo personalizado e pronto para ser impresso ou compartilhado
        digitalmente.
      </h2>
    </v-col>

    <v-col
      class="mx-auto mt-5"
      cols="8">
      <router-link to="/GeradorCv"
        ><v-btn
          color="success"
          size="x-large"
          >COMEÇAR</v-btn
        >
      </router-link>
    </v-col>
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
    background-color: rgba(255, 255, 255, 0.409);
    color: black;
    margin: 10px 50px;
    height: 80vh;
  }
</style>
