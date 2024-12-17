<template>
  <v-container>
    <v-row justify="center">
      <v-col cols="12" md="6">
        <h1 class="text-center">OCR with Flask and Vue.js</h1>
        <v-file-input
          v-model="selectedFile"
          label="Choose an Image"
          outlined
          dense
          accept="image/*"
          @change="onFileChange"
          style="width: 100%;"
        ></v-file-input>


        <v-btn @click="uploadImage"> Submit</v-btn>
        <v-progress-circular
          v-if="isLoading"
          indeterminate
          color="primary"
          class="mt-4"
        ></v-progress-circular>

        <v-card v-if="ocrText" class="mt-4">
          <v-card-title>Extracted Text</v-card-title>
          <v-card-text>
            <pre>{{ ocrText }}</pre>
          </v-card-text>
        </v-card>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
import axios from "axios";

export default {
  data() {
    return {
      selectedFile: null,
      ocrText: null,
      isLoading: false,
    };
  },
  methods: {
    onFileChange(file) {
      console.log("File selected:", file);
    },
    async uploadImage() {
      if (!this.selectedFile) {
        alert("Please select a file.");
        return;
      }

      const formData = new FormData();
      formData.append("file", this.selectedFile);

      this.isLoading = true; 

      try {
        const response = await axios.post("http://localhost:5000/ocr", formData, {
          headers: {
            "Content-Type": "multipart/form-data",
          },
        });
        this.ocrText = response.data.text;
      } catch (error) {
        console.error("Error uploading image:", error);
        alert(error);
      } finally {
        this.isLoading = false; // Reset loading state
      }
    },
  },
};
</script>

<style scoped>
/* Add any custom styles here */
</style>
