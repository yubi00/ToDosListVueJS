<template>

  <div id="updateBox">
      <input type="text" placeholder="Update item" id="updateitem"  v-model="updateditem"/>
      <input type = "button" id="btn2" value="Edit" style="height: 45px; width: 50px;" @click="editItem()"/>
      <input type = "button" id="btn3" value="Cancel" style="height: 45px; width: 55px; margin-left: 10px;" @click="cancel()" />
  </div> 

</template>

<script>

import {EventBus} from '../main.js'

  export default  {
    name: 'UpdateTodo',
    props:  {
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

    created() {
      EventBus.$on('updatetodos', (data) => {
        document.getElementById('updateBox').style.display = 'inline';
        document.getElementById("selectall").style.display = 'none';
        this.updatingitem = data; 
        document.getElementById('updateitem').value = this.updatingitem;
        console.log("updating item: " + this.updatingitem);
      });
    },

    data() {
      return {
        updateditem: '',
        updatingitem: '',
        itemindex: 0,
        title: this.todos.title,
        completed: this.todos.completed

      }
    },
    
    methods: {
      editItem() {
        console.log("To do list are : "+ JSON.stringify(this.todos));
        this.itemindex = this.todos.findIndex(data => data.title == this.updatingitem);
        console.log("The item index of the updating item is "+this.itemindex);
        console.log("the updated item is "+this.updateditem);
        EventBus.$emit('updatetodo', this.itemindex, this.updateditem);
        document.getElementById("updateBox").style.display = 'none';
      },

      cancel() {
        document.getElementById('updateBox').style.display = 'none';
        document.getElementById("selectall").style.display = 'inline';
      }
    },
    computed: {

    }
}
</script>

<style>
   #updateBox
    {
        display: none;
        text-align: center;
        margin-right: 155px;
    }

    #updateitem {
    width: 500px;
    height: 40px;
    margin-right: 10px;
}
</style>
