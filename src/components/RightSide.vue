<template>
            <div id="right-side">
            <div id="round-bordure-right-side"></div>
            <div id="screen-right">
                <p id="pokemon-numero-p">Número: <span id="numero_pokemon">{{ numero_pokemon }}</span></p>
                <p v-if="this.nombre_pokemon != ''" id="pokemon-title-p">Nombre: <span id="nombre">{{ nombre_pokemon }}</span></p>
                <p v-if="this.hp != ''" id="pokemon-hp-p" class="pokemon-stats-left">Hp : <span id="pokemon-hp">{{ hp }}</span></p>
                <p v-if="this.ataque != ''" id="pokemon-attack-p" class="pokemon-stats-right">Ataque : <span id="pokemon-attack">{{ ataque }}</span></p>
                <p v-if="this.defensa != ''" id="pokemon-defense-p" class="pokemon-stats-left">Defensa : <span id="pokemon-defense">{{ defensa }}</span></p>
                <p v-if="this.velocidad != ''" id="pokemon-speed-p" class="pokemon-stats-right">Velocidad : <span id="pokemon-speed">{{ velocidad }}</span></p>
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
            <button id="right-side-yellow-light" class="key light" @click="buscarPokemon(true)"></button>
            <div id="white-buttons">
                <button class="key clickable" @click="buscarPokemon">Buscar</button>
                <button class="key clickable" @click="buscarRandomPokemon">Random</button>
                <button class="key clickable" onclick="obtenerImagenPokemon()">Shiny</button>
                <button class="key clickable" @click="agregarEquipo">+Equipo</button>                

            </div>
            <div id="grey-buttons">
                <div class="key">
                    <p>{{ tipoPokemon1 }}</p>
                </div>
                <div class="key">
                    <p>{{ tipoPokemon2 }}</p>
                </div>
            </div>
        </div>
</template>

<script>
export default {
    props: {
        listaEquipo: {
        type: Array,
        required: true
        }
    },
    data() {
        const datos = {
            numero_pokemon: "",
            nombre_pokemon: "",
            hp: "",
            ataque: "",
            defensa: "",
            velocidad: "",
            tipoPokemon1: '',
            tipoPokemon2: '',
            pokemon_activo: {}
        };
        return datos;
    },

    methods: {
        agregarEquipo() {
            if (!this.lista_equipo) {
                this.lista_equipo = []; // Asignar una lista vacía si no está definida
            }

            if (this.lista_equipo.length >= 5) {
                alert("¡El equipo ya tiene 5 Pokémon! No puedes agregar más.");
                return;
            }
            this.lista_equipo.push(this.pokemon_activo);
            this.recargarPagina();
        },
        buscarPokemon() {
            fetch('https://pokeapi.co/api/v2/pokemon/' + this.numero_pokemon)
                .then(response => response.json())
                .then(data => {
                    this.nombre_pokemon = data.name;
                    this.hp = data.stats[0].base_stat;
                    this.ataque = data.stats[1].base_stat;
                    this.defensa = data.stats[2].base_stat;
                    this.velocidad = data.stats[3].base_stat;

                    this.$emit('pokemon-obtenido', { altura: data.height, peso: data.weight });
                    this.$emit('imagen-obtenida', data.sprites.front_default);

                    const tipos = data.types.map(tipo => tipo.type.name);
                    this.tipoPokemon1 = tipos[0] || '';
                    this.tipoPokemon2 = tipos[1] || '';

                    this.pokemon_activo = {
                        numero_pokemon: parseInt(this.numero_pokemon),
                        nombre_pokemon: data.name,
                        hp: data.stats[0].base_stat,
                        ataque: data.stats[1].base_stat,
                        defensa: data.stats[2].base_stat,
                        velocidad: data.stats[3].base_stat,
                        altura: data.height,
                        peso: data.weight
                    };
                });
        },

        buscarRandomPokemon() {
            const numero_random = this.randomPokemon();
            this.numero_pokemon = numero_random.toString();
            this.buscarPokemon();
        },

        randomPokemon(min = 1, max = 1025) {
            return Math.floor(Math.random() * (max - min + 1)) + min;
        },

        obtenerTipoPokemon(tipos) {
            const tiposTraducidos = tipos.map(tipo => {
            switch (tipo.type.name) {
                case 'normal':
                return 'normal';
                case 'fighting':
                return 'lucha';
                case 'flying':
                return 'volador';
                case 'poison':
                return 'veneno';
                case 'ground':
                return 'tierra';
                case 'rock':
                return 'roca';
                case 'bug':
                return 'bicho';
                case 'ghost':
                return 'fantasma';
                case 'steel':
                return 'acero';
                case 'fire':
                return 'fuego';
                case 'water':
                return 'agua';
                case 'grass':
                return 'planta';
                case 'electric':
                return 'electrico';
                case 'psychic':
                return 'psiquico';
                case 'ice':
                return 'hielo';
                case 'dragon':
                return 'dragon';
                case 'dark':
                return 'siniestro';
                case 'fairy':
                return 'hada';
                default:
                return '???';
            }
            });
            return tiposTraducidos.join(', ');
        }
    }
}
</script>


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
    -webkit-transform: skew(50deg);
    tranform: skew(50deg);
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
