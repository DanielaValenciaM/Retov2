<template>
  <div class="container">
    <h1 class="header">Subir Lista de Contactos</h1>
    <div class="upload-section">
      <label for="file-input" class="file-label">Seleccionar archivo CSV</label>
      <input id="file-input" class="file-input" type="file" accept=".csv" @change="handleFileChange" />
    </div>
    <div v-if="uploadedData.length > 0" class="data-section">
      <h2 class="data-header">Datos Subidos</h2>
      <div class="data-list">
        <div v-for="(contact, index) in uploadedData" :key="index" class="data-item">
          <p class="data-name">{{ contact.name }}</p>
          <p class="data-phone">{{ contact.phone }}</p>
          <p class="data-email">{{ contact.email }}</p>
        </div>
      </div>
      <button @click="uploadCSV">Enviar CSV</button> 
      <button class="btn btn-primary" @click="downloadCSV">Descargar CSV</button>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      uploadedData: [],
      csvFile: null,
      jsonData: null,
      url: 'https://8j5baasof2.execute-api.us-west-2.amazonaws.com/production/tests/trucode/items',
    };
  },
  methods: {
    async handleFileChange(event) {
      const file = event.target.files[0];
      if (file) {
        const csvData = await this.readFileAsync(file);
        this.uploadedData = this.processCSV(csvData);
      }
    },
    async readFileAsync(file) {
      return new Promise((resolve, reject) => {
        const reader = new FileReader();
        reader.onload = (event) => {
          resolve(event.target.result);
        };
        reader.onerror = (error) => {
          reject(error);
        };
        reader.readAsText(file);
      });
    },
    processCSV(csvData) {
      const lines = csvData.split("\n");
      const result = [];
      const headers = lines[0].split(",");
      for (let i = 1; i < lines.length; i++) {
        const obj = {};
        const currentline = lines[i].split(",");
        for (let j = 0; j < headers.length; j++) {
          obj[headers[j]] = currentline[j];
        }
        result.push(obj);
      }
      console.log(result);
      return result;
    },
    async uploadCSV() {
      try {
        for (const contact of this.uploadedData) {
          const dataToSend = {
            name: contact.name,
            phone: contact.phone,
            email: contact.email,
          };
  
          const response = await fetch(this.url, {
            method: 'POST',
            headers: {
              'Content-Type': 'application/json',
            },
            body: JSON.stringify(dataToSend),
          });
  
          if (response.ok) {
            console.log('Solicitud POST exitosa para:', contact.name);
          } else {
            console.error('Error en la solicitud POST para:', contact.name);
          }
        }
      } catch (error) {
        console.error('Error en la solicitud POST', error);
      }
    },
    async downloadCSV() {
      const response = await fetch(this.url);
      const data = await response.json();
  
      if (!data || data.length === 0) {
        console.error('No se recibieron datos válidos del servidor.');
        return;
      }
  
      const csv = this.convertToCSV(data);
      const blob = new Blob([csv], { type: 'text/csv' });
      const url = window.URL.createObjectURL(blob);
      const a = document.createElement('a');
      a.style.display = 'none';
      a.href = url;
      a.download = 'data.csv';
      document.body.appendChild(a);
      a.click();
      window.URL.revokeObjectURL(url);
    },
    convertToCSV(data) {
      const csv = [];
      const headers = Object.keys(data[0]);
      csv.push(headers.join(','));
      for (const row of data) {
        const values = headers.map((header) => row[header]);
        csv.push(values.join(','));
      }
      return csv.join('\n');
    },
  },
};
</script>

<style scoped>
@import '~bootstrap/dist/css/bootstrap.min.css';

/* Tus estilos CSS aquí */
</style>
