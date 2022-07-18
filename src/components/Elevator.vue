<template>
  <div class="main">
    <div class="mine">
      <div class="elevator" v-bind:style="{ marginTop: elevatorTop + 'px' }" :class="{'elevator-wait': elevatorWait}" >

      </div>
    </div>
    <div class="buttons">
      <div class="button-wrapper">
        <button class="button" @click="moveElevator(1)">1</button>
      </div>
      <div class="button-wrapper">
        <button class="button" @click="moveElevator(2)">2</button>
      </div>
      <div class="button-wrapper">
        <button class="button" @click="moveElevator(3)">3</button>
      </div>
      <div class="button-wrapper">
        <button class="button" @click="moveElevator(4)">4</button>
      </div>
      <div class="button-wrapper">
        <button class="button" @click="moveElevator(5)">5</button>
      </div>
    </div>
  </div>
</template>

<script>
  export default {
    data() {
      let elevatorWait = false;
      let ElevatorFloor = 5;
      let elevatorTop = 400;
      const floors = [0, 100, 200, 300, 400];
      return {
        elevatorWait,
        ElevatorFloor,
        elevatorTop,
        floors
      }
    },
    methods: {
      moveElevator (resultFloor) {
        let elevatorStep;
        if ((this.floors[resultFloor - 1] - this.elevatorTop) > 0) {
          elevatorStep = 10;
        } else {
          elevatorStep = -10;
        };
        let timeStop = Math.abs(this.ElevatorFloor - resultFloor);
        console.log(timeStop);
        let moveIntereval = setInterval(() => {
            if(Math.abs(this.elevatorTop - this.floors[resultFloor - 1]) > 1)
            this.elevatorTop = this.elevatorTop + elevatorStep;
        }, 10);

        setTimeout(() => { 
          clearInterval(moveIntereval);
          this.ElevatorFloor = resultFloor;
          this.elevatorWait = true;
          setTimeout(() => { 
          this.elevatorWait = false;
          }, 3000);
          }, timeStop*1000);

        
      },

    },
    computed: {

    },
    mounted() {
      
    }
  }
</script>


<style scoped>

.main {
  display: flex;
}
.mine {
  width: 100px;
  height: 500px;
  border: 2px solid black;
}

.elevator {
  width: 100px;
  height: 100px;
  background-color: blue;
}

.elevator-wait {
  animation: blink 1s infinite;
}

@keyframes blink {
  0% { opacity: 1.0; }
  100% { opacity: 0.0; }
  0% { opacity: 1.0; }
}

.button-wrapper {
  width: 100px;
  height: 100px;
}

.button {
  margin-top: 40px;
  margin-left: 40px;
}
</style>