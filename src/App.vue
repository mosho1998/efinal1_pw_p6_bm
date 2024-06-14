<template>
  <div class="padre">
    <div class="enunciados">
      <p>Puntaje: {{ puntaje }}</p>
      <p>Intento: {{ intentos }}</p>
    </div>

    <div class="lista">
      <Pokemon :img="imagen1.img" :texto="imagen1.texto" />
      <Pokemon :img="imagen2.img" :texto="imagen2.texto" />
      <Pokemon :img="imagen3.img" :texto="imagen3.texto" />
    </div>

    <button @click="jugar">JUGAR</button>
  </div>
</template>

<script>
import Pokemon from "./components/Pokemon.vue";

export default {
  name: "App",
  components: {
    Pokemon,
  },

  data() {
    return {
      imagen1: {
        img: "https://via.placeholder.com/250",
        texto: "XXXXX",
      },

      imagen2: {
        img: "https://via.placeholder.com/250",
        texto: "XXXXX",
      },

      imagen3: {
        img: "https://via.placeholder.com/250",
        texto: "XXXXX",
      },

      puntaje: 0,
      intentos: 0,

      fijos: [1, 2, 4, 5, 6], 
    };
  },

  methods: {
    async jugar() {
      this.intentos++;
      let seleccion = [];

     
      while (seleccion.length < 3) {
        let randomIndex = Math.floor(Math.random() * this.fijos.length);
        let id = this.fijos[randomIndex];
        if (!seleccion.includes(id)) {
          seleccion.push(id);
        }
      }

     
      const imagenes = [this.imagen1, this.imagen2, this.imagen3];
      for (let i = 0; i < seleccion.length; i++) {
        let id = seleccion[i];
        let data = await this.obtenerPokemon(id);
        imagenes[i].img = `https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/other/dream-world/${id}.svg`;
        imagenes[i].texto = data.name;
      }
    },

    async obtenerPokemon(id) {
      try {
        let response = await fetch(`https://pokeapi.co/api/v2/pokemon/${id}`);
        if (response.ok) {
          let datas = await response.json();
          return datas;
        } else {
          console.error("Error al obtener el Pokémon:", response.status);
        }
      } catch (error) {
        console.error("Error al hacer la solicitud:", error);
      }
    },
  },

};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

.lista {
  display: flex;
  align-content: center;
  justify-content: center;
}

.enunciados {
  display: flex;
  justify-content: center;
}

p {
  margin: 50px;
}
</style>
