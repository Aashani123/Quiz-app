<script setup>
import {ref,computed}from 'vue'

const questions=ref([
  {
    question:'What is the difference between public cloud and private cloud? ',
    answer:0,
    options:[
      'Public cloud is less secure than private cloud.',
      'Private cloud is more expensive than public cloud.',
      'Public cloud is owned by a single organization.',
      'Private cloud is dedicated to a single organization.'
    ],
    selected:null

  },

  {
    question:'Which cloud service model involves the highest level of control for the user? ',
    answer:3, //this is the answered option index;
    options:[
      ' Infrastructure as a Service (IaaS)',
      'Software as a Service (SaaS)',
      'Platform as a Service (PaaS)',
      'None of the above '
    ],
    selected:null

  },

  {
    question:'What is a hybrid cloud?',
    answer:0, //this is the answered option index;
    options:[
      'A combination of public and private clouds.',
      'A cloud service that is shared by multiple organizations.',
      'A cloud service that is hosted on a physical server.',
      'A cloud service that is only accessible from a single device.'
    ],
    selected:null
  },

  {
    question:'What is a virtual machine in cloud computing?',
    answer:1, //this is the answered option index;
    options:[
      'A physical server that is divided into multiple logical servers.',
      'A software implementation of a computer that behaves like a physical computer.',
      'A cloud service that provides access to a single application.',
      'A network device that connects multiple servers in a cloud environment.'
    ],
    selected:null

  },

  {
    question:'What is serverless computing?',
    answer:2, //this is the answered option index;
    options:[
      'A cloud service that eliminates the need for physical servers.',
      'A software implementation of a computer that behaves like a physical computer.',
      'A cloud service that allows users to deploy applications without managing server infrastructure.',
      'A cloud service that provides server management tools to users.'
    ],
    selected:null

  },

])

const quizcompleted=ref(false)
const currentQuestion=ref(0)
const score=computed(()=>{
  let value=0
  questions.value.map(q=>{
    if(q.selected==q.answer){
      value++
    }
  })
  return value
})

const getcurrentQuestion=computed(()=>{
  let question=questions.value[currentQuestion.value]
  question.index=currentQuestion.value
  return question
})

const SetAnswer= evt =>{
   questions.value[currentQuestion.value].selected=evt.target.value
   evt.target.value=null
}

const NextQuestion=()=>{
  if(currentQuestion.value<questions.value.length-1){
    currentQuestion.value++
  }
  else{
    quizcompleted.value=true
  }
} 

</script>

<template>
     <main class="app">
      <h1>Quiz related to Cloud Computing</h1>
      <section class="quiz" v-if="!quizcompleted">
        <div class="quiz-info">
          <span class="question">{{ getcurrentQuestion.question }}</span>
          <span class="score">Score {{ score }}/{{ questions.length }}</span>
        </div>

        <div class="options">
          <label v-for="(option,index) in getcurrentQuestion.options" 
          :key="index"
          :class="`option ${
            getcurrentQuestion.selected==index
              ?index==getcurrentQuestion.answer
                    ?'correct'
                    :'wrong'
              :''
         } ${
           getcurrentQuestion.selected!=null &&
           index!=getcurrentQuestion.selected
                ?'disabled'
                :''
         }`">
            <input 
            type="radio" 
            :name="getcurrentQuestion.index"
            :value="index"
            v-model="getcurrentQuestion.selected"
            :disabled="getcurrentQuestion.selected"
            @change="SetAnswer">
            <span>{{ option }}</span>
          </label>
        </div>

        <button
          @click="NextQuestion"
          :disabled="!getcurrentQuestion.selected">
          {{ 
             getcurrentQuestion.index==questions.length-1
                 ?'Finish'
                 :getcurrentQuestion.selected==null
                     ?'Select an option'
                     :'Next Question'
          }}
        </button>
      </section>

      <section v-else>
        <h2>You finished the quiz</h2>

        <p>Your Score is {{ score }}/{{ questions.length }}</p>

      </section>
     </main>
</template>

<style>
*{
  margin:0;
  padding: 0;
  box-sizing: border-box;
  font-family: 'Montserrat',sans-serif;

}

body{
  background-color:rgba(73, 8, 80, 0.362) ;
  color: aliceblue;

}

.app{
  display: flex;
  flex-direction: column;
  align-items: center;
  padding: 2rem;
  min-height: 100vh;
}

h1{
  font-size: 2rem;
  margin-bottom: 2rem;
} 

.quiz{
  background-color:rgb(48, 10, 46);
  padding: 1rem;
  width: 100% ;
  max-width:640px ;
  border-radius: 0.5rem;
}

.quiz-info{
  display: flex;
  justify-content: space-between;
  margin-bottom: 1 rem;
}

.quiz-info .question{
  color:thistle;
  font-size: 1.25rem;
}

.quiz-info .score{
  color: antiquewhite;
  font-size: 1.25rem;
}

.options{
  margin-bottom: 1rem;
}

.option{
  display: block;
  padding: 1rem;
  background-color:rgba(113, 13, 124, 0.571);
  margin-bottom: 0.5rem;
  border-radius: 0.5rem;
  cursor: pointer;

}

.option:hover{
  background-color: rgb(179, 54, 196);

}

.option.correct{
  background-color: rgb(215, 117, 230);
}

.option.wrong {
  background-color: rgb(234, 101, 103);
}

.option:last-of-type{
  margin-bottom: 0;
}

.option.disabled{
  opacity: 0.5;
}

.option.input{
  display: none;
}

button{
  appearance: none;
  outline: none;
  border: none;
  cursor: pointer;

  padding: 0.5rem 1rem;
  background-color: rgb(101, 3, 143);
  color: rgb(175, 219, 235);
  font-weight: 700;
  text-transform: uppercase;
  font-size: 1.25rem;
  border-radius: 0.5rem;
}

button:disabled{
  opacity: 0.5;
}

h2{
  font-size: 2rem;
  margin-bottom: 2rem;
  text-align: center;
}

p{
  color:beige;
  font-size: 1.25rem;
  text-align: center;
}
</style>
