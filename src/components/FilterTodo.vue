<template>
    <div id="filters">
        <button  @click="showAll">All </button>
        <button @click="completedTodos"> Completed </button>
        <button @click="incompletedTodos" >Not-completed </button>
        <button @click="clearCompleted">Clear Completed</button>

        <div id="incompletedtodos" >
            <p v-if="showIncompleted.length"><strong>Incompleted todos</strong></p>
            <ul v-if="showIncompleted.length" v-for="(todo, index) in showIncompleted" v-bind:key="index">
                <li>{{ todo.title }}</li>
            </ul>
        </div>
        <div id="completedtodos" >
            <p  v-if="showCompleted.length"><strong>Completed todos</strong></p>
            <ul v-if="showCompleted.length" v-for="(todo, i) in showCompleted" v-bind:key="i">
                <li>{{ todo.title }}</li>
            </ul>
        </div>
        
    </div>
    
</template>

<script>
import {EventBus} from '../main.js'

export default {
    name: 'FilterTodo',
    props: {
      todos: {
        title: {
          type: String,
          required: true
        },
        completed: {
          type: Boolean,
          required: true
        }
        
      },
        
    },
    data() {
        return {

        }
    },
    methods: {
       showAll(){
           console.log("All todos: "+JSON.stringify(this.todos));
           EventBus.$emit('showalltodos', this.todos);

       },

       completedTodos() {
           EventBus.$emit('showcompletedtodos', this.showCompleted);

       },

       incompletedTodos() {
           EventBus.$emit('showincompletedtodos', this.showIncompleted);
       },

       clearCompleted() {
           EventBus.$emit('clearcompletedtodos', this.showCompleted);
       }
       
    },
    computed: {
        showCompleted: function() {
            return this.todos.filter(todo => todo.completed);
        },

        showIncompleted: function() {
            return this.todos.filter(todo => !todo.completed);
        }
    },
  

}
</script>

<style>
 #filters {
     float: left;
     margin: 20px;
     margin-left: -135px;
 }

 #filters button {
     margin: 10px;
 }

 #completedtodos {
     float: right;
     margin: 20px;
 }

 #incompletedtodos {
     float: left;
     margin: 20px;
 }
</style>
