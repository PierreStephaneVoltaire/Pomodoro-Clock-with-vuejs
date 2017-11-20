<template>
  <div id="app">
    <el-container>
    
    <!--welcome message
  
    
  
      #requirements
  
    
  
      should display the name of the user
  
    
  
      # TODO
  
    
  
    refactor code to display any user that sucessfully logged in
  
    
  
      
  
    
  
      -->
  
  
  <el-header>
    <el-card>
    <h1 v-for="(user, index) in profile" :key="user">welcome {{user.name}}!!!</h1>
  </el-card>
  </el-header>
  <el-main>
    <el-card class="box-card">
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
      <el-row>
  
  
  
        <!--promodor clock
  
    
  
      #requirements
  
    
  
      should let user input a break time
  
    
  
      should let the user input a session time
  
    
  
      session time should be more than 0 seconds
  
    
  
      break time should be more than 0 seconds
  
    
  
    
  
      -->
  
  
  
        <el-col :lg="11" class="clock">
  
  
  
  
  
          <promodoro></promodoro>
  
  
  
  
  
        </el-col>
  
  
  
  
  
  
        <el-col :lg="11" class="taskManager" >
  
  
  
          <!--task manager
  
    
  
      #requirements
  
    
  
      should let user input a tasks
  
    
  
      should let the user delete tasks
  
    
  
      -->
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
          <el-card class="box-card">
  
  
  
  
  
  
  
            <div slot="header" class="clearfix">
  
  
  
  
  
  
  
              <span class="title">tasks</span>
  
  
  
  
  
  
  
              <el-button style="float: left; padding: 3px " type="primary" icon="plus" size="mini" @click="openNewDialog">add new task</el-button>
  
  
  
  
  
  
  
            </div>
  
  
  
  
  
  
  
            <el-menu class="tasks" v-for="(myTask, index) in tasks" :key="myTask">
  
  
  
  
  
  
  
              <el-menu-item :index="myTask.title">
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
                <el-row>
  
  
  
  
  
  
  
                  <el-col :md="4">
  
  
  
  
  
  
  
                    <span class="badge">
  
    
  
      
  
    
  
            {{index+1}}
  
    
  
      
  
    
  
          </span>
  
  
  
  
  
  
  
                  </el-col>
  
  
  
  
  
  
  
                  <el-col :md="8">
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
                    <span class="text">
  
    
  
      
  
    
  
            {{myTask.title}}
  
    
  
      
  
    
  
          </span>
  
  
  
  
  
  
  
                  </el-col>
  
  
  
  
  
  
  
                  <el-col :md="6">
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
                    <el-button class="options" type="primary" plain v-on:click="removeTask(myTask)">Completed</el-button>
  
  
  
  
  
  
  
                  </el-col>
  
  
  
  
  
<!--   
  
                  <el-col :md="4">
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
                    <el-button class="options" plain type="primary" v-on:click="editTask(myTask)">Edit</el-button>
  
  
  
  
  
  
  
                  </el-col> -->
  
  
  
  
  
  
  
                  <el-col :md="6">
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
                    <el-button class="options" type="info" plain v-on:click="viewDetails(myTask)">details</el-button>
  
  
  
  
  
  
  
                  </el-col>
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
                </el-row>
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
              </el-menu-item>
  
  
  
  
  
  
  
            </el-menu>
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
          </el-card>
  
  
        </el-col>
  
      </el-row>

    </el-card>
  
  
  </el-main>
  
  <el-footer>
   <el-card>
     <el-row>
       <el-col :lg="24" >
  <h3>Instructions</h3>
    <el-steps   simple >
  <el-step title="step 1:select a session time" ></el-step>
     <el-step title="step 2:select a break time" ></el-step>
   <el-step title="step 3:add a few tasks you'd like to work on" ></el-step>
  <el-step title="step 4:start working" ></el-step>
  <el-step title="step 5:take a break" ></el-step>
    <el-step title="step 6:go back to step 4" ></el-step>

</el-steps>
       </el-col>
     </el-row>


   </el-card>
  </el-footer>
  
  
  
  
  
  
  
  
  
  
    <el-dialog title="New Task" :visible.sync="dialogFormVisible">
  
  
  
  
  
  
  
      <el-form :model="newTask" :rules="rules" ref="newTask">
  
  
  
  
  
  
  
        <el-form-item label="Title" prop="title">
  
  
  
  
  
  
  
          <el-input v-model="newTask.title" auto-complete="off"></el-input>
  
  
  
  
  
  
  
        </el-form-item>
  
  
  
  
  
  
  
        <el-form-item label="Description" prop="description">
  
  
  
  
  
  
  
          <el-input type="textarea" v-model="newTask.description" auto-complete="off"></el-input>
  
  
  
  
  
  
  
        </el-form-item>
  
  
  
  
  
  
  
      </el-form>
  
  
  
  
  
  
  
      <span slot="footer" class="dialog-footer">
  
    
  
      
  
    
  
      <el-button @click="dialogFormVisible = false">Cancel</el-button>
  
    
  
      
  
    
  
      <el-button type="primary" @click="addTask('newTask')
  
  ">Confirm</el-button>
  
    
  
      
  
    
  
      </span>
  
  
  
  
  
  
  
    </el-dialog>
  
  
  
  
  
  
  
    <el-dialog :title="newTask.title" :visible.sync="dialogDetailsVisible" width="30%">
  
  
  
      <span>{{newTask.description}}</span>
       

  
  
      <span slot="footer" class="dialog-footer">
  
    
  
        <el-button @click="dialogDetailsVisible = false">ok</el-button>
  
    
  
      </span>
  
  
  
    </el-dialog>
  
  
  
  
  
  
  
  
  
    </el-container>
  
  
  
  
  
  
  
  </div>
