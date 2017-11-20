<template>
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
  
    <el-row>
  
  
  
  
  
  
  
      <el-col :lg="24">
  
  
  
  
  
  
  
  
  
  
  
        <h2> {{inBreak ? "stop working":"Work!"}}</h2>
  
  
  
  
  
  
  
  
  
      </el-col>
  
  
  
  
  
  
  
    </el-row>
  
  
  
  
  
    <!--reset button
  
    
  
      #requirements
  
    
  
      it should reset the timer and session/break time 
  
    
  
    
  
    
  
      
  
    
  
      -->
  
  
  
    <el-row id="btns">
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
      <!--start button
  
    
  
      #requirements
  
    
  
     it should start the clock
  
    
  
     once the clock starts, it should change color (red) and display stop
  
    
  
    
  
    
  
      
  
    
  
      -->
  
  
  
  
  
  
  
      <el-col :lg="24">
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
        <el-button class="btn" :type='timerRunning?"danger":"success"' @click="startClock()">{{timerRunning?"stop timer":"start timer"}}</el-button>
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
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
  
    
  
          selectableRange: '00:01:00 - 00:30:00'
  
    
  
        }" placeholder="session time">
  
  
  
        </el-time-picker>
  
  
  
  
  
  
  
  
  
  
  
      </el-col>
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
      <el-col :lg="12">
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
        <h1>Session Time</h1>
  
  
  
  
  
        <!--session time
  
    
  
      #requirements
  
    
  
      
  
    
  
      should let the user input a session time
  
    
  
      1 minute to an 2 hours
  
    
  
      -->
  
  
  
  
  
  
  
        <el-time-picker v-model="sessionTime" :picker-options="{
  
    
  
          selectableRange: '00:01:00 - 02:00:00'
  
    
  
        }" placeholder="session time">
  
  
  
        </el-time-picker>
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
      </el-col>
  
  
  
  
  
  
  
    </el-row>
  
  
  
  </el-card>
</template>

<script>
export default {
  name: "PromodoroClock",

  computed: {
    btnColor: function() {
      // `this` points to the vm instance

      return timerRunning ? "success" : "danger";
    }
  },

  data() {
    return {
      status: "success",

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

      interval: null
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

      // this.status=this.timerRunning?"danger":"success"

      if (!this.timerRunning) {
        let Sessiontime = new Date(this.sessionTime);

        let sessionHours = Sessiontime.getHours();

        let sessionMinutes = Sessiontime.getMinutes();

        let sessionSeconds = Sessiontime.getSeconds();

        let session =
          sessionHours * 60 * 60 + sessionMinutes * 60 + sessionSeconds;

        let breakIntervaltime = new Date(this.breakTime);

        let breakHours = breakIntervaltime.getHours();

        let breakMinutes = breakIntervaltime.getMinutes();

        let breakSeconds = breakIntervaltime.getSeconds();

        let breakPeriod =
          breakHours * 60 * 60 + breakMinutes * 60 + breakSeconds;

        let currentTime = 0;

        if (breakPeriod > 0 && session > 0) {
          this.timerRunning = true;

          let that = this;

          this.interval = setInterval(() => {
            currentTime++;

            let progress = 0;

            console.log("progress is" + progress);

            console.log("current time is" + currentTime);

            console.log("break time is" + breakPeriod);

            if (that.inBreak == false) {
              console.log("in success");

              progress = Math.ceil(currentTime * 100 / session);

              console.log(currentTime * 100);

              console.log(session);

              console.log(currentTime * 100 / session);

              //this.status="success"
            } else {
              progress = Math.ceil(currentTime * 100 / breakPeriod);

              console.log("in fail");

              console.log(currentTime * 100);

              console.log(breakPeriod);

              console.log(currentTime * 100 / breakPeriod);

              //this.status="exception"
            }

            this.percentage = progress;

            console.log(progress);

            if (this.percentage >= 99.99) {
              // debugger

              currentTime = 0;

              this.percentage = 0;

              console.log(!that.inBreak);

              that.inBreak = !that.inBreak;

              console.log(!that.inBreak);
            }
          }, 1000);
        } else {
          if (breakPeriod <= 0) {
            this.ShowWarning("please select a valid break time", "warning");
          }

          if (session <= 0) {
            this.ShowWarning("please select a valid session time", "warning");
          }
        }
      } else {
        clearInterval(this.interval);

        console.log("stop");

        this.timerRunning = false;

        this.percentage = 0;
      }
    },

    ShowWarning(message, status) {
      this.$message({
        message: message,

        type: status
      });
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h1,
h2 {
  font-weight: normal;
}

ul {
  list-style-type: none;

  padding: 0;
}

li {
  display: inline-block;

  margin: 0 10px;
}

a {
  color: #42b983;
}

startendbtn {
  width: 50%;
}
</style>
