<template>
  <v-row class="mb-4 form text-black">
    <v-col
      md="9"
      class="d-flex align-center">
      <h2 class="mx-auto">Preencha os dados para gerar o seu cv</h2>
    </v-col>

    <v-col
      cols="3"
      class="py-0 mb-6">
      <v-avatar
        size="150"
        for="file-upload"
        class="cursor-pointer d-flex justify-start"
        @click="$refs.uploadInput.click()">
        <div class="avatar-label">Adicionar uma imagem</div>
        <v-img
          height="150px"
          cover
          :src="items.foto"
          class="foto"
          alt="foto"></v-img>
      </v-avatar>
      <input
        ref="uploadInput"
        id="file-upload"
        class="d-none file-uploads"
        @input="atualizarDados"
        @change="listar"
        type="file" />
    </v-col>

    <v-col
      cols="12"
      :md="3"
      :sm="12"
      class="py-0">
      <v-text-field
        :counter="30"
        maxlength="30"
        v-model="items.nome"
        @input="atualizarDados"
        label="nome"
        variant="outlined"></v-text-field>
    </v-col>

    <v-col
      cols="12"
      md="3"
      :sm="12"
      class="pt-0">
      <v-text-field
        :counter="25"
        maxlength="25"
        v-model="items.areaAtuacao"
        @input="atualizarDados"
        label="areaAtuacao"
        variant="outlined"></v-text-field>
    </v-col>

    <v-col
      cols="12"
      md="3"
      :sm="12"
      class="py-0">
      <v-text-field
        :counter="30"
        maxlength="30"
        v-model="items.contato.email"
        @input="atualizarDados"
        :rules="[validarEmail]"
        label="email"
        variant="outlined"></v-text-field>
    </v-col>

    <v-col
      cols="12"
      md="3"
      :sm="12"
      class="py-0">
      <v-text-field
        v-model="items.contato.telefone"
        @change="atualizarDados"
        label="telefone"
        placeholder="Ex: (11) 96453-7363"
        :rules="[validarTelefone]"
        @input="formatarTelefone"
        variant="outlined"></v-text-field>
    </v-col>

    <v-col
      cols="12"
      md="3"
      :sm="12"
      class="py-0">
      <v-text-field
        :counter="30"
        maxlength="30"
        v-model="items.endereco.rua"
        label="rua"
        @input="atualizarDados"
        variant="outlined"></v-text-field>
    </v-col>

    <v-col
      cols="12"
      md="2"
      :sm="12"
      class="py-0">
      <v-text-field
        v-model="items.endereco.numero"
        maxlength="10"
        label="numero"
        @input="atualizarDados"
        :rules="[validarNumero]"
        variant="outlined"></v-text-field>
    </v-col>

    <v-col
      cols="12"
      md="3"
      :sm="12"
      class="py-0">
      <v-text-field
        v-model="items.endereco.bairro"
        @input="atualizarDados"
        :counter="30"
        maxlength="30"
        label="bairro"
        variant="outlined"></v-text-field>
    </v-col>

    <v-col
      cols="12"
      md="2"
      :sm="12"
      class="py-0">
      <v-text-field
        v-model="items.endereco.cidade"
        @input="atualizarDados"
        :counter="30"
        maxlength="30"
        label="cidade"
        variant="outlined"></v-text-field>
    </v-col>

    <v-col
      cols="12"
      md="2"
      :sm="12"
      class="py-0">
      <v-text-field
        @input="atualizarDados"
        v-model="items.endereco.estado"
        maxlength="2"
        label="estado"
        variant="outlined"></v-text-field>
    </v-col>

    <v-col
      cols="12"
      md="12"
      :sm="12"
      class="py-0">
      <v-textarea
        v-model="items.objetivo"
        @input="atualizarDados"
        label="objetivo"
        variant="outlined"
        rows="2"
        :counter="340"
        maxlength="340"></v-textarea>
    </v-col>

    <v-col
      cols="12"
      md="12"
      :sm="12"
      class="py-0">
      <h2>Experiências</h2>
    </v-col>
    <v-col
      cols="12"
      md="12"
      :sm="12"
      class="py-0 mt-4">
      <v-row
        class="px-0"
        v-for="(experiencia, index) in items.experiencias"
        :key="experiencia.identificacao">
        <v-col
          cols="12"
          md="12"
          :sm="12"
          class="py-0 mb-3 d-flex text-left">
          <h3>{{ `Empresa ${experiencia.identificacao}` }}</h3>
        </v-col>
        <v-col
          class="py-0"
          cols="12">
          <v-checkbox
            class="py-0 d-flex justify-end ma-0"
            label="Atual"
            @change="empresaAtual(index)"></v-checkbox>
        </v-col>
        <v-col
          class="py-0"
          cols="12"
          :md="3">
          <v-text-field
            :counter="30"
            maxlength="30"
            v-model="experiencia.cargo"
            @input="atualizarDados"
            label="Cargo"
            variant="outlined"></v-text-field>
        </v-col>
        <v-col
          class="py-0"
          cols="12"
          :md="3">
          <v-text-field
            :counter="30"
            maxlength="30"
            v-model="experiencia.empresa"
            @input="atualizarDados"
            label="Empresa"
            variant="outlined"></v-text-field>
        </v-col>
        <v-col
          class="py-0"
          cols="12"
          :md="3">
          <v-text-field
            v-model="experiencia.entrada"
            @change="atualizarDados"
            label="Entrada"
            variant="outlined"
            @input="formatarData(experiencia, 'entrada')"
            :rules="[validarData(experiencia.entrada)]"></v-text-field>
        </v-col>

        <v-col
          class="py-0"
          cols="12"
          :md="3">
          <v-text-field
            v-model="experiencia.saida"
            @change="atualizarDados"
            label="Saída"
            variant="outlined"
            @input="formatarData(experiencia, 'saida')"
            :rules="[validarData(experiencia.saida)]"></v-text-field>
        </v-col>
        <v-col
          class="pt-0"
          cols="12"
          :md="12">
          <v-textarea
            :counter="200"
            maxlength="200"
            rows="2"
            v-model="experiencia.responsabilidades"
            @input="atualizarDados"
            label="Responsabilidades"
            variant="outlined"></v-textarea>
        </v-col>
      </v-row>
    </v-col>
    <v-col
      cols="12"
      md="12"
      :sm="12"
      class="py-0 mb-4">
      <h2>Escolaridade</h2>
    </v-col>
    <v-col
      cols="12"
      md="12"
      :sm="12"
      class="py-0">
      <v-row
        class="mx-auto"
        v-for="item in items.educacao"
        :key="item.identificacao">
        <v-col
          cols="12"
          md="12"
          :sm="12"
          class="py-0 mb-3 d-flex text-left">
          <h3>{{ `Curso ${item.identificacao}` }}</h3>
        </v-col>
        <v-col
          class="mx-auto"
          cols="12"
          :md="3">
          <v-text-field
            :counter="40"
            maxlength="40"
            v-model="item.escolaridade"
            @input="atualizarDados"
            label="Curso"
            variant="outlined"></v-text-field>
        </v-col>
        <v-col
          class="mx-auto"
          cols="12"
          :md="3">
          <v-text-field
            :counter="40"
            maxlength="40"
            v-model="item.instituicao"
            @input="atualizarDados"
            label="Instituicao"
            variant="outlined"></v-text-field>
        </v-col>
        <v-col
          class="mx-auto"
          cols="12"
          :md="3">
          <v-text-field
            v-model="item.inicio"
            @change="atualizarDados"
            label="Inicio"
            variant="outlined"
            @input="formatarData(item, 'inicio')"
            :rules="[validarData(item.inicio)]"></v-text-field>
        </v-col>
        <v-col
          class="mx-auto"
          cols="12"
          :md="3">
          <v-text-field
            v-model="item.termino"
            @change="atualizarDados"
            label="Termino"
            variant="outlined"
            @input="formatarData(item, 'termino')"
            :rules="[validarData(item.termino)]"></v-text-field>
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

    data() {
      return {
        items: { ...this.dadosCv },
      };
    },

    methods: {
      empresaAtual(index) {
        this.items.experiencias[index].saida = "Atual";
      },

      atualizarDados() {
        this.$emit("atualizarDados", this.items);
      },

      validarNumero(value) {
        if (!/^\d+$/.test(value)) {
          return "Por favor, insira apenas números";
        }

        return true;
      },

      validarTelefone(value) {
        if (!/^\d+|\(|\)|-+$/.test(value)) {
          return "Digite um telefone válido";
        }

        if (value.replace(/\D+/g, "").length !== 11) {
          return "O telefone deve ter 11 dígitos";
        }

        return true;
      },

      formatarTelefone() {
        let telefone = this.items.contato.telefone;
        telefone = telefone
          .replace(/\D+/g, "")
          .replace(/^(\d{2})(\d{5})(\d{4}).*/, "($1) $2-$3");
        this.items.contato.telefone = telefone;
      },

      validarEmail(value) {
        if (!/^[\w-]+(\.[\w-]+)*@([\w-]+\.)+[a-zA-Z]{2,7}$/.test(value)) {
          return "Por favor, insira um e-mail válido";
        }

        return true;
      },

      formatarData(item, campo) {
        item[campo] = item[campo].replace(/\D/g, "");

        if (item[campo].length > 7) {
          let dia = item[campo].substring(0, 2);
          let mes = item[campo].substring(2, 4);
          let ano = item[campo].substring(4, 8);
          item[campo] = `${dia}/${mes}/${ano}`;
        }
      },

      validarData(data) {
        if (!/^[0-9]{2}\/[0-9]{2}\/[0-9]{4}$/.test(data)) {
          return;
        }
        return true;
      },

      listar(event) {
        const files = event.target.files;
        const file = files[0];
        const reader = new FileReader();
        reader.onload = () => {
          const base64 = reader.result;
          //this.items = this.items || {};
          this.items.foto = base64;
        };
        reader.readAsDataURL(file);
      },
    },
  };
</script>

<style>
  .form {
    padding: 0 50px;

    @media (max-width: 600px) {
      padding: 0 15px;
    }
  }

  .foto:hover {
    background-color: rgba(78, 78, 78, 0.26);
  }

  .foto {
    background-color: rgba(78, 78, 78, 0.132);
  }

  .avatar-label {
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    text-align: center;
    font-size: 1.2rem;
  }
</style>
