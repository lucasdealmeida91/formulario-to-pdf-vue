<template>
  <div id="app">
    <form @submit.prevent="generatePDF">
      <label for="nome">Nome:</label>
      <input type="text" id="nome" v-model="nome" required />
      
      <label for="email">Email:</label>
      <input type="email" id="email" v-model="email" required />

      <button type="submit">Gerar PDF</button>
    </form>
  </div>
</template>

<script>
import jsPDF from "jspdf";
import logoImg from "@/assets/logo.png"; // Importe sua imagem aqui

export default {
  data() {
    return {
      nome: "", // Valor padrão
      email: "", // Valor padrão
    };
  },
  methods: {
    async generatePDF() {
      const pdf = new jsPDF();

      // Carrega a imagem
      const imgBlob = await this.getBlobFromImage(logoImg);

      // Cria uma URL do Blob da imagem
      const imgBlobUrl = URL.createObjectURL(imgBlob);

      // Insere a imagem no PDF
      pdf.addImage(imgBlobUrl, "PNG", 10, 10, 40, 40); // Posição e tamanho da imagem

      // Insere o texto do nome e email
      const texto = `Nome: ${this.nome}\nEmail: ${this.email}`;
      pdf.text(texto, 10, 60);

      // Salva o PDF em um Blob
      const pdfBlob = pdf.output("blob");

      // Cria uma URL do Blob e abre em uma nova guia
      const pdfUrl = URL.createObjectURL(pdfBlob);
      window.open(pdfUrl, "_blank");
    },

    getBlobFromImage(image) {
      return new Promise((resolve) => {
        fetch(image)
          .then((response) => response.blob())
          .then((blob) => resolve(blob));
      });
    },
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
