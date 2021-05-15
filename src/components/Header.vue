<template>
    <header>
        <h1>{{title}}</h1>
        <Button v-show="homePage" :text="showAddTask ? 'Close' : 'Add Task'" :color="showAddTask ? 'red' : 'green'" @btn-click="$emit('btn-click')"></Button>
    </header>
</template>

<script>
import Button from './Button'
export default {
    name: "Header",
    components: { Button },
    props: {
        title: String,
        showAddTask: Boolean
    },
    computed: {
        homePage() {
            return this.$route.path === "/"
        }
    },
    emits: ['btn-click']
}
</script>

<style scoped>
    header {
        display: flex;
        justify-content: space-between;
        align-items: center;
        margin-bottom: 20px;
    }
</style>
