<script setup>
  import RightSide from './components/RightSide.vue';
</script>

<script>
export default {
  data() {
    return {
      imagenPokemon: '',
      altura: '',
      peso: '',
      nombre_pokemon_editar: '',
      lista_equipo: [] // Agrega una lista de Pokémon vacía
    };
  },
  methods: {
    agregarEquipo() {
      if (this.lista_equipo.length >= 5) {
          alert("¡El equipo ya tiene 5 Pokémon! No puedes agregar más.");
          return;
      }

      this.pokemon_activo.weight = parseFloat(this.pokemon_activo.weight);
      this.pokemon_activo.height = parseFloat(this.pokemon_activo.height);

      console.log(this.pokemon_activo.weight);
      console.log(this.pokemon_activo.height);

      console.log(JSON.stringify(this.pokemon_activo));
     
      fetch("http://127.0.0.1:8000/api/equipo", {
          method: "POST",
          body: JSON.stringify(this.pokemon_activo),
          headers: {
              "Content-Type": "application/json; charset=UTF-8"
          }
      })
      .then(response => {
          // Si la solicitud es exitosa, recargamos la página
          this.recargarPagina();
      })
      // Emite un evento con los datos del Pokémon activo y la lista de equipo
      this.$emit('equipo-agregado', {
            pokemon_activo: this.pokemon_activo,
            lista_equipo: this.lista_equipo
      });
    },
    obtenerDatosPokemon(datosPokemon) {
      this.altura = datosPokemon.altura;
      this.peso = datosPokemon.peso;
    },
    mostrarImagenPokemon(imagen) {
    this.imagenPokemon = imagen;
   },
   eliminarPokemon(id) {
      // Lógica para eliminar el Pokémon del arreglo lista_equipo
      this.lista_equipo = this.lista_equipo.filter(pokemon => pokemon.id !== id);
      // Llamar a la API para eliminar el Pokémon de la base de datos si es necesario
    },
    mostrarModalEditarPokemon(id) {
      // Lógica para mostrar el modal con los datos del Pokémon seleccionado
      // this.nombre_pokemon_editar = nombre;
      // Abrir el modal
    },
    modificarPokemonEnLaTabla() {
      // Lógica para modificar el Pokémon en la tabla
      // Obtener el ID y el nuevo nombre del Pokémon desde this.id_editar y this.nombre_pokemon_editar
      // Llamar a la API para modificar el Pokémon en la base de datos si es necesario
    },

  }
};
</script>

