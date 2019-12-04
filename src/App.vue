<template>
  <div id="app">
    <Header :numCorrect = "numCorrect"/>
    
    <b-container class="bv-example-row">
      <b-row>
        <b-col sm="6" offset="3">
          <QuestionBox 
          v-if="questions.length > 0"
          :currentQuestion="questions[index]"
          :next = "next"
          :num = "num"
          :justI = "justI"
          
          />
        </b-col>
      </b-row>
    </b-container>
  </div>
</template>

<script>
import Header from './components/Header.vue'
import QuestionBox from './components/QuestionBox.vue'


export default {
  name: 'app',
  components: {
    Header,
    QuestionBox
  },
  methods:{
    next(){
      this.index++ 
      this.justI=false
    },
    num(tof){
      if(tof){
        this.numCorrect++
        this.justI = true
      }
    }
  },
  data(){
      return{
        questions: [],
        index: 0,
        numCorrect: 0,
        justI :false
      }
    },
  
  mounted: function(){
    this.numCorrect=0
    fetch("https://opentdb.com/api.php?amount=10&category=15",
    {
      method: 'get'
    })
    
    .then((response)=>{
      return response.json()
    })
    .then((jsonData)=>{
      this.questions = jsonData.results
    })
  },
  incrementNumCorrect(value){
    this.numCorrect = value
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
