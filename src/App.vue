<template>
  <div id="app">
    <div class="buttons">
      <button @click="activateSnackbar('default')">Click me!</button>
      <button class="blue" @click="activateSnackbar('blue')">Click me!</button>
      <button class="red" @click="activateSnackbar('red')">Click me!</button>
      <button class="green" @click="activateSnackbar('green')">Click me!</button>
      <button class="yellow" @click="activateSnackbar('yellow')">Click me!</button>
    </div>
    <snackbar/>
  </div>
</template>

<script>
import EventBus from '@/EventBus'
import Snackbar from '@/components/Snackbar'

export default {
  name: 'app',
  components: { Snackbar },
  methods: {
    activateSnackbar (color) {
      let snackbarMsg = {
        message: 'You clicked on ' + color + ' button.'
      }
      if (color !== 'default') {
        snackbarMsg.emphasis = color
        snackbarMsg.actions = [
          {
            label: 'Alert',
            color: 'darkred',
            callback: () => { alert('Action text-button is clicked!') }
          }
        ]
      }
      EventBus.$emit('show-snackbar', snackbarMsg)
    }
  }
}
</script>

<style lang="scss" rel="stylesheet/scss">
#app {
  font-family: 'Courier New', Courier, monospace;
  margin: 30px;
  @media screen and (max-width: 767px) {
    margin: 15px;
  }
  .buttons {
    display: flex;
    flex-direction: column;
    align-items: center;
    button {
      font-family: 'Courier New', Courier, monospace;
      font-size: 14px;
      font-weight: 900;
      padding: .5rem 1rem;
      margin: 1.5rem 0;
      border-radius: 10px;
      border: none;
      outline: none;
      box-shadow: 1px 2px 6px 0 rgba(0, 0, 0, 0.5);
      cursor: pointer;
      @media screen and (max-width: 767px) {
        margin: 1rem 0;
      }
      &:active {
        box-shadow: 1px 1px 3px 0 rgba(0, 0, 0, 0.25);
      }
      &.blue {
        background-color: blue;
      }
      &.red {
        background-color: red;
      }
      &.green {
        background-color: green;
      }
      &.yellow {
        background-color: yellow;
      }
    }
  }
}
</style>
