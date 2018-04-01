<template>
    <div 
        class="home"
        :class="[
        {'animate-down': preview},
        {'animate-up': !preview}
    ]">
        <div class="header is-centered">
            <h1>Coolors<span>AI</span></h1>
            <transition name="fade" mode="out-in">
                <p v-if="preview">Find the perfect color scheme using <span>artificial intellgence</span></p>
            </transition>
        </div>

        <transition name="fade" mode="out-in">
            <button v-if="!preview" @click="restartApp" class="btn-restart">Home</button>

            <div
                v-if="preview"
                class="info"
            >
                <Info />
                <button @click="preview = false" class="btn-start">Start</button>
            </div>
        </transition>

        <transition name="fade" mode="out-in">
            <div v-if="!preview" class="app">
                <neural-network  />
            </div>
        </transition>

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
        }
    }
}
</script>

<style>
html,
body {
    background: #333;
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
.animate-up .header h1 {
    animation: reduce-text;
    animation-duration: 1s;
    animation-fill-mode: forwards;
}
@keyframes reduce-text {
    from {
        font-size: 5em;
    }
    to {
        font-size: 2em;
    }
}
.animate-down {
    animation: go-down;
    animation-duration: 1s;
    animation-fill-mode: forwards;
}
.animate-down .header h1 {
    animation: increase-text;
    animation-duration: 1s;
    animation-fill-mode: forwards;
}
@keyframes increase-text {
    from {
        font-size: 2em;
    }
    to {
        font-size: 5em;
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
.fade-enter-active,
.fade-leave-active {
    transition: opacity 0.5s;
}
.fade-enter,
.fade-leave-to {
    opacity: 0;
}
.is-centered {
    text-align: center;
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
    position: absolute;
    right: 0;
    top: 0;
}
.info {
    text-align: center;
}
</style>
