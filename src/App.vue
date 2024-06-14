<template>
   <div class="container" v-if="empezarJuego">
    <div class="head">
      <h1>Juego Pokemon 2024</h1>
      <h2>Puntaje: {{ puntaje }}</h2>
      <h2>Intento: {{ intento }}</h2>
    </div>
    <JuegoPokemon :texto="text1" :urlImg="url1" />
    <JuegoPokemon :texto="text2" :urlImg="url2" />
    <JuegoPokemon :texto="text3" :urlImg="url3" />
    <button class="jugar" @click="jugar">Jugar</button>
  </div>
  <!--Cuadro dialogos si pasa los 5 intentos-->
  <div class="loose" v-if="verPerdedor">
    <h1>Has utilizado tus 5 intentos</h1>
    <h1>El juego ha termindo, intentalo nuevamente</h1>
    <button class="new_game" @click="reiniciar">Nuevo Juego</button>
  </div>
   <!--Cuadro dialogos si gana con puntaje =>10-->
  <div class="win" v-if="verGanador">
    <h1>Puntaje: {{ puntaje }}</h1>
    <h1>Felicitaciones has ganado un premio de $10.000,00 dolares</h1>
    <button class="new_game" @click="reiniciar">Nuevo Juego</button>
  </div>
  
</template>

<script>
import JuegoPokemon from './components/JuegoPokemon.vue';


export default {
  name: "App",
  components: {
    JuegoPokemon,
  },
  data() {
    return {
      puntaje: 0,
      intento: 0,
      url1: "https://via.placeholder.com/250",
      url2: "https://via.placeholder.com/250",
      url3: "https://via.placeholder.com/250",
      text1: "xxxxxxxxxx",
      text2: "xxxxxxxxxx",
      text3: "xxxxxxxxxx",
      verGanador: false,
      verPerdedor: false,
      empezarJuego: true,
      
      
    };
  },
  methods: {
    async jugar() {
      var data = [
        await this.consumirAPI(),
        await this.consumirAPI(),
        await this.consumirAPI(),
      ];
      this.text1 = data[0].pokemon;
      this.text2 = data[1].pokemon;
      this.text3 = data[2].pokemon;
      this.url1 = "https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/other/dream-world/1.svg";
      this.url2 = "https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/other/dream-world/3.svg";
      this.url3 = "https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/other/dream-world/9.svg";
      this.url1 = "https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/other/dream-world/9.svg";
      this.url2 = "https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/other/dream-world/10.svg";
      this.url3 = "https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/other/dream-world/8.svg";

      this.evaluarResultado();
    },
    async consumirAPI() {
      return await fetch("https://pokeapi.co/api/v2").then((r) => r.json());
    },
    evaluarResultado() {
      this.intento++;
      var aux = 0;

      if (this.text1 === "bulbasur") aux++;
      if (this.text2 === "bulbasur") aux++;
      if (this.text3 === "bulbasur") aux++;

      if (aux === 3) this.puntaje += 5;
      if (aux === 2) this.puntaje += 2;
      if (aux === 1) this.puntaje += 1;

      if (this.puntaje >= 10) {
        this.verGanador = true;
        this.verPerdedor = false;
        this.empezarJuego = false;
      }
      if (this.intento >= 5) {
        this.verPerdedor = true;
        this.verGanador = false;
        this.empezarJuego = false;
      }
    },
    reiniciar() {
      this.puntaje = 0;
      this.intento = 0;
      this.url1 =
        "https://via.placeholder.com/250";
      this.url2 =
        "https://via.placeholder.com/250";
      this.url3 =
        "https://via.placeholder.com/250";
      this.text1 = "xxxxxxxxxx";
      this.text2 = "xxxxxxxxxx";
      this.text3 = "xxxxxxxxxx";
      this.verGanador = false;
      this.verPerdedor = false;
      this.empezarJuego = true;
    },
  },
};
</script>
<style>
img {
  height: 250px;
  width: 250px;
}

body{
  font-size: 20px;
}

.container {
  display: grid;
  grid-template-columns: repeat(3, 300);
  justify-content: center;
  background: whitesmoke;
  width: auto;
  justify-content: center;
  text-align: center;
  margin: 50px;
  border-radius: 15px;
  padding: 30px;
}
.head {
  grid-column: span 3;
  display: grid;
  grid-template-columns: repeat(2, 450px);
}
h1,
.jugar {
  grid-column: span 3;
}

.win,
.loose {
  background: whitesmoke;
  width: auto;
  justify-content: center;
  text-align: center;
  margin: 50px;
  border-radius: 15px;
  padding: 30px;
}

.win {
  color: blue;
}

.new_game {
  width: 300px;
}

button{
    margin: 30px 10px;
    padding: 20px;
    font-size: 30px;
    font-weight: bold;
}

.loose {
  color: red;
}
</style>
