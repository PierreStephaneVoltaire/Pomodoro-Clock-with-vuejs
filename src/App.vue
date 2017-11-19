<template>
  <div id="app">
  
  
  
  
  
    <!--nav bar
  
    #requirements
  
    should display an info option
  
    should display a login option 
  
    # task
  
  displays menu items
  
  
  
    -->
  
    {{inBreak}}
  
    <el-menu theme="dark" class="el-menu-demo" mode="horizontal">
  
  
  
      <el-menu-item index="1">promoto clock</el-menu-item>
  
  
  
    </el-menu>
  
    <!--welcome message
  
    #requirements
  
    should display the name of the user
  
    # TODO
  
  refactor code to display any user that sucessfully logged in
  
    
  
    -->
  
    <h1 v-for="(user, index) in profile" :key="user">welcome {{user.name}}!!!</h1>
  
    <el-card class="box-card">
  
  
  
  
  
  
  
      <el-row>
  
        <!--promodor clock
  
    #requirements
  
    should let user input a break time
  
    should let the user input a session time
  
    session time should be more than 0 seconds
  
    break time should be more than 0 seconds
  
  
    -->
  
        <el-col :lg="9">
  
  
  
          <el-card class="box-card">
  
  
  
            <div slot="header" class="clearfix">
  
  
  
              <h1>Progress</h1>
  
  
  
  
  
            </div>
  
            <!--clock
  
    #requirements
  
    displays the progress of the session in percent
  
    the session circle should be blue,
  
    the break circle should be red 
  
    the clock should stop when tasks.length=0
  
    -->
  
            <el-row>
  
  
  
              <el-col :lg="24">
  
  
  
  
  
                <el-progress :width="200" :stroke-width="10" class="clock" type="circle" :percentage="percentage"></el-progress>
  
  
  
              </el-col>
  
  
  
            </el-row>
  
  
  
            <!--reset button
  
    #requirements
  
    it should reset the timer and session/break time 
  
  
  
    
  
    -->
  
            <el-row id="btns">
  
  
  
              <el-col :lg="11">
  
  
  
  
  
  
  
                <el-button class="options">reset</el-button>
  
  
  
  
  
  
  
              </el-col>
  
  
  
              <!--start button
  
    #requirements
  
   it should start the clock
  
   once the clock starts, it should change color (red) and display stop
  
  
  
    
  
    -->
  
  
  
              <el-col :lg="11">
  
  
  
  
  
  
  
                <el-button class="options" type="success" @click="startClock()">start</el-button>
  
  
  
  
  
  
  
              </el-col>
  
  
  
  
  
  
  
            </el-row>
  
  
  
  
  
  
  
            <el-row>
  
  
  
  
  
  
  
              <el-col :lg="12">
  
  
  
                <h1>Break Time</h1>
  
  
        <!--break time
  
    #requirements
  
    should let user input a break time
  
   10 to 30 minutes
  
    -->
  
  
  
                <el-time-picker v-model="breakTime" :picker-options="{
  
        selectableRange: '00:10:00 - 00:30:00'
  
      }" placeholder="session time">
  
                </el-time-picker>
  
  
  
  
  
              </el-col>
  
  
  
  
  
  
  
              <el-col :lg="12">
  
  
  
  
  
  
  
                <h1>Session Time</h1>
  
  
        <!--session time
  
    #requirements
  
    
  
    should let the user input a session time
  
    10 minutes to an 2 hours
  
    -->
  
  
  
                <el-time-picker v-model="sessionTime" :picker-options="{
  
        selectableRange: '00:10:00 - 02:00:00'
  
      }" placeholder="session time">
  
                </el-time-picker>
  
  
  
  
  
  
  
              </el-col>
  
  
  
            </el-row>
  
          </el-card>
  
        </el-col>
  
  
  
        <el-col :lg="13" :offset="2">
  
        <!--task manager
  
    #requirements
  
    should let user input a tasks
  
    should let the user delete tasks
  
    -->
  
  
  
  
  
  
  
  
  
  
          <el-card class="box-card">
  
  
  
            <div slot="header" class="clearfix">
  
  
  
              <span>tasks</span>
  
  
  
              <el-button style="float: left; padding: 3px " type="primary" icon="plus" size="mini" @click="openNewDialog">add new task</el-button>
  
  
  
            </div>
  
  
  
            <el-menu class="tasks" v-for="(thing, index) in tasks" :key="thing">
  
  
  
              <el-menu-item :index="thing.title">
  
  
  
  
  
  
  
                <el-row>
  
  
  
                  <el-col :md="4">
  
  
  
                    <span class="badge">
  
    
  
          {{index+1}}
  
    
  
        </span>
  
  
  
                  </el-col>
  
  
  
                  <el-col :md="8">
  
  
  
  
  
  
  
                    <span class="text">
  
    
  
          {{thing.title}}
  
    
  
        </span>
  
  
  
                  </el-col>
  
  
  
                  <el-col :md="4">
  
  
  
  
  
  
  
  
  
  
  
                    <el-button class="options" type="primary" plain>Completed</el-button>
  
  
  
                  </el-col>
  
  
  
                  <el-col :md="4">
  
  
  
  
  
  
  
                    <el-button class="options" plain type="primary" v-on:click="editTask(thing)">Edit</el-button>
  
  
  
                  </el-col>
  
  
  
                  <el-col :md="4">
  
  
  
  
  
  
  
                    <el-button class="options" type="info" plain>details</el-button>
  
  
  
                  </el-col>
  
  
  
  
  
  
  
                </el-row>
  
  
  
  
  
  
  
  
  
  
  
              </el-menu-item>
  
  
  
            </el-menu>
  
  
  
  
  
  
  
          </el-card>
  
  
  
  
  
  
  
        </el-col>
  
  
  
  
  
  
  
  
  
  
  
      </el-row>
  
  
  
    </el-card>
  
  
  
  
  
  
  
    <el-dialog title="New Task" :visible.sync="dialogFormVisible">
  
  
  
      <el-form :model="newTask">
  
  
  
        <el-form-item label="Title">
  
  
  
          <el-input v-model="newTask.title" auto-complete="off"></el-input>
  
  
  
        </el-form-item>
  
  
  
        <el-form-item label="Description">
  
  
  
          <el-input type="textarea" v-model="newTask.description" auto-complete="off"></el-input>
  
  
  
        </el-form-item>
  
  
  
      </el-form>
  
  
  
      <span slot="footer" class="dialog-footer">
  
    
  
    <el-button @click="dialogFormVisible = false">Cancel</el-button>
  
    
  
    <el-button type="primary" @click="dialogFormVisible = false">Confirm</el-button>
  
    
  
    </span>
  
  
  
    </el-dialog>
  
  
  
    <el-dialog :title="newTask.title" :visible.sync="dialogDetailsVisible" width="30%">
  
      <span>{{newTask.details}}</span>
  
      <span slot="footer" class="dialog-footer">
  
      <el-button @click="dialogDetailsVisible = false">ok</el-button>
  
    </span>
  
    </el-dialog>
  
  
  
  
  
    <router-view/>
  
  
  
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
  
  
  
  let userRef = db.ref('users')
  
  
  
  let tasksRef = db.ref("users/1/tasks");
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  function removeTask(e) {}
  
  
  
  
  
  
  
  function Login() {}
  
  
  
  
  
  
  
  function Logout() {}
  
  
  
  
  
  
  
  function Register() {}
  
  
  
  export default {
  
  
  
    name: "app",
  
  
  
  
  
  
  
    firebase: {
  
  
  
      tasks: tasksRef,
  
  
  
      profile: userRef
  
  
  
    },
  
  
  
    data() {
  
  
  
      return {
  
  
  //to chexk if the user is logged in
        loggedin: true,
  
        status: "success",
  
  
  //toggles the details dialog
        dialogDetailsVisible: false,
  
  
  //toggle the form dialog
        dialogFormVisible: false,
  
  
  //toggles the session dialog
        sessionTime: new Date(2017, 0, 0, 0, 0),
  
  
  //toggles the break dialog
        inBreak: false,
  
  
  //break duration
        breakTime: new Date(2017, 0, 0, 0, 0),
  
  
  //percentage of progress
        percentage: 0,
  
  
  //to check if the timer is running
        timerRunning: false,
  
  
  // task object
        newTask: {
  
  
  
          title: "",
  
  
  
          description: ""
  
  
  
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
 
      startClock: function() {
  
        this.percentage = 0;
  
        let Sessiontime = new Date(this.sessionTime);
  
        let sessionHours = Sessiontime.getHours();
  
        let sessionMinutes = Sessiontime.getMinutes();
  
        let sessionSeconds = Sessiontime.getSeconds();
  
        let session = (sessionHours * 60 * 60) + (sessionMinutes * 60) + sessionSeconds;
  
        let breakIntervaltime = new Date(this.breakTime);
  
        let breakHours = breakIntervaltime.getHours();
  
        let breakMinutes = breakIntervaltime.getMinutes();
  
        let breakSeconds = breakIntervaltime.getSeconds();
  
        let breakPeriod = (breakHours * 60 * 60) + (breakMinutes * 60) + breakSeconds;
  
        let currentTime = 0;
  
        this.timerRunning = true;
  
  
  
        let interval = setInterval(() => {
  
          currentTime++;
  
          let progress = 0;
  
          console.log("progress is" + progress)
  
          console.log("current time is" + currentTime)
  
          console.log("break time is" + breakPeriod)
  
          if (this.inBreak == false) {
  
            console.log("in success")
  
            progress = Math.ceil((currentTime * 100) / session);
  
            console.log(currentTime * 100)
  
            console.log(session)
  
            console.log((currentTime * 100) / session)
  
            //this.status="success"
  
          } else {
  
            progress = Math.ceil((currentTime * 100) / breakPeriod);
  
            console.log("in fail")
  
            console.log(currentTime * 100)
  
            console.log(breakPeriod)
  
            console.log((currentTime * 100) / breakPeriod)
  
            //this.status="exception"
  
          }
  
          this.percentage = progress;
  
          console.log(progress)
  
  
  
          if (this.percentage >= 99.99) {
  
            currentTime = 0;
  
            this.percentage = 0;
  
            console.log(!this.inBreak)
  
           this.inBreak != this.inBreak;
  
            
  
            console.log(!this.inBreak)
  
  
  
  
  
          }
  
        }, 1000);
  
        // clearInterval(interval)
  
        // currentTime=0;
  
        //     this.percentage=0;
  
  
  
        // }
  
        //     },1000);
  
      },
  
      editTask: function(e) {
  
  
  
        this.dialogFormVisible = true;
  
  
  
        this.newTask.title = e.title;
  
  
  
        this.newTask.description = e.description;
  
  
  
      },
  
  
  
      openNewDialog() {
  
        this.newTask.title = "";
  
        this.newTask.description = "";
  
        this.dialogFormVisible = true;
  
  
  
  
  
  
  
      },
  
  
  
      addTask() {
  
  
  
        tasksRef.push(this.newTask);
  
  
  
        this.newTask.title = "";
  
  
  
        this.newTask.description = "";
  
  
  
      },
  
  
  
  
  
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
  
    top: -3px;
  
  }
  
  
  
  .options {
  
    width: 100%;
  
  }
  
  
  
  .clock {}
  
  
  
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
