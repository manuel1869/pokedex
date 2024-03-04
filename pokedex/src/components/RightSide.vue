<script>
export default{
  // Sirve para guardar y administrar los datos
  // para poder utilizar o propiedades los datos se utiliza la palabra this
  data() { 
     const datos = {
          numero_pokemon : "",
          nombre_pokemon : "",
          hp : "",
          ataque: "",
          defensa: "",
          velocidad: "",
          pokemon_activo: {},
        };
        return datos;
    },
    //Sirve para poder colocar las funciones que se van
    //a utilizar en el componente
    methods: {
       agregartNumero(props) {
         this.numero_pokemon += props.target.innerText;
      },
     buscarPokemon() {
       fetch("https://pokeapi.co/api/v2/pokemon/" +this.numero_pokemon)
       .then(response => response.json())
8       .then(data => {
            this.nombre_pokemon = data.name;
           //contenedor/imagen.innerHTML = '<img id="screen-left-image" src="+data

          this.hp = data.stats[0].base_stat;
          this.ataque = data.stats[1].base_stat;
          this.defensa = data.stats[2].base_stat;
          this.velocidad = data.stats[3].base_stat;

           this.pokemon_activo = {
              numero_pokemon: parseInt (texte_buscar),
              nombre_pokemon: data.name,
              hp: data.stats [0].base_stat,
              ataque: data.stats [1].base_stat,
              defensa: data.stats[2].base_stat,
              velocidad: data.stats[3].base_stat,
            };
         });
      }, 
   }   
        
}     
</script>
<template>
    <div id="right-side">
            <div id="round-bordure-right-side"></div>
            <div id="screen-right">
                <p id="pokemon-numero-p">Número: <span id="numero_pokemon"> {{numero_pokemon}}</span></p>
                <p v-if="this.nombre_pokemon != ''" id="pokemon-title-p">Nombre: <span id="nombre"></span> {{nombre_pokemon}}</p>
                <p v-if="this.hp != ''" id="pokemon-hp-p" class="pokemon-stats-left">Hp : <span id="pokemon-hp"> {{hp}}</span></p>
                <p v-if="this.ataque != ''" id="pokemon-attack-p" class="pokemon-stats-right">Ataque : <span id="pokemon-attack"> {{ataque}}</span></p>
                <p v-if="this.defensa != ''" id="pokemon-defense-p" class="pokemon-stats-left">Defensa : <span id="pokemon-defense"> {{defensa}}</span></p>
                <p v-if="this.velocidad != ''" id="pokemon-speed-p" class="pokemon-stats-right">Velocidad : <span id="pokemon-speed"> {{velocidadd}}</span></p>
            </div>
            <div id="keyboard">
                <button class="key clickable" @click="asignarNumero">1</button>
                <button class="key clickable" @click="asignarNumero">2</button>
                <button class="key clickable" @click="asignarNumero">3</button>
                <button class="key clickable" @click="asignarNumero">4</button>
                <button class="key clickable" @click="asignarNumero">5</button>
                <button class="key clickable" @click="asignarNumero">6</button>
                <button class="key clickable" @click="asignarNumero">7</button>
                <button class="key clickable" @click="asignarNumero">8</button>
                <button class="key clickable" @click="asignarNumero">9</button>
                <button class="key clickable" @click="asignarNumero">0</button>
            </div>
            <div id="elongated-lights">
                <div class="key light"></div>
                <div class="key light"></div>
            </div>
            <button id="right-side-yellow-light" class="key light" @click="buscarPokemon"></button>
            <div id="white-buttons">
                <button class="key clickable" @click="buscarPokemon">Buscar</button>
                <button class="key clickable" @click="buscarRandomPokemon">Random</button>
                <button class="key clickable" @click="obtenerImagenPokemon">Shiny</button>
                <button class="key clickable" @click="agregarEquipo">+Equipo</button>                

            </div>
            <div id="grey-buttons">
                <div class="key">
                    <p id="pokemon-type-1"></p>
                </div>
                <div class="key">
                    <p id="pokemon-type-2"></p>
                </div>
            
        
              //Botón para agregar Pokémon
               <button class="key clickable" @click="agregarPokemon">Agregar</button>

               //Botón para modificar nombre del Pokémon
               <button class="key clickable" @click="modificarNombre">Modificar_Nombre</button>

                //Botón para eliminar Pokémon
              <button class="key clickable" @click="eliminarPokemon">Eliminar</button>

      </div>
        
        
 export default {
  data() {
    return {
      // Lista de Pokémon seleccionados
      pokemonesSeleccionados: [],
      // Nombre del Pokémon a modificar
      nombreModificado: '',
    };
  },
  methods: {
    agregarPokemon() {
      // Verificar si el Pokémon ya está en la lista
      if (!this.pokemonesSeleccionados.includes(this.pokemonActivo)) {
        // Verificar límite de 6 pokemones
        if (this.pokemonesSeleccionados.length < 6) {
          this.pokemonesSeleccionados.push(this.pokemonActivo);
        } else {
          alert('No puedes tener más de 6 Pokémon en tu equipo.');
        }
      } else {
        alert('Este Pokémon ya está en tu equipo.');
      }
    },
    modificarNombre() {
      // Lógica para modificar el nombre del Pokémon seleccionado
      // Se puede implementar usando un cuadro de diálogo para ingresar el nuevo nombre
      // y luego actualizar el nombre del Pokémon en la lista
      // this.pokemonActivo.nombre = this.nombreModificado;
    },
    eliminarPokemon() {
      // Eliminar el Pokémon seleccionado de la lista
      const index = this.pokemonesSeleccionados.indexOf(this.pokemonActivo);
      if (index !== -1) {
        this.pokemonesSeleccionados.splice(index, 1);
      }
    }
  }
};
</template>

