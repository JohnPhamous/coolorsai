<template>
    <div class="neural-network">
        <code-preview class="active-preview" :colors="currentScheme" />
        <div class="actions-container">
            <button class="btn btn-dislike" @click="rate('dislike')">Dislike</button>
            <button class="btn btn-neutral" @click="rate('neutral')">Neutral</button>
            <button class="btn btn-like" @click="rate('like')">Like</button>
            <button class="btn btn-train" @click="train" :disabled="!trainingData.length">Train Data</button>
            <button class="btn btn-train" @click="generate" :disabled="!trainingData.length">Generate Color Schemes</button>
        </div>
        {{ trainingData }}
        <hr/>
        {{ sampleData }}
        <div class="preview-grid">
            <code-preview v-for="s in generatedSchemes" :key="s.keyword" :colors="s"/>
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
            neuralNetwork: {},
            net: {},
            sampleData: [
                {
                    input: [
                        0.36,
                        0.31,
                        0.59,
                        0.85,
                        0.47,
                        0.47,
                        0.49,
                        0.97,
                        0.83,
                        0.11,
                        0.1,
                        0.15,
                        0.54,
                        0.52,
                        0.24,
                        0.32,
                        0.43,
                        1
                    ],
                    output: [1]
                },
                {
                    input: [
                        0,
                        0,
                        0,
                        0,
                        0,
                        0,
                        1,
                        1,
                        0,
                        0.14,
                        0.18,
                        0.13,
                        0.78,
                        0.36,
                        0.8,
                        0.24,
                        0.95,
                        0.3
                    ],
                    output: [0]
                }
            ]
        }
    },
    created() {
        // let net = new brain.NeuralNetwork()
        this.setColors()
        this.net = new brain.NeuralNetwork()
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

            this.net.train(this.trainingData)

            var [output] = this.net.run([
                0,
                0,
                0,
                0,
                0,
                0,
                1,
                1,
                0,
                0.14,
                0.18,
                0.13,
                0.78,
                0.36,
                0.8,
                0.24,
                0.95,
                0.3
            ])
            console.log(output)
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
            console.log('training')
            this.neuralNetwork = new brain.NeuralNetwork({
                activation: 'leaky-relu'
            })
            this.neuralNetwork.train(this.trainingData)
        },
        generate() {
            console.log(this.neuralNetwork)

            for (let i = 0; i < 100; i++) {
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

                let score = this.neuralNetwork.run(data)
                console.log(score[0])
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
    flex-wrap: wrap;
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
.btn-train {
    width: 48%;
    margin-top: 10px;
    background: purple;
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
