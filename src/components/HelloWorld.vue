<template>
  <section class="lift-shaft">
    <div id="elevator" ref="elevator">
    </div>
  </section>
  <section class="building">
    <div class="floor">
      <button @click="greet">Press me</button>
    </div>
    <div class="floor">
      <button @click="greet">Press me</button>
    </div>
    <div class="floor">
      <button @click="greet">Press me</button>
    </div>
    <div class="floor">
      <button @click="greet">Press me</button>
    </div>
    <div class="floor">
      <button @click="greet">Press me</button>
    </div>
  </section>
</template>

<script lang="ts">
import { Options, Vue } from 'vue-class-component'

@Options({
  props: {
    msg: String
  }
})
export default class HelloWorld extends Vue {
  declare $refs: {
    elevator: HTMLDivElement
  }

  elevatorPosition = 100;

  mounted () {
    this.$refs.elevator.style.transform = 'translateY(' + this.elevatorPosition + '%)'
  }

  goDown (floors = 1) {
    for (let i = 1; i <= floors; i++) {
      if (this.elevatorPosition === 400) {
        console.log('Elevator: I can\'t go down!')
        return
      }
      this.elevatorPosition += 100
      this.$refs.elevator.style.transition = 'transform ' + 1000 * floors + 'ms'
      this.$refs.elevator.style.transform = 'translateY(' + this.elevatorPosition + '%)'
      console.log('Elevator: I went down!')
    }
  }

  goUp (floors = 1) {
    for (let i = 1; i <= floors; i++) {
      if (this.elevatorPosition === 0) {
        console.log('Elevator: I can\'t go up!')
        return
      }
      this.elevatorPosition -= 100 * floors
      this.$refs.elevator.style.transition = 'transform ' + 1000 * floors + 'ms'
      this.$refs.elevator.style.transform = 'translateY(' + this.elevatorPosition + '%)'
      console.log('Elevator: I went up!')
    }
  }

  greet ($event: any): void {
    //  console.log($event.target.parentElement.)
    this.goDown(1)
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">

section.lift-shaft {
  position: absolute;
  width: 100px;
  left: 16px;
  height: calc(100% - 16px);
  border-left: #6e6e6e 3px solid;
  border-right: #6e6e6e 3px solid;
}

#elevator {
  height: calc(100% / 5);
  background-color: #00ffc8;
}

section.building {
  width: 100%;
  height: 100%;
  display: grid;
  grid-template-rows: repeat(5, 1fr);
}

.floor {
  & {
    border-top: 2px solid #6e6e6e;
    display: grid;
    justify-content: start;
    padding-left: 125px;
  }

  &:last-child {
    border-bottom: 2px solid #6e6e6e;
  }
}

</style>
