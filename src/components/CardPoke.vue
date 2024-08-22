<template>
  <div>
    <div class="text-center">
      <div class="card border-0 mb-3">
        <img
          id="pokeimg"
          :src="imagenPokemon"
          class="card-img-top"
          :class="esconderPokemon ? 'borroso' : ''"
          alt=""
        />
        <div class="card-body">
          <h5 v-if="!esconderPokemon" class="card-title">
            {{ infoPokemon.name }}
          </h5>
          <div v-if="esconderPokemon">
            <input
              v-model="inputName"
              @keyup.enter="descubrirPokemon"
              class="form-control my-3"
              type="text"
              placeholder="Escribe Aquí el nombre del Pokémon."
            />
            <div class="d-grid">
              <button
                @click.prevent="descubrirPokemon"
                class="btn btn-danger fs-3"
              >
                Descubrir
              </button>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
export default {
  name: "CardPoke",
  props: {
    pokemon: Object,
  },
  data() {
    return {
      infoPokemon: {},
      esconderPokemon: true,
      inputName: "",
    };
  },
  computed: {
    imagenPokemon() {
      return this.infoPokemon.sprites?.other["official-artwork"].front_default;
    },
  },

  created() {
    this.getInfoPokemon();
  },
  methods: {
    async getInfoPokemon() {
      try {
        let { data } = await axios.get(this.pokemon.url);
        this.infoPokemon = data;
      } catch (error) {
        console.log(error);
      }
    },
    descubrirPokemon() {
      if (
        this.inputName.trim().toLowerCase() === // Comprobación del nombre ingresado trim()elimina los espacios en blanco en ambos extremos del string.
        this.infoPokemon.name.toLowerCase() // ToLowerCase devuelve el valor en minúsculas de la cadena que realiza la llamada.Comparación con el nombre del Pokémon
      ) {
        // Si el nombre ingresado coincide con el nombre del Pokémon:
        this.esconderPokemon = false; // Se establece esconderPokemon en false para mostrar el nombre del Pokémon
        this.$emit("pokemonDescubierto", this.pokemon); // Se emite un evento al componente padre indicando que el Pokémon ha sido descubierto
      }
      // Si el nombre ingresado no coincide con el nombre del Pokémon:
      else {
        console.log("Nombre incorrecto, intenta nuevamente.");
        const name = this.pokemon.name;
        this.$swal.fire({
          title: "Ups!",
          text: `Nombre incorrecto,El nombre del pokemon comienza con ${name.substring(
            0,
            3
          )}`,
          icon: "error",
          confirmButtonText: "Continuar",
        });
      }
    },
  },
};
</script>

<style scoped>
.borroso {
  filter: blur(5px) grayscale(100%);
}
.card {
  background-color: rgba(46, 169, 218, 0.596);
  border-radius: 15px;
}
#pokeimg {
  transition: transform 0.3s ease;
}
#pokeimg:hover {
  transform: scale(1.2);
}
img {
  -user-drag: none;
  -webkit-user-drag: none;
  user-select: none;
  -moz-user-select: none;
  -webkit-user-select: none;
  -ms-user-select: none;
}
</style>