<template>
  <div id="container">
        <div id="left-side">
            <div id="round-bordure-left-side"></div>
            <div id="round-bordure-left-side2"></div>
            <div id="big-blue-light-background" class="light">
                <div id="big-blue-light" class="light"></div>
                <div id="big-blue-light-glint" class="light"></div>
            </div>
            <div id="little-lights">
                <div class="light little-light red-light"></div>
                <div class="light little-light yellow-light"></div>
                <div class="light little-light green-light"></div>
            </div>
            <div id="screen-left-background">
                <div id="screen-left-little-lights">
                    <div class="screen-left-little-red-light light"></div>
                    <div class="screen-left-little-red-light light"></div>
                </div>
                <div id="screen-left">
                    <img id="screen-left-image" :src="imagenPokemon" alt="">
                </div>
                <div id="screen-left-big-red-light" class="light"></div>
                <div id="screen-left-burger">
                    <div id="screen-left-burger-inside"></div>
                </div>
            </div>
            <div id="elongated-buttons">
                <div class="elongated-button elongated-button1 clickable"></div>
                <div class="elongated-button elongated-button2 clickable"></div>
            </div>
            <div id="rounded-button" class="clickable" onclick="recargarPagina()"></div>
            <div id="screen-left-little">
                <p id="pokemon-height">Altura: <span>{{ altura }}</span></p>
                <p id="pokemon-weight">Peso: <span>{{ peso }}</span></p>
            </div>
            <div id="cross">
                <div class="cross cross-top clickable">
                    <div class="arrow arrow-top" @click="asignarDireccion('arriba')"></div>
                </div>
                <div class="cross cross-mid"></div>
                <div class="cross cross-bottom clickable">
                    <div class="arrow arrow-bottom" @click="asignarDireccion('abajo')"></div>
                </div>
                <div class="cross cross-left clickable" @click="asignarDireccionImagen">
                    <div class="arrow arrow-left"></div>
                </div>
                <div class="cross cross-right clickable" @click="asignarDireccionImagen">
                    <div class="arrow arrow-right"></div>
                </div>
            </div>
        </div>

        <!-- MIDDLE -->
        <div class="binding">
            <div class="hinge hinge1"></div>
            <div class="hinge hinge2"></div>
            <div class="hinge hinge3"></div>
        </div>

        <!-- RIGHT SIDE -->
        <!--<RightSide/>-->
        <RightSide @pokemon-obtenido="obtenerDatosPokemon" @imagen-obtenida="mostrarImagenPokemon" />
    </div>
    <!-- Tabla de Pokémon -->
    <div id="tabla-container">
            <table border="1">
                <thead>
                    <tr>
                        <th>No</th>
                        <th>Nombre</th>
                        <th>Hp</th>
                        <th>Defensa</th>
                        <th>Velocidad</th>
                        <th>Acciones</th>
                    </tr>
                </thead>
                <tbody>
                    <tr v-for="(pokemon, index) in lista_equipo" :key="pokemon.id">
                        <td>{{ index + 1 }}</td>
                        <td>{{ pokemon.nombre }}</td>
                        <td>{{ pokemon.hp }}</td>
                        <td>{{ pokemon.defensa }}</td>
                        <td>{{ pokemon.velocidad }}</td>
                        <td>
                            <button @click="eliminarPokemon(pokemon.id)">Eliminar</button>
                            <button @click="mostrarModalEditarPokemon(pokemon.id)">Editar</button>
                        </td>
                    </tr>
                </tbody>
            </table>
        </div>
</template>

<style scoped>
  /*------ GENERAL ------*/

body {
    overflow-x: hidden;
    overflow-y: auto;
    margin: 0;
    padding: 0;
    width: 100vw;
    height: 100vh;
}

#container {
    display: flex;
    justify-content: center;
    align-items: center;
    width: 100%;
    height: 100%;
}
table{
    width: 60%;
    margin-left: 20%;
}   

.modal {
    display: none;
    position: fixed;
    z-index: 999;
    left: 0;
    top: 0;
    width: 100%;
    height: 100%;
    overflow-x: hidden;
    overflow-y: auto;
    background-color: #a29e9e8c;
}

.modal-content {
    width: 20%;
    height: 20%;
    background-color: #fefefe;
    margin: 15% auto;
    padding: 10px;
    border: 1px solid #dddada;
}
#left-side,
#right-side {
    background-color: #dd0b2d;
    position: relative;
    width: 380px;
}


/* Class for all the buttons */

.clickable {
    cursor: pointer;
}

.elongated-button:active,
#rounded-button:active,
#cross .cross:active,
#keyboard .key:active,
#white-buttons .key:active {
    box-shadow: -1px 2px 10px 5px rgba(0, 0, 0, 0.3) inset;
}


/* Class for all the lights */

.light {
    border-radius: 50px;
    border: 1px solid #4d4d4d;
}


/*------ LEFT SIDE ------*/

#left-side {
    box-shadow: -5px 5px 0px 5px rgba(136, 8, 22, 1);
    height: 600px;
    border-radius: 20px 0 0 20px;
}


/* Rounded bordure left */

#round-bordure-left-side {
    width: 160px;
    height: 60px;
    position: absolute;
    z-index: 2;
    border-bottom: 2px solid rgba(136, 8, 22, 1);
    border-right: 2px solid rgba(136, 8, 22, 1);
    top: 50px;
    left: 30px;
    -webkit-transform: skew(-50deg);
    tranform: skew(-50deg);
    box-shadow: 6px 4px 1px 0px rgba(136, 8, 22, 1);
}

