<template>
  <div class="container">
    <ResultComponent
      v-if="showResult"
      :numberOfQuestions="numberOfQuestions"
      :score="correctAnswers"
      @retry="showResult = false"/>
    <QuizComponent
      v-else
      @finished="showResult = true"
      @result="(score) => correctAnswers = score"
      :numberOfQuestions="numberOfQuestions"/>
      <div class="footer"></div>
  </div>
</template>

<script>
import QuizComponent from './components/QuizComponent.vue';
import ResultComponent from './components/ResultComponent.vue';

export default {
  name: 'App',
  data(){
    return {
      showResult: false,
      numberOfQuestions: 3,
      correctAnswers: 0
    }
  },
  components:{
    QuizComponent,
    ResultComponent
  }
}
</script>

<style>
*{
    margin: 0;
    padding: 0;
}

.container{
    width: 100%;
    height: 100%;
    background-color: #FCF6E7;
    display: flex;
    flex-direction: column;
    min-height: 100vh;
}

.logo{
    height: 100%;
}

.footer{
    background-image: url('./assets/pawprints2-removebg-preview.png');
    height: 110px;
    margin-top: auto;
}

button{
  align-items: center;
  background-color: #FADBD7;
  border: 2px solid #111;
  border-radius: 8px;
  box-sizing: border-box;
  color: #111;
  cursor: pointer;
  display: flex;
  font-family: Inter,sans-serif;
  font-size: 16px;
  height: 48px;
  justify-content: center;
  line-height: 24px;
  max-width: 100%;
  padding: 0 25px;
  position: relative;
  text-align: center;
  text-decoration: none;
  user-select: none;
  touch-action: manipulation;
  margin-bottom: 15px;
  /* z-index: 1; */
}


button:after {
  background-color: #111;
  border-radius: 8px;
  content: "";
  display: block;
  height: 48px;
  width: 100%;
  position: absolute;
  left: 0;
  top: -2px;
  z-index: -1;
  transform: translate(8px, 8px);
  transition: transform .2s ease-out;
}

button:hover:after {
  transform: translate(0, 0);
}

button:active {
  background-color: #FFDEDA;
  outline: 0;
}

button:disabled{
    background-color: white;
    color: black;
    cursor:default;
}

button:hover {
  outline: 0;
}

.buttons-container{
    display: flex;
    justify-content: space-between;
    z-index:1;
}
</style>