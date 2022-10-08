<template>
  <section class="lift-shaft">
    <div id="elevator">
    </div>
  </section>
  <section class="building">
    <div class="floor">Floor 5</div>
    <div class="floor">Floor 4</div>
    <div class="floor">Floor 3</div>
    <div class="floor">Floor 2</div>
    <div class="floor">Floor 1</div>
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
  msg!: string
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">

section.lift-shaft {
  position: absolute;
  width: 100px;
  left: 16px;
  height: calc(100% - 16px);
  background-color: #6e6e6e;
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
    border-top: 2px solid red;
  }

  &:last-child {
    border-bottom: 2px solid red;
  }
}

</style>
