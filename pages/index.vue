<template>
  <v-container class="d-flex justify-center align-center" style="height: 100vh; background-color: #121212;">
    <v-card class="pa-6" max-width="400px" color="#2C2C2C" text-color="white" elevation="12">
      <v-card-title class="justify-center">
        <h1 class="display-1" style="color: #8A2BE2;">Baixar Áudio/Vídeo</h1>
      </v-card-title>

      <v-text-field
        label="Insira o link do YouTube"
        v-model="url"
        outlined
        dense
        class="mb-4"
        style="background-color: #333;"
        :rules="[urlRule]"
      />

      <v-checkbox
        label="Baixar vídeo"
        v-model="isVideo"
        color="deep-purple"
        class="mb-4"
      />

      <v-btn @click="download" color="deep-purple" block>
        Baixar
      </v-btn>

      <v-alert v-if="message" type="info" class="mt-4">
        {{ message }}
      </v-alert>

      <v-alert v-if="result" type="success" class="mt-4">
        Base64 recebido com sucesso! <br />
        <span style="color: #bbb;">Clique abaixo para copiar ou salvar o arquivo:</span>
        <pre class="result-content">{{ result }}</pre>
      </v-alert>
    </v-card>
  </v-container>
</template>

<script>
export default {
  data() {
    return {
      url: '',
      isVideo: false,
      message: '',
      result: '',
    };
  },
  computed: {
    urlRule() {
      return this.url ? null : 'Por favor, insira um link válido!';
    }
  },
  methods: {
    async download() {
      if (!this.url) {
        this.message = 'Por favor, insira um link válido!';
        this.result = '';
        return;
      }

      this.message = 'Processando...';

      try {
        const response = await this.$fetch(
          `http://localhost:3000/download?url=${encodeURIComponent(this.url)}&video=${this.isVideo}`
        );

        const data = await response.json();

        if (data.success) {
          this.result = data.base64;
          this.message = 'Clique no link abaixo para copiar ou salvar o arquivo.';
        } else {
          this.result = '';
          this.message = 'Falha ao processar o download.';
        }
      } catch (error) {
        console.error('Erro ao baixar:', error);
        this.message = 'Erro ao baixar o conteúdo!';
        this.result = '';
      }
    }
  }
};
</script>

<style scoped>
.result-content {
  white-space: pre-wrap;
  word-wrap: break-word;
  max-height: 200px;
  overflow-y: auto;
  background-color: #333;
  color: #fff;
  padding: 10px;
  border-radius: 5px;
}
</style>
