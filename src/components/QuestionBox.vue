<template>
    <div>
        <b-jumbotron >
            <template slot="lead">

            {{ currentQuestion.question }}

             <hr class="my-4" />
             <b-list-group>
                 <b-list-group-item
                 v-for="(answer,index) in ans" :key="index"
                 @click.prevent="selectAnswer(index)" :class="viewAns(index)">
                {{ answer}}  
                 </b-list-group-item>
             </b-list-group>
            
            </template>

            <b-button variant="primary" herf="#"  @click="decrement" :disabled="index==0">prev </b-button>   &nbsp;   
            <b-button variant="success" herf="#" @click="increment" :disabled="index==9">next </b-button><br><br>
            <b-button variant="primary" @click="submitAnswer" :disabled="selectedIndex == null || answered"> submit </b-button>      

        </b-jumbotron>
    </div>
</template>

<script>
import _ from 'lodash'
export default {
  props:{
   'currentQuestion':Object,
   increment:Function,
   decrement:Function,
   countQ:Function,
   index:Number
  },
  watch:
  {
      currentQuestion:
      {
      
      immediate:true,
      handler()
      {

        this.selectedIndex = null;
        this.answered = false;
        this.shuffleAnswer()
      
      }
      }
      
  }, 
  data()
  {
   return {
       selectedIndex:null,
       shuffledAnswer:[],
       correctIndex:null,
       answered:false
   }
  },
  methods:
  {
   selectAnswer:function(index)
   {

   this.selectedIndex = index;
   
   
   
   
   },
    submitAnswer:function()
    {

    let isCorrect = false;

    if(this.selectedIndex == this.correctIndex){


     isCorrect = true;
    }
    this.answered = true;

     this.countQ(isCorrect) 
     return isCorrect;

    },
    viewAns:function(index)
    {

     let result = '';

  if(!this.answered && this.selectedIndex === index)
   result = 'selected'
   else if(this.answered && this.correctIndex === index )
   result = 'correct'
   else if(this.answered && this.selectedIndex === index && this.correctIndex !== index)
    result = 'incorrect'
   return result;
   },
   shuffleAnswer:function()
   {
       let answers = [...this.currentQuestion.incorrect_answers,this.currentQuestion.correct_answer];
       this.shuffledAnswer =_.shuffle(answers);
       this.correctIndex = this.shuffledAnswer.indexOf(this.currentQuestion.correct_answer);

       

   }
  },
  computed:
   {
     ans()
     {
       let  answers =[...this.currentQuestion.incorrect_answers];

       answers.push(this.currentQuestion.correct_answer);
       return answers;
     }
   },
   mounted()
   {
       this.shuffleAnswer();
   }  
}
</script>
<style scoped>
.list-group
{
    margin-bottom:15px;

}
.list-group-item:hover{
 background:#eee;
 cursor:pointer;   
}
.selected
{
    background:lightblue;

}
.correct
{
    background:lightgreen;
}
.incorrect
{
    background:red;
}
.b-button
{
margin:0 5px;
}
</style>