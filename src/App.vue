<script setup>

</script>

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
          <input type="file" placeholder="Select your file" />
          <button type="button">
            Upload
          </button>
        </div>
      </div>
    </div>

    <div class="top-blur-circle">
    </div>
    <div class="bottom-blur-circle">
    </div>
  </div>
</template>

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
