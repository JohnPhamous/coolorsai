<template>
    <div class="neural-network">
        <code-preview class="active-preview" :colors="currentScheme" />
        <div class="actions-container">
            <button class="btn btn-dislike" @click="rate('dislike')">DISLIKE</button>
            <button class="btn btn-neutral" @click="rate('neutral')">NEUTRAL</button>
            <button class="btn btn-like" @click="rate('like')">LIKE</button>
            <button class="btn btn-train" @click="train" :disabled="trainingData.length < 5">Train Data</button>
            <button class="btn btn-train" @click="generate" :disabled="isTraining">Generate Color Schemes</button>
        </div>     
        <div class="preview-grid">
            <code-preview v-for="s in generatedSchemes" :key="Math.floor(s.score)" :colors="s"/>
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
            },
            generatedSchemes: [],
            sortedSchemes: [],
            neuralNetwork: {},
            isTraining: true
        }
    },
    created() {
        this.setColors()
        this.neuralNetwork = new brain.NeuralNetwork()
    },
    components: {
        CodePreview
    },
    methods: {
        setColors() {
            this.currentScheme.keyword = this.getRandomRgb()
            this.currentScheme.variable = this.getRandomRgb()
            this.currentScheme.string = this.getRandomRgb()
            this.currentScheme.background = this.getRandomRgbDark()
            this.currentScheme.method = this.getRandomRgb()
            this.currentScheme.secondary = this.getRandomRgb()
        },
        generateColors() {
            return {
                keyword: this.getRandomRgb(),
                variable: this.getRandomRgb(),
                string: this.getRandomRgb(),
                background: this.getRandomRgbDark(),
                method: this.getRandomRgb(),
                secondary: this.getRandomRgb()
            }
        },
        rate(type) {
            let score = 0
            this.isTraining = true
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

            this.trainingData.push({
                output: [score],
                input: [
                    this.normalizeData(this.currentScheme.keyword.red),
                    this.normalizeData(this.currentScheme.keyword.green),
                    this.normalizeData(this.currentScheme.keyword.blue),
                    this.normalizeData(this.currentScheme.variable.red),
                    this.normalizeData(this.currentScheme.variable.green),
                    this.normalizeData(this.currentScheme.variable.blue),
                    this.normalizeData(this.currentScheme.string.red),
                    this.normalizeData(this.currentScheme.string.green),
                    this.normalizeData(this.currentScheme.string.blue),
                    this.normalizeData(this.currentScheme.background.red),
                    this.normalizeData(this.currentScheme.background.green),
                    this.normalizeData(this.currentScheme.background.blue),
                    this.normalizeData(this.currentScheme.method.red),
                    this.normalizeData(this.currentScheme.method.green),
                    this.normalizeData(this.currentScheme.method.blue),
                    this.normalizeData(this.currentScheme.secondary.red),
                    this.normalizeData(this.currentScheme.secondary.green),
                    this.normalizeData(this.currentScheme.secondary.blue)
                ]
            })

            this.setColors()
        },
        getRandomRgb() {
            return {
                red: Math.round(Math.random() * 205 + 50),
                green: Math.round(Math.random() * 205 + 50),
                blue: Math.round(Math.random() * 205 + 50)
            }
        },
        getRandomRgbDark() {
            return {
                red: Math.round(Math.random() * 50),
                green: Math.round(Math.random() * 50),
                blue: Math.round(Math.random() * 50)
            }
        },
        normalizeData(data) {
            return Math.round(data / 2.55) / 100
        },
        train() {
            this.neuralNetwork = new brain.NeuralNetwork({
                activation: 'leaky-relu'
            })
            this.neuralNetwork.train(this.trainingData)
            this.isTraining = false
        },
        generate() {
            for (let i = 0; i < 10; i++) {
                let scheme = this.generateColors()
                let data = [
                    this.normalizeData(scheme.keyword.red),
                    this.normalizeData(scheme.keyword.green),
                    this.normalizeData(scheme.keyword.blue),
                    this.normalizeData(scheme.variable.red),
                    this.normalizeData(scheme.variable.green),
                    this.normalizeData(scheme.variable.blue),
                    this.normalizeData(scheme.string.red),
                    this.normalizeData(scheme.string.green),
                    this.normalizeData(scheme.string.blue),
                    this.normalizeData(scheme.background.red),
                    this.normalizeData(scheme.background.green),
                    this.normalizeData(scheme.background.blue),
                    this.normalizeData(scheme.method.red),
                    this.normalizeData(scheme.method.green),
                    this.normalizeData(scheme.method.blue),
                    this.normalizeData(scheme.secondary.red),
                    this.normalizeData(scheme.secondary.green),
                    this.normalizeData(scheme.secondary.blue)
                ]

                let [score] = this.neuralNetwork.run(data)
                scheme.score = score
                this.generatedSchemes.push(scheme)
            }

            this.sortedSchemes = this.generatedSchemes.sort((left, right) => {
                let a = left.score
                // console.log('a', a)
                let b = right.score
                // console.log('b', b)

                return b > a
            })

            // this.sortedSchemes.forEach(item => {
            //     console.log(item.score)
            // })
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
    flex-wrap: wrap;
    padding-left: 30%;
    padding-right: 30%;
}
.btn {
    padding: 9px 12px 7px;
    width: 150px;
    flex-shrink: 2;
    font-family: 'Source Sans Pro', sans-serif;
    transition: 0.3s ease all;
}
.btn-dislike {
    background: #ff0072;
}
.btn-neutral {
    background: #09f;
}
.btn-train {
    width: 48%;
    margin-top: 10px;
    background: #fdc307;
    font-weight: 800;
}
.btn-train:disabled {
    opacity: 0.5;
    cursor: not-allowed;
}
.btn-like {
    background: green;
}
.preview-grid {
    display: flex;
    flex-wrap: wrap;
}
</style>
