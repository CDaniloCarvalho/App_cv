<template>
  <div class="container rounded py-5 px-5">

    <v-dialog
      v-model="dialog"
      max-width="900px"
      class="px-0 py-0"
    >
      <template v-slot:activator="{ props }">
        <div v-bind="props" class="mx-2 py-2 text-black cursor-pointer visualizar" color="grey" @click="modeloA()"> 
          <v-icon class="icons">mdi-magnify-plus-outline</v-icon> Pré-visualizar
        </div>
      </template>

      <v-card>
        <div v-bind="props" class="mx-2 py-2 text-black cursor-pointer close d-flex justify-end" color="grey" @click="close"> 
          <v-icon class="icons fs-3">mdi-close-circle-outline </v-icon>
        </div>
        <v-row class="my-4 mx-auto py-0 text-center" v-if="dialog">
          <v-col class="mx-auto py-0">
            <v-btn  v-if="templateA || templateB" class=" mb-1 " color="success" @click="generatePDF">Gerar PDF</v-btn>
          </v-col>
          <v-col class="mx-auto py-0">
            <v-btn class=" mb-1 " color="grey" @click="modeloA()">Modelo 1</v-btn>
          </v-col>
          <v-col class="mx-auto py-0">
            <v-btn class=" mb-1 " color="grey" @click="modeloB()">Modelo 2</v-btn>
          </v-col>
        </v-row>

        <v-row >
          <v-col :md="11" :sm="11" class="mx-auto py-4" min-heigth="100%">
            <div  v-if="templateA || templateB" class="cv-template" ref="pdfContent">
              <curriculoV1
                v-if="templateA"
              :dadosCv="dadosCv"
              >
              </curriculoV1>
            </div>
          </v-col>
        </v-row>

      </v-card>
    </v-dialog>

    <v-row v-if="!templateA && !templateB" class="mb-4 pa-12 text-black">
      <v-col cols="12" class="py-0 mb-8"><h2>Preencha os dados para gerar o seu cv</h2></v-col>
      <v-col cols="3" class="py-0">
        <v-text-field
          v-model="dadosCv.nome"
          label="nome"
          variant="outlined"
        ></v-text-field>
      </v-col>

      <v-col cols="3" class="py-0">
        <v-text-field
          v-model="dadosCv.areaAtuacao"
          label="areaAtuacao"
          variant="outlined"
        ></v-text-field>
      </v-col>

      <v-col cols="3" class="py-0">
        <v-text-field
          v-model="dadosCv.contato.email"
          label="email"
          variant="outlined"
        ></v-text-field>
      </v-col>

      <v-col cols="3" class="py-0">
        <v-text-field
          v-model="dadosCv.contato.telefone"
          label="telefone"
          variant="outlined"
        ></v-text-field>
      </v-col>

      <v-col cols="3" class="py-0">
        <v-text-field
          v-model="dadosCv.endereco.rua"
          label="rua"
          variant="outlined"
        ></v-text-field>
      </v-col>

      <v-col cols="2" class="py-0" >
        <v-text-field
          v-model="dadosCv.endereco.numero"
          label="numero"
          variant="outlined"
        ></v-text-field>
      </v-col>

      <v-col cols="3" class="py-0">
        <v-text-field
          v-model="dadosCv.endereco.bairro"
          label="bairro"
          variant="outlined"
        ></v-text-field>
      </v-col>

      <v-col cols="2" class="py-0">
        <v-text-field
          v-model="dadosCv.endereco.cidade"
          label="cidade"
          variant="outlined"
        ></v-text-field>
      </v-col>

      <v-col cols="2" class="py-0">
        <v-text-field
          v-model="dadosCv.endereco.estado"
          label="estado"
          variant="outlined"
        ></v-text-field>
      </v-col>

      <v-col cols="12" class="py-0">
        <v-textarea 
          v-model="dadosCv.objetivo"
          label="objetivo"
          variant="outlined"
          rows="3"
          hint="Max 350 caracteres"
          :counter="350"
          maxlength="350"
          ></v-textarea>
      </v-col>

      <v-col cols="12" class="py-0">
        <v-text-field
          :counter="60"
          maxlength="60"
          rows="1"
          v-model="dadosCv.habilidades"
          label="habilidades"
          variant="outlined"
        ></v-text-field>
      </v-col>
      <v-col cols="12" class="py-0"><h2>Experiências</h2></v-col>
      <v-col cols="12" class="py-0 mt-4">
        <v-row class="mx-auto px-0" v-for="experiencia in dadosCv.experiencias" :key="experiencia.identificacao">
          <v-col class="mx-auto " cols="12">
            <v-text-field v-model="experiencia.cargo" label="Cargo" variant="outlined"></v-text-field>
            <v-text-field v-model="experiencia.empresa" label="Empresa" variant="outlined"></v-text-field>
            <v-text-field v-model="experiencia.entrada" label="Entrada" variant="outlined"></v-text-field>
            <v-text-field v-model="experiencia.saida" label="Saída" variant="outlined"></v-text-field>
            <v-text-field v-model="experiencia.responsabilidades" label="Responsabilidades" variant="outlined"></v-text-field>
          </v-col>
        </v-row>
      </v-col>
      <v-col cols="12" class="py-0 mb-8"><h2>Escolaridade</h2></v-col>
      <v-col cols="12" class="py-0">
        <v-row class="mx-auto">
          <v-col class="mx-auto" cols="6" v-for="item in dadosCv.educacao" :key="item.identificacao">
            <v-text-field v-model="item.escolaridade" label="escolaridade" variant="outlined"></v-text-field>
            <v-text-field v-model="item.instituicao" label="instituicao" variant="outlined"></v-text-field>
            <v-text-field v-model="item.inicio" label="inicio" variant="outlined"></v-text-field>
            <v-text-field v-model="item.termino" label="termino" variant="outlined"></v-text-field>
          </v-col>
        </v-row>
      </v-col>
    </v-row>
  </div>
</template>

<script>
import html2pdf from 'html2pdf.js';
import curriculoV1 from '@/components/cvs.vue/curriculoV1.vue';

export default {
  components: { 
    curriculoV1 
  },

  data() {
    return {
      dialog: false,
      templateA: false,
      templateB: false,
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
            inicio: "Set 2018",
            termino: "maio de 2020",
            descrição: "Graduado com distinção",
          },
          {
            identificacao: 1,
            escolaridade: "Bacharel em Ciência da Computação",
            instituicao: "Universidade de XYZ",
            inicio: "Set 2014",
            termino: "maio de 2017",
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

      this.templateB = false
      this.templateA = false
    },

    close(){
      this.templateB = false
      this.templateA = false
      this.dialog = false
    },

    modeloA(){
      this.templateA =  true
      this.templateB = false
    },

    modeloB(){
      this.templateB = true
      this.templateA = false
    }
  },

}
</script>

<style>
@import '~vuetify/dist/vuetify.min.css';

  .cv-template {
    background-color: white;
    padding: 12px;
    color: black;
    font-family: Arial, Helvetica, sans-serif;
    margin: auto;
    border: 1px solid #ddd;
    overflow: auto;
  }

  .container{
    background-color: #ffffff;
    height: auto;
    margin:0 50px;
    min-height: 90vh;
  }

  .cursor-pointer{
    cursor: pointer;
  }
  
  .visualizar:hover{
    font-size: 18px;
    color:rgb(113, 2, 2) ;
  }

  .close{
    font-size: 30px;
  }

  .icons:hover{
    color: red;
  }

  @media (max-width: 400px) {
    .container{
      overflow: auto;
    }
  }
    
</style>
