<template>
  <div id="app" class="background mb-4">
    <div class="container">
      <header id="logo">
        <img class="logo" src="./assets/Pokemon (1).svg" alt="Logo Pokémon" />
        <h2>¿Quién es ese Pokémon?</h2>
        <h5>Pokémones Descubiertos: {{ contador }} / 82</h5>
      </header>
      <main>
        <div class="row">
          <div
            class="col-12 col-lg-4 col-md-4"
            v-for="(pokemon, index) in pokemones"
            :key="index"
          >
            <CardPoke
              :pokemon="pokemon"
              @pokemonDescubierto="incrementarContador"
            />
          </div>
        </div>
      </main>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import CardPoke from "./components/CardPoke.vue";

export default {
  name: "App",
  components: {
    CardPoke,
  },
  data() {
    return {
      pokemones: [],
      contador: 0,
    };
  },
  methods: {
    async getPoke() {
      try {
        let response = await axios.get(
          "https://pokeapi.co/api/v2/pokemon/?offset=80&limit=82"
        );
        this.pokemones = response.data.results;
      } catch (error) {
        console.log(error);
      }
    },
    incrementarContador() {
      this.contador++;
    },
  },
  mounted() {
    this.getPoke();
  },
};
</script>

<style scoped>
* {
  font-family: "Pixelify Sans", sans-serif;
}
#app {
  text-align: center;
}
.background {
  background-image: url("https://wallpaper.forfun.com/fetch/7b/7b18be1395d99e3035b310dfc109cb40.jpeg?w=1470&r=0.5625");
  background-position: center;
  background-attachment: fixed;
  background-size: cover;
}
#logo {
  padding-top: 50px;
}
.logo {
  transition: transform 0.3s ease; /* Añade una transición suave para el hover */
}
.logo:hover {
  transform: scale(1.5); /* Agranda la imagen al 120% de su tamaño original */
}
h2 {
  font-size: 55px;
  color: rgb(21, 21, 133);
}
h5 {
  font-size: 35px;
  color: rgb(252, 215, 6);
}
</style>
