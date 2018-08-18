<template>
  <div id="app">
    <div class="header-container">
      <app-header class="app-header"></app-header>
      <select v-model="selectedStyle" id="style">
        <option value="style1">style1</option>
        <option value="style2">style2</option>
        <option value="style3">style3</option>
      </select>
    </div>

    <div class="containers">
      <div class="container">
        <textarea v-model="input" id="input" @input="update"></textarea>
      </div>
      <div class="container">
        <div id="output" v-bind:class="selectedStyle" v-html="compiledMarkdown"></div>
      </div>
    </div>
  </div>
</template>

<script>
import Header from './components/Header.vue'


export default {
  data() {
    return {
      input: '',
      selectedStyle: 'style1'
    }
  },
  computed: {
    compiledMarkdown() {
      return marked(this.input, {sanitzie: true})
    }
  },
  methods: {
    update: _.debounce(function (e) {
      this.input = e.target.value
    }, 300)

  },
  components: {
    'app-header': Header
  },
  created() {
    this.input = `
# Hi1
## This is a simple web app.
    `;
  }
}
</script>

<style lang="scss">
.style1 { @import './styles/style1.scss'; }
.style2 { @import './styles/style2.scss'; }

.header-container {
  display: flex;
  flex-direction: row;
  height: 60px;
}

.app-header {
  flex-grow: 1;
}

#style {
  flex-basis: 30%;
  font-size: 1.2em;
}

.containers {
  display: flex;
  height: calc(100vh - 80px);
}

.container {
  flex-basis: 50%;
  padding: 5px;
  display: flex;
  flex-direction: column;
  height: 100%;
  box-sizing: border-box;
}

#input {
  margin-top: 5px;
  padding: 0.6em;
  border: 1px solid #ddd;
  overflow: auto;
  flex-grow: 1;
  flex-shrink: 1;

}

#output {
  margin-top: 5px;
  border: 1px solid #ddd;
  padding: 0.6em;
  flex-grow: 1;
  overflow: auto;
}



</style>