#round-bordure-left-side2 {
    width: 155px;
    height: 2px;
    position: absolute;
    border-bottom: 2px solid rgba(136, 8, 22, 1);
    top: 48px;
    left: 226px;
    box-shadow: -1px 3px 2px 0px rgba(136, 8, 22, 1);
}


/* Top left lights */

#big-blue-light-background {
    background-color: #ffffff;
    height: 60px;
    width: 60px;
    position: relative;
    top: 25px;
    left: 25px;
}

#big-blue-light {
    background-image: radial-gradient(#ffffff, #31aafe, #1b6a9e 60%);
    height: 50px;
    width: 50px;
    position: relative;
    top: 5px;
    left: 5px;
    border: 0;
}

#big-blue-light-glint {
    height: 15px;
    width: 25px;
    position: absolute;
    top: 12px;
    left: 18px;
    border: 0;
    background-color: rgba(255, 255, 255, 0.5);
}

#little-lights {
    position: absolute;
    top: 22px;
    left: 100px;
}

#little-lights .little-light {
    display: inline-block;
    float: left;
    height: 20px;
    width: 20px;
    margin-right: 10px;
}

#little-lights .red-light {
    background: radial-gradient(#ffffff, #dd0b2d, #8a0606 60%);
}

#little-lights .yellow-light {
    background: radial-gradient(#ffffff, #fce21b, #a08a0e 60%);
}

#little-lights .green-light {
    background: radial-gradient(#ffffff, #559f5d, #336d3f 60%);
}


/* Left side big screen with red lights */

#screen-left-background {
    background-color: #dedede;
    height: 250px;
    width: 280px;
    border-radius: 10px;
    position: relative;
    left: 50px;
    top: 100px;
    box-shadow: 1px 1px 3px 2px rgba(0, 0, 0, 0.5);
}

#screen-left-little-lights {
    position: relative;
    top: 15px;
    left: 120px;
}

#screen-left-little-lights .screen-left-little-red-light {
    display: inline-block;
    float: left;
    background-color: #dd0b2d;
    height: 6px;
    width: 6px;
    margin-right: 15px;
    border: 1px solid #474747;
}

/* Screen for the Pokemon images */

#screen-left {
    background-color: #232323;
    box-shadow: -1px 2px 10px 5px rgba(0, 0, 0, 0.3) inset;
    height: 175px;
    width: 220px;
    border-radius: 10px;
    position: relative;
    left: 30px;
    top: 35px;
}

#screen-left-image {
    width: 210px;
    height: auto;
    max-height: 160px;
    display: block;
    margin: 0 auto;
}

#screen-left-big-red-light {
    background-color: #dd0b2d;
    height: 15px;
    width: 15px;
    position: relative;
    top: 48px;
    left: 30px;
    animation: winkLight 1s infinite;
}

@keyframes winkLight {
    0% { background-color: #dd0b2d; }
    100% { background-color: #8a0606; }
}


#screen-left-burger {
    border-top: 1px solid #232323;
    border-bottom: 1px solid #232323;
    height: 15px;
    width: 30px;
    position: absolute;
    right: 31px;
    bottom: 10px;
}

#screen-left-burger-inside {
    border-top: 1px solid #232323;
    border-bottom: 1px solid #232323;
    height: 5px;
    width: 30px;
    position: absolute;
    top: 4px;
}


/* Console */

#elongated-buttons {
    position: relative;
    top: 130px;
    left: 113px;
}

.elongated-button {
    width: 50px;
    height: 6px;
    display: inline-block;
    border-radius: 10px;
    margin-right: 20px;
    border: 1px solid #474747;
}

.elongated-button1 {
    background-color: red;
    box-shadow: -2px 2px 0px 0px rgba(136, 8, 22, 1);
}

