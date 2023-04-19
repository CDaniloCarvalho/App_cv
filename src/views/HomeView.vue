<template>
  <div
    v-if="!templateA && !templateB"
    class="container rounded py-4">
    <div
      class="mx-5 text-left cursor-pointer visualizar"
      color="grey"
      @click="preVisualizar()">
      <v-icon class="icons">mdi-magnify-plus-outline</v-icon> Pré-visualizar
    </div>

    <FormCv
      v-if="!templateA && !templateB"
      @atualizarDados="atualizarDados"
      :dadosCv="dadosCv" />
  </div>
  <v-dialog
    v-model="dialog"
    max-width="95%"
    width="800px"
    class="px-0 py-0"
    persistent>
    <v-card>
      <div
        class="mx-2 py-2 text-black cursor-pointer close d-flex justify-end"
        color="grey"
        @click="close">
        <v-icon class="icons fs-3 my-2">mdi-close-circle-outline </v-icon>
      </div>
      <v-row
        class="mx-auto py-0 text-center"
        v-if="dialog">
        <v-col class="mx-auto py-0">
          <v-btn
            v-if="templateA || templateB"
            class="mb-1"
            color="success"
            @click="generatePDF">
            <span v-if="!loading"> Gerar PDF </span>
            <span v-else>
              <v-progress-circular
                indeterminate
                color="secundary"></v-progress-circular>
              Carreganco
            </span>
          </v-btn>
        </v-col>
        <v-col class="mx-auto py-0">
          <v-btn
            class="mb-1"
            color="grey"
            @click="modeloA()"
            >Modelo 1</v-btn
          >
        </v-col>
        <v-col class="mx-auto py-0">
          <v-btn
            class="mb-1"
            color="grey"
            @click="modeloB()"
            >Modelo 2</v-btn
          >
        </v-col>
      </v-row>

      <v-row>
        <v-col
          :md="11"
          :sm="11"
          class="mx-auto py-1"
          min-heigth="100%">
          <div
            v-if="templateA || templateB"
            class="cv-template"
            ref="pdfContent">
            <curriculoV1
              v-if="templateA"
              :dadosCv="dadosCv">
            </curriculoV1>
            <curriculoV2
              v-if="templateB"
              :dadosCv="dadosCv">
            </curriculoV2>
          </div>
        </v-col>
      </v-row>
    </v-card>
  </v-dialog>
</template>

<script>
  import html2pdf from "html2pdf.js";
  import curriculoV1 from "@/components/cvs.vue/curriculoV1.vue";
  import curriculoV2 from "@/components/cvs.vue/curriculoV2.vue";
  import FormCv from "@/components/form/FormCv.vue";

  export default {
    components: {
      curriculoV1,
      curriculoV2,
      FormCv,
    },

    data() {
      return {
        loading: false,
        dialog: false,
        templateA: false,
        templateB: false,
        dadosCv: {
          nome: "Leonardo	Cavalcanti",
          areaAtuacao: "Desenvolvedor",
          objetivo:
            "Sou um desenvolvedor apaixonado por tecnologia, com experiência em JavaScript, Vue.js, Node.js, HTML e CSS. Tenho como objetivo aplicar meu conhecimento e habilidades em projetos desafiadores, contribuindo para o desenvolvimento de soluções inovadoras e impactantes. Busco oportunidades que me permitam crescer profissionalmente e aprender",
          contato: {
            email: "dev@develop.com.br",
            telefone: "(11) 94983-7353",
          },
          endereco: {
            rua: "São joão",
            numero: "1000",
            bairro: "Centro",
            cidade: "São paulo",
            estado: "Sp",
          },
          experiencias: [
            {
              identificacao: 1,
              cargo: "Desenvolvedor Full Stack",
              empresa: "ABC",
              entrada: "01/12/2020",
              saida: "Presente",
              responsabilidades:
                "Site da empresa desenvolvido e mantido usando Vue.js e Node.js,Design responsivo implementado usando HTML/CSS, Integrado com várias APIs de terceiros para processamento de pagamentos e email marketing",
            },
            {
              identificacao: 2,
              cargo: "Desenvolvedor Júnior",
              empresa: "XYZ",
              entrada: "05/10/2018",
              saida: "12/05/2019",
              responsabilidades:
                "Auxiliou desenvolvedores seniores em vários projetos Ferramentas internas desenvolvidas e mantidas usando JavaScript e Node.js",
            },
          ],
          educacao: [
            {
              identificacao: 1,
              escolaridade: "Bacharel em Ciência da Computação",
              instituicao: "Universidade de XYZ",
              inicio: "05/10/2015",
              termino: "02/01/2019",
              descrição: "Graduado com distinção",
            },
            {
              identificacao: 2,
              escolaridade: "Bacharel em Ciência da Computação",
              instituicao: "Universidade de XYZ",
              inicio: "10/10/2010",
              termino: "15/08/2014",
              descrição: "Graduado com distinção",
            },
          ],
          habilidades: "HTML, CSS, JavaScript e Node.js",
          foto: "",
        },
      };
    },

    methods: {
      atualizarDados(item) {
        this.dadosCv = item;
      },

      generatePDF() {
        this.loading = true;
        setTimeout(() => {
          const options = {
            filename: `${this.dadosCv.nome}_cv.pdf`,
            html2canvas: {},
            jsPDF: {
              orientation: "portrait",
            },
          };

          // Salva o conteúdo HTML
          const pdfContent = this.$refs.pdfContent;

          // Converter o HTML em PDF
          html2pdf()
            .set({ async: true, ...options })
            .from(pdfContent)
            .save();

          this.dialog = false;
          this.loading = false;
          this.templateB = false;
          this.templateA = false;
        }, 1000);
      },

      preVisualizar() {
        this.dialog = true;
        this.templateA = true;
      },

      close() {
        this.dialog = false;
        setTimeout(() => {
          this.templateB = false;
          this.templateA = false;
        }, 200);
      },

      modeloA() {
        this.templateA = true;
        this.templateB = false;
      },

      modeloB() {
        this.templateB = true;
        this.templateA = false;
      },
    },
  };
</script>

<style>
  @import "~vuetify/dist/vuetify.min.css";

  .cv-template {
    background-color: white;
    color: black;
    font-family: Arial, Helvetica, sans-serif;
    margin: auto;
    border: 1px solid #ddd;
    height: 100%;
  }

  .container {
    background-color: white;
    height: auto;
    margin: 0 100px;
    min-height: 90vh;
  }

  .cursor-pointer {
    cursor: pointer;
  }

  .visualizar {
    color: black;
    width: 140px;
  }

  .visualizar:hover {
    color: rgb(96, 101, 130);
    background-color: rgba(96, 101, 130, 0.116);
    font-size: 17px;
    border-radius: 5px;
  }

  .close {
    font-size: 30px;
  }

  @media (max-width: 600px) {
    .container {
      overflow: auto;
      margin: 0 10px;
    }
  }
</style>
