<template>
  <div
    v-if="!templateA && !templateB"
    class="container rounded py-4">
    <div
      class="mx-5 text-left cursor-pointer"
      color="grey"
      @click="preVisualizar()">
      <v-btn
        prepend-icon="mdi-magnify-plus-outline"
        class="btn-comecar text-black mx-5 text-left cursor-pointer"
        height="48"
        variant="outlined"
        @click="preVisualizar()">
        <template v-slot:prepend>
          <v-icon size="30"></v-icon>
        </template>
        <span>Pré-visualizar</span>
      </v-btn>
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
          areaAtuacao: "Auxiliar Administrativo",
          objetivo:
            "Meu principal objetivo é uma oportunidade, onde possa mostrar minhas habilidades, aplicar tudo que já aprendi e continuar evoluindo profissionalmente.",
          contato: {
            email: "meu_email.adm@adm.com.br",
            telefone: "(11) 94983-7353",
          },
          endereco: {
            rua: "São joão",
            numero: "10000",
            bairro: "Centro",
            cidade: "São paulo",
            estado: "Sp",
          },
          experiencias: [
            {
              identificacao: 1,
              cargo: "Auxiliar Financeiro / Administrativo",
              empresa: "ABC",
              entrada: "01/12/2020",
              saida: "20/10/2022",
              responsabilidades:
                " Responsável pelo controle de receitas e despesas e pelo gerenciamento das tarefas de rotina essenciais ao funcionamento de qualquer organização, como emissão de documentos, atualização de cadastro e atendimento a fornecedores e clientes",
            },
            {
              identificacao: 2,
              cargo: "Assistente Administrativo",
              empresa: "XYZ",
              entrada: "05/10/2018",
              saida: "12/05/2019",
              responsabilidades:
                "Recebimento e envio de documentos, controle de contas, elaboração de relatórios, emissão de notas fiscais e atualização de arquivos e cadastros",
            },
          ],
          educacao: [
            {
              identificacao: 1,
              escolaridade: "Graduado em Administração",
              instituicao: "Universidade de São paulo",
              inicio: "05/02/2015",
              termino: "30/11/2019",
              descrição:
                "Abordagem de temas como: planejamento e organizar e gerenciar o uso dos recursos pessoais e financeiros de uma organização, elaborar estratégias para melhorar o desempenho da empresa, maximizar os lucros, evitar desperdícios e reduzir custos",
            },
            {
              identificacao: 2,
              escolaridade: "Técnico Administrativo",
              instituicao: "Escola Técnica",
              inicio: "10/08/2010",
              termino: "30/06/2012",
              descrição:
                "Realizei atividades de apoio administrativo, organização de arquivos, elaboração de documentos e planilhas.",
            },
          ],
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
