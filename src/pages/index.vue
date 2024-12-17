<template>
  <div>
    <div
      class="text-center d-flex align-center justify-center"
      style="
        width: 100%;
        height: 80px;
        background-color: #3a23fc;
        font-size: 32px;
        color: white;
      "
    >
      OCR with Flask convertor
    </div>
    <v-row justify="center">
      <v-col cols="12" md="9" class="d-flex flex-column justify-center align-center">
        <div class="mt-12" style="font-size: 24px;">Та OCR хийх зургаа оруулна уу.</div>
        <div class="d-flex align-center mt-8">
          <v-file-input
            hide-details
            v-model="selectedFile"
            label="Зургаа сонгон уу"
            outlined
            dense
            accept="image/*"
            @change="onFileChange"
            style="width: 400px"
          ></v-file-input>
          <v-btn width="160px" variant="flat" color="primary" height="56px" @click="uploadImage">Хөрвүүлэх</v-btn>
        </div>

    
        <v-progress-circular
          v-if="isLoading"
          indeterminate
          color="primary"
          class="mt-4"
        ></v-progress-circular>
        <div class="mt-8" style="font-size: 24px;">Таны зургаас таньж авсан text:</div>
        <v-card variant="outlined" v-if="ocrText" class="mt-8 pa-4" width="560px">
          <v-card-text>
            <pre style="font-size: 20px;">{{ ocrText }}</pre>
          </v-card-text>
        </v-card>
      </v-col>
    </v-row>
  </div>
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
        const response = await axios.post(
          "http://localhost:5000/ocr",
          formData,
          {
            headers: {
              "Content-Type": "multipart/form-data",
            },
          }
        );
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
