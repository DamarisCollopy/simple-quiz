<template >
  <div class="container">
    <div>
        <b-jumbotron bg-variant="success" text-variant="white" border-variant="dark">
        <template #header>Teste</template>

        <template #lead>
         Teste seu perfil de investidor
        </template>

        <hr class="my-4">
      </b-jumbotron>
    </div>
    
    <div class="mt-3">
      <b-card-group deck>
      <b-card
        border-variant="warning"
        text-variant="white"
        header= "Questao"
        header-bg-variant="success"
        align="center">
    <div>
    <b-card no-body class="text-center">
      <div class="bg-secondary text-light" v-for="item in questions" v-bind:key="item.title">
      <b-form-group 
      class="questions" 
      v-slot="{ ariaDescribedby }"  
      name="radio-options-slots" >
        <label class='questions'>{{item.title}}</label>
          <b-form-radio 
          class="answers" 
          id="radio-slots"
          v-model="item.selectedAnswer"
          :aria-describedby="ariaDescribedby"
          v-for="answer in item.answers" :key="answer.answer"
          v-bind:value="answer.value" >
          {{answer.answer}}
          </b-form-radio>
      </b-form-group>
      <div class="mt-3">Selected: <strong> {{item.selectedAnswer}}</strong></div>
      </div>
    </b-card>
    </div>
    </b-card>
    </b-card-group>
    <b-button variant="success">Next</b-button>
      <div class="mt-3">Total points: <strong> {{total}}</strong></div>
    </div>
  </div>
</template>

<script>

export default {
  name: 'App',
  components: {

  },
  data() {
      return {
        index: 0,
        questions: [
          {id: 1, title:'Qual a principal finalidade de investir seu patrimônio conosco?',
            answers:[
              {id: 1, answer:'Preservar meu patrimônio assumindo um risco menor', value: 1},
              {id: 2, answer:'Uma combinação entre preservação do meu patrimônio e sua valorização.',value: 2},
              {id: 3, answer:'Maximizar o potencial de ganho assumindo um risco maior.', value: 2}
            ], selectedAnswer: '',
          },
          {
            id:2,
            title:'Por quanto tempo pretende deixar seus recursos investidos conosco?',
            answers:[
              {id: 1, answer:'Até 1 ano.', value:1},
              {id: 2, answer:'1 a 5 anos.',value:2},
              {id: 3, answer:'Mais de 5 anos.', value:3},
              {id: 4, answer:'Essa reserva não será utilizada, a não ser em caso de emergência.', value:4}
            ], selectedAnswer: '',
          },
          {
            id:3,
            title:'Em relação aos seus investimentos na SLW, qual é a necessidade futura dos recursos aplicados?',
            answers:[
              {id: 1, answer:'Preciso desse dinheiro como complemento de renda', value:1},
              {id: 2, answer:'Eventualmente posso precisar utilizar uma parte dele.',value:2},
              {id: 3, answer:'3 Não tenho necessidade imediata desse dinheiro.', value:3}
            ], selectedAnswer: '',
          },
        ],
        selected:[]
        
      }
    },
  computed: {
    total() {
    let value = 0;
    this.questions.forEach(x => {value += x.selectedAnswer ? x.selectedAnswer.value : 0;   
    });
    return value;
    }
  },

  methods: {
    showAnswer(selectedAnswer) {
      if(selectedAnswer) {
      return 'Selected: ' + selectedAnswer.answer + ', points: ' + selectedAnswer.value
      }
      return '';
    }
  }
}
</script>

