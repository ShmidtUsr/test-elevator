<template>
  <div id="app">
    <div class="elevator-wrapper">
      <elevator-main 
      :elevatorsCount="elevatorsCount"
      :floorsCount="floorsCount"
      />
    </div>
  </div>
</template>

<script>
  import ElevatorMain from "@/components/ElevatorMain";

  export default {
    components: {
      ElevatorMain
    },
    data() {
      const elevatorsCount = 4;
      const floorsCount = 4;
      return {
        elevatorsCount,
        floorsCount
      }
    }
  }
</script>