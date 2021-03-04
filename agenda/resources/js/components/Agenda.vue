<template>
  <div>
    <div class="header">
        <h1 class="text-center m-4">AGENDA</h1>
    <img src="https://img.icons8.com/ios-filled/50/000000/apple-phone.png"/>
    </div>


  <button @click="modificar=false; abrirModal();" type="button" class="icon add my-4">Nuevo</button>

    <!-- The Modal -->
    <div class="modal" :class="{mostrar: modal}">
      <div class="modal-dialog">
        <div class="modal-content">
          <!-- Modal Header -->
          <div class="modal-header">
            <h4 class="modal-title">{{tituloModal}}</h4>
            <button @click="cerrarModal();"  type="button" class="close" data-dismiss="modal">
              &times;
            </button>
          </div>

          <!-- Modal body -->
          <div class="modal-body">
            <div class="my-4">
              <label for="Nombre">Nombre</label>
              <input v-model="agenda.name" type="text" class="form-control" id="Nombre">
            </div>
            <div class="my-4">
              <label for="Numero">Número</label>
              <input v-model="agenda.phonenumber" type="number" class="form-control" id="Numero">
            </div>
            <div class="my-4">
              <label for="Direccion">Dirección</label>
              <input v-model="agenda.address" type="text" class="form-control" id="Direccion">
            </div>
          </div>

          <!-- Modal footer -->
          <div class="modal-footer">
            <button @click="cerrarModal();" type="button" class="btn btn-secondary" data-dismiss="modal">
              Cancelar
            </button>
            <button  @click="guardar();" type="button" class="btn btn-success" data-dismiss="modal">
              Guardar
            </button>


          </div>
        </div>
      </div>
    </div>

    <table class="table table-striped">
      <thead class="bg-dark text-white">
        <tr>
          <th scope="col" class="text-center">ID</th>
          <th scope="col" class="text-center">Nombre</th>
          <th scope="col" class="text-center">Número</th>
          <th scope="col" class="text-center">Dirección</th>
          <th scope="col" colspan="2" class="text-center">Acción</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="agen in agendas" :key="agen.id">
          <th scope="row">{{ agen.id }}</th>
          <td>{{ agen.name }}</td>
          <td>{{ agen.phonenumber }}</td>
          <td>{{ agen.address }}</td>
          <td>
            <button  @click="modificar=true; abrirModal(agen);" class="icon pencil">Editar</button>
          </td>
          <td>
            <button @click="eliminar(agen.id)" class="icon delete">Eliminar</button>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
export default {
  data() {
    return {
      agenda:{
        name:'yy',
        phonenumber:'yuu',
        address:'ss',
      },
      id:0,
      modificar:true,
      modal:0,
      tituloModal:'',
      agendas: [],
    };
  },
  methods: {
    async listar() {
      const res = await axios.get('/agendas');
      this.agendas = res.data;
    },
    async eliminar(id) {
      const res = await axios.delete('/agendas/' + id);
      this.listar();
    },
    async guardar() {
      if(this.modificar){
        const res = await axios.put('/agendas/'+this.id, this.agenda);
        // console.log(this.id);
      }else{
        const res = await axios.post('/agendas/', this.agenda);
      }
      this.cerrarModal();
      this.listar();
    },
    abrirModal(data={}){
      this.modal=1;
      if(this.modificar){
        this.id=data.id;
        this.tituloModal="Editar Contacto: "+data.name ;
        this.agenda.name=data.name;
        this.agenda.phonenumber=data.phonenumber;
        this.agenda.address=data.address;
      }else{
        this.id=0;
        this.tituloModal="Registrar Contacto";
        this.agenda.name='';
        this.agenda.phonenumber='';
        this.agenda.address='';
      }
    },
    cerrarModal(){
      this.modal=0;
    },
  },
  created() {
    this.listar();
  },
};
</script>

<style>
  .mostrar{
    display: list-item;
    opacity: 1;
    background: #69696966;
  }

  .icon{
    border: none;
    display: block;
    text-indent: -9999px;
    width: 30px;
    height: 30px;
    background-size: 30px 30px !important;
    background-size: contain !important;
  }

  .pencil{
    background: url('./icons/edit.svg') no-repeat top left !important;;
  }

  .delete{
    color: red;
    background: url('./icons/delete.svg') no-repeat top left !important;
  }

  .add{
    background: url('./icons/add.svg') no-repeat top left !important;
    cursor: cell;
    width: 60px;
    height: 60px;
    background-size: 60px 60px !important;
  }

  .header {
    display: flex;
    justify-content: center;
    align-items: center;
  }


</style>
