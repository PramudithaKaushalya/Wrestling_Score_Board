<template>

  <div class="hello">
    
    <nav class="navbar navbar-expand-lg navbar-dark bg-dark">
      <h1> {{ msg }}</h1>
    </nav>
    <br><br><br>
    
      <div style="background:#ECECEC; padding:50px; ">
        <center>
        <div class="card bg-light mb-3 col-12" style="max-width: 40rem; max-height: 220px">
            <p style="font-size:150px">
              <b>
              {{ minutes | zeroPad }} :
              {{ seconds | zeroPad }}
              </b>
            </p>
        </div>
        <div class="row justify-content-center">
        <div class="row">
         <div class="card bg-light mb-3 col-sm-6" style="max-width: 50rem; max-height: 180px; float: right">
            <p style="font-size:120px;">
              {{ passiveSeconds | zeroPad }} 
            </p>
        </div>
        <div class= "col-sm-6" style="float: right">
          <vs-avatar size="150px" text=" " :color="color"/>
        </div>
        </div>
        </div>        
        </center>
       
        
      </div>
      <br>      
      <button type="button" class="btn btn-secondary" @click="startTimer" :disabled="isRunning">Start</button>
      <button type="button" class="btn btn-secondary" @click="stopTimer" :disabled="!isRunning">Stop</button>     
      <button type="button" class="btn btn-secondary" @click="clearAll">Restart</button>
      <button type="button" class="btn btn-primary" @click="bluePassive">Passive</button>
      <button type="button" class="btn btn-danger" @click="redPassive">Passive</button>
      <button type="button" class="btn btn-info" @click="passiveStart">Start</button>
      <button type="button" class="btn btn-info" @click="stopPassTimer">Stop</button>
      <button type="button" class="btn btn-info" @click="clearPass">Restart</button> 
  </div>
</template>

<script type="text/babel">

import Vue from 'vue'
import Vuesax from 'vuesax'
import 'vuesax/dist/vuesax.css' 

Vue.use(Vuesax);

export default {
    name: 'HelloWorld',

    props: {
      msg: String
    },

    components: {
      
    },

    mounted() {

    },

    data() {
      return {
        animateFrame: 0,
        nowTime: 0,
        diffTime: 0,
        startTime: 0,
        animatePassFrame: 0,
        passTime: 0,
        nowPassTime: 0,
        startPassTime: 0,
        isRunning: false,
        color: "#ECECEC"
      };
    },

    methods: {
      setSubtractStartTime: function (time) {
        typeof time !== 'undefined' ? time : 0;
        this.startTime = Math.floor(performance.now() - time);
      },

      setSubtractStartPassTime: function (time) {
        typeof time !== 'undefined' ? time : 0;
        this.startPassTime = Math.floor(performance.now() - time);
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

      stopPassTimer: function () {
        cancelAnimationFrame(this.animatePassFrame);
        this.stopTimer();
      },

      clearAll: function () {
        this.startTime = 0;
        this.nowTime = 0;
        this.diffTime = 0;
        this.stopTimer();
        this.animateFrame = 0;
        this.clearPass();
      },

      clearPass: function () {
        this.passTime = 0; 
        this.startPassTime = 0;
        this.nowPassTime = 0;
        this.stopPassTimer();
        this.animatePassFrame = 0;
        this.color = "#ECECEC";
      },

      passiveStart: function () {
        var vm = this;

        vm.startTimer();
        vm.setSubtractStartPassTime(vm.passTime);

        (function loop(){
          vm.nowPassTime = Math.floor(performance.now());
          vm.passTime = vm.nowPassTime - vm.startPassTime;
          vm.animatePassFrame = requestAnimationFrame(loop);
          var x = Math.floor(this.passTime / 1000) % 31;
          alert(x);
        }());        
      },

      redPassive: function () {
        this.color = "#FF0000"
      },

      bluePassive: function () {
        this.color = "#0000FF"
      }
    },

    computed: {
      minutes: function () {
        return Math.floor(this.diffTime / 1000 / 60) % 6;
      },

      seconds: function () {
        return Math.floor(this.diffTime / 1000) % 60;
      },

      passiveSeconds: function () {
        return Math.floor(this.passTime / 1000) % 31;
      },
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