</template>

<script>
import Firebase from "firebase";

let config = {
  apiKey: "AIzaSyB9FSgZlVlhphizfwOKW0VbJdMQKKhD824",

  authDomain: "promotoclock.firebaseapp.com",

  databaseURL: "https://promotoclock.firebaseio.com",

  projectId: "promotoclock",

  storageBucket: "",

  messagingSenderId: "222877538116"
};

let app = Firebase.initializeApp(config);

let db = app.database();

let userRef = db.ref("users");

let tasksRef = db.ref("users/1/tasks");

function Login() {}

function Logout() {}

function Register() {}

import Promodoro from "./components/PromodoroClock";

export default {
  name: "app",

  components: {
    Promodoro
  },

  firebase: {
    tasks: tasksRef,

    profile: userRef
  },

  data() {
    return {
      childRef:null,
      //to chexk if the user is logged in

      loggedin: true,

      //toggles the details dialog

      dialogDetailsVisible: false,

      //toggle the form dialog

      dialogFormVisible: false,

      // task object

      newTask: {
        title: "",

        description: ""
      },
      rules: {
          title: [
            { required: true, message: 'Please input a valid title', trigger: 'change' },
          ],
           description: [
            { required: true, message: 'Please input a valid description', trigger: 'change' },
          ],
        
        }
      
    };
  },

  //methods

  methods: {
    /**
  
       * starts/ends the time loop depending the clock's status
  
       * should check if session time is > 0
  
       * should check if break time is >0
  
       * interval ref should not be in this function because it's needed to stop the 
  
       * setInterval loop
  
       * 
  
       */

    removeTask(task) {
      tasksRef.child(task[".key"]).remove();
    },
/**
 * fix firebase duplicate on update issue
 */
//     editTask: function(e) {
//                   this.childref = e['.key'];

//       this.dialogFormVisible = true;

//       this.newTask.title = e.title;

//       this.newTask.description = e.description;
//       this.updateTask(e)
//     },
//       updateTask: function(e) {
// e={...e}
//    this.profile.tasks.child(e['.key']).update(e)

// //  this.tasks.child(e).set({ title:this.newTask.title,description:  this.newTask.description});

//     },

    openNewDialog() {
      this.newTask.title = "";

      this.newTask.description = "";

      this.dialogFormVisible = true;
    },

    addTask(formName) {
       this.$refs[formName].validate((valid) => {
          if (valid) {
            tasksRef.push(this.newTask);

      this.dialogFormVisible = false;

      this.newTask.title = "";

      this.newTask.description = "";
          } 
        });
      }
     
    
    ,viewDetails(task){
this.newTask.title=task.title
this.newTask.description=task.description
this.dialogDetailsVisible = true;
    }
  }
};
</script>

<style>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;

  -webkit-font-smoothing: antialiased;

  -moz-osx-font-smoothing: grayscale;

  text-align: center;

  color: #2c3e50;
}

#btns {
  padding-top: 20px;
}

.selected {
  background-color: #cfd8dc !important;

  color: white;
}
.title{
  font-weight: bold;
  font-size: 20px;
}

.tasks {
  margin: 0 0 2em 0;
  list-style-type: none;

  padding: 0;
}

.tasks el-menu-item {
  cursor: pointer;

  position: relative;

  left: 0;

  background-color: #eee;

  margin: 0.5em;

  padding: 0.3em 0;

  border-radius: 4px;
}

.tasks el-menu-item.selected:hover {
  background-color: #bbd8dc !important;

  color: white;
}

.taks el-menu-item :hover {
  color: #607d8b;

  background-color: #ddd;

  left: 0.1em;
}

.tasks .text {
  position: relative;
font-size:20px; 

  top: -3px;
}
.btn{
  width: 25%;
}
.taskManager{
  margin-left:30px; 
  margin-right: 30px;
}

.options {
  width: 100%;
}

.clock {
 margin-right: 30px;
  margin-left: 30px;
}

.tasks .badge {
  display: inline-block;

  font-size: small;

  color: white;

  padding: 1em;

  background-color: #607d8b;

  line-height: 1em;

  position: relative;

  left: -1px;

  top: -4px;

  height: 2em;

  border-radius: 4px;
}
</style>
