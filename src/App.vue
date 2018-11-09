<template>
  <div class="row" id="app">
    <div class="col-md-8 col-md-offset-2 panel panel-default task">
        <div class="col-md-12"><img class="img-responsive" src="../dist/static/img/tasks-banner.jpg" alt="Tasks!" /></div>
        <div class="col-md-6">
            <h1>{{name}}</h1>
            <form class="form-horizontal" v-on:submit.prevent="addTask">
                <h2>Agregando tareas</h2>
                <!-- //- Vamos a agregar el HTML, para esto necesitamos dos “inputs” y un “button”. También debemos agregar las directivas correspondientes para enlazar el código con la vista. -->
                <div class="form-group">
                  <label class="col-sm-2 control-label">Titulo</label>
                    <div class="col-sm-10">
                      <input class="form-control" type="text" v-model="newTask.title" placeholder="tiulo" />
                    </div>
                </div>
                <div class="form-group">
                  <label class="col-sm-2 control-label">Horas</label>
                    <div class="col-sm-10">
                      <input class="form-control" type="number" v-model="newTask.time" placeholder="time" />
                    </div>
                </div>
                <div class="form-group">
                    <div class="col-sm-offset-2 col-sm-10">
                      <button class="btn btn-primary">Agregar tarea</button>
                      <button class="btn btn-danger" type="button" v-on:click="cancel">Cancelar</button>
                    </div>
                </div>
            </form>
        </div>
        <div class="col-md-6">
            <h2>Lista de Tareas</h2>
            <p>Total de Horas: <strong>{{ totalTime }}</strong></p>
            <ul style="list-style:none;" v-if="tasks.length">
                <li class="list" :key= "index" v-for="(task, index) in tasks">
                    <div>
                      <button class="btn btn-danger btn-sm" type="button" v-on:click="removeTask(index)">
                      <strong> X </strong>
                      </button>
                      <span> <strong>Tarea</strong> {{ task.title }}<strong> Tiempo:</strong> {{ task.time }}</span>
                    </div>
                </li>
            </ul>
            <div class="alert alert-info" v-else="">
              <strong>No hay tareas aun!</strong>
            </div>
        </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'app',
  data () {
    return {
      // variable a mostrar en h1
      name: 'Guarda tus tareas!',
      // array vacio para ir mostrando las tareas
      tasks: [],
      // Dentro de este objeto también agregamos una propiedad “tilte” de tipo String y una propiedad “time” de tipo Number. Recuerda inicializar las propiedades con valores default.
      newTask: {
        title: '',
        time: 0
      }
    }
  },
  // al momento de crearse el componente, debemos leer esta información para poder cargar la lista de tareas. Para eso dentro del hook “created”, escribimos el siguiente código: “this.tasks = JSON.parse(localStorage.getItem(‘tasks’)) || []”
  created: function () {
    this.tasks = JSON.parse(localStorage.getItem('tasks')) || []
  },
  methods: {
    // agregue una nueva tarea al array “tasks”. Una vez agregada también va a reiniciar los valores dentro de “newTaks”.
    addTask: function () {
      if (this.newTask.title && this.newTask.title && !isNaN(this.newTask.time)) {
        let newTaskObj = {
          title: this.newTask.title,
          time: this.newTask.time
        }
        // integrar la app con el local storage del browser. Dentro del metodo “addTask”, guardamos toda la lista de tareas en dicho storage usando este metodo: “localStorage.setItem(‘tasks’, JSON.stringify(this.tasks))
        this.tasks.push(newTaskObj)
        localStorage.setItem('tasks', JSON.stringify(this.tasks))
        this.newTask = {}
      } else {
        alert('Empty fields or Time is not a number')
      }
    },
    removeTask: function (index) {
      // recibir por parámetro el indice de la tarea y podemos utilizar ese indice (en conjunto con el método “splice” de Array)
      console.log(index)
      this.tasks.splice(index, 1)
      localStorage.setItem('tasks', JSON.stringify(this.tasks))
    },
    // Creamos también una funcionalidad para cancelar, para eso debemos crear un método llamado “cancel” que simplemente reinicie los valores de las propiedades de newTask.
    cancel: function () {
      this.newTask = {}
    }
  },
  computed: {
    //  recorrer todas las tareas y se calculo el total del tiempo trabajado.
    totalTime: function () {
      let time = 0
      this.tasks.forEach(function (task) {
        time += parseInt(task.time)
      })
      return time
    }
  }
}
</script>

<style lang="stylus" scoped>
.task{

}
.list{
  margin-bottom 10px;
}
</style>