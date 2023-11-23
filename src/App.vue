<template>
  <div class="main-container">
    <div class="container-text">
      <h1>
        Transform your Sounds into Canvases of Visual Emotions
      </h1>
      <div class="container-input">
        <p>
          Drop your file here
        </p>
        <div class="container-component-input">
          <input type="text" placeholder="Enter your text" class="inputText" v-model="textInput" />
          <!-- <input type="file" placeholder="Select your file" @change="handleFileChange" /> -->
          <button type="button" @click="fetchQuery">
            Upload
          </button>
        </div>

        <p v-if="loader === true">
          Loading...
        </p>
        <img v-else :src="img" v-if="img" class="imgIA" />
      </div>
    </div>

    <div class="top-blur-circle">
    </div>
    <div class="bottom-blur-circle">
    </div>
  </div>
</template>

<script setup>
import { ref } from "vue";
import { client } from "@gradio/client";

const textInput = ref('');
const file = ref(null);
const img = ref(null);
const loader = ref(false);

const handleFileChange = (event) => {
  const fileInput = event.target;
  const selectedFile = fileInput.files[0];

  if (selectedFile && selectedFile instanceof File) {
    file.value = selectedFile;
  } else {
    console.log("oui");
    console.error('Aucun fichier sélectionné ou le fichier sélectionné n\'est pas un objet File valide.');
    file.value = null;
  }
};

const run = async () => {
  console.log('Contenu de file avant la vérification :', file);

  try {
    const app = await client("https://seungheondoh-lp-music-caps-demo.hf.space/");
    app.config.dev_mode = true;
    // app.config.enable_queue = false;

    console.log("APP", app);
    const promise = Promise.all(await app.predict("/predict", [file.value]))
    console.log("PROMISE", promise);
    const result = promise;

    console.log("Résultat:", result?.data);
  } catch (error) {
    console.error('Une erreur est survenue lors de l\'envoi du fichier:', error);
  }
};

const query = async (data) => {
  loader.value = true;
  const response = await fetch(
    "https://api-inference.huggingface.co/models/stabilityai/stable-diffusion-xl-base-1.0",
    {
      headers: { Authorization: "Bearer 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" },
      method: "POST",
      body: JSON.stringify(data),
    }
  );
  console.log(response);
  const result = await response.blob();
  return result;
}

const fetchQuery = async () => {
  query({ "inputs": textInput.value }).then(async (response) => {
    const url = URL.createObjectURL(response)
    img.value = url;
    console.log("url", url);
    loader.value = false;
  });
}

</script>

<style scoped>
.main-container {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  height: 100vh;
  width: 100vw;
}

.container-text {
  width: 70%;
  height: 50%;
}

.container-text h1 {
  text-align: center;
}

.imgIA {
  width: 50%;
  height: auto;
}

.inputText {
  width: 70%;
  border: 0;
}

.container-input {
  height: 70%;
  width: 100%;
  display: flex;
  flex-direction: column;
  align-items: flex-start;
  gap: 8px;
}

.container-component-input {
  width: 100%;
  padding: 8px 24px;
  background-color: white;
  border: 1px solid white;
  border-radius: 16px;
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.container-component-input button {
  background: rgba(42, 56, 182, 0.87);
  color: white;
}

.top-blur-circle,
.bottom-blur-circle {
  position: absolute;
  width: 567px;
  height: 567px;
  flex-shrink: 0;
  border-radius: 567px;
  background: linear-gradient(180deg, #242C77 0%, rgba(222, 222, 228, 0.59) 100%);
  filter: blur(20px);
}

.top-blur-circle {
  bottom: 50%;
  right: 85%;
}

.bottom-blur-circle {
  top: 50%;
  left: 85%;
}
</style>
