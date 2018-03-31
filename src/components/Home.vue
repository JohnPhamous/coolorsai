<template>
  <div 
    class="home"
    :class="[
      {'animate-down': preview},
      {'animate-up': !preview}
    ]">
    <div class="header is-centered">
      <h1>Coolors<span>AI</span></h1>
      <p>Find the perfect color scheme using <span>artificial intellgence</span></p>
      <button v-if="!preview" @click="restartApp" class="btn-restart">Restart</button>
    </div>

    <div v-if="preview" class="info">
      <Info v-if="preview" />
      <button @click="preview = false" class="btn-start">Start</button>
    </div>

    <div v-else class="app">
      <neural-network  />
      <div class="preview-grid">
        <code-preview v-for="i in schemes" :key="i" colors="sdfsdf"/>
      </div>
    </div>

  </div>
</template>

<script>
import Info from './Info'
import NeuralNetwork from './NeuralNetwork'
import CodePreview from './CodePreview'

export default {
    name: 'Home',
    data() {
        return {
            schemes: [],
            preview: true
        }
    },
    components: {
        NeuralNetwork,
        CodePreview,
        Info
    },
    methods: {
        createDataEntry() {},
        restartApp() {
            this.preview = true
            this.schemes.length = 0
        }
    }
}
</script>

<style>
html,
body {
    background: #000;
    color: #f8f8f2;
    font-family: Colfax, -apple-system, BlinkMacSystemFont, Segoe UI, Roboto,
        Oxygen, Ubuntu, Cantarell, Fira Sans, Droid Sans, Helvetica Neue,
        sans-serif;
}
.center-down {
    transform: translateY(30vh);
}
.animate-up {
    animation: go-up;
    animation-duration: 1s;
    animation-fill-mode: forwards;
}
.animate-down {
    animation: go-down;
    animation-duration: 1s;
    animation-fill-mode: forwards;
}
@keyframes go-up {
    from {
        transform: translateY(30vh);
    }
    to {
        transform: translateY(0vh);
    }
}
@keyframes go-down {
    from {
        transform: translateY(0vh);
    }
    to {
        transform: translateY(30vh);
    }
}
.header h1 {
    font-size: 5em;
    margin-bottom: 0px;
}
.header span {
    color: #09f;
}
.header p {
    font-size: 1.5em;
    margin-top: 10px;
}
.is-centered {
    text-align: center;
}
.preview-grid {
    display: flex;
    flex-wrap: wrap;
}
button {
    box-shadow: inset 0 -1px 0 rgba(0, 0, 0, 0.1),
        0 1px 3px rgba(0, 85, 255, 0.3);
    border-radius: 4px;
    color: white;
    border: none;
    text-align: center;
    cursor: pointer;
    display: block;
    margin-left: auto;
    margin-right: auto;
}
.btn-start {
    font-weight: 800;
    background: #09f;
    padding: 18px 24px 15px;
}
.btn-restart {
    font-weight: 600;
    background: white;
    font-size: 1em;
    color: black;
    padding: 9px 12px 7px;
}
.info {
    text-align: center;
}
</style>
