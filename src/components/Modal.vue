<template>
    <div class="modal">
        <hr />
        <h1>Properties</h1>
        <p>Keywords: <span :style="{ 'background': createKeyword }">{{ createKeyword }}</span></p>
        <p>Variables: <span :style="{ 'background': createVariable }">{{ createVariable }}</span></p>
        <p>Literals: <span :style="{ 'background': createString }">{{ createString }}</span></p>
        <p>Methods: <span :style="{ 'background': createMethod }">{{ createMethod }}</span></p>
        <p>Secondarys: <span :style="{ 'background': createSecondary }">{{ createSecondary }}</span></p>
        <p>Background: <span :style="{ 'background': createBackground }">{{ createBackground }}</span></p>
    </div>
</template>

<script>
export default {
    props: ['colors'],
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
    }
}
</script>

<style>

</style>
