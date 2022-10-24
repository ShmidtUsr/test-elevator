<template>
  <div class="main">
    <elevator 
    v-for="i in elevatorsCount"
    :floorsCount="floorsCount"
    :elevatorTurn="elevatorTurn[i-1]"
    :iElevator="i-1"
    @deleteFloor="deleteFloor"
    />

    <div class="buttons">
      <div v-for="floor in floorsCount" class="button-wrapper">
        <button class="button" @click="addFloor(floor)">{{floor}}</button>
      </div>
    </div>
  </div>
</template>

<script>
  import Elevator from "@/components/Elevator";
  export default {
    components: {
      Elevator
    },
    props: {
      elevatorsCount: {
        type: Number,
        required: true,
      },
      floorsCount: {
        type: Number,
        required: true,
      }
    },
    data() {
      let elevatorTurn = []
      return {
        elevatorTurn
      }
    },
    methods: {
      addFloor(resultFloor) {
        let minTurn = 1000;
        let iMinTurn = 0;

        for(let i = 0; i < this.elevatorsCount; i++) {
          if (this.elevatorTurn[i].length < minTurn) {
            minTurn = this.elevatorTurn[i].length;
            iMinTurn = i;
          }
        }

        let floorBusy = false;
        if(this.elevatorTurn[iMinTurn]) {
          for(let i = 0; i < this.elevatorTurn[iMinTurn].length; i++) {
            if (resultFloor == this.elevatorTurn[iMinTurn][i]) {
              floorBusy = true;
            }
          }
        }
        
        if(!floorBusy) {
          this.elevatorTurn[iMinTurn].push(resultFloor);
        }

      },
      deleteFloor(iElevator) {
        if(this.elevatorTurn[iElevator]) {
          this.elevatorTurn[iElevator].splice(0, 1);
        }
        
      }
    },
    mounted() {
      for(let i = 0; i < this.elevatorsCount; i++) {
          this.elevatorTurn[i] = [];
      }
      
    },
  }
</script>

<style>

.main {
  display: flex;
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