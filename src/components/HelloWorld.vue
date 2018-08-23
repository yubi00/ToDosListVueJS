<template>
  <div class="hello">
    <h1>{{ msg }}</h1>
    <div id= "newItem" style="width: 800px; height: 50px; margin-left: 40px; margin-bottom: 10px;">
      <input id="inputbox"  placeholder="Add a new item" v-model="newitem"  @keyup.enter="addItem()"> 
      <input type = "button" id="btn" value="Add" style="height: 45px; width: 50px"  v-on:click="addItem()">
      <input type = "button" id="btn1" value="Reset" style="height: 45px; width: 55px; margin-left: 10px"  v-on:click="clearAll()">
    </div>
    <UpdateTodo :todos = "todos" />
   <div id="selectall" style="float:left; margin-left: 40px;">
        <input type="checkbox" v-model="checked" id="selectall" v-on:change="CheckUncheck()">
        <label>Select All</label>
    </div> 
    <div style="margin-top: 40px;">
      <ul v-for="(todo, index) in todos" v-bind:key="index">
        <li  :class="{completed: todo.completed}" style="background-color: lightblue; width: 800px; height: 50px;" >
            <input type="checkbox" id="checkbox" style="float: left;" v-model="todo.completed"  @click="isCompleted(index)"   >
          <strong >{{ todo.title }}</strong>
          <span><input type="button" value="X"  id="deleteitem" v-on:click="deleteItem(index)"></span>
          <span><button><img src="http://www.iconarchive.com/download/i80229/custom-icon-design/flatastic-1/edit.ico" @click = "callUpdate(index)" width="13" height="13"/></button></span> 
        </li>  
     </ul>

    </div>
    
    <div style="float:left; margin-left: 40px;">
        Task Completed: {{ taskcompleted }}
    </div>
  </div>
</template>

<script>
import UpdateTodo from './UpdateTodo.vue'
import {EventBus} from '../main.js'

export default {
  name: 'HelloWorld',
  components: {
    UpdateTodo
  },
  props: {
    msg: String
    
  }
  ,
  created(){
    EventBus.$on('updatetodo', this.editTodo );
  },
  data() {
    return {
      newitem: '',
      todos: localStorage.getItem('items') ? JSON.parse(localStorage.getItem('items')) : [{
        title: 'Add a new todo list',
        completed: false
      }],
      taskcompleted: 0,
      checked: false,
      itemindex: 0, 
      todo: ''

    }
  },

  computed: {
    
  },

  methods:{
    editTodo(index, todo) {
      this.todos[index].title = todo ;
      console.log("The updated index is "+index);
      console.log("The udpated item name is" + todo);
      localStorage.setItem("items", JSON.stringify(this.todos));
    },
    callUpdate(index) {
      this.todo = this.todos[index].title;
      EventBus.$emit('updatetodos', this.todo);
      
    },
    isCompleted: function(index) {
      this.todos[index].completed = !this.todos[index].completed; 
      
      if(this.todos[index].completed === true){
        this.taskcompleted++;
      }
      else{
        this.taskcompleted--;
      }
     
    },

    CheckUncheck() {
      if(this.checked === true)
      {
          this.todos.filter(todo => {
            todo.completed = true;
            this.taskcompleted = this.todos.length;
          })
      }
      else
      {
        this.todos.filter(todo => {
            todo.completed = false;
            this.taskcompleted = 0; 
          })
      }
    },

    addItem(){
      if(this.newitem == '')
      {
          alert("Invalid item");
      }
      else{
          this.todos.push({
        title: this.newitem,
        completed: false
      });
      console.log(this.newitem);
      this.newitem = '';
      console.log(this.todos);
      localStorage.setItem("items", JSON.stringify(this.todos));
      }
      
      },
    
    deleteItem(index) {
      this.todos.splice(index,1);
      localStorage.setItem("items", JSON.stringify(this.todos));
      console.log(this.todos);
    },

    clearAll() {
      localStorage.removeItem("items");
      this.todos = [];
      this.taskcompleted = 0;
    },
 
  },
  
  
}

</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
ul {
  list-style-type: none;
  
}


#inputbox {
    width: 500px;
    height: 40px;
    margin-right: 10px;
}



h1 {
  background-color: slategray;
  width: 800px; 
  height: 60px;
  margin-left: 40px;
  padding-top: 20px;
}
li {
  margin-top: 10px;
  padding-top: 20px;
 
}
a {
  color: #42b983;
}

.hello {
  margin-left: 300px;

}

#newItem {
 text-align: center;
}
 span {
   float: right;
   margin-right: 10px;
 }


 .completed
 {
   text-decoration: line-through;
 }

</style>
