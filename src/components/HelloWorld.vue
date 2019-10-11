<template>

  <div class="hello">
    
    <nav class="navbar navbar-expand-lg navbar-dark bg-dark">
      <h1> {{ msg }}</h1>
    </nav>
    <br><br><br>
    <center>
      <div style="background:#ECECEC; padding:50px; padding-right:100px">
        <div class="card bg-light mb-3 col-12" style="max-width: 20rem;">
          <div class="card-body">
            <div class="column">
            <h2>
              {{ minutes | zeroPad }} :
              {{ seconds | zeroPad }} :
              {{ milliSeconds | zeroPad(3) }}
            </h2>
            </div>
          </div>
        </div>
        <div class="card bg-light mb-3 col-12" style="max-width: 20rem;">
          <div class="card-body">
            <div class="column">
            <h2>
              {{ seconds | zeroPad }} :
              {{ milliSeconds | zeroPad(3) }}
            </h2>
            </div>
          </div>
        </div>
     </div>
    </center>
   
      <button type="button" class="btn btn-secondary" @click="startTimer" :disabled="isRunning">Start</button>
      <button type="button" class="btn btn-secondary" @click="stopTimer" :disabled="!isRunning">Stop</button>     
      <button type="button" class="btn btn-secondary" @click="clearAll">Restart</button>
      <button type="button" class="btn btn-primary">Passive</button>
      <button type="button" class="btn btn-danger">Passive</button>
      <!-- <div class="alert alert-dismissible alert-danger">
        <button type="button" class="close" data-dismiss="alert">&times;</button>
        <strong>Red is passive!</strong> <br> Start active period.
      </div> -->
  </div>
</template>

<script type="text/babel">

export default {
    name: 'HelloWorld',

    props: {
      msg: String
    },

    components: {
      
    },

    data() {
      return {
        animateFrame: 0,
        nowTime: 0,
        diffTime: 0,
        startTime: 0,
        isRunning: false
      };
    },

    methods: {
       setSubtractStartTime: function (time) {
        typeof time !== 'undefined' ? time : 0;
        this.startTime = Math.floor(performance.now() - time);
      },
   
      startTimer: function () {

        var vm = this;
        vm.setSubtractStartTime(vm.diffTime);

        (function loop(){
          vm.nowTime = Math.floor(performance.now());
          vm.diffTime = vm.nowTime - vm.startTime;
          vm.animateFrame = requestAnimationFrame(loop);
        }());
        vm.isRunning = true;
      },

      stopTimer: function () {
        this.isRunning = false;
        cancelAnimationFrame(this.animateFrame);
      },

      clearAll: function () {
        this.startTime = 0;
        this.nowTime = 0;
        this.diffTime = 0;
        this.stopTimer();
        this.animateFrame = 0;
      }
    },

    computed: {
      minutes: function () {
        return Math.floor(this.diffTime / 1000 / 60) % 60;
      },

      seconds: function () {
        return Math.floor(this.diffTime / 1000) % 60;
      },

      milliSeconds: function () {
        return Math.floor(this.diffTime % 1000);
      }
    },

    filters: {
      
      zeroPad: function(value, num){
        typeof num !== 'undefined' ? num : 2;
        return value.toString().padStart(num,"0");
      }
    }    
  }    
</script>

<style scoped>

h1 {
  color: #ADD8E6;
  border-color:  #000;
  margin-left: 600px;
}
h3 {
  margin: 40px 0 0;
}
</style>
