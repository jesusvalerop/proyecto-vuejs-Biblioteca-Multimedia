<template>
    <div>
        <div class="card mb-3 w-100">
            <div class="card-body">
                <div v-if="!mostrarEditar">
                    <!--<div v-for="dato in item.data" :key="dato.value">-->
                    <div v-for="(dato,index) in item.data" :key="index">
                        <h5 class="mb-3" v-if="dato.type == 'text' && dato.name == 'title'">{{dato.prompt+": "+dato.value}}</h5>
                        <p v-if="dato.type == 'text' && dato.name != 'title'">{{dato.prompt+": "+dato.value}}</p>
                        <p v-if="dato.type == 'textarea'" class="cortar">{{dato.prompt+": "+dato.value}}</p>
                        <div v-if="dato.type == 'embeddedVideo'" class="embed-responsive embed-responsive-16by9 mb-3">
                            <iframe class="embed-responsive-item" :src="dato.value" allowfullscreen></iframe>
                        </div>
                        <img v-if="dato.name == 'image'" class="card-img-top img-fluid mb-3" :src="dato.value" alt="Card image cap">
                        <p v-if="dato.type == 'date'" >{{dato.prompt+": "+dato.value}}</p>
                    </div>
                    <a @click="toggleEditFormVisibility" class="btn btn-primary mr-3">Editar</a>
                    <a @click="toggleModal" class="btn btn-danger">Eliminar</a>
                    <!-- <a @click="deleteItem(item.href)" class="btn btn-danger">Eliminar</a>-->

                    <!-- Inicio Modal --->
                    <div v-if="showModal" 
                        class="modal-backdrop fade show" 
                        @click="toggleModal">
                    </div>
                    <div :class="{'show': showModal, 'd-block': showModal}" class="modal fade" role="dialog">
                        <div class="modal-dialog" role="document">
                        <div class="modal-content">
                        <div class="modal-header">
                            <h5 class="modal-title" id="exampleModalLabel">
                            Se va a eliminar el elemento seleccionado
                            </h5>
                            <!-- Botón de cierre en forma de X -->
                            <button type="button" 
                                    class="close" 
                                    aria-label="Close" 
                                    @click="toggleModal">
                            <span aria-hidden="true">&times;</span>
                            </button>
                        </div>
                        <div class="modal-body">
                            ¿Desea proceder?
                        </div>
                        <div class="modal-footer">
                            <!-- Botón para cancelar -->
                            <button @click="toggleModal" type="button" class="btn btn-secondary">
                            Cancelar
                            </button>
                            
                            <!-- Botón para aceptar -->
                            <button @click="deleteItem(item.href)" type="button" class="btn btn-primary">
                            Eliminar
                            </button>
                        </div>
                        </div>
                    </div>
                </div>
                <!--- Fin Modal -->
                </div>
                <div v-if="mostrarEditar">
                    <FormularioEditar :templateEditar="item" :path="this.path" v-on:cancelar="toggleEditFormVisibility()" v-on:actualizar="actualizarItems"></FormularioEditar>
                </div>
            </div>
        </div> 
    </div>
</template>

<script>

// TODO: Importar componente 

import FormularioCrear from './FormularioCrear.vue';
import FormularioEditar from './FormularioEditar.vue';

export default {
    name: 'ApiItems',
    components: {
        // TODO: Importar componente 
        FormularioCrear,
        FormularioEditar,
    },
    props: {
        item: Object,
        path: String,
    },
    data: function() {
        return {
            // TODO: crear variables de datos para el funcionamiento del componente
            //ruta: window.location,
            //path: window.location.pathname,
            mostrarEditar: false,
            showModal: false,
        }
    },
    methods: {
        botonCrear: function(){
            this.mostrarCrear = !this.mostrarCrear; 
            this.valorCrear == 'Crear' ? this.valorCrear = 'Cerrar' : this.valorCrear = 'Crear';
        },
        deleteItem: function(item){
            let url = this.$APIURL;
            console.log(url+''+item);
            console.log(this.path);

            fetch(url+''+item, {
                method: "DELETE",
            })
            .then(function(response){
                if(!response.ok){
                    console.log("Error "+response.status+": no se ha podido eliminar la peícula/libro en la API");
                }else{
                    console.log("Pelicula/libro eliminado/a correctamente");
                }
            })
            .catch(
                err => console.log(err)
            )
            .finally(()=>{
                this.toggleModal();
                //this.$emit('actualizar', this.path);
                this.$emit('actualizar', this.path);
            });
        },
        toggleEditFormVisibility: function(){
            this.mostrarEditar = !this.mostrarEditar;
        },
        actualizarItems: function(path){
            this.$emit('actualizar', path);
        },
        toggleModal: function() {
            // Función para ocultar o mostrar modal
            this.showModal = ! this.showModal;
        }
    }
}
</script>

<style scoped>

.cortar{
    white-space: nowrap;
    text-overflow: ellipsis;
    overflow: hidden;
}

.cortar:hover{
  width: 100%;
  white-space: initial;
  overflow:visible;
  cursor: pointer;
}

img{
    object-fit: cover !important;
    width: 100% !important;
    height: 400px !important;
}

.crear{
    margin-left: 50%;
    transform: translateX(-50%);
}

</style>