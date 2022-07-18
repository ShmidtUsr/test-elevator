<template>
  <div class="main">
    <div class="mine">
      <div class="elevator" v-bind:style="{ marginTop: elevatorTop + 'px' }" :class="{'elevator-wait': elevatorWait}" >
        <strong class="result-floor">{{resultFloor}}</strong>
        <div v-if="downArrow" class="arrow">↓</div>
        <div v-if="upArrow" class="arrow">↑</div>
      </div>
    </div>
    <div class="buttons">
      <div class="button-wrapper">
        <button class="button" @click="addFloor(1)">1</button>
      </div>
      <div class="button-wrapper">
        <button class="button" @click="addFloor(2)">2</button>
      </div>
      <div class="button-wrapper">
        <button class="button" @click="addFloor(3)">3</button>
      </div>
      <div class="button-wrapper">
        <button class="button" @click="addFloor(4)">4</button>
      </div>
      <div class="button-wrapper">
        <button class="button" @click="addFloor(5)">5</button>
      </div>
    </div>
  </div>
</template>

<script>
  export default {
    data() {
      let downArrow = false;
      let upArrow = false;
      let elevatorAvailable = true;
      let elevatorTurn = [];
      let elevatorWait = false;
      let elevatorFloor = 5;
      let elevatorResultFloor;
      let elevatorTop = 400;
      const floors = [0, 100, 200, 300, 400];
      return {
        elevatorAvailable,
        elevatorTurn,
        elevatorWait,
        elevatorFloor,
        elevatorResultFloor,
        elevatorTop,
        floors
      }
    },
    methods: {
      moveElevator (resultFloor) {
        this.elevatorAvailable = false;
        this.resultFloor = resultFloor;
        let elevatorStep;
        if ((this.floors[resultFloor - 1] - this.elevatorTop) > 0) {
          elevatorStep = 10;
          this.downArrow = true;
        } else {
          elevatorStep = -10;
          this.upArrow = true;
        };
        let timeStop = Math.abs(this.elevatorFloor - resultFloor);
        console.log(timeStop);
        let moveInterval = setInterval(() => {
            if(Math.abs(this.elevatorTop - this.floors[resultFloor - 1]) > 1)
            this.elevatorTop = this.elevatorTop + elevatorStep;
        }, 10);

        setTimeout(() => { 
          clearInterval(moveInterval);
          this.elevatorWait = true;
          setTimeout(() => { 
          this.elevatorWait = false;
          this.elevatorAvailable = true;
          this.elevatorFloor = resultFloor;
          this.resultFloor = '';
          this.upArrow = false;
          this.downArrow = false;
          }, 3000);
          }, timeStop*1000);

        
      },

      addFloor(resultFloor) {
        let floorBusy = false;
        for(let i; i < this.elevatorTurn.length; i++) {
          if (resultFloor == this.elevalorTurn[i]) {
            floorBusy = true;
          }
        }
        if((this.elevatorFloor !== resultFloor) && !floorBusy)
        this.elevatorTurn.push(resultFloor);
      }


    },
    computed: {

    },
    mounted() {
      setInterval(() => {
        if(this.elevatorAvailable && (this.elevatorTurn.length > 0)) {
          this.moveElevator(this.elevatorTurn[0]);
          this.elevatorTurn.splice(0, 1);
        }
      }, 100)
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

.result-floor {
  font-size: 35px;
  margin-left: 45px;
}

.arrow {
  font-size: 35px;
  margin-left: 45px;
}
</style>