<template>
    <div class="code-preview background" :style="{ background: createBackground }" @click="showProperties()">
        <p>
        <span 
            class="keyword"
            :style="{ color: createKeyword }"
        >import</span>
        <span
            class="variable"
            :style="{ color: createVariable }"
        >Vue</span>
        <span
            class="keyword"
            :style="{ color: createKeyword }"
        >from</span>
        <span
            class="string"
            :style="{ color: createString }"
        >'Vue'</span>
        <br />
        <span
            class="keyword"
            :style="{ color: createKeyword }"
        >import</span>
        <span
            class="variable"
            :style="{ color: createVariable }"
        >App</span>
        <span
            class="keyword"
            :style="{ color: createKeyword }"
        >from</span>
        <span
            class="string"
            :style="{ color: createString }"
        >'./App'</span>
        <br /><br />
        <span
            class="keyword"
            :style="{ color: createKeyword }"
        >let</span>
        <span
            class="variable"
            :style="{ color: createVariable }"
        >foo</span>
        <span
            class="keyword"
            :style="{ color: createKeyword }"
        >=</span>
        <span
            class="string"
        >'bar'</span><span
            class="keyword"
            :style="{ color: createKeyword }"
        >;</span>
        <br /><br />
        <span
            class="keyword"
            :style="{ color: createKeyword }"
        >export default</span> 
        <span
            class="variable"
            :style="{ color: createVariable }"
        >foo</span><span
            class="keyword"
            :style="{ color: createKeyword }"
        >;</span>
        <br /><br />
        <span
            class="method" 
            :style="{ color: createMethod }"
        >console.log</span><span
            class="secondary"
            :style="{ color: createSecondary }"
        >(</span><span 
            class="string" 
            :style="{ color: createString }"
        >'Hello World!'</span><span
            class="secondary"
            :style="{ color: createSecondary }"
        >)</span><span
            class="keyword"
            :style="{ color: createKeyword }"
        >;</span>
        </p>
        <modal v-if="showModal" :colors="colors" />
    </div>
</template>

<script>
import Modal from './Modal'

export default {
    props: ['colors'],
    components: {
        Modal
    },
    computed: {
        createKeyword() {
            return (
                'rgb(' +
                this.colors.keyword.red +
                ',' +
                this.colors.keyword.green +
                ',' +
                this.colors.keyword.blue +
                ')'
            )
        },
        createVariable() {
            return (
                'rgb(' +
                this.colors.variable.red +
                ',' +
                this.colors.variable.green +
                ',' +
                this.colors.variable.blue +
                ')'
            )
        },
        createString() {
            return (
                'rgb(' +
                this.colors.string.red +
                ',' +
                this.colors.string.green +
                ',' +
                this.colors.string.blue +
                ')'
            )
        },
        createBackground() {
            return (
                'rgb(' +
                this.colors.background.red +
                ',' +
                this.colors.background.green +
                ',' +
                this.colors.background.blue +
                ')'
            )
        },
        createMethod() {
            return (
                'rgb(' +
                this.colors.method.red +
                ',' +
                this.colors.method.green +
                ',' +
                this.colors.method.blue +
                ')'
            )
        },
        createSecondary() {
            return (
                'rgb(' +
                this.colors.secondary.red +
                ',' +
                this.colors.secondary.green +
                ',' +
                this.colors.secondary.blue +
                ')'
            )
        },
        createBackground() {
            return (
                'rgb(' +
                this.colors.background.red +
                ',' +
                this.colors.background.green +
                ',' +
                this.colors.background.blue +
                ')'
            )
        }
    },
    methods: {
        showProperties() {
            this.showModal = !this.showModal
        }
    },
    data() {
        return {
            showModal: false
        }
    }
}
</script>

<style scoped>
.code-preview {
    width: 25%;
    flex-grow: 1;
    padding: 40px 80px;
    margin: 20px;
    font-family: Menlo, Monaco, Consolas, 'Droid Sans Mono', 'Inconsolata',
        'Courier New', monospace;
}
@keyframes code-hover {
    from {
        transform: translateY(0px);
    }
    to {
        transform: translateY(-10px);
    }
}
.code-preview:hover {
    background: #09f;
    opacity: 1;
    cursor: pointer;
    animation-name: code-hover;
    animation-duration: 0.1s;
    animation-fill-mode: forwards;
    box-shadow: 0 14px 28px rgba(0, 0, 0, 0.25), 0 10px 10px rgba(0, 0, 0, 0.22);
}
.background {
    background: #1e1e1e;
}
.keyword {
    color: red;
}
.variable {
    color: blue;
}
.string {
    color: green;
}
.secondary {
    color: yellow;
}
.method {
    color: cyan;
}
</style>
