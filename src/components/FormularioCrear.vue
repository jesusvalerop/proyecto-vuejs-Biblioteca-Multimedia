<template>
    <div class="container">
        <form class="algo">
            <!--<div v-for="elemento in templateCrear.template.data" :key="elemento.prompt">-->
            <div v-for="(elemento,index) in templateCrear.template.data" :key="index">
                <input v-if="elemento.type == 'text'" type="text" class="form-control mb-4" v-model="elemento.value" :placeholder="elemento.prompt">
                <input v-if="elemento.type == 'date'" type="date" class="form-control mb-4" v-model="elemento.value">
                <textarea v-if="elemento.type == 'textarea'" type="textarea" class="form-control mb-4" v-model="elemento.value" :placeholder="elemento.prompt"></textarea>
            </div>
            <!--<button @click="createItem(path)" type="button" class="btn btn-success mb-3 mt-4 crear">Aceptar</button>-->
            <button @click="createItem(templateCrear.href)" type="button" class="btn btn-success mb-3 mt-4 crear">Aceptar</button>
        </form>  
    </div>
</template>

<script>

// TODO: Importar componente

export default {
    name: 'FormularioCrear',
    components: {
    },
    props: {
        templateCrear: Object
    },
    data: function() {
        return {
            // TODO: crear variables de datos para el funcionamiento del componente
            //path: window.location.pathname,
        }
    },
    methods: {
        createItem: function(pathActual){
        console.log(this.templateCrear);
        console.log(pathActual);

            let url = this.$APIURL+pathActual;
            console.log(url);

            fetch(url, {
            method: "POST",
            headers:{
                'Content-Type': 'application/json;charset=utf-8'
            },
            body: JSON.stringify(this.templateCrear)
            })
            .then(function(response){
                if(!response.ok){
                    console.log("Error "+response.status+": no se ha podido crear la peícula/libro en la API");
                }else{
                    console.log("Pelicula/libro creado/a correctamente");
                }
            })
            .catch(
                err => console.log(err)
            )
            .finally(()=>{
                this.$emit('actualizar', pathActual);
            });
                
        },

    }
}
</script>

<style scoped>
.algo{
    width: 60%;
    margin: 0 auto;
}

.crear{
    margin-left: 50%;
    transform: translateX(-50%);
}
</style>