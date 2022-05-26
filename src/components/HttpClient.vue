<template>
  <section class="src-components-http-client">
    <div class="jumbotron">
      <h2>HttpClient</h2>
      <hr>
      <hr>
      
      <div class="btn btn-primary my-3 mr-3" @click="getUsuariosXHRPromise()">Pedir usuarios (XHRProm)</div>
      <div class="btn btn-primary my-3 mr-3" @click="getUsuariosFetch()">Pedir usuarios (fetch)</div>
      <div class="btn btn-primary my-3 mr-3" @click="getUsuariosAxios()">Pedir usuarios (axios)</div>
      <div class="btn btn-danger my-3 mr-3" @click="usuarios=[]">CLEAR</div>
      <br>

      <div v-if="usuarios.length" class="table-responsive">
        <table class="table table-dark">
          <tr>
            <th>Usuario</th>
            <th>Email</th>
            <th>Numero de telefono</th>
          </tr>

          <tr v-for="(user, index) in usuarios" :key="index">
            <td>{{user.name}}</td>
            <td>{{user.email}}</td>
            <td>{{user.numTel}}</td>
          </tr>
        </table>
      </div>
      <h5 v-else class="alert alert-danger text-center">No se encontraron usuarios</h5>
    </div>
  </section>

</template>

<script>

  export default  {
    name: 'src-components-http-client',
    props: [],
    mounted () {

    },
    data () {
      return {
        url: 'https://628e9f0c368687f3e719d47f.mockapi.io/usuarios',
        usuarios:[], 
      }
    },
    methods: {
      wrapperXHRPromise(){
        return new Promise((resolve, reject) => {
          
          const xhr = new XMLHttpRequest();
          
          xhr.open('get',this.url);

          xhr.addEventListener('load', () =>{
            if (xhr.status == 200) {
              let rta = JSON.parse(xhr.response);
              resolve(rta)
            }
            else{
              console.error('Error XHR cb (status)', xhr.status)
              let error= {
                tittle: 'Error XHR cb (status)',
                status: xhr.status
              }
              reject(error)
            }
          })

          xhr.addEventListener('error', e =>{
            console.error('ERROR XHR cb (ajax)', e)
            let error = {
              tittle: 'ERROR XHR cb (ajax)',
              info: e
            }
            reject(error)
          })

          xhr.send()
        })
      },
      getUsuariosXHRPromise(){
        this.wrapperXHRPromise()
        .then(rta => {
          this.usuarios = rta
        })
        .catch(error => console.error('Error XHRPromise', error))
      },
      getUsuariosFetch(){
        fetch(this.url)
        .then(response => response.json())
        .then(rta => {
          this.usuarios = rta;
        })
        .catch(error => console.error('Error Fetch', error))
      },
      async getUsuariosAxios(){
        try {
          let  rta  = await this.axios(this.url)
          this.usuarios =  rta.data 
        } catch (error) {
          console.error('Error Axios', error)
        }
        
      },
    },
    computed: {

    }
}


</script>

<style scoped lang="css">
  .src-components-http-client {

  }
  .jumbotron{
    background-color: rgb(49, 192, 49);
    color: black;
  }
</style>
