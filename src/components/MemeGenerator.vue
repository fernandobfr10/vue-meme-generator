<template>
  <div class="wrapper">
    <h1><img src="../assets/logo.svg" alt="Meme Maker Logo"></h1>
    <div class="card" v-if="!generatedMeme">
      <h2 class="title">Selecione um template</h2>

      <div class="templates">
        <button class="btn" :class="template.id === selectedTemplate.id ? 'selected' : ''" type="button" v-for="template in templates" :key="template.id" @click="handleSelectTemplate(template)">
          <img :src="template.url" :alt="template.name">
        </button>
      </div>


      <div class="form__container" v-if="selectedTemplate">
        <h2 class="title">Textos</h2>

        <form @submit="handleSubmit">
          <input type="text" :placeholder="`Texto #${index + 1}`" v-for="(input, index) in selectedTemplate.box_count" :key="index" @change="handleInputChange(index)" v-model="boxes[index]">

          <Button text="Gerar Meme"/>
        </form>
      </div>
    </div>
    <div class="card" v-else>
      <img class="generated-meme" :src="generatedMeme" alt="Generated Meme">
      <Button text="Gerar Novo Meme" @onClick="generateNewMeme"/>
    </div>
  </div>
</template>

<script>
import qs from 'qs'
import Button from './Button.vue'


export default {
  name: 'MemeGenerator',
  components: {
    Button
  },
  data () {
    return {
      templates: [],
      selectedTemplate: '',
      boxes: [],
      generatedMeme: null
    }
  },
  methods: {
    async getMemes () {
      const response = await fetch('https://api.imgflip.com/get_memes')
      const { data: { memes }} = await response.json()
      this.templates = memes
      console.log(this.templates)
    },
    setSelectedTemplate (template) {
      this.selectedTemplate = template
    },
    handleInputChange (index) {
      (event) => {
        const newValues = this.boxes
        newValues[index] = event.target.value

        this.boxes = newValues
      }
    },
    async handleSubmit (event) {
      event.preventDefault()
      
      const params = qs.stringify({
        template_id: this.selectedTemplate.id,
        username: 'iNando85',
        password: '7f@YyzeGTyiL9F',
        boxes: this.boxes.map(text => ({ text }))
      })

      const response = await fetch(`https://api.imgflip.com/caption_image?${params}`)
      const { data: { url }} = await response.json()

      this.generatedMeme = url

    },
    handleSelectTemplate(template) {
      this.selectedTemplate = template
      this.boxes = []
    },
    generateNewMeme () {
      this.generatedMeme = null
      this.boxes = []
    }
  },
  mounted () {
    this.getMemes()
  }
}
</script>

<style>

.wrapper {
  width: 90%;
  height: 100vh;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  margin: 0 auto;
}

.card {
  background-color: #fff;
  width: 100%;
  max-width: 600px;
  border-radius: 8px;
  padding: 20px;
  margin-top: 15px;
  box-shadow: 0 6px 10px 0 rgba(0, 0, 0, .20);
}

.title {
  font-size: 22px;
  color: #392d2d;
  margin-bottom: 10px;
}

.templates {
  width: 100%;
  height: 90px;
  background-color: #eee;
  border-radius: 8px;
  overflow-y: auto;
  display: flex;
  align-items: center;
  padding: 0 15px;
  margin-bottom: 30px;
}

.btn {
  border: none;
  background: transparent;
  margin-right: 10px;
  border: 2px solid transparent;
}

.btn img {
  width: 53px;
  height: 53px;
}

.btn.selected {
  border-color: #3672a3;
}

form input{
  width: 100%;
  height: 40px;
  border-radius: 8px;
  border: 1px solid #dbdbdb;
  padding: 0 15px;
  font-size: 14px;
  margin-bottom: 10px;
}

.generated-meme {
  margin-bottom: 20px;
  max-width: 100%;
}

</style>
