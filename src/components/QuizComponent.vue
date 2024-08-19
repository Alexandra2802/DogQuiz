<template>
    <div class="header">
      <img src="../assets/dog-quiz-logo.png" alt="logo" class="logo">
      <div class="timer" :class="{'red-timer': duration <= 5}">{{ timerText }}</div>
    </div>
    <div class="quiz-container">
      <div class="question">Which of the following dogs is a {{ correctBreed }}?</div>
      <div class="answers-container">
        <div v-for="(dogImage, index) in dogImageUrls" :key="index">
          <img class="answer"
                :src="dogImage" @click="selectAnswer(dogImage, index)"
                :class="{
                    correct: answered && index === correctImageIndex,
                    incorrect: answered && index !== correctImageIndex && selectedImageIndex === index}">
        </div>
      </div>
      <p class="question-number">Question {{currentQuestionNumber}}/{{numberOfQuestions}}</p>
      <div class="buttons-container">
        <button :disabled="!answered" @click="displayNextQuestion">Next</button>
        <button :disabled="answered" @click="displayNextQuestion">Skip</button>
      </div>
    </div>
</template>

<script>
const API_URL = 'https://dog.ceo/api/breeds/image/random';
let countdown;

export default {
    name: 'QuizComponent',
    emits: ['finished', 'result'],
    props: ['numberOfQuestions'],
    data(){
        return {
            dogImageUrls: [],
            breeds: [],
            correctBreed: '',
            correctImageIndex: 0,
            currentQuestionNumber: 1,
            answered: false,
            selectedImageIndex: 0,
            score: 0,
            timerText: '',
            duration: 16
        }
    },
    methods:{
        async fetchDogImage(){
            const response = await fetch(API_URL);
            const data = await response.json();
            return data.message;
        },
        extractBreedFromUrl(url){
            const urlParts = url.split('/');
            const breed = urlParts[urlParts.length - 2];
            const formattedBreed = breed.split('-')
                                        .map(word => word.charAt(0).toUpperCase() + word.slice(1).toLowerCase())
                                        .reverse()
                                        .join(' ');
            return formattedBreed;
        },
        async loadQuestion(){
            this.duration = 16;
            clearInterval(countdown);
            this.startTimer();
            this.answered = false;
            this.dogImageUrls.splice(0, this.dogImageUrls.length);
            this.breeds.splice(0, this.dogImageUrls.length);

            while(this.dogImageUrls.length < 4){
                const dogImage = await this.fetchDogImage();
                const breed = this.extractBreedFromUrl(dogImage);
                if(!this.dogImageUrls.includes(dogImage) && !this.breeds.includes(breed)){
                    this.dogImageUrls.push(dogImage);
                }
            }

            this.correctImageIndex = Math.floor(Math.random()*4);
            const correctImage = this.dogImageUrls[this.correctImageIndex];
            this.correctBreed = this.extractBreedFromUrl(correctImage);
        },
        displayNextQuestion(){
            if(this.currentQuestionNumber < this.numberOfQuestions){
                this.currentQuestionNumber++;
                this.loadQuestion();
            }else{
                this.$emit('finished', true);
                this.$emit('result', this.score);
            }
        },
        selectAnswer(selectedDogImage, index){
            this.answered = true;
            this.selectedImageIndex = index;

            if(this.correctBreed === this.extractBreedFromUrl(selectedDogImage)){
                this.score++;
            }

            clearInterval(countdown);
            this.duration = 16;
        },
        startTimer(){
            countdown = setInterval(() => {
                if(this.duration > 0){
                    this.duration--;
                    this.timerText = `Time left: ${this.duration} seconds`;
                }
                if(this.duration === 0){
                    clearInterval(countdown);
                    this.timerText = 'Time\'s up!';
                    this.displayNextQuestion();
                }
            },1000)
        }
    },
    created(){
        this.loadQuestion();
    }
}
</script>

<style scoped>

.header{
    height: 150px;
}

.timer{
    float: inline-end;
    font-size: x-large;
    padding: 30px;
}

.red-timer{
    color: red;
}

.quiz-container{
    display: flex;
    flex-direction: column;
    width: 50%;
    margin: 0 auto;
}

.answers-container{
    display: grid;
    grid-template-columns: 1fr 1fr;
    grid-template-rows: 1fr 1fr;
    gap: 20px;
}

.answer{
    width: 100%;
    height: 250px;
    cursor: pointer;
    border-radius: 8px;
    border: 2px solid black;
}

.question{
    font-size:xx-large;
    background-color:#fadbd7;
    border-radius: 8px;
    border: 2px solid black;
    padding-block: 20px;
    padding-inline: 10px;
    margin-bottom: 20px;
}

p.question-number{
    font-size: x-large;
    padding-block: 5px;
}

.correct{
    border: 8px solid green;
}

.incorrect{
    border: 8px solid red;
}

@media (max-width: 768px){
    .quiz-container{
        width: 80%;
    }
    .answer{
        height: 200px;
    }
}

@media (max-width: 425px){
    .timer{
        font-size: large;
    }
}

@media (max-width: 375px){
    .timer{
        font-size: medium;
    }

    .logo{
       width: 100px;
       height: 100px;
    }

    .answer{
        height: 150px;
    }
}

</style>