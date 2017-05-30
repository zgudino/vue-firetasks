<template>
<div>
  <section class="hero is-info is-bold">
    <div class="hero-head">
      <header class="nav">
        <div class="container">
          <div class="nav-left">
            <a class="nav-item heading">VueFire-Tasks</a>
          </div>
        </div>
      </header>
    </div>
    <div class="hero-body">
      <div class="columns">
        <div class="column is-two-thirds-tablet is-offset-2-tablet is-half-desktop is-offset-one-quarter-desktop">
          <div class="container">
            <div class="control has-addons">
              <input type="text"
                     v-model="taskText"
                     @keyup.enter="addTask"
                     class="input is-white is-expanded"
                     placeholder="¿¡Que tarea sigue!?">
              <button class="button is-info is-inverted is-outlined" @click="addTask">Done</button>
            </div>
          </div>
        </div>
      </div>
    </div>
  </section>

  <main class="">
    <div class="container">
      <div class="column is-gapless is-two-thirds-tablet is-offset-2-tablet is-half-desktop is-offset-one-quarter-desktop">
        <div class="card" v-for="task of tasks">
          <div class="card-content">
            <h1 class="title">{{ task.text }}</h1>
            <small>
            {{ task.createdAt }}
            </small>
            <div class="card-actions">
              <button class="button is-danger is-outlined" @click="deleteTask(task)">Delete</button>
            </div>
          </div>
        </div>
      </div>
    </div>
  </main>
</div>
</template>

<style lang="scss">
  @import './node_modules/bulma/bulma';

  .card-actions {
    padding-top: 24px;
  }
</style>

<script>
  import Firebase from 'firebase'

  const db = Firebase.initializeApp({
    // NOTA: Mi propio deck, por favor utilizar con cuidado!
    databaseURL: 'https://playground-1fd9c.firebaseio.com'
  }).database()

  export default {
    name: 'App',
    data () {
      return {
        taskText: ''
      }
    },

    firebase () {
      return {
        tasks: db.ref('tasks').limitToLast(10)
      }
    },

    methods: {
      addTask () {
        this.$firebaseRefs.tasks.push({
          text: this.taskText,
          createdAt: new Date().toString()
        })
        this.taskText = ''
      },
      deleteTask (task) {
        this.$firebaseRefs.tasks.child(task['.key']).remove()
        // this.$firebaseRefs.tasks.child([task['.key']])
        //     .remove()
        //     .then(snap => console.log('Task removed: ', snap))
      }
    }
  }
</script>
