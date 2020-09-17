<template>
  <div id="home">
    <div class="game-container">
      <div class="countdown">
        <h5>{{ countdown }}</h5>
      </div>
      <div class="left-panel panel">
        <h1>jService</h1>
        <h2>a new</h2>
        <h2>point of</h2>
        <h2><em>vue</em></h2>
        <div class="button-container">
          <button @click="fetchQuestion">new question</button>
          <button v-if="questionObj.question" @click="toggleRevealed">
            reveal question
          </button>
        </div>
        <h3 class="score">score: {{ userScore }}</h3>
      </div>
      <div class="right-panel panel">
        <h3 class="category">category: {{ questionObj.category }}</h3>
        <h3 class="points">points: {{ questionObj.points }}</h3>
        <h3 class="answer">answer: {{ questionObj.question }}</h3>
        <h3 class="question">
          question:
          <p v-if="revealed">{{ questionObj.answer }}</p>
        </h3>
        <div v-if="revealed" class="score-buttons-container">
          <button @click="incrementScore">nailed it</button
          ><button @click="decrementScore">blew it</button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "Home",
  data() {
    return {
      questionObj: {},
      revealed: false,
      userScore: 0,
      countdown: "",
    };
  },
  methods: {
    async fetchQuestion() {
      try {
        this.revealed = false;
        let res = await fetch("https://jservice.io/api/random");
        let data = await res.json();
        if (
          !data[0].question ||
          !data[0].answer ||
          !data[0].value ||
          !data[0].category
        ) {
          this.fetchQuesiton();
        }
        this.questionObj = {
          question: data[0].question,
          answer: data[0].answer,
          points: data[0].value,
          category: data[0].category.title,
        };
      } catch (error) {
        console.log(error);
      }
    },
    toggleRevealed() {
      this.revealed = !this.revealed;
    },
    incrementScore() {
      this.userScore += this.questionObj.points;
      this.revealed = false;
      setTimeout(() => {
        this.fetchQuestion();
      }, 3000);
      this.setCountdown();
    },
    decrementScore() {
      this.userScore -= this.questionObj.points;
      this.revealed = false;
      setTimeout(() => {
        this.fetchQuestion();
      }, 3000);
      this.setCountdown();
    },
    setCountdown() {
      this.countdown = "3...";
      setTimeout(() => {
        this.countdown = "2...";
      }, 1000);
      setTimeout(() => {
        this.countdown = "1...";
      }, 2000);
      setTimeout(() => {
        this.countdown = "";
      }, 3000);
    },
  },
};
</script>

<style scoped>
@import url("https://fonts.googleapis.com/css2?family=Roboto:wght@100;400;700&display=swap");

#home {
  height: 100%;
}

.game-container {
  height: 100%;
  background-color: #134074;
  display: flex;
  justify-content: space-around;
}

.countdown {
  position: absolute;
  z-index: 100;
  display: flex;
  justify-content: center;
  align-items: center;
}

.countdown h5 {
  color: white;
  font-size: 10rem;
  font-weight: bold;
  font-family: "Roboto";
}

.panel {
  height: 65%;
  width: 48%;
  padding: 5%;
  display: flex;
  flex-direction: column;
  color: white;
  font-family: "Roboto";
}

.left-panel {
  align-items: center;
  justify-content: center;
  margin-top: 100px;
  background-color: #13315c;
}

.left-panel h1 {
  text-decoration: underline;
  margin: 10px 0;
  font-size: 3rem;
}

.left-panel h2 {
  font-weight: 200;
  margin: 0;
  font-size: 2.5rem;
}

.left-panel h3 {
  font-weight: 200;
  margin: 0;
  font-size: 2rem;
}

.button-container {
  display: flex;
  justify-content: space-between;
  margin: 40px 0 20px 0;
}

.button-container button {
  margin: 10px;
  padding: 10px 0;
  width: 200px;
  color: #0b2545;
  font-weight: bold;
  font-size: 1rem;
}

.right-panel {
  align-items: flex-start;
  justify-content: flex-start;
  margin-top: 200px;
  background-color: #0b2545;
}

.right-panel h3 {
  font-weight: 200;
  font-size: 1.5rem;
  margin: 10px 0;
}

.score-buttons-container {
  display: flex;
  justify-content: space-between;
  margin: 50px auto;
}

.score-buttons-container button {
  margin: 0 5px;
  padding: 5px 0;
  width: 200px;
  color: #0b2545;
  font-weight: bold;
  font-size: 0.8rem;
}
</style>
