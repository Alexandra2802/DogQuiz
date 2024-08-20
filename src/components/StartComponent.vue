<template>
    <div class="start-container">
        <div class="subtitle">Test your knowledge of dog breeds with our</div>
        <div class="title">Dog Quiz</div>
        <div class="instructions-title">All you have to do is:</div>
        <ul>
            <li>match the dog image to the breed to win points</li>
            <li>only one answer is correct</li>
            <li>you have 15 seconds for each question</li>
            <li>if the time runs out you get 0 points</li>
        </ul>
        <div class="button-container">
            <button @click="$emit('start-quiz')">Start</button>
        </div>
    </div>
</template>

<script>
export default {
    name: 'StartComponent',
    emits: ['start-quiz']
}
</script>

<style scoped>
.start-container {
    /* background-image: url('../assets/start-page-bg.png');
    width: 100%;
    height: 100vh;
    background-size: cover;
    background-position: center; */
    display: flex;
    flex-direction: column;
    justify-content: center;

}

/* @media (max-width: 768px) {
  .start-container {
    background-position: top; 
    background-size: contain; 
} */

 .subtitle{
    font-size: x-large;
    margin: 0 auto;
    margin-top: 50px;
 }

 .title{
    margin: 0 auto;
    margin-top: 50px;
    font-size: xxx-large;
 }

 .instructions-title{
    margin: 0 auto;
    margin-top: 20px;
    font-size: large;
 }

 ul{
    margin: 0 auto;
    margin-top: 30px;
    font-size: larger;
 }

 .button-container{
    margin: 0 auto;
    margin-top: 40px;
    display: flex;
    z-index: 1;
 }
</style>