<template>

  <section class="src-componentes-usuarios">
    <div class="jumbotron">
      <h2>Usuarios</h2>
      <hr>
      <hr>
      <br>

      <!-- <pre>{{ posts }}</pre> -->
      <!-- <pre>{{ usuarios }}</pre> -->

      <button class="btn btn-success mr-2 mb-3" @click="getUsuariosXHR()">GET XMLHttpRequest</button>
      <button class="btn btn-success mr-2 mb-3" @click="getUsuariosAxios()">GET Axios</button>
      <button class="btn btn-success mr-2 mb-3" @click="getUsuariosFetch()">GET Fetch</button>
      

      <div v-if="usuarios.length">
        <div class="table-responsive">
            <table class="table table-dark">
                <tr>
                    <th>ID</th>
                    <th>Nombre</th>
                    <th>Telefono</th>
                    <th>Email</th>
                </tr>
                <tr v-for="(usuario, index) in usuarios" :key="index">
                    <td>{{ usuario.id }}</td>
                    <td>{{ usuario.nombre }}</td>
                    <td>{{ usuario.telefono }}</td>
                    <td>{{ usuario.email }}</td>
                </tr>
            </table>
        </div>

        <br>
      <button class="btn btn-success mr-2 mb-3" @click="limpiarDatos()">Borrar tabla</button>
    </div>
    <h3 v-if="usuarios.length == 0 && consulta" class="alert alert-warning">
        No se encontraron usuarios.
    </h3>


    </div>
  </section>

</template>

<script>

  export default  {
    name: 'src-componentes-usuarios',
    props: [],
    mounted () {

    },
    data () {
      return {
        url: 'https://63681115d1d09a8fa6202fd8.mockapi.io/Usuarios',
        consulta: false,
        usuarios: []
      }
    },
    methods: {
      getUsuariosXHR() {
        console.log('GET con XMLHttpRequest ! ')
        try {
          const xhr = new XMLHttpRequest()
          xhr.open('get', this.url)
          xhr.addEventListener('load', () => {
            if(xhr.status == 200) {
              let respuesta = JSON.parse(xhr.response)
              this.usuarios = respuesta
            }
          })
          xhr.send( () => {
            this.consulta = true
          })
        }
        catch(error) {
           console.error('ERROR en getUsuarios', error)
           this.consulta = true
           this.usuarios = []
        }
      },
      async getUsuariosAxios() {
        console.log('GET con Axios ! ')
        try {
          let response = await this.axios(this.url)
          let respuesta = response.data
          this.usuarios = respuesta
        }
        catch(error) {
           console.error('ERROR en getUsuarios con Axios', error)
           this.usuarios = []
        }
        finally{
          this.consulta = true
        }
      },
      limpiarDatos() {
        this.usuarios = [],
        this.consulta = false
      },
      async getUsuariosFetch() {
        console.log('GET con Fetch ! ')
        try {
          let response = await fetch(this.url)
          let respuesta = await response.json()
          this.usuarios = respuesta
        }
        catch(error) {
            console.error('ERROR en getUsuarios con FETCH', error)
            this.usuarios = []
        }
        finally {
          this.consulta = true
        }
      }
    },
    computed: {

    }
}


</script>

<style scoped lang="css">

</style>
