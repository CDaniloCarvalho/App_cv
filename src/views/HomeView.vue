<template>
  <div class="container">
    <div class="mb-3">
      <v-btn color="success" @click="generatePDF">Gerar PDF</v-btn>
    </div>
    <v-row>

      <v-col :md="8" :sm="11" class="mx-auto" min-heigth="100%">
        <div class="cv-template" ref="pdfContent">
          <curriculoV1
          :dadosCv="dadosCv"
          >
          </curriculoV1>
        </div>
      </v-col>
    </v-row>
  </div>
</template>


<script>
import html2pdf from 'html2pdf.js';
import curriculoV1 from '@/components/cvs.vue/curriculoV1.vue';

export default {
  components: { curriculoV1 },

  data() {
    return {
      dadosCv: {
        nome: "Leonardo	Cavalcanti",
        areaAtuacao: "Desenvolvedor",
        objetivo: "Sou um desenvolvedor apaixonado por tecnologia, com experiência em JavaScript, Vue.js, Node.js, HTML e CSS. Tenho como objetivo aplicar meu conhecimento e habilidades em projetos desafiadores, contribuindo para o desenvolvimento de soluções inovadoras e impactantes. Busco oportunidades que me permitam crescer profissionalmente e aprender continuamente.",
        contato:{
          email:"dev@develop.com.br",
          telefone: "(11) 94983-7353",
        },
        endereco:{
          rua: "São joão",
          numero: "1000",
          bairro: "Centro",
          cidade: "São paulo",
          estado: "Sp"
        },
        experiencias: [
          {
            identificacao: 1,
            cargo: "Desenvolvedor Full Stack",
            empresa: "ABC",
            entrada: "janeiro de 2020",
            saida: "Presente",
            responsabilidades: "Site da empresa desenvolvido e mantido usando Vue.js e Node.js,Design responsivo implementado usando HTML/CSS, Integrado com várias APIs de terceiros para processamento de pagamentos e email marketing"
          },
          {
            identificacao: 2,
            cargo: "Desenvolvedor Júnior",
            empresa: "XYZ",
            entrada: "maio de 2018",
            saida: "dezembro de 2019",
            responsabilidades: "Auxiliou desenvolvedores seniores em vários projetos Ferramentas internas desenvolvidas e mantidas usando JavaScript e Node.js"
          },
        ],
        educacao: [
          {
            identificacao: 1,
            escolaridade: "Bacharel em Ciência da Computação",
            instituicao: "Universidade de XYZ",
            inicio: "Set 2014",
            termino: "maio de 2018",
            descrição: "Graduado com distinção",
          },
        ],
        habilidades:"HTML, CSS, JavaScript e Node.js"
      }
    };
  },

  methods: {
    generatePDF() {
      // Opções de configuração para a função html2pdf
      const options = {
        filename: `${this.dadosCv.nome}_cv.pdf`,
        html2canvas: {},
        jsPDF: { orientation: 'portrait' }
      };

      // Salva o conteúdo HTML
      const pdfContent = this.$refs.pdfContent;

      // Converter o HTML em PDF
      html2pdf()
        .set(options)
        .from(pdfContent)
        .save();
    }
  }
}
</script>


<style>

  .cv-template {
    background-color: white;
    padding: 12px;
    color: black;
    font-family: Arial, Helvetica, sans-serif;
    margin: auto;
    border: 1px solid #ddd;
    overflow: auto;
  }

  @media (max-width: 400px) {
    .container{
      overflow: auto;
    }
  }
  
  
</style>
<!-- .container{
  padding: 20px 300px;
} -->