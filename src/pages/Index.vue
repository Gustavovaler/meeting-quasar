<template>
<div class="q-pa-md">
   <q-editor
      v-model="editor"
      :definitions="{
        save: {
          tip: 'Guarde su trabajo',
          icon: 'save',
          label: 'Guardar',
          handler: saveWork
        }
      
      }"
      :toolbar="[
        ['bold', 'italic', 'strike', 'underline'],
        ['upload', 'save']
      ]"
    />
    <q-card  class="row"
     v-for="(item, index) of tasks" :key="index">
      <q-card-section class="col" v-html="item.texto" :class="item.estado ? 'tachar' : '' " />
      <q-btn flat color="blue" @click="item.estado = !item.estado">Accion</q-btn>
       <q-btn flat color="red" @click="eliminar(index)">Eliminar</q-btn>
    </q-card>
    <div class="flex flex-center" v-if="tasks.length == 0" >
      <h5>Sin notas</h5>
    </div>
  
  
</div>
</template>

<script>
export default {
  data(){
    return{
      editor: "", 
      tasks:[
        
      ]
    }
  },
  methods: {
    saveWork () {
      if (this.editor != '') {      
      
      this.tasks.push({
        texto: this.editor,
        estado: false
      });
      this.editor = '';
      this.$q.notify({
        message: 'La nota ha si guardada !',
        color: 'green-4',
        textColor: 'white',
        icon: 'cloud_done'
      })
      }
    } ,
    eliminar(index){
      
      this.$q.dialog({
        title: 'Advertencia !',
        message: 'Realmente desea eliminar esta tarea?',
        cancel: true,
        persistent: true
      }).onOk(() => {
        this.tasks.splice(index, 1)
      })     
    }
  }
}
</script>
<style scoped>
.tachar{
  text-decoration: line-through;
}

</style>
