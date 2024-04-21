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


  </div>
</template>


<script>
export default {
  name: 'HomePage',
  props: {
    msg: String
  },
  data() {
    return {
      imageSent: null,
      imageReceived: null,
      selectedFileName: null
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
    transferImage() {
      
      const api = "TODO";
      const postData = {
        "image": this.imageSent
      };

    },
    clear() {
      this.imageSent = null;
      this.imageReceived = null;
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
}


</style>