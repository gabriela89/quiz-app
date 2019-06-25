<template>
    <div class="question-box-container">
        <b-jumbotron>

            <template slot="lead">
                {{currentQuestion.question}}
            </template>

            <hr class="my-4">

            <b-list-group>
                <b-list-group-item
                        v-for="(answer, index) in answers"
                        :key="index"
                        @click="selectAnswer(index)"
                        :class="[
                        !answered && selectedIndex === index ? 'selected' :
                        answered && correctIndex === index ? 'correct' :
                        answered && selectedIndex === index && correctIndex !== index ? 'incorrect' : ''

                        ]">
                    {{answer}}
                </b-list-group-item>

            </b-list-group>

            <b-button variant="primary" @click="submitAnswer" :disabled="selectedIndex === null || answered">Submit</b-button>
            <b-button @click="next" variant="success" href="#">Next</b-button>
            <b-button @click="previous" href="#">Previous</b-button>
        </b-jumbotron>
    </div>
</template>
<script>
    import _ from 'lodash'
    export default {
        props:{
            currentQuestion: Object,
            next: Function,
            increment: Function,
            previous: Function
        },
        data: function(){
            return {
                selectedIndex: null,
                correctIndex: null,
                shuffledAnswers: [],
                answered: false
            }
        },
        watch:{
            currentQuestion: {
                immediate: true,
                handler(){
                    this.selectedIndex = null
                    this.shuffleAnswers()
                    this.answered = false
                }
            }
        },
        computed: {
          answers() {
              let answers= [...this.currentQuestion.incorrect_answers]
               answers.push(this.currentQuestion.correct_answer)
              return answers
          }
        },
        methods: {
            selectAnswer(index) {
                this.selectedIndex = index
            },
            submitAnswer() {
                let isCorrect = false;

                if (this.selectedIndex === this.correctIndex) {
                    isCorrect = true;
                }
                this.answered = true;
                this.increment(isCorrect)
            },
            shuffleAnswers() {
                let answers = [...this.currentQuestion.incorrect_answers, this.currentQuestion.correct_answer]
                this.shuffledAnswers = _.shuffle(answers)
                this.correctIndex = this.shuffledAnswers.indexOf(this.currentQuestion.correct_answer)

            }
        }
    }
</script>

<style>
    .list-group{
        padding-bottom: 20px;
    }
    .list-group-item:hover{
        background: #e9ecef;
        cursor: pointer;
    }
    .btn-primary{
        margin-right: 20px;
    }
    .btn-success{
        margin-right: 20px;
    }
    .selected{
        background-color: lightblue;
    }
    .correct{
        background-color: lightgreen;
    }
    .incorrect{
        background-color: red;
    }

</style>
