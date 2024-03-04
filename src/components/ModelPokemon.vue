<script setup>
    import {ref} from 'vue';
    const nombre_pokemon_editar = ref("");
</script>

<script>
    export default {
        data(){
        },
        props: {
            pokemon_id: null,
            nombre_pokemon: null,
            pokemon_activo: null,
            lista_equipo: []
        },

        methods: {
            cerrarModal(){
                this.pokemon_id = null;
            },
            editaPokemon()
            {
                this.pokemon_editar = this.lista_equipo[index];
                pokemon_editar.nombre_pokemon = this.nombre_pokemon;
                this.lista_equipo[index] = pokemon_editar;
                cerraModal();
            }
        }
    }
</script>

<template>
    <div class="modal" v-if="pokemon_id != null">
        <div class="modal-content">
            <td> <img :src="pokemon_activo.imagen_pokemon.front_default" alt="pokemon.nombre_pokemon"></td>
            <label>Nombre</label>
            <br>
            <input v-model="nombre_pokemon">
            <br>
            <button @click="cerraModal">Cancelar</button>
            <button @click="editaPokemon">Guardar</button>
        </div>
    </div>
</template>

<style scoped>
    
    .modal 
    {
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
    .modal-content
    {
        width: 20%;
        height: 20%;
        background-color: #fefefe;
        margin: 15% auto;
        padding: 10px;
        border: 1px solid #dddada;
    }

</style>