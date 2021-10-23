<template >
  <div class="container">
    <div>
      <b-jumbotron
        bg-variant="success"
        text-variant="white"
        border-variant="dark"
      >
        <template #header>Teste</template>

        <template #lead> Teste seu perfil de investidor </template>

        <hr class="my-4" />
      </b-jumbotron>
    </div>

    <div class="mt-3">
      <b-card-group deck>
        <b-card
          border-variant="warning"
          text-variant="white"
          header="Quiz"
          header-bg-variant="success"
          align="center"
        >
          <h2 class="h6 bg-success text-light">
            Questao {{ b }}/{{ questions.length }}
          </h2>
          <div>
            <b-card class="text-center" v-show="quiz">
              <div
                class="card text-dark bg-light text-sm-start fs-6 mb-3"
                v-for="item in questions.slice(a, b)"
                v-bind:key="item.title"
              >
                <b-form-group
                  class="questions"
                  v-slot="{ ariaDescribedby }"
                  name="radio-options-slots"
                >
                  <label class="questions">{{ item.title }}</label>
                  <b-form-radio
                    class="answers mt-3"
                    id="radio-slots"
                    v-model="item.selectedAnswer"
                    :aria-describedby="ariaDescribedby"
                    v-for="answer in item.answers"
                    :key="answer.answer"
                    v-bind:value="answer.value"
                  >
                    <p>{{ answer.answer }}</p>
                  </b-form-radio>
                </b-form-group>
              </div>
            </b-card>
            <div class="mt-3 questions card" v-if="score_show">
              <div
                class="text-warning card-title"
                v-if="total >= 36 && total <= 52"
              >
                <strong>'Perfil de Risco Conservador'</strong>
              </div>
              <div
                class="text-warning card-title"
                v-if="total >= 53 && total <= 65"
              >
                <strong>'Perfil de Risco Moderado'</strong>
              </div>
              <div
                class="text-warning card-title"
                v-if="total >= 66 && total <= 71"
              >
                <strong>'Perfil de Risco Dinâmico'</strong>
              </div>
              <div
                class="text-danger card-title"
                v-if="total >= 72 && total <= 83"
              >
                <strong>'Perfil de Risco Arrojado'</strong>
              </div>
              <div
                class="text-danger card-title"
                v-if="total >= 84 && total <= 104"
              >
                <strong>'Perfil de Risco Agressivo'</strong>
              </div>
              <div class="text-info card-title">
                <strong>Pontos : {{ total }}</strong>
              </div>
              <b-button
                class="mt-3 btn-lg"
                variant="success"
                @click="restartQuiz"
                >Restart</b-button
              >
            </div>
          </div>
        </b-card>
      </b-card-group>
      <div class="d-grid gap-2 d-md-flex justify-content-md-end">
        <b-button class="mt-3 btn-lg" variant="success" @click="skipQuestion"
          >Skip</b-button
        >
        <b-button class="mt-3 btn-lg" variant="success" @click="nextQuestion"
          >Next</b-button
        >
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "App",
  components: {},
  data() {
    return {
      index: 0,
      questions: [
        {
          id: 1,
          title:
            "Qual a principal finalidade de investir seu patrimônio conosco?",
          answers: [
            {
              id: 1,
              answer: "Preservar meu patrimônio assumindo um risco menor",
              value: 3,
            },
            {
              id: 2,
              answer:
                "Uma combinação entre preservação do meu patrimônio e sua valorização.",
              value: 6,
            },
            {
              id: 3,
              answer:
                "Maximizar o potencial de ganho assumindo um risco maior.",
              value: 9,
            },
          ],
          selectedAnswer: "",
        },
        {
          id: 2,
          title:
            "Por quanto tempo pretende deixar seus recursos investidos conosco?",
          answers: [
            { id: 1, answer: "Até 1 ano.", value: 2 },
            { id: 2, answer: "1 a 5 anos.", value: 4 },
            { id: 3, answer: "Mais de 5 anos.", value: 6 },
            {
              id: 4,
              answer:
                "Essa reserva não será utilizada, a não ser em caso de emergência.",
              value: 8,
            },
          ],
          selectedAnswer: "",
        },
        {
          id: 3,
          title:
            "Em relação aos seus investimentos na SLW, qual é a necessidade futura dos recursos aplicados?",
          answers: [
            {
              id: 1,
              answer: "Preciso desse dinheiro como complemento de renda",
              value: 3,
            },
            {
              id: 2,
              answer: "Eventualmente posso precisar utilizar uma parte dele.",
              value: 6,
            },
            {
              id: 3,
              answer: "Não tenho necessidade imediata desse dinheiro.",
              value: 9,
            },
          ],
          selectedAnswer: "",
        },
        {
          id: 4,
          title: "Qual a sua renda mensal?",
          answers: [
            { id: 1, answer: "Até R$ 3.000,00.", value: 1 },
            { id: 2, answer: "Entre R$ 3.000,00 e R$ 5.000,00.", value: 2 },
            { id: 3, answer: " Entre R$ 5.000,00 e R$ 10.000,00", value: 3 },
            { id: 4, answer: "Entre R$ 10.000,00 e R$ 30.000,00", value: 4 },
            { id: 5, answer: "Mais de R$ 30.000,00.", value: 5 },
          ],
          selectedAnswer: "",
        },
        {
          id: 5,
          title:
            "Qual percentual da sua renda o (a) Sr.(a) investe regularmente?",
          answers: [
            { id: 1, answer: "Até 10%.", value: 5 },
            { id: 2, answer: "De 10 a 20%.", value: 10 },
            { id: 3, answer: "De 20% a 50%.", value: 15 },
            { id: 4, answer: "Acima de 50%", value: 20 },
          ],
          selectedAnswer: "",
        },
        {
          id: 6,
          title:
            "Por conta de oscilações do mercado, considere que seus investimentos percam 10% do valor aplicado. Neste caso, o que o (a) Sr.(a) faria?",
          answers: [
            { id: 1, answer: "Não sei o que faria.", value: 6 },
            { id: 2, answer: "Venderia toda a posição.", value: 12 },
            { id: 3, answer: "Manteria a posição.", value: 18 },
            { id: 4, answer: "Aumentaria a posição.", value: 24 },
          ],
          selectedAnswer: "",
        },
        {
          id: 7,
          title:
            "Quais dos produtos listados abaixo você tem familiaridade? (Esta questão permite múltiplas respostas. Para fins de cálculo de Perfil, deve ser utilizada a resposta de maior valor de pontuação entre as respostas assinaladas).",
          answers: [
            {
              id: 1,
              answer: "Poupança, Fundos DI, CDB, Fundos de Renda Fixa.",
              value: 2,
            },
            {
              id: 2,
              answer: "Fundos Multimercados, Títulos Públicos, LCI, LCA.",
              value: 4,
            },
            {
              id: 3,
              answer:
                "Fundos de Ações, Ações, Fundos Imobiliários, Debêntures, Fundos Cambiais, Clubes de Investimento.",
              value: 6,
            },
            {
              id: 4,
              answer:
                "Fundos de Investimentos em Participações (FIP), Derivativos (Futuros, Opções e Swaps).",
              value: 8,
            },
          ],
          selectedAnswer: "",
        },
        {
          id: 8,
          title:
            "Quais investimentos você realizou frequentemente nos últimos 24 meses?",
          answers: [
            { id: 1, answer: "Nunca investi. Primeiro aporte.", value: 3 },
            {
              id: 2,
              answer: "Investi apenas em produtos ou fundos de renda fixa",
              value: 6,
            },
            {
              id: 3,
              answer:
                "Investi em produtos ou fundos de renda fixa e/ ou de multimercado e/ou de renda variável e/ou com derivativos comfinalidade de hedge",
              value: 9,
            },
            {
              id: 4,
              answer:
                "Investi em produtos de renda fixa e/ou de multimercado e/ou de renda variável e/ou com derivativos com finalidadede especulação ou alavancagem.",
              value: 12,
            },
          ],
          selectedAnswer: "",
        },
        {
          id: 9,
          title: "Qual é o valor do seu Patrimônio?",
          answers: [
            { id: 1, answer: "Até R$ 20.000,00.", value: 2 },
            { id: 2, answer: "Entre R$ 20.000,01 e R$ 100.000,00.", value: 4 },
            {
              id: 3,
              answer: "Entre R$ 100.000,01 a R$ 1.000.000,00",
              value: 6,
            },
            { id: 4, answer: "Acima de R$ 1.000.000,01.", value: 8 },
          ],
          selectedAnswer: "",
        },
        {
          id: 10,
          title:
            "Como você classificaria a relação de sua formação acadêmica e da sua experiência profissional em relação aosseus conhecimentos sobre o mercado financeiro?",
          answers: [
            {
              id: 1,
              answer:
                "Não tenho formação acadêmica na área financeira, mas desejo operar no mercado de capitais e financeiro.",
              value: 2,
            },
            {
              id: 2,
              answer:
                "Apesar de não ter a formação acadêmica na área financeira possuo experiência no mercado de capitais e financeiro.",
              value: 4,
            },
            {
              id: 3,
              answer:
                "Tenho formação na área financeira e conheço as regras do mercado financeiro.",
              value: 6,
            },
            {
              id: 4,
              answer:
                "Tenho formação acadêmica e experiência profissional na área financeira, por isto conheço profundamente o mercadofinanceiro, como operações de derivativos e estruturadas.",
              value: 8,
            },
          ],
          selectedAnswer: "",
        },
        {
          id: 11,
          title:
            "Qual das respostas abaixo mais se assemelha à sua personalidade como investidor?",
          answers: [
            {
              id: 1,
              answer:
                "Não admito perder nada do capital investido. Procuro um retorno seguro e sem oscilações. Segurança é mais importante do que rentabilidade.",
              value: 2,
            },
            {
              id: 2,
              answer:
                "Não admito perder nada do capital investido, no entanto posso arriscar uma parte do capital para alcançar resultadosmelhores que a renda fixa tradicional. Valorizo mais a segurança do que a rentabilidade. ",
              value: 4,
            },
            {
              id: 3,
              answer:
                "Posso correr riscos para conseguir uma rentabilidade acima da média, no entanto, prezo a preservação de 100% do capital investido. Divido minhas preferências entre segurança e rentabilidade, mas ainda prefiro segurança à rentabilidade.",
              value: 6,
            },
            {
              id: 4,
              answer:
                "Admito perdas de até 20% do capital investido, se a proposta de investimento gerar possibilidade de altos retornos. A procura por rentabilidade é mais importante do que a segurança.",
              value: 8,
            },
            {
              id: 5,
              answer:
                "Minha prioridade é maximizar a rentabilidade, com a segurança em segundo plano. Posso correr grande riscos para obter elevados retornos, admitindo perder mais de 20% do meu capital investido.",
              value: 10,
            },
          ],
          selectedAnswer: "",
        },
      ],
      a: 0,
      b: 1,
      id:0,
      quiz: true,
      score_show: false,
      next: false,
    };
  },
  computed: {
    total() {
      let value = 0;
      this.questions.forEach((x) => {
        value += x.selectedAnswer ? x.selectedAnswer : 0;
      });
      return value;
    },
  },

  methods: {
    nextQuestion() {
      if (this.questions.length - 1 == this.a) {
        this.score_show = true;
        this.quiz = false;
      } else {
        this.a++;
        this.b++;
      }
    },

    skipQuestion() {
      if (this.questions.length - 1 == this.a) {
        this.score_show = true;
        this.quiz = false;
      } else {
        this.a--;
        this.b--;
      }
    },

    restartQuiz() {
      Object.assign(this.$data, this.$options.data());
    },

    showAnswer(selectedAnswer) {
      if (selectedAnswer) {
        return "Selected: " + selectedAnswer + ", points: " + selectedAnswer;
      }
      return "";
    },
  },
};
</script>

