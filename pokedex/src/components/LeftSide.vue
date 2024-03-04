<script>
export default {
  data() {
    return {
      numeroPokemon: '',
      imagenPokemon: '/Pokedex/tipos/profesor.svg',
      alturaPokemon: '',
      pesoPokemon: ''
    };
  },
  methods: {
     asignarNumero(numero) {
         this.numeroPokemon += numero;
        },
     asignarDireccion(direccion) {
         let nuevoNumero = 1;

         if (this.numeroPokemon !== '') {
             if (direccion === "arriba") {
             if (parseInt(this.numeroPokemon) > 1) {
                 nuevoNumero = parseInt(this.numeroPokemon) - 1;
               }
            } 
            else {
              if (parseInt(this.numeroPokemon) < 1025) {
                 nuevoNumero = parseInt(this.numeroPokemon) + 1;
                }
            }
            }

          this.numeroPokemon = nuevoNumero.toString();
          this.shiny = false;
          this.orientacion = "front";
          this.buscarPokemon();
        },
     asignarDireccionImagen() {
          if (this.nombrePokemon !== '') {
              this.orientacion = this.orientacion === "front" ? "back" : "front";
              this.buscarPokemon();
            }
        },
       buscarRandomPokemon() {
           const numeroRandom = this.randomPokemon();
           this.numeroPokemon = numeroRandom.toString();
           this.buscarPokemon();
        },
      randomPokemon(min = 1, max = 1025) {
         return Math.floor(Math.random() * (max - min + 1)) + min;
        },
      buscarPokemon() {
          fetch("https://pokeapi.co/api/v2/pokemon/" + this.numeroPokemon)
          .then(response => response.json())
          .then(data => {
             this.nombrePokemon = data.name;
             this.imagenPokemon = data.sprites.front_default;
              this.alturaPokemon = (data.height / 10).toFixed(1) + ' m';
              this.pesoPokemon = (data.weight / 10).toFixed(1) + ' kg';
              this.tiposPokemon = data.types.map(type => type.type.name).join(', ');
              this.statsPokemon = data.stats.map(stat => {
              return {
                 nombre: stat.stat.name,
                 valor: stat.base_stat
                };
            });
        })
       .catch(error => {
             console.error('Error al buscar el Pokémon:', error);
             this.resetearDatos();
            });
       },
       obtenerImagenPokemon() {
             if (this.nombrePokemon) {
             this.imagenPokemon = this.shiny ? this.spritesPokemon.back_shiny : this.spritesPokemon.back_default;
            }
        },
    }
};
</script>
<template>
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
          <img :src="imagenPokemon" alt="">
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
      <div id="rounded-button" class="clickable" @click="recargarPagina"></div>
      <div id="screen-left-little">
        <p>Altura: {{ alturaPokemon }}</p>
        <p>Peso: {{ pesoPokemon }}</p>
      </div>
      <div id="cross">
        <div class="cross cross-top clickable" @click="asignarDireccion('arriba')">
          <div class="arrow arrow-top"></div>
        </div>
        <div class="cross cross-mid"></div>
        <div class="cross cross-bottom clickable" @click="asignarDireccion('abajo')">
          <div class="arrow arrow-bottom"></div>
        </div>
        <div class="cross cross-left clickable" @click="asignarDireccionImagen">
          <div class="arrow arrow-left"></div>
        </div>
        <div class="cross cross-right clickable" @click="asignarDireccionImagen">
          <div class="arrow arrow-right"></div>
        </div>
      </div>
    </div>
</template>


<style scoped>
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
    transform: skew(-50deg);
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
</style>