.elongated-button2 {
    background-color: rgba(40, 170, 253, 1);
    box-shadow: -2px 2px 0px 0px rgb(1, 60, 86);
}

#rounded-button {
    position: relative;
    top: 120px;
    left: 40px;
    width: 40px;
    height: 40px;
    display: inline-block;
    border-radius: 50px;
    background-color: #424141;
    box-shadow: -2px 2px 0px 0px #292929;
}


/* Left side little screen */

#screen-left-little {
    background-color: #52ae5f;
    box-shadow: -1px 2px 10px 3px rgba(0, 0, 0, 0.3) inset;
    height: 80px;
    width: 130px;
    border-radius: 10px;
    position: relative;
    left: 110px;
    top: 105px;
}

#screen-left-little p {
    margin: 0;
    text-align: center;
    padding-top: 15px;
}


/* Directionnal cross used to choose the Pokemon */

#cross {
    position: relative;
    left: 300px;
    top: 20px;
    width: 150px;
    height: 150px;
}

#cross .cross {
    height: 28px;
    width: 28px;
    position: absolute;
    background-color: #424141;
}

#cross .cross-top {
    border-radius: 5px 5px 0 0;
    box-shadow: -3px 2px 0px 0px #292929;
}

#cross .cross-mid {
    top: 28px;
    background: radial-gradient(circle, rgba(66, 65, 65, 1) 0%, rgba(56, 55, 55, 1) 50%, rgba(66, 65, 65, 1) 100%);
}

#cross .cross-bottom {
    top: 56px;
    border-radius: 0 0 5px 5px;
    box-shadow: -2px 1px 0px 1px #292929;
}

#cross .cross-left {
    left: -28px;
    top: 28px;
    border-radius: 5px 0 0 5px;
    box-shadow: -1px 2px 0px 1px #292929;
}

#cross .cross-right {
    left: 28px;
    top: 28px;
    border-radius: 0 5px 5px 0;
    box-shadow: 0px 3px 0px 0px #292929;
}

.arrow {
    display: inline-block;
    height: 0;
    width: 0;
    position: relative;
}

.arrow-top {
    top: 4px;
    left: 5px;
    border-right: 8px solid transparent;
    border-bottom: 15px solid #333232;
    border-left: 8px solid transparent;
}

.arrow-bottom {
    top: 10px;
    left: 5px;
    border-top: 15px solid #333232;
    border-right: 8px solid transparent;
    border-left: 8px solid transparent;
}

.arrow-left {
    top: 7px;
    left: 3px;
    border-right: 15px solid #333232;
    border-top: 8px solid transparent;
    border-bottom: 8px solid transparent;
}

.arrow-right {
    top: 7px;
    left: 9px;
    border-left: 15px solid #333232;
    border-bottom: 8px solid transparent;
    border-top: 8px solid transparent;
}


/*------ MIDDLE ------*/


/* Bindings */

.binding {
    height: 600px;
    width: 50px;
    border-left: 1px solid rgba(136, 8, 22, 1);
    border-right: 1px solid rgba(136, 8, 22, 1);
    box-shadow: 0px 10px 0px 0 rgba(136, 8, 22, 1);
    background: rgb(193, 12, 12);
    background: linear-gradient(90deg, rgba(221, 11, 43, 1) 0%, rgba(218, 95, 95, 1) 35%, rgba(221, 11, 43, 1) 58%, rgba(136, 8, 22, 1) 100%);
}

.hinge {
    height: 50px;
    width: 50px;
    position: relative;
    background: rgb(193, 12, 12);
    background: linear-gradient(90deg, rgba(221, 11, 43, 1) 0%, rgba(218, 95, 95, 1) 35%, rgba(221, 11, 43, 1) 58%, rgba(136, 8, 22, 1) 100%);
    border-bottom: 1px solid rgba(136, 8, 22, 1);
    border-top: 1px solid rgba(136, 8, 22, 1);
}

.hinge2 {
    top: 50px;
}

.hinge3 {
    top: 445px;
}

</style>