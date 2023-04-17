<template>
  <div class="container rounded py-4">
    <div
      class="mx-2 mb-2 py-2 text-black cursor-pointer visualizar"
      color="grey"
      @click="preVisualizar()">
      <v-icon class="icons">mdi-magnify-plus-outline</v-icon> Pré-visualizar
    </div>

    <v-dialog
      v-model="dialog"
      max-width="900px"
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
    <FormCv @atualizarDados="atualizarDados" />
  </div>
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
          nome: "",
          areaAtuacao: "",
          objetivo: "",
          contato: {
            email: "",
            telefone: "",
          },
          endereco: {
            rua: "",
            numero: "",
            bairro: "",
            cidade: "",
            estado: "",
          },
          experiencias: [
            {
              identificacao: 1,
              cargo: "",
              empresa: "",
              entrada: "",
              saida: "",
              responsabilidades: "",
            },
            {
              identificacao: 2,
              cargo: "",
              empresa: "",
              entrada: "",
              saida: "",
              responsabilidades: "",
            },
          ],
          educacao: [
            {
              identificacao: 1,
              escolaridade: "",
              instituicao: "",
              inicio: "",
              termino: "",
              descrição: "",
            },
            {
              identificacao: 2,
              escolaridade: "",
              instituicao: "",
              inicio: "",
              termino: "",
              descrição: "",
            },
          ],
          habilidades: "",
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
          // Opções de configuração para a função html2pdf
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
          html2pdf().set(options).from(pdfContent).save();

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
        this.templateB = false;
        this.templateA = false;
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
    padding: 11px 6px;
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

  .visualizar:hover {
    font-size: 18px;
    color: rgb(113, 2, 2);
  }

  .close {
    font-size: 30px;
  }

  .icons:hover {
    color: red;
  }

  @media (max-width: 400px) {
    .container {
      overflow: auto;
    }
  }
</style>
