<template>
  <div class="neural-network">
    <code-preview class="active-preview" :colors="currentScheme" />
    <div class="actions-container">
      <button class="btn btn-dislike" @click="rate('dislike')">Dislike</button>
      <button class="btn btn-neutral" @click="rate('neutral')">Neutral</button>
      <button class="btn btn-like" @click="rate('like')">Like</button>
    </div>
  </div>
</template>

<script>
import brain from 'brain.js'
import CodePreview from './CodePreview'

export default {
    data() {
        return {
            trainingData: [],
            currentScheme: {
                keyword: 'pink',
                variable: 'red',
                string: 'cyan',
                background: 'gray',
                method: 'coffee',
                secondary: 'purple'
            }
        }
    },
    created() {
        let net = new brain.NeuralNetwork()
    },
    components: {
        CodePreview
    },
    methods: {
        rate(type) {
            let score = 0
            switch (type) {
                case 'dislike':
                    score = 0
                    console.log('dislike')
                    break
                case 'neutral':
                    score = 0.5
                    console.log('neutral')
                    break
                case 'like':
                    score = 1
                    console.log('like')
                    break
            }
            this.currentScheme.keyword = this.getRandomRgb()
            this.currentScheme.variable = this.getRandomRgb()
            this.currentScheme.string = this.getRandomRgb()
            this.currentScheme.background = this.getRandomRgb()
            this.currentScheme.method = this.getRandomRgb()
            this.currentScheme.secondary = this.getRandomRgb()
        },
        getRandomRgb() {
            return {
                red: Math.round(Math.random() * 205 + 50),
                green: Math.round(Math.random() * 205 + 50),
                blue: Math.round(Math.random() * 205 + 50)
            }
        }
    }
}
</script>

<style scoped>
.active-preview {
    display: block;
    margin-left: auto !important;
    margin-right: auto !important;
    pointer-events: none;
}
.actions-container {
    display: flex;
    padding-left: 30%;
    padding-right: 30%;
}
.btn {
    padding: 9px 12px 7px;
    width: 150px;
    flex-shrink: 2;
}
.btn-dislike {
    background: red;
}
.btn-neutral {
    background: blue;
}
.btn-like {
    background: green;
}
</style>
