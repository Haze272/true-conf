<template>
  <section class="lift-shaft">
    <div id="elevator" ref="elevator">
      <div class="indication" ref="elevator-indication">{{ this.currentFloor }}</div>
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

  protected _elevatorPosition = 300;
  protected _currentFloor = 2;
  queue: number[] = [];

  public set elevatorPosition (position: number) {
    this._elevatorPosition = position
  }

  public get elevatorPosition () {
    return this._elevatorPosition
  }

  public set currentFloor (floor: number) {
    this._currentFloor = floor
  }

  public get currentFloor () {
    return this._currentFloor
  }

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

      this.currentFloor = this.getCurrentFloor()
    }
  }

  goUp (floors: number) {
    for (let i = 0; i < floors; i++) {
      if (this.elevatorPosition === 0) {
        console.log('Elevator: I can\'t go up!')
        return
      }
      this.elevatorPosition -= 100
      this.$refs.elevator.style.transition = 'transform ' + 1000 * floors + 'ms cubic-bezier(0.46, 0.03, 0.52, 0.96) 0s'
      this.$refs.elevator.style.transform = 'translateY(' + this.elevatorPosition + '%)'

      this.currentFloor = this.getCurrentFloor()
    }
  }

  getCurrentFloor (): number {
    return 5 - (this.elevatorPosition / 100)
  }

  moveToTarget (target: number) {
    let floorDiff = target - this.getCurrentFloor()

    if (floorDiff > 0) {
      this.goUp(floorDiff)
    } else if (floorDiff < 0) {
      floorDiff = Math.abs(floorDiff)
      this.goDown(floorDiff)
    } else if (floorDiff === 0) {
      return
    }

    setTimeout(() => {
      this.queue.shift()
      console.log(this.queue)
      if (this.queue.length >= 1) {
        setTimeout(() => {
          this.moveToTarget(this.queue[0])
        }, 1000)
      }
    }, 1000 * floorDiff)
  }

  callElevator ($event: any): void {
    const target: number = $event.target.parentElement.getAttribute('floor-number')
    this.queue.push(target)
    console.log(this.queue)
    if (this.queue.length === 1) {
      console.log()
      this.moveToTarget(target)
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
  display: grid;
  justify-items: center;
}

.indication {
  width: 50%;
  height: 20%;
  background-color: #d3d3d3;
  display: grid;
  justify-content: center;
  align-items: center;
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
