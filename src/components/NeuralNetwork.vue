<template>
  <div class="neural-network">
    <h1>Neural Network</h1>
  </div>
</template>

<script>
import brain from 'brain.js'

export default {
    data() {
        return {
            trainingData: []
        }
    },
    created() {
        let net = new brain.NeuralNetwork()
    }
}
</script>

<style>

</style>
