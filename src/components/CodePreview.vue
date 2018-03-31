<template>
  <div class="code-preview background">
    <p>
      <span class="keyword">import</span> <span class="variable">Vue</span> <span class="keyword">from</span> <span class="string">'Vue'</span>
      <br />
      <span class="keyword">import</span> <span class="variable">App</span> <span class="keyword">from</span> <span class="string">'./App'</span>
      <br /><br />
      <span class="keyword">let</span> <span class="variable">foo</span> <span class="keyword">=</span> <span class="string">'bar'</span><span class="keyword">;</span>
      <br /><br />
      <span class="keyword">export default</span> <span class="variable">foo</span><span class="keyword">;</span>
      <br /><br />
      <span class="method">console.log</span><span class="secondary">(</span><span class="string">'Hello World!'</span><span class="secondary">)</span><span class="keyword">;</span>
      {{ colors }}
    </p>
  </div>
</template>

<script>
export default {
    props: ['colors']
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
    opacity: 0.5;
    cursor: pointer;
    animation-name: code-hover;
    animation-duration: 0.1s;
    animation-fill-mode: forwards;
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
