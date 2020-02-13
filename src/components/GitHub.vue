<template>
    <div>
        <!-- TODO: Crear componente GitHub -->
    </div>
</template>

<script>
// Librería axios para realizar peticiones AJAX: https://github.com/axios/axios
import axios from 'axios'
// TODO: Importar componente GitHubRepos

export default {
    name: 'GitHub',
    components: {
        // TODO: Importar componente GitHubRepos
    },
    data: function() {
        return {
            user: '',
            userData: {},
            repos: [],
            isError : false,
            disabled: false,
            showRepos: false,
            validUser: false
        }
    },
    methods: {
        obtenerUsuario: function() {
            // Función para obtener los datos de usuario de la API de GitHub

            // Deshabilitar campo de texto
            this.disabled = true;

            // Resetear variable de error
            this.isError = false;

            // No mostrar lista de repositorios
            this.showRepos = false;

            // Crear URL del usuario
            var url = `https://api.github.com/users/${this.user}`;

            // Lanzar petición AJAX con Axios
            axios
                .get(url)
                .then(response => {
                    // La petición es válida y la respuesta contiene los datos del usuario.
                    // El parámetro 'response' contiene el resultado de la petición Ajax.
                    // Los datos se hayan en el campo 'response.data'
                    // El objeto con los datos del usuario se almacena en la variable 'userData'
                    this.userData = response.data;

                    // Indicamos que el usuario es válido
                    this.validUser = true;
                })
                .catch(error => {
                    // La petición se ha encontado con un error (posiblemente, usuario inexistente)
                    error;
                    // Indicamos que no se han cargado correctamente los datos de usuario
                    this.validUser = false;

                    // Indicar que hay un error
                    this.isError = true;
                })

            // Al terminar, volver a habilitar el campo de texto
            this.disabled = false;
        },
        obtenerRepos: function() {
            // Función para obtener los repositorios del usuario desde la API de GítHub
            // La URL de los repositorios de usuario se puede obtener a través del campo 'repos_url' de los datos del usuario
            var url = this.userData.repos_url;

            // Lanzar petición AJAX con Axios
            axios
                .get(url)
                .then(response => {
                    // La petición es válida. Almacenamos los datos de la respuesta en la variable local 'repos'
                    this.repos = response.data;

                    // Mostrar lista de repositorios
                    this.showRepos = true;
                })
                .catch(error => {
                    // La petición no es válida. Indicar que no se han cargado correctamente los repositorios
                    error;
                    this.validUser = false;
                    this.showRepos = false;

                    // Indicar que hay un error
                    this.isError = true;
                })
        }
    }
}
</script>

<style scoped>
</style>
