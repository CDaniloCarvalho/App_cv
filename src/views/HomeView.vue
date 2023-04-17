<template>
<div class="container rounded py-4 ">

    <div class="mx-2 mb-2 py-2 text-black cursor-pointer visualizar" color="grey" @click="preVisualizar()">
        <v-icon class="icons">mdi-magnify-plus-outline</v-icon> Pré-visualizar
    </div>

    <v-dialog v-model="dialog" max-width="900px" class="px-0 py-0" persistent>
        <v-card>
            <div class="mx-2 py-2 text-black cursor-pointer close d-flex justify-end" color="grey" @click="close">
                <v-icon class="icons fs-3 my-2 ">mdi-close-circle-outline </v-icon>
            </div>
            <v-row class="mx-auto py-0 text-center" v-if="dialog">
                <v-col class="mx-auto py-0">
                    <v-btn v-if="templateA || templateB" class=" mb-1 " color="success" @click="generatePDF">
                        <span v-if="!loading">
                            Gerar PDF
                        </span>
                        <span v-else>
                          <v-progress-circular  indeterminate color="secundary"></v-progress-circular>
                          Carreganco
                        </span>

                    </v-btn>
                </v-col>
                <v-col class="mx-auto py-0">
                    <v-btn class=" mb-1 " color="grey" @click="modeloA()">Modelo 1</v-btn>
                </v-col>
                <v-col class="mx-auto py-0">
                    <v-btn class=" mb-1 " color="grey" @click="modeloB()">Modelo 2</v-btn>
                </v-col>
            </v-row>

            <v-row>
                <v-col :md="11" :sm="11" class="mx-auto py-1" min-heigth="100%">
                    <div v-if="templateA || templateB" class="cv-template" ref="pdfContent">
                        <curriculoV1 v-if="templateA" :dadosCv="dadosCv">
                        </curriculoV1>
                        <curriculoV2 v-if="templateB" :dadosCv="dadosCv">
                        </curriculoV2>
                    </div>
                </v-col>
            </v-row>

        </v-card>
    </v-dialog>

    <v-row v-if="!templateA && !templateB" class="mb-4 pa-12 text-black">
        <v-col md="12" class="py-0 mb-8">
            <h2>Preencha os dados para gerar o seu cv</h2>
        </v-col>
        <v-col cols="12" :md="3" :sm="12" class="py-0">
            <v-text-field :counter="25" maxlength="25" v-model="dadosCv.nome" label="nome" variant="outlined"></v-text-field>
        </v-col>

        <v-col cols="12" md="3" :sm="12" class="py-0">
            <v-text-field :counter="25" maxlength="25" v-model="dadosCv.areaAtuacao" label="areaAtuacao" variant="outlined"></v-text-field>
        </v-col>

        <v-col cols="12" md="3" :sm="12" class="py-0">
            <v-text-field :counter="30" maxlength="30" v-model="dadosCv.contato.email" :rules="[validarEmail]" label="email" variant="outlined"></v-text-field>
        </v-col>

        <v-col cols="12" md="3" :sm="12" class="py-0">
            <v-text-field  v-model="dadosCv.contato.telefone" label="telefone" placeholder="Ex: (11) 96453-7363" :rules="[validarTelefone]" @input="formatarTelefone" variant="outlined"></v-text-field>
        </v-col>

        <v-col cols="12" md="3" :sm="12" class="py-0">
            <v-text-field :counter="30" maxlength="30" v-model="dadosCv.endereco.rua" label="rua" variant="outlined"></v-text-field>
        </v-col>

        <v-col cols="12" md="2" :sm="12" class="py-0">
            <v-text-field v-model="dadosCv.endereco.numero" maxlength="10" label="numero" :rules="[validarNumero]" variant="outlined"></v-text-field>
        </v-col>

        <v-col cols="12" md="3" :sm="12" class="py-0">
            <v-text-field v-model="dadosCv.endereco.bairro" :counter="20" maxlength="20" label="bairro" variant="outlined"></v-text-field>
        </v-col>

        <v-col cols="12" md="2" :sm="12" class="py-0">
            <v-text-field v-model="dadosCv.endereco.cidade" :counter="20" maxlength="20" label="cidade" variant="outlined"></v-text-field>
        </v-col>

        <v-col cols="12" md="2" :sm="12" class="py-0">
            <v-text-field v-model="dadosCv.endereco.estado" maxlength="2" label="estado" variant="outlined"></v-text-field>
        </v-col>

        <v-col cols="12" md="12" :sm="12" class="py-0">
            <v-textarea v-model="dadosCv.objetivo" label="objetivo" variant="outlined" rows="2" :counter="340" maxlength="340"></v-textarea>
        </v-col>

        <v-col cols="12" md="12" :sm="12" class="py-0">
            <v-text-field :counter="50" maxlength="50" rows="1" v-model="dadosCv.habilidades" label="habilidades" variant="outlined"></v-text-field>
        </v-col>
        <v-col cols="12" md="12" :sm="12" class="py-0">
            <h2>Experiências</h2>
        </v-col>
        <v-col cols="12" md="12" :sm="12" class="py-0 mt-4">
            <v-row class=" px-0" v-for="experiencia in dadosCv.experiencias" :key="experiencia.identificacao">
                <v-col cols="12" md="12" :sm="12" class="py-0 mb-3 d-flex text-left">
                    <h3>{{ experiencia.cargo ? `Empresa ${experiencia.identificacao}` : '' }}</h3>
                </v-col>
                <v-col class=" " cols="12" :md="3">
                    <v-text-field :counter="30" maxlength="30" v-model="experiencia.cargo" label="Cargo" variant="outlined"></v-text-field>
                </v-col>
                <v-col class=" " cols="12" :md="3">
                    <v-text-field :counter="30" maxlength="30" v-model="experiencia.empresa" label="Empresa" variant="outlined"></v-text-field>
                </v-col>
                <v-col class=" " cols="12" :md="3">
                    <v-text-field v-model="experiencia.entrada" label="Entrada" variant="outlined"  @input="formatarData(experiencia, 'entrada')" :rules="[validarData(experiencia.entrada)]"></v-text-field>
                </v-col>
                <v-col class=" " cols="12" :md="3">
                    <v-text-field v-model="experiencia.saida" label="Saída" variant="outlined"  @input="formatarData(experiencia, 'saida')" :rules="[validarData(experiencia.saida)]"></v-text-field>
                </v-col>
                <v-col class="mb-4" cols="12" :md="12">
                    <v-textarea :counter="200" maxlength="200" rows="2" v-model="experiencia.responsabilidades" label="Responsabilidades" variant="outlined"></v-textarea>
                </v-col>
            </v-row>
        </v-col>
        <v-col cols="12" md="12" :sm="12" class="py-0 mb-4">
            <h2>Escolaridade</h2>
        </v-col>
        <v-col cols="12" md="12" :sm="12" class="py-0">
          <v-row class="mx-auto" v-for="item in dadosCv.educacao" :key="item.identificacao">
            <v-col cols="12" md="12" :sm="12" class="py-0 mb-3 d-flex text-left">
                <h3>{{ item.escolaridade ? `Curso ${item.identificacao}` : '' }}</h3>
            </v-col>
            <v-col class="mx-auto" cols="12" :md="3">
                <v-text-field :counter="40" maxlength="40" v-model="item.escolaridade" label="Escolaridade" variant="outlined"></v-text-field>
            </v-col>
            <v-col class="mx-auto" cols="12" :md="3">
                <v-text-field :counter="40" maxlength="40" v-model="item.instituicao" label="Instituicao" variant="outlined"></v-text-field>
            </v-col>
            <v-col class="mx-auto" cols="12" :md="3">
                <v-text-field v-model="item.inicio" label="Inicio" variant="outlined" @input="formatarData(item, 'inicio')" :rules="[validarData(item.inicio)]"></v-text-field>
            </v-col>
            <v-col class="mx-auto" cols="12" :md="3">
                <v-text-field v-model="item.termino" label="Termino" variant="outlined"  @input="formatarData(item, 'termino')" :rules="[validarData(item.termino)]"></v-text-field>
            </v-col>
          </v-row>
        </v-col>
    </v-row>
</div>
</template>

<script>
import html2pdf from 'html2pdf.js';
import curriculoV1 from '@/components/cvs.vue/curriculoV1.vue';
import curriculoV2 from '@/components/cvs.vue/curriculoV2.vue';

export default {
  components: {
      curriculoV1,
      curriculoV2
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
        objetivo: "Sou um desenvolvedor apaixonado por tecnologia, com experiência em JavaScript, Vue.js, Node.js, HTML e CSS. Tenho como objetivo aplicar meu conhecimento e habilidades em projetos desafiadores, contribuindo para o desenvolvimento de soluções inovadoras e impactantes. Busco oportunidades que me permitam crescer profissionalmente e aprender",
        contato: {
            email: "dev@develop.com.br",
            telefone: "(11) 94983-7353",
        },
        endereco: {
            rua: "São joão",
            numero: "1000",
            bairro: "Centro",
            cidade: "São paulo",
            estado: "Sp"
        },
        experiencias: [{
                identificacao: 1,
                cargo: "Desenvolvedor Full Stack",
                empresa: "ABC",
                entrada: "01/12/2020",
                saida: "Presente",
                responsabilidades: "Site da empresa desenvolvido e mantido usando Vue.js e Node.js,Design responsivo implementado usando HTML/CSS, Integrado com várias APIs de terceiros para processamento de pagamentos e email marketing"
            },
            {
                identificacao: 2,
                cargo: "Desenvolvedor Júnior",
                empresa: "XYZ",
                entrada: "05/10/2018",
                saida: "12/05/2019",
                responsabilidades: "Auxiliou desenvolvedores seniores em vários projetos Ferramentas internas desenvolvidas e mantidas usando JavaScript e Node.js"
            },
        ],
        educacao: [{
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
        habilidades: "HTML, CSS, JavaScript e Node.js"
      },
    };
  },

  methods: {
    generatePDF() {
      this.loading = true
      setTimeout(() => {

        // Opções de configuração para a função html2pdf
        const options = {
            filename: `${this.dadosCv.nome}_cv.pdf`,
            html2canvas: {},
            jsPDF: {
                orientation: 'portrait'
            }
        };

        // Salva o conteúdo HTML
        const pdfContent = this.$refs.pdfContent;

        // Converter o HTML em PDF
        html2pdf()
        .set(options)
        .from(pdfContent)
        .save();

        this.dialog = false
        this.loading = false
        this.templateB = false
        this.templateA = false
      }, 1000)

    },

    preVisualizar() {
      this.dialog = true
      this.templateA = true
    },

    close() {
      this.dialog = false
      this.templateB = false
      this.templateA = false
    },

    modeloA() {
      this.templateA = true
      this.templateB = false
    },

    modeloB() {
        this.templateB = true
        this.templateA = false
    },

    validarNumero(value) {

      if (!/^\d+$/.test(value)) {
        return 'Por favor, insira apenas números';
      }

      return true;
    },

    validarTelefone(value) {
      if (!/^\d+|\(|\)|-+$/.test(value)) {
        return 'Digite um telefone válido';
      }
      
      if (value.replace(/\D+/g, '').length !== 11) {
        return 'O telefone deve ter 11 dígitos';
      }
      
      return true;
    },

    formatarTelefone() {
      let telefone = this.dadosCv.contato.telefone;
      telefone = telefone.replace(/\D+/g, '').replace(/^(\d{2})(\d{5})(\d{4}).*/, '($1) $2-$3');
      this.dadosCv.contato.telefone = telefone;
    },

    validarEmail(value) {
      if (!/^[\w-]+(\.[\w-]+)*@([\w-]+\.)+[a-zA-Z]{2,7}$/.test(value)) {
        return 'Por favor, insira um e-mail válido';
      }
      
      return true;
    },

    formatarData(item, campo) {
      item[campo] = item[campo].replace(/\D/g, '');

      if (item[campo].length > 7) {
          let dia = item[campo].substring(0, 2);
          let mes = item[campo].substring(2, 4);
          let ano = item[campo].substring(4, 8);
          item[campo] = `${dia}/${mes}/${ano}`;
      }
    },

    validarData(data) {
      if (!/^[0-9]{2}\/[0-9]{2}\/[0-9]{4}$/.test(data)) {
          return 'A data deve estar no formato dd/mm/yyyy';
      }
      return true;
    }

  },

}
</script>

<style>
@import '~vuetify/dist/vuetify.min.css';

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
    background-color: #ffffff;
    height: auto;
    margin: 0 20px;
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
