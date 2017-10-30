<template>
  <div id="app">

  <el-menu theme="dark" :default-active="activeIndex" class="el-menu-demo" mode="horizontal" @select="handleSelect">
    <el-menu-item index="1">Processing Center</el-menu-item>
   
  </el-menu>

    <el-card class="box-card">

      <el-row>
        <el-col :offset="3" :lg="9">

          <el-menu class="el-menu-vertical-demo" @open="handleOpen" @close="handleClose" v-for="thing in tasks" :key="thing">

            <el-row :gutter="20">

              <el-col :lg="24">

                <el-row :gutter="20">

                  <el-col :lg="12">

                    <h1>tasks</h1>

                  </el-col>

                  <el-col :offset="7" :lg="4">

                    <el-button type="primary" icon="plus" size="mini">add new task</el-button>
                  </el-col>
                </el-row>

                <el-menu-item index="3">

                  <el-row :gutter="20">

                    <el-col :lg="12">

                      <span>
                        {{thing.title}}
                      </span>
                    </el-col>

                    <el-col :lg="6">

                      <el-button >Completed</el-button>
                      
                    </el-col>
                  </el-row>
                </el-menu-item>
              </el-col>
            </el-row>
          </el-menu>
        </el-col>

        <el-col :lg="9">

          <el-row :gutter="20">

            <el-col :lg="12" :offset="6">
              <h1>Progress</h1>
              <el-progress type="circle" :percentage="0"></el-progress>
            </el-col>
          </el-row>

          <el-row id="btns">
            <el-col :lg="12">

              <el-button>reset</el-button>

            </el-col>

            <el-col :lg="12">

              <el-button type="success">start</el-button>

            </el-col>

          </el-row>

          <el-row>

            <el-col :lg="12">
              <h1>Break Time</h1>
              <el-time-picker v-model="breakTime" :picker-options="{
                  selectableRange: '00:00:00 - 23:59:59'
                }" placeholder="00:00:00">
              </el-time-picker>

            </el-col>

            <el-col :lg="12">

              <h1>Session Time</h1>

              <el-time-picker v-model="sessionTime" :picker-options="{
                 
               selectableRange: '00:00:00 - 23:59:59' }" placeholder="00:00:00">

              </el-time-picker>

            </el-col>

          </el-row>

        </el-col>
      </el-row>
    </el-card>

    <router-view/>
  </div>
</template>

<script>
import Firebase from 'firebase'
let config = {
  apiKey: "AIzaSyDgdQxg2DWNMCsoaLxzorHbfD4AnVn10X4",
  authDomain: "promoto-clock.firebaseapp.com",
  databaseURL: "https://promoto-clock.firebaseio.com",
  projectId: "promoto-clock",
  storageBucket: "promoto-clock.appspot.com",
  messagingSenderId: "947399309047"
};

let app = Firebase.initializeApp(config)
let db = app.database()
let tasksRef = db.ref('users/Stephane/tasks')
function addTask() { }
function removeTask(e) { }
function Login() { }
function Logout() { }
function Register() { }
export default {
  name: 'app',
  loggedin:true,
  sessionTime: new Date(2017, 0, 0, 0, 0),
  breakTime: new Date(2017, 0, 0, 0, 0),
  percentage:0,
  timerRunning:false,
  newTask:{title:"",
  description:""},
  firebase: {
    tasks: tasksRef
  },
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}

#btns {
  padding-top: 20px
}
</style>
