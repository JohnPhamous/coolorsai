<template>
    <div class="neural-network">
        <code-preview class="active-preview" :colors="currentScheme" v-intro="'This is the color scheme you will be voting on.'"/>
        <div class="training-data-info" v-intro="'This is the number of votes you\'ve made. Each session\'s votes are saved so the result of the neural network will be a result of the aggregated votes.'">
            Dataset Size: {{ trainingData.length }}
        </div>
        <div class="actions-container" v-intro="'You can vote on the above color scheme here.'">
            <button class="btn btn-dislike" @click="rate('dislike')">DISLIKE</button>
            <button class="btn btn-neutral" @click="rate('neutral')">NEUTRAL</button>
            <button class="btn btn-like" @click="rate('like')">LIKE</button>
            <button class="btn btn-train" @click="train" :disabled="trainingData.length < 1" v-intro="'Once you\'ve created enough data, you can train the neural network'">Train Data</button>
            <button class="btn btn-train" @click="clearTrainingData" :disabled="trainingData.length < 1" v-intro="'If you\'d like a clean start, you can delete the training data'">Clear Data</button>
            <button class="btn btn-train" @click="generate" :disabled="isTraining" v-intro="'Once you\'ve trained the network, you can generate a few color schemes based on your preferences'">Generate</button>
        </div>
        <div class="preview-grid" v-intro="'Your generated color schemes will appear here'">
            <code-preview
                v-for="(s, index) in sortedSchemes"
                :key="index"
                :colors="s"
            />
        </div>
        
    </div>
</template>

<script>
import brain from 'brain.js'
import CodePreview from './CodePreview'

export default {
    updated() {},
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
        this.trainingData =
            JSON.parse(localStorage.getItem('trainingData')) || []
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
        clearTrainingData() {
            if (confirm('Are you sure you want to delete the data?')) {
                localStorage.clear()
                this.trainingData = []
            }
        },
        rate(type) {
            let score = 0
            this.isTraining = true
            console.log(this.trainingData)

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

            console.log(typeof this.trainingData)
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

            window.localStorage.trainingData = JSON.stringify(this.trainingData)

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
            for (let i = 0; i < 500; i++) {
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
                let b = right.score
                return b > a
            })

            let highestSchemes = this.sortedSchemes.slice(0, 6)
            let lowestSchemes = this.sortedSchemes.slice(450, 456)
            let allSchemes = highestSchemes.concat(lowestSchemes)
            this.sortedSchemes = allSchemes
        }
    }
}
</script>

<style>
@import '../../node_modules/intro.js/minified/introjs.min.css';
.training-data-info {
    text-align: center;
    color: #9c9c9c;
    display: block;
    margin-left: auto;
    margin-right: auto;
    width: 30%;
}
.active-preview {
    display: block;
    margin-left: auto !important;
    margin-right: auto !important;
    pointer-events: none;
    box-shadow: 0 14px 28px rgba(0, 0, 0, 0.25), 0 10px 10px rgba(0, 0, 0, 0.22);
}
.actions-container {
    display: flex;
    flex-wrap: wrap;
    margin-left: 30%;
    margin-right: 30%;
    margin-top: 20px;
    margin-bottom: 50px;
    z-index: 5;
}
.btn {
    padding: 9px 12px 7px;
    width: 30%;
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
    margin-top: 15px;
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
.introjs-tooltiptext {
    color: black;
}
.introjs-helperLayer {
    background-color: transparent !important;
    border: 1px white solid;
}
.introjs-overlay {
    opacity: 0 !important;
}
</style>
