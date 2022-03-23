<template>
    <div>
        <h3 class="text-center mb-3">Editar Elemento</h3>
        <form class="anchura">
            <!--<div v-for="elemento in templateEditar.data" :key="elemento.prompt">-->
            <div v-for="(elemento,index) in templateEditar.data" :key="index">
                <input v-if="elemento.type == 'text'  || elemento.type == 'image' || elemento.type == 'embeddedVideo'" type="text" class="form-control mb-4" v-model="elemento.value">
                <input v-if="elemento.type == 'date'" type="date" class="form-control mb-4" v-model="elemento.value">
                <textarea v-if="elemento.type == 'textarea'" type="textarea" class="form-control mb-4" v-model="elemento.value" ></textarea>
            </div>
            <a @click="editItem(templateEditar.href)" type="button" class="btn btn-success mb-3 mt-4 mr-3">Guardar</a>
            <a @click="$emit('cancelar')" type="button" class="btn btn-danger mb-3 mt-4">Cancelar</a>
        </form>  
    </div>
</template>

<script>

export default {
    name: 'FormularioEditar',
    components: {

    },
    props: {
        templateEditar: Object,
        path: String,
    },
    data: function() {
        return {
            // TODO: crear variables de datos para el funcionamiento del componente
            //path: window.location.pathname,
        }
    },
    methods: {
        editItem: function(item){
            let url = this.$APIURL;
            console.log(url+''+item);
            console.log(this.templateEditar);

            let template = {
                "template":{
                    data: this.templateEditar.data
                }
            };

            fetch(url+''+item, {
                method: "PUT",
                headers:{
                'Content-Type': 'application/json;charset=utf-8'
            },
                body: JSON.stringify(template)
            })
            .then(function(response){
                if(!response.ok){
                    console.log("Error "+response.status+": no se ha podido actualizar la peícula/libro en la API");
                }else{
                    console.log("Pelicula/libro actualizado/a correctamente");
                }
            })
            .catch(
                err => console.log(err)
            )
            .finally(()=>{
                console.log(this.path);
                this.$emit('cancelar');
                this.$emit('actualizar', this.path);
            });
        },
    }
}
</script>

<style scoped>
.anchura{
    width: 90%;
    margin: 0 auto;
}
</style>