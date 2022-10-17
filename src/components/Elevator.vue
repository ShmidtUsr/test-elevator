<template>
    <div class="mine" :style="{ height: floorsCount*100 + 'px' }">
      <div class="elevator" v-bind:style="{ marginTop: elevatorTop + 'px' }" :class="{'elevator-wait': elevatorWait}" >
        <strong class="result-floor">{{resultFloor}}</strong>
        <div v-if="downArrow" class="arrow">↓</div>
        <div v-if="upArrow" class="arrow">↑</div>
      </div>
    </div>
</template>

<script>
import { watch } from '@vue/runtime-core';
  export default {
    props: {
      floorsCount: {
        type: Number,
        required: true,
      },
      elevatorTurn: {
        type: Object,
        required: true,
      },
      iElevator: {
        type: Number,
        required: true,
      }
    },
    data() {
      let downArrow = false;
      let upArrow = false;
      let elevatorAvailable = true;
      let elevatorWait = false;
      let elevatorFloor = 0;
      let elevatorResultFloor;
      let elevatorTop = 0;
      let floors = [];
      return {
        elevatorAvailable,
        elevatorWait,
        elevatorFloor,
        elevatorResultFloor,
        elevatorTop,
        floors,
        downArrow,
        upArrow
      }
    },
    methods: {
      moveElevator (resultFloor) {
        this.elevatorAvailable = false;
        this.resultFloor = resultFloor;
        let timeStop = Math.abs(this.elevatorFloor - resultFloor);
        let elevatorStep = (this.floors[resultFloor - 1] - this.elevatorTop) / timeStop /100 ;
        if (elevatorStep > 0) {  
          this.downArrow = true;
        } else {
          this.upArrow = true;
        };
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

          this.$emit('deleteFloor', this.iElevator)
          }, 3000);
          }, timeStop*1000);
      },

    },
    computed: {

    },
    mounted() {
      this.elevatorFloor = this.floorsCount;
      this.elevatorTop = (this.floorsCount - 1)*100;
      for(let i = 0; i < this.floorsCount; i++) {
        this.floors.push(i*100);
      }


      // setInterval(() => {
      //   if(this.elevatorAvailable && (this.elevatorTurn.length > 0)) {
      //     this.moveElevator(this.elevatorTurn[0]);
      //     this.elevatorTurn.splice(0, 1);
      //   }
      // }, 100)

    },
    watch: {
      elevatorTurn: {
        handler(){
          if(this.elevatorAvailable && (this.elevatorTurn.length > 0)) {
            this.moveElevator(this.elevatorTurn[0]);
          }   
        },
        deep: true
      }
    }
  }
</script>


<style scoped>

.mine {
  width: 100px;
  border: 2px solid black;
  margin-right: 15px;
}

.elevator {
  width: 100px;
  height: 100px;
  background-color: blue;
}

.elevator-wait {
  animation: flashing 1s infinite;
}

@keyframes flashing {
  0% { opacity: 1.0; }
  100% { opacity: 0.0; }
  0% { opacity: 1.0; }
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