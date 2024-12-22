<template>
  <div id="app">
    <div class="title">
      <h1>Piedra, Papel o ¡Revancha!</h1>
    </div>
    <div class="score-table">
      <div class="score-row">
          <h2>Puntaje</h2>
        </div>
      <div class="score-column">
        <div class="score-row">
          <h3>{{user}}</h3>
        </div>
        <div class="score-row">
          <h3>Computadora</h3>
        </div>
        <div class="score-row">
          <h4>{{user_score}}</h4>
        </div>
        <div class="score-row">
          <h4>{{computer_score}}</h4>
        </div>
      </div>
    </div>
    <div v-if="!gameOver">
      <h3>Elige una opción:</h3>
      <div class="choices">
        <button @click="play('piedra')">Piedra</button>
        <button @click="play('papel')">Papel</button>
        <button @click="play('tijera')">Tijera</button>
      </div>
    </div>
    
    <div v-if="gameOver">
      <h3>Tu elección: {{ userChoice }} | Opción de la computadora: {{ computerChoice }}</h3>
      <div v-if="result_Gain">
        <div class="resultGain">
          <h3>{{ resultMessage }}</h3>
        </div>
      </div>
      <div v-else-if="result_Draw">
        <div class="resultDraw">
          <h3>{{ resultMessage }}</h3>
        </div>
      </div>
      <div v-else>
        <div class="resultLost">
          <h3>{{ resultMessage }}</h3>
        </div>
      </div>
      <div v-if="!game_over">
        <button @click="tryAgain">Intentar de nuevo</button>
      </div>
      <div v-else>
        <button @click="restartGame">Jugar de nuevo</button>
      </div>
    </div>
  </div>
</template>

<script>
import { ref, computed } from 'vue';

export default {
  setup() {
    const userChoice = ref('');
    const computerChoice = ref('');
    const resultMessage = ref('');
    const gameOver = ref(false);
    const result_Gain = ref(false); 
    const result_Draw = ref(false); 
    const user_score = ref(0);
    const computer_score = ref(0);
    const game_over = ref(false);
    const counter = ref(0);
    const user = prompt("Introduce tu nombre:");
    // Opciones disponibles
    const choices = ['piedra', 'papel', 'tijera'];

    // Lógica para jugar
    const play = (choice) => {
      userChoice.value = choice;
      computerChoice.value = choices[Math.floor(Math.random() * choices.length)];
      determineWinner();
      gameOver.value = true;
    };

    // Función para determinar el ganador
    const determineWinner = () => {
      if (userChoice.value === computerChoice.value) {
        resultMessage.value = '¡Empate!';
        result_Draw.value = true;
        result_Gain.value = false;
      } else if (
        (userChoice.value === 'piedra' && computerChoice.value === 'tijera') ||
        (userChoice.value === 'papel' && computerChoice.value === 'piedra') ||
        (userChoice.value === 'tijera' && computerChoice.value === 'papel')
      ) {
        resultMessage.value = '¡Ganaste!';
        result_Gain.value = true;
        result_Draw.value = false;
        user_score.value++;
      } else {
        resultMessage.value = '¡Perdiste!';
        result_Gain.value = false;
        result_Draw.value = false;
        computer_score.value++;
      }
      counter.value++;
      if((user_score.value === 3 || computer_score.value === 3) && counter.value >= 2){
        if(user_score.value === 3){
          alert("¡Ganaste!");
        }else if(computer_score.value === 3){
          alert("¡Perdiste!");
        }
        game_over.value = true;
        counter.value = 0;
        gameOver.value = true;
        user_score.value = 0;
        computer_score.value = 0;
      }else{
        alert(game_over.value);
        game_over.value = false;
      }
    };

    // Función para reiniciar el juego
    const restartGame = () => {
      userChoice.value = '';
      computerChoice.value = '';
      resultMessage.value = '';
      gameOver.value = true;
    };
    
    // Función para intentar de nuevo
    const tryAgain = () => {
      userChoice.value = '';
      computerChoice.value = '';
      resultMessage.value = '';
      gameOver.value = false;
    };

    // Determina la clase que se aplicará en función del resultado
    const resultClass = computed(() => {
      if (result_Gain.value) {
        return 'resultGain';
      } else if (result_Draw.value) {
        return 'resultDraw';
      } else {
        return 'resultLost';
      }
    });

    return {
      user,
      user_score,
      computer_score,
      counter,
      result_Gain,
      result_Draw,
      userChoice,
      computerChoice,
      resultMessage,
      gameOver,
      play,
      restartGame,
      tryAgain,
      resultClass
    };
  }
};
</script>

<style>
body {
  background-color: gray;
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  text-align: center;
  margin: 30px;
  padding: 30px;
  background-color: white;
  box-shadow: 2px 2px 2px 1px rgba(0, 0.1, 0.2, 0.3);
  border-radius: 40px;
}

.title {
  margin: 10px;
  padding: 10px;
  border-radius: 10px;
  background-color: skyblue;
  color: white;
  text-shadow: 2px 2px 5px rgba(0, 0, 0, 0.9);
  box-shadow: 2px 2px 2px 1px rgba(0, 0.1, 0.2, 0.3);
}

.choices button {
  padding: 20px 30px;
  font-size: 18px;
  margin: 10px;
  cursor: pointer;
  background-color: #42b983;
  border: none;
  color: white;
  border-radius: 15px;
}

.choices button:hover {
  background-color: #369b73;
}

button {
  padding: 10px 20px;
  font-size: 18px;
  cursor: pointer;
  background-color: #42b983;
  border: none;
  color: white;
  border-radius: 5px;
}

button:hover {
  background-color: #369b73;
}

h3 {
  font-size: 20px;
  margin-top: 20px;
}

.resultGain {
  background-color: green;
  color: white;
  margin: 10px;
  padding: 10px;
  border-radius: 10px;
}

.resultLost {
  background-color: red;
  color: white;
  margin: 10px;
  padding: 10px;
  border-radius: 10px;
}

.resultDraw {
  background-color: yellow;
  color: black;
  margin: 10px;
  padding: 10px;
  border-radius: 10px;
}

.score-table {
  transform: scale(0.7);
  background-color: skyblue;
  border: 1px solid #000;
}

.score-column{
  display: grid;
  grid-template-columns: 0.5fr 0.5fr;
  border: 1px solid #000;
}

.score-row{
  display: grid;
  grid-template-rows: 0.5fr;
  border: 2px solid #000;
}
</style>
