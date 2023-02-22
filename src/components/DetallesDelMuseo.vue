<template>
    
    <!-- Modal -->
    <div class="modal fade" id="exampleModal" tabindex="-1" aria-labelledby="exampleModalLabel" aria-hidden="true">
    <div class="modal-dialog modal-xl">
        <div class="modal-content">
        <div class="modal-header">
            <h1 class="modal-title fs-5" id="exampleModalLabel">{{museoActual.museoNombre}}</h1>
            <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
        </div>
        <div class="modal-body">            
            <div class="mt-3 mb-3">
                <p>{{ museoActual.datosGenerales}}</p>
                <p>{{ museoActual.hoariosYCostos}}</p>
                <p>{{ museoActual.museoAdscripcion}}</p>
                <p>{{ museoActual.museoCalleNumero}} {{museoActual.museoColonia}}</p>
                <a :href="museoActual.paginaWeb">{{ museoActual.paginaWeb }}</a>                
            </div>

            <div id="carouselExampleAutoplaying" class="carousel slide" data-bs-ride="carousel">
                <div class="carousel-inner">
                    <template v-for="(urlDeLaImagen, index) in museoActual.listUrlImg" :key="urlDeLaImagen">
                        <!-- <span>{{urlDeLaImagen}} {{index}}</span> -->
                        <template v-if="index == 0">
                            <div class="carousel-item active">
                                <div class="text-center">
                                    <img :src="urlDeLaImagen" class="img-fluid">
                                </div>
                            </div>
                        </template>       
                        <template v-else>                 
                            <div class="carousel-item">
                                <div class="text-center">
                                    <img :src="urlDeLaImagen" class="img-fluid">
                                </div>
                            </div>
                        </template>                        
                    </template>                
                </div>
                <button class="carousel-control-prev" type="button" data-bs-target="#carouselExampleAutoplaying" data-bs-slide="prev">
                    <span class="carousel-control-prev-icon" aria-hidden="true"></span>
                    <span class="visually-hidden">Previous</span>
                </button>
                <button class="carousel-control-next" type="button" data-bs-target="#carouselExampleAutoplaying" data-bs-slide="next">
                    <span class="carousel-control-next-icon" aria-hidden="true"></span>
                    <span class="visually-hidden">Next</span>
                </button>
            </div>

        </div>
        <div class="modal-footer">
            <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">Cerrar</button>            
        </div>
        </div>
    </div>
    </div>
</template>

<script>

export default{
    data(){
        return{
            museoActual:{
                museoNombre:''
            }
        }
    },
    props:['museo'],
    methods:{
        colocarMuseo(museo){
            console.log('Colocar museo',museo)
        }
    },
    watch:{
        museo:function(valorNuevo, valorAnterior){
            console.log(valorNuevo)
            console.log(valorAnterior)
            this.museoActual = Object.assign({}, valorNuevo)
            //this.colocarMuseo(valorNuevo)
        }
    }
}
</script>
