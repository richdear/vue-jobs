<script setup>
import {ref, onMounted} from 'vue';


    const name=ref("Van Mann");
    const status =ref("active");
    const tasks=ref(["task 1", "task 2", "task 3"]);
    const link = ref("https://google.com");
    const newTask=ref('');

    const addTask=()=>{
      console.log("adding task");
      if(newTask.value.trim() !== ''){
        tasks.value.push(newTask.value);
        newTask.value="";
      }
    };
    const deleteTask=(index)=>{
      tasks.value.splice(index, 1);
      console.log("deleting task at index", index);
    };

    const toggleUserStatus=()=>{
      console.log("Chaing user status");
      if(status.value=="active"){
          status.value = "pending";
        }else if(status.value=="pending"){
          status.value = "disabled";
        }else{
          status.value = "active";
        } 
    };

    onMounted(async ()=>{
      try{
          const response= await fetch('https://jsonplaceholder.typicode.com/todos');
          const data=await response.json();
          tasks.value=data.map((task)=>task.title);

      }catch(error){
        console.log("Error fetching data");
          console.log(error);
      }
    })
 
</script>


<template>
  <h1> Hello {{name}}!</h1><br>
  <p v-if="status==='active'">User is active</p>
  <p v-else-if="status=='pending'">Use is pending</p>
  <p v-else>Use is disabled</p>
  <br>

  <form @submit.prevent="addTask">
    <label for="newTask">Add a new task:</label>
    <input type="text" id="newTask" name="newtask" v-model="newTask">
    <button type="submit">Submit</button>
  </form>

<br>
  <h3>Tasks</h3>
<br>
  <ul>
    <li v-for="(task, index) in tasks" :key="task">
     <span > {{ task}} </span> <button @click="deleteTask(index)">X</button>
    </li>
  </ul>

<br>
<!-- <a v-bind:href="link"> Click for Google</a> -->
<a :href="link"> Click for Google</a>


<button @click="toggleUserStatus"> Change Status</button>
</template>
