<template>
    <div class="container mt-5">
        <form @submit.prevent="obtenerMuseos">
            <div class="input-group mb-3">
                <span class="input-group-text" id="basic-addon1">Buscar</span>
                <input type="text" class="form-control" placeholder="Buscar" aria-label="Username" v-model="buscar"  aria-describedby="basic-addon1">
                <button class="btn btn-outline-primary">Buscar</button>
            </div>
        </form>
        <div class="table-responsive">
            <table class="table table-hover">
                <thead class="text-white bg-primary">
                    <tr>
                        <td>Nombre</td>
                        <td>Tematica</td>
                        <td>Alcaldía</td>
                        <td></td>
                    </tr>
                </thead>
                <tbody>
                <tr v-for="museo in museos" :key="museo.id">
                    <td>{{museo.museoNombre}}</td>
                    <td>{{museo.museoTematicaN1}}</td> 
                    <td>{{museo.nomMun}}  </td>                 
                    <td>
                        <!-- Button trigger modal -->
                        <button type="button" class="btn btn-primary" data-bs-toggle="modal" data-bs-target="#exampleModal" @click="mostrarDetalles(museo)">
                            Detalles
                        </button>
                    </td>
                </tr>
                </tbody>
            </table>
            <detalles-del-museo :museo="museo"/>
        </div>
        <div>
            <div>
                <p>
                    Total de registros: {{totalDeRegistros}}, Total de registros filtrados: {{totalDeRegistrosFiltrados}}
                    |
                    Página {{paginaActual}} de {{numeroDePaginas}}
                </p>
            </div>
            <div class="text-center">
            <div class="btn-group" role="group" aria-label="Basic example">                
                <button type="button" class="btn btn-outline-primary" @click="cargarLista(paginaActual-1)">
                    <i class="bi bi-arrow-left"></i>
                </button>
                <template v-for="pagina in numeroDePaginas" :key="pagina">
                    <template v-if="pagina == paginaActual">
                        <button type="button" class="btn btn-primary" @click="cargarLista(pagina)" disabled>{{pagina}}</button>                
                    </template>
                    <template v-else>
                        <button type="button" class="btn btn-primary" @click="cargarLista(pagina)">{{pagina}}</button>
                    </template>
                </template>
                <button type="button" class="btn btn-outline-primary" @click="cargarLista(paginaActual + 1)">
                    <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" fill="currentColor" class="bi bi-arrow-right" viewBox="0 0 16 16">
                        <path fill-rule="evenodd" d="M1 8a.5.5 0 0 1 .5-.5h11.793l-3.147-3.146a.5.5 0 0 1 .708-.708l4 4a.5.5 0 0 1 0 .708l-4 4a.5.5 0 0 1-.708-.708L13.293 8.5H1.5A.5.5 0 0 1 1 8z"/>
                    </svg>
                </button>
            </div>
            </div>
        </div>
    </div>
</template>

<script>
import DetallesDelMuseo from './DetallesDelMuseo.vue'
const urlBase = "http://192.168.1.102:9081/Api/"
//const urlBase = "https://localhost:1983/Api/"
const url = urlBase + "Museums"

export default {
  components: { DetallesDelMuseo },
    data(){
        return{
            buscar: '',
            museos:[],
            paginaActual: 1,
            registrosPorPAgina: 10,
            numeroDePaginas:0,
            totalDeRegistros:0,
            totalDeRegistrosFiltrados:0,   
            museo:{}         
        }
    },
    created(){
        this.obtenerMuseos()
    },
    methods:{
        async obtenerMuseos(){
            //console.log(this.buscar)
            var urlSearchParams
            var response         
            var data
            
            urlSearchParams = new URLSearchParams({
                Search : this.buscar,
                PageCurrent : this.paginaActual,
                RecordsPerPage: this.registrosPorPAgina
            })
            response = await fetch(url + "?" + urlSearchParams,{method: 'GET'})
            //console.log(response)            
            data = await response.json()          
            console.log(data)
            this.museos = data.listMuseums
            this.numeroDePaginas = data.countPage
            this.totalDeRegistros = data.totalRecords
            this.totalDeRegistrosFiltrados = data.totalRecordsFiltered
            //console.log(this.numeroDePaginas)
        },
        cargarLista(paginaActual){
            if(paginaActual == 0){
                paginaActual = this.numeroDePaginas
            }
            if(paginaActual == (this.numeroDePaginas + 1)){
                paginaActual = 1
            }            
            this.paginaActual = paginaActual
            this.obtenerMuseos();            
        }, 
        mostrarDetalles(museo){
            //console.log(museo)
            this.museo = museo
        }
    }
}
</script>