<template>
  <div id="padre">
    <div class="info">
      <p>Intentos: {{ intentos }}</p>
      <p>Puntaje: {{ puntaje }}</p>
    </div>

    <div v-if="finJuego">
      <div :class="{ blue: ganar }" v-if="ganar">
        <p>Felicidades has ganado un premio de $10.000</p>
      </div>
      <button @click="reiniciar">Nuevo Juego</button>
      <div :class="{ red: !ganar }">
        <p v-if="!ganar">Has utilizado tus 5 intentos</p>
        <p v-if="!ganar">El juego ha terminado, inténtalo de nuevo</p>
      </div>
    </div>

    <div v-if="!finJuego">
      <div class="hijo">
        <Hijo
          v-for="(hijo, index) in hijos"
          :key="index"
          :image="hijo.image"
          :hiddenText="hijo.hiddenText"
          :mostrarTexto="true"
        />
      </div>

      <button @click="jugar">JUGAR</button>
    </div>
  </div>
</template>

<script>
import Hijo from "./Hijo.vue";

export default {
  components: {
    Hijo,
  },
  data() {
    return {
      hijos: [
        {
          image:
            "https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/other/dream-world/1.svg",
          hiddenText: "XXXXX",
        },
        {
          image:
            "https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/other/dream-world/2.svg",
          hiddenText: "XXXXX",
        },
        {
          image:
            "https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/other/dream-world/3.svg",
          hiddenText: "XXXXX",
        },
      ],
      puntaje: 0,
      intentos: 0,
      finJuego: false,
      ganar: false,
    };
  },
  methods: {
    async jugar() {
      this.intentos++;
      for (let i = 0; i < this.hijos.length; i++) {
        const res = await fetch("https://yesno.wtf/api");
        const data = await res.json();

        this.hijos[i].image = data.image;
        this.hijos[i].hiddenText = data.answer;
      }
      this.calcularPuntaje();
      this.perdio();
    },

    calcularPuntaje() {
      let yesContador = this.hijos.filter(
        (hijo) => hijo.hiddenText === "yes"
      ).length;

      if (yesContador === 3) {
        this.puntaje += 5;
      } else if (yesContador === 2) {
        this.puntaje += 2;
      } else if (yesContador === 1) {
        this.puntaje += 1;
      } else {
        this.puntaje += 0;
      }
    },

    perdio() {
      if (this.intentos >= 5 || this.puntaje >= 10) {
        this.finJuego = true;
        this.ganar = this.puntaje >= 10;
      }
    },

    reiniciar() {
      this.hijos = [
        {
          image:
            "https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/other/dream-world/1.svg",
          hiddenText: "XXXXX",
        },
        {
          image:
            "https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/other/dream-world/2.svg",
          hiddenText: "XXXXX",
        },
        {
          image:
            "https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/other/dream-world/3.svg",
          hiddenText: "XXXXX",
        },
      ];
      this.puntaje = 0;
      this.intentos = 0;
      this.ganar = false;
      this.finJuego = false;
    },
  },
};
</script>

<style>
.red {
  color: red;
}
.blue {
  color: blue;
}
.mensaje {
  text-align: center;
}

button {
  width: 100px;
  height: 25px;
  border: solid 2px black;
}

.hijo {
  display: flex;
  justify-content: center;
  align-items: center;
}

.info{
display: flex;
justify-content: space-evenly;
}
</style>
