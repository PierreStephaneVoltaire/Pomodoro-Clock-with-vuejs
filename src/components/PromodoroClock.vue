<template>
  <el-card class="box-card">
    <div slot="header" class="clearfix">
      <span class="title">Progress</span>
    </div>
    <!--clock
  
    
  
      #requirements
  
    
  
      displays the progress of the session in percent
  
    
  
      the session circle should be green,
  
    
  
      the break circle should be red 
    
    
  
      -->

    <el-row>
      <el-col :lg="24">

        <el-progress :width="200" :stroke-width="10" class="clock" :status="status" type="circle" :percentage="percentage"></el-progress>
      </el-col>

    </el-row>
  

  <!--countdoun
  #requirements

  displays remaining time
  #todo make another component display it
  -->
  <el-row>
    <el-col :lg="8"><span :class='inBreak ? "dangerText":"successText"' >{{hour}}:Hours</span></el-col>
        <el-col :lg="8"><span :class='inBreak ? "dangerText":"successText"'>{{minute}}:Minutes</span></el-col>
    <el-col :lg="8"><span :class='inBreak ? "dangerText":"successText"'>{{second}}:Seconds</span></el-col>

  </el-row>

    <el-row id="btns">
 
      <!--start button
  
    
  
      #requirements
  
    
  
     it should start the clock
  
    
  
     once the clock starts, it should change color (red) and display stop
  
      -->

      <el-col :lg="24">
  
        <el-button class="btn" :type='timerRunning?"danger":"success"' @click="startClock()">{{timerRunning?"End Working Session":"Start Working"}}</el-button>
  
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
  name: "Promodoro",

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
      status: "success",
      interval: null,
      hour: 0,
      minute: 0,
      second: 0
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
        //get session time and breaktime. then convert the sum into seconds
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
        //set current elapsed time to 0
        let currentTime = 0;
        //check if the break period and session are > 0 . I don't want to have to deal with a division by 0
        if (breakPeriod > 0 && session > 0) {
          // indicate that the time is running
          this.timerRunning = true;
          //function that will run every second to update the progress and time
          this.interval = setInterval(() => {
            //increment the current elapsed time
            currentTime++;
            //reset progress
            let progress = 0;

            // console.log("progress is" + progress);

            // console.log("current time is" + currentTime);

            // console.log("break time is" + breakPeriod);
            // if we're not on break calculate session progress
            if (this.inBreak == false) {
              // console.log("in session");
              //update timer to match current progress
              this.hour = Math.floor((session - currentTime) / 60 / 60);
              let secondsLeft = session - currentTime - this.hour * 60 * 60;
              this.minute = Math.floor(secondsLeft / 60);
              secondsLeft = secondsLeft - this.minute * 60;
              this.second = Math.floor(secondsLeft);
              //update progress
              progress = Math.ceil(currentTime * 100 / session);

              // console.log(currentTime * 100);

              // console.log(session);

              // console.log(currentTime * 100 / session);

              this.status = "success";
            } else {
              //if we're on break
              // update countdown
              this.hour = Math.floor((breakPeriod - currentTime) / 60 / 60);
              let secondsLeft = breakPeriod - currentTime - this.hour * 60 * 60;
              this.minute = Math.floor(secondsLeft / 60);
              secondsLeft = secondsLeft - this.minute * 60;
              this.second = Math.floor(secondsLeft);
              //update progress
              progress = Math.ceil(currentTime * 100 / breakPeriod);

              // console.log("in break");

              // console.log(currentTime * 100);

              // console.log(breakPeriod);

              // console.log(currentTime * 100 / breakPeriod);

              this.status = "exception";
            }
            //assign the new progress to the progress circle
            this.percentage = progress;

            // console.log(progress);
            // if session/break done
            if (this.percentage >= 99.99) {
              // debugger
              //reset timer
              currentTime = 0;

              this.percentage = 0;
              this.second = 0;
              this.minute = 0;
              this.hour = 0;
              // console.log(!this.inBreak);
              //change working/break status
              this.inBreak = !this.inBreak;
              // console.log(!this.inBreak);
            }
          }, 1000);
        } else {
          //error handling for divisions by 0
          if (breakPeriod <= 0) {
            this.ShowWarning("please select a valid break time", "warning");
          }

          if (session <= 0) {
            this.ShowWarning("please select a valid session time", "warning");
          }
        }
      } else {
        //if stop clicked stop the loop
        clearInterval(this.interval);

        // console.log("stop");

        this.timerRunning = false;
        this.percentage = 0;
        this.second = 0;
        this.minute = 0;
        this.hour = 0;
      }
    },
    /**
 * displays a toast
 * with a message
 */
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
.dangerText {
  color: #fa5555;
  font-size: 36px;
  font-weight: bold;
}
.successText {
  color: #13ce66;
  font-size: 36px;
  font-weight: bold;
}
a {
  color: #42b983;
}

startendbtn {
  width: 50%;
}
</style>
