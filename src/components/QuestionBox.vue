<template>
    <div class = "question_box_container">
        <b-jumbotron>

            <template slot = "lead">
                {{ currentQuestion.question }}
            </template>

            <hr class = "my-4">

            <b-list-group>
                <b-list-group-item
                v-for = "(answer, index) in answers"
                :key = "index"
                @click = "selectAnswer(index)"
                :class = "answerClass(index)"
                >
                    {{ answer }}
                </b-list-group-item>
            </b-list-group>

            <p>
                
            </p>

            <b-button 
                variant = "primary"
                @click = "submitAnswer"
                :disabled = "selectedIndex === null || answered"
                >
                Submit
            </b-button>
            <b-button @click = "next" variant = "success" href = "#">Next</b-button>
        </b-jumbotron>
    </div>
</template>

<script>
import _ from 'lodash'
    export default {
        props : {
            currentQuestion : Object,
            next: Function,
            increment : Function
        },
        data(){
            return{
                selectedIndex : null,
                correctIndex : null,
                shuffledAnswers : [],
                answered : false
            }
        },
        computed : {
            answers() {
                let answers = [...this.currentQuestion.incorrect_answers]
                answers.push(this.currentQuestion.correct_answer)
                return answers
            }
        },
        watch :{
            currentQuestion : {
                immediate : true,                //will run handler function first when currentQuestion() is passed as props and also everytime currentQuestion() gets updated
                handler(){
                    this.selectedIndex = null
                    this.answered = false
                    this.shuffleAnswers()
                }
            }
            // (){
            //     this.selectedIndex = null
            //     this.shuffleAnswers()
            // }
        },
        methods:{
            selectAnswer(index){
                this.selectedIndex = index
            },
            shuffleAnswers() {
                let answers = [...this.currentQuestion.incorrect_answers, this.currentQuestion.correct_answer]
                this.shuffledAnswers = _.shuffle(answers)
                this.correctIndex = this.shuffledAnswers.indexOf(this.currentQuestion.correct_answer)
            },
            submitAnswer(){
                let isCorrect = false

                if (this.selectedIndex === this.correctIndex){
                    isCorrect = true
                }
                this.answered = true
                
                this.increment(isCorrect)
            },
            answerClass(index){
                let answerClass = ''

                if(!this.answered && this.selectedIndex === index){
                    answerClass = 'selected'
                } else if (this.answered && this.correctIndex === index){
                    answerClass = 'correct'
                } else if (this.answered && this.selectedIndex === index && this.correctIndex !== index){
                    answerClass = 'incorrect'
                }
            
                return answerClass
            }
        },
        
    }
</script>

<style scoped>
.list-group{
    margin-bottom : 5%;
}

.list-group-item :hover{
    background : silver;
    cursor :pointer;
}
.btn{
    margin : 0 1%;
}

.selected{
    background-color : dodgerblue;
}

.correct{
    background-color : lightgreen;
}

.incorrect{
    background-color : red;
}
</style>