<style scoped>
 /*------ RIGHT SIDE ------*/

 #right-side {
    border-radius: 0 20px 20px 0;
    box-shadow: 0 10px 0px 0px rgba(136, 8, 22, 1);
    height: 550px;
    top: 25px;
 }


 /* Rounded bordure right  */

 #round-bordure-right-side {
    width: 250px;
    height: 60px;
    background: #ffffff;
    position: absolute;
    top: 0;
    right: -50px;
    transform: skew(50deg);
 }


 /* Right side screen */

 #screen-right {
    background-color: #232323;
    height: 100px;
    width: 250px;
    border-radius: 10px;
    position: relative;
    left: 50px;
    top: 110px;
    padding: 15px;
    box-shadow: -1px 2px 10px 5px rgba(0, 0, 0, 0.3) inset;
 }

 #screen-right p {
    margin: 0;
    padding-bottom: 10px;
    width: 110px;
    color: #ffffff;
 }

 #screen-right #pokemon-title-p,
 #screen-right #pokemon-numero-p {
    text-align: center;
    width: 100%;
    padding-bottom: 12px;
    font-weight: bold;
 }
 #screen-right .pokemon-stats-left {
    float: left;
    padding-left: 30px;
 }

 #screen-right .pokemon-stats-right {
    float: right;
 }

 /* Right side keyboard */

 #keyboard {
    height: 90px;
    width: 280px;
    position: relative;
    top: 140px;
    left: 50px;
    justify-content: space-between;
    align-items: center;
    display: flex;
    flex-wrap: wrap;
 }

 #keyboard .key {
    width: 54px;
    height: 42px;
    border-radius: 10px;
    background: #31aafe ;
    box-shadow: -2px 2px 0px 0px rgb(1, 60, 86);
 }


 /* Right elongated lights */

 #elongated-lights {
    position: relative;
    top: 150px;
    left: 232px;
 }

 #elongated-lights .key {
    width: 45px;
    height: 6px;
    display: inline-block;
    border-radius: 10px;
    background-color: #232323;
 }


 /* Right side yellow light */

 #right-side-yellow-light {
    background: radial-gradient(#ffffff 5%, #f7df15, #a28a08 60%);
    height: 30px;
    width: 30px;
    position: relative;
    top: 190px;
    left: 300px;
 }


 /* Right side white buttons */

 #white-buttons {
    position: relative;
    top: 150px;
    left: 50px;
 }

 #white-buttons .key {
    width: 70px;
    height: 42px;
    display: inline-block;
    border-radius: 10px;
    background-color: #dedede;
    box-shadow: -2px 2px 0px 0px rgb(1, 60, 86);
 }


 /*  Right side grey buttons */

 #grey-buttons {
    position: relative;
    top: 180px;
    width: 280px;
    left: 50px;
    display: flex;
    justify-content: space-between;
 }

 #grey-buttons .key {
    width: 130px;
    height: 42px;
    border-radius: 10px;
    background-color: #232323;
    border: 1px solid #292929;
    box-shadow: -1px 2px 10px 5px rgba(0, 0, 0, 0.3) inset;
 }

 #grey-buttons p {
    margin: 12px;
    text-align: center;
    color: #ffffff;
 }
</style>