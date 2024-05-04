<template>
  <div class="container">
    <h2>Inserisci un'immagine</h2>
    <div>
      <label for="fileInput" class="custom-file-input">
        <span>Scegli un file</span>
        <input type="file" @change="handleFileChange" accept="image/*">
      </label>
      <span v-if="selectedFileName">{{ selectedFileName }}</span>
    </div>

    <div class="containerColumns">
      <div class="column">
        <button class="technology" v-if="imageSent" @click="transferImage">Invia alla rete neurale</button>
      </div>

      <div class="column">
        <button class="technology" v-if="imageSent" @click="clear">Clear</button>
      </div>
    </div>

    <div class="containerColumns">
      <div class="column">
        <div class="inserita" v-if="imageSent">
          <h3>Immagine Inserita</h3>
          <img :src="imageSent" alt="Immagine Inserita">
        </div>
      </div>

      <div class="column">
        <div class="ricevuta-container column">
          <div class="ricevuta" v-if="imageReceived">
            <h3>Immagine Ricevuta</h3>
            <img :src="imageReceived" alt="Immagine Ricevuta">
          </div>
        </div>
      </div>
    </div>

    <div class="containerColumns">
      <div class="column">
        <div v-if="base64StringSent">
          <h3>Immagine Trasferita come Base64</h3>
          <textarea :value="base64StringSent" rows="10" cols="50"></textarea>
        </div>
      </div>

      <div class="column">
        <div v-if="base64StringReceived">
          <h3>Immagine Ricevuta come Base64</h3>
          <textarea :value="base64StringReceived" rows="10" cols="50"></textarea>
        </div>
      </div>
    </div>

  </div>
</template>


<script>
import axios from 'axios';

export default {
  name: 'HomePage',
  props: {
    msg: String
  },
  data() {
    return {
      imageSent: null,
      imageReceived: null,
      base64StringSent: null,
      base64StringReceived: null,
      selectedFileName: null,
    };
  },
  methods: {
    handleFileChange(event) {
      const file = event.target.files[0];
      if (file) {
        this.selectedFileName = file.name;
        const reader = new FileReader();
        reader.onload = () => {
          this.imageSent = reader.result;
        };
        reader.readAsDataURL(file);
      }
    },
    async transferImage() {
      
      const api = "https://b858-2a07-7e87-24b1-0-e984-3726-b636-6e70.ngrok-free.app/imageProcess";
      const postData = {
        "image": this.imageSent
      };

      try {
        const response = await axios.post(api, postData);
        console.log('Response:', response.data);
        this.imageReceived = "data:image/jpeg;base64," + response.data.imageProcessed;
        this.base64StringReceived = this.imageReceived.split(",")[1];
      } catch (error) {
        console.error('Error:', error);
      }

      this.base64StringSent = this.imageSent.split(",")[1];

    },
    clear() {
      this.imageSent = null;
      this.imageReceived = null;
      this.base64StringSent = null;
      this.base64StringReceived = null;
      this.selectedFileName = null;
    }
  }
}

</script>

<style>
.container {
  text-align: center;
  margin-top: 50px;
}

.containerColumns {
  display: flex;
}

.column {
  flex: 1;
  padding: 10px;
}

.column img {
  width: 100%;
  height: 200;
}

.technology {
  background-color: rgb(218, 74, 74);
  color: white;
  border: none;
  padding: 10px 20px;
  margin: 1px
}

.custom-file-input {
  position: relative;
  display: inline-block;
  overflow: hidden;
  cursor: pointer;
  padding: 10px 20px;
  background-color: rgb(218, 74, 74);
  color: #fff;
  border-radius: 5px;
}

.custom-file-input input[type="file"] {
  position: absolute;
  left: 0;
  top: 0;
  opacity: 0;
  cursor: pointer;
}

.ricevuta-container {
  position: relative;
}

</style>