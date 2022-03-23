<template>
    <div id="app">
        <div>
            <!-- TODO: Crear componente GitHub -->
            <header>
                <!--<h1>Cliente Biblioteca Multimedia</h1>-->
                <!-- TODO: collection title -->
                <!-- Navbar -->
                <nav class="navbar navbar-expand-lg navbar-dark bg-dark ">
                    <!--<a class="navbar-brand" href="/">Cliente Biblioteca Multimedia</a>-->
                    <a class="navbar-brand" type="button" @click="readCollection('/')">Cliente Biblioteca Multimedia</a>
                    <button class="navbar-toggler" type="button" data-toggle="collapse" data-target="#navbarNav" aria-controls="navbarNav" aria-expanded="false" aria-label="Toggle navigation">
                        <span class="navbar-toggler-icon"></span>
                    </button>
                    <div class="collapse navbar-collapse" id="navbarNav">
                        <ul class="navbar-nav">
                            <li class="nav-item" v-for="(nombre,index) in coleccion.links" :key="index">
                            <!--<li class="nav-item" v-for="nombre in coleccion.links" :key="nombre.prompt">-->
                                <!--<a class="nav-link" :href="nombre.href" @click="mostrarBotonCrear = true">{{nombre.prompt}}</a>-->
                                <a class="nav-link" type="button" @click="readCollection(nombre.href)">{{nombre.prompt}}</a>
                            </li>
                        </ul>
                    </div>
                </nav>
            </header>
            <main>
                <h1 v-if="this.coleccion.type == 'movie'" class="text-center">{{coleccion.title}}</h1>
                <h1 v-if="this.coleccion.type == 'book'" class="text-center">{{coleccion.title}}</h1>
                <h1 v-if="this.coleccion.href == '/'" class="text-center">{{coleccion.title}}</h1>
                <img class="imgInicio" v-if="this.coleccion.href == '/'" src="https://www.comunidadbaratz.com/wp-content/uploads/Enriquece-los-registros-bibliograficos-del-catalogo-de-tu-biblioteca-con-archivos-multimedia-consultables-y-descargables.jpg" alt="imagen de inicio">
                <button @click="botonCrear()" v-if="this.coleccion.type == 'movie' || this.coleccion.type == 'book'" type="button" class="btn btn-primary mb-3 mt-4 crear">{{valorCrear}}</button>
                <FormularioCrear v-on:actualizar="actualizarItems" v-if="mostrarCrear" :templateCrear="coleccion"></FormularioCrear>
                <div class="container-fluid">
                    <div class="row">
                        <!--<div class="col-4" v-for="item in coleccion.items" :key="item.data.value">-->
                        <div class="col-4" v-for="(item,index) in coleccion.items" :key="index">
                            <ApiItems :item="item" :path="coleccion.href" v-on:actualizar="readCollection"></ApiItems>
                        </div>
                    </div>
                </div>
            </main> 
            <footer class="row bg-dark text-white">
                <div class="col-6">
                    <p class="text-start">Autor: Jesús Valero Portugués 2ºDAW-Y</p>
                </div>
                <div class="col-6 text-right">
                    <p class="text-end">Política de privacidad y cookies</p>
                </div>
            </footer>
        </div>
    </div>
</template>

<script>
import ApiItems from './components/ApiItems.vue';
import FormularioCrear from './components/FormularioCrear.vue';

export default {
    name: 'App',
    components: {
        // TODO: Añadir componentes si se van a utilizar
        ApiItems,
        FormularioCrear,
    },
    created: function() {

        // TODO: Inicialización: se ejecuta al crear el componente
        // Cuando se carga la aplicación, se conecta al punto de entrade de la API, '/'
        this.readCollection('/');
    },
    data: function() {
        return {

            // TODO: variables de datos
            // Como mínimo debe haber una variable para almacenar el objeto 'collection' que se esté procesando
            coleccion: {},
            //path: window.location.pathname,
            //path: this.coleccion.type,
            mostrarCrear: false,
            valorCrear: 'Crear',
        }
    },
    methods: {

        readCollection: function(elemento){
            var url = 'https://zzkr1nkz5b.execute-api.eu-west-1.amazonaws.com/latest'+elemento;

            fetch(url, {
                method: "GET",
            })
            .then(response => response.json())
            .then(data => {
                this.coleccion = data.collection;
            })
            .catch(error => {
                console.log(error);
            });
        },
        botonCrear: function(){
            this.mostrarCrear = !this.mostrarCrear; 
            this.valorCrear == 'Crear' ? this.valorCrear = 'Cerrar' : this.valorCrear = 'Crear';
        },
        actualizarItems: function(path){
            this.readCollection(path);
            this.mostrarCrear = false;
            this.valorCrear = 'Crear';
        },
    },
}
</script>

<style>

body{
    background-color:  #DCDCDC;
}

.imgInicio{
    width: 80% !important;
    height: 100% !important;
    margin: 0px auto;
    display:block;
    margin-top: 25px !important;
}

.crear{
    margin-left: 50%;
    transform: translateX(-50%);
}

footer{
    width: 100%;
    height: 150px;
    margin-left: 0px !important;
    margin-top: 40px !important;
}

footer p{
    margin: 75px 30px 0 30px !important;
}

</style>
