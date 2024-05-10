<template>
  <div class="container">
    <h1>Excel to YAML Converter</h1>
    <div class="uploader">
      <input type="file" @change="handleFileUpload" accept=".xls,.xlsx" id="fileInput" />
      <label for="fileInput">Choose an Excel file</label>
    </div>
    <button @click="downloadYAML" :disabled="!yamlContent">Download YAML</button>
    <pre>{{ yamlContent }}</pre>
  </div>
</template>

<script>
import * as XLSX from 'xlsx';
import * as yaml from 'js-yaml';

export default {
  data() {
    return {
      yamlContent: '',
      jsonData: null,
    };
  },
  methods: {
    handleFileUpload(event) {
      const file = event.target.files[0];
      if (file) {
        const reader = new FileReader();
        reader.onload = (e) => {
          const data = new Uint8Array(e.target.result);
          const workbook = XLSX.read(data, { type: 'array' });
          const firstSheet = workbook.Sheets[workbook.SheetNames[0]];
          // Convert Excel to JSON
          this.jsonData = XLSX.utils.sheet_to_json(firstSheet, { header: 0 });
          // Convert JSON to YAML with key-value pairs
          this.yamlContent = yaml.dump(this.jsonData);
        };
        reader.readAsArrayBuffer(file);
      }
    },
    downloadYAML() {
      const blob = new Blob([this.yamlContent], { type: 'text/yaml' });
      const url = URL.createObjectURL(blob);
      const a = document.createElement('a');
      a.href = url;
      a.download = 'converted.yaml';
      a.click();
      URL.revokeObjectURL(url);
    },
  },
};
</script>

<style scoped>
.container {
  display: flex;
  flex-direction: column;
  align-items: center;
  margin: 20px auto;
  padding: 20px;
  border: 1px solid #e0e0e0;
  border-radius: 10px;
  background-color: #ffffff;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
  max-width: 600px;
}

h1 {
  color: #333;
  margin-bottom: 20px;
}

.uploader {
  position: relative;
  margin-bottom: 20px;
}

.uploader input[type='file'] {
  display: none;
}

.uploader label {
  background-color: #007bff;
  color: #fff;
  padding: 10px 20px;
  border-radius: 5px;
  cursor: pointer;
  transition: background-color 0.2s;
}

.uploader label:hover {
  background-color: #0056b3;
}

button {
  background-color: #28a745;
  color: #fff;
  padding: 10px 20px;
  border: none;
  border-radius: 5px;
  cursor: pointer;
  transition: background-color 0.2s;
}

button:disabled {
  background-color: #6c757d;
  cursor: not-allowed;
}

button:not(:disabled):hover {
  background-color: #218838;
}

pre {
  color: #000000;
  background-color: #f8f9fa;
  padding: 10px;
  border-radius: 5px;
  border: 1px solid #e0e0e0;
  white-space: pre-wrap;
  word-wrap: break-word;
  max-width: 100%;
  overflow-x: auto;
}
</style>
