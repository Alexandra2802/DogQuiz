<template>
    <div class="result-container">
        <video width="500" autoplay loop>
            <source src="../assets/result.mp4" type="video/mp4">
            Your browser does not support the video tag.
        </video>
        <p class="result">{{ score }} / {{ numberOfQuestions }}</p>
        <button @click="retry">Retry</button>
    </div>
</template>

<script>
export default {
    name: 'ResultComponent',
    props: ['numberOfQuestions', 'score'],
    emits: ['retry'],
    methods: {
        retry() {
            this.$emit('retry');
        }
    }
}
</script>

<style scoped>
.result-container {
    display: flex;
    flex-direction: column;
    justify-content: center;
    z-index: 1;
}

.result {
    font-size: xxx-large;
    text-align: center;
    padding-bottom: 50px;
}

.result-image {
    height: 400px;
    width: 400px;
    margin: auto;
}

button {
    width: 100px;
    margin: 0 auto;
}

video{
    margin: 0 auto;
}
</style>