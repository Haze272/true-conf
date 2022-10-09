<template>
  <section class="lift-shaft">
    <div id="elevator" ref="elevator">
    </div>
  </section>
  <section class="building">
    <div class="floor" floor-number="5">
      <button @click="callElevator">Press me</button>
    </div>
    <div class="floor" floor-number="4">
      <button @click="callElevator">Press me</button>
    </div>
    <div class="floor" floor-number="3">
      <button @click="callElevator">Press me</button>
    </div>
    <div class="floor" floor-number="2">
      <button @click="callElevator">Press me</button>
    </div>
    <div class="floor" floor-number="1">
      <button @click="callElevator">Press me</button>
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

  elevatorPosition = 300;
  queue: number[] = [];
  isMoving = false;

  mounted () {
    this.$refs.elevator.style.transform = 'translateY(' + this.elevatorPosition + '%)'
  }

  goDown (floors: number) {
    for (let i = 0; i < floors; i++) {
      if (this.elevatorPosition === 400) {
        console.log('Elevator: I can\'t go down!')
        return
      }
      this.elevatorPosition += 100
      this.$refs.elevator.style.transition = 'transform ' + 1000 * floors + 'ms'
      this.$refs.elevator.style.transform = 'translateY(' + this.elevatorPosition + '%)'
    }

    this.queue.shift()
  }

  goUp (floors: number) {
    for (let i = 0; i < floors; i++) {
      if (this.elevatorPosition === 0) {
        console.log('Elevator: I can\'t go up!')
        return
      }
      this.elevatorPosition -= 100
      this.$refs.elevator.style.transition = 'transform ' + 1000 * floors + 'ms'
      this.$refs.elevator.style.transform = 'translateY(' + this.elevatorPosition + '%)'
    }

    this.queue.shift()
  }

  getCurrentFloor (): number {
    return 5 - (this.elevatorPosition / 100)
  }

  callElevator ($event: any): void {
    const target: number = $event.target.parentElement.getAttribute('floor-number')
    this.queue.push(target)

    if (!this.isMoving) {
      this.isMoving = true
      const floorDiff = this.queue[0] - this.getCurrentFloor()

      if (floorDiff > 0) {
        this.goUp(floorDiff)
      } else if (floorDiff < 0) {
        this.goDown(Math.abs(floorDiff))
      }
      console.log(this.getCurrentFloor())
    }
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
