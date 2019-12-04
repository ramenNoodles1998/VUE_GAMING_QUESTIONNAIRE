<template>
    <div>
        <b-jumbotron>
            <template slot="lead">
                {{currentQuestion.question}}
            </template>
            <b-list-group>
                <b-list-group-item 
                    v-for="(answer,index) in shuffleAnswer" 
                    :key="index"
                    @click="selectAnswer(index)"
                    :disabled="justI&&selectedIndex !==index"
                    :class=
                    "[selectedIndex=== index? 'selected': '',
                    correct&&selectedIndex==index&&justI? 'correct': '',
                    incorrect&&selectedIndex==index&&justI? 'incorrect': '']"
                    >
                    {{answer}}
                
                </b-list-group-item>
            </b-list-group>
            
            <b-button 
            v-on:click ="check" 
            :disabled="justI">Submit</b-button>
            <b-button @click="next">Next</b-button>
        </b-jumbotron>
    </div>
</template>
<script>
import _ from 'lodash'
export default {
    props:{
        currentQuestion: Object,
        next: Function,
        num: Function,
        justI: Boolean
    },
    data(){
        return{
            selectedIndex: null,
            shuffleAnswer: [],
            correct: false,
            incorrect: false
        }
    },
    watch:{
        currentQuestion(){
            this.selectedIndex = null
            this.shuffleAnswers()
        }
    },
    methods:{
        selectAnswer(index){
            this.selectedIndex = index
            
            if(this.currentQuestion.correct_answer == this.shuffleAnswer[index]){
                this.correct = true
                this.incorrect = false
   
            }
            else{
                this.correct = false
                this.incorrect = true
            }
            
            
        },
        shuffleAnswers(){

            let answers = [...this.currentQuestion.incorrect_answers, this.currentQuestion.correct_answer]
            this.shuffleAnswer = _.shuffle(answers)
            

        },
        check(){
            
            if(this.currentQuestion.correct_answer == this.shuffleAnswer[this.selectedIndex]){  
                           
                this.num(this.correct)
            }
            else{
                this.justI = true
            }
        }
        
        
    },
    computed:{
        answers(){
            let answers = [...this.currentQuestion.incorrect_answers]
            answers.push(this.currentQuestion.correct_answer)
            return answers
        }
    },
    mounted(){
        this.shuffleAnswers()
        
    }
}
</script>

<style scoped>
.list-group{
    margin-bottom: 15px;

}
.list-group-item:hover{
    background: #eee;
    cursor: pointer;
}
.btn{
    margin: 0 5px;
}
.selected {
    background-color: rgb(103, 103, 199);
}
.correct{
    background: green;
}
.incorrect{
    background: red;
}
</style>