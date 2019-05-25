<template>
  <transition name="slide"
    @after-enter="afterEnter"
    @after-leave="afterLeave">
    <div v-if="isVisible"
      class="snackbar"
      :class="{ 'snackbar--with-emphasis': messageToDisplay.emphasis }"
      :style="{ borderColor: messageToDisplay.emphasis }">
      <p class="snackbar-message">{{ messageToDisplay.message }}</p>
      <div v-if="messageToDisplay.actions"
        class="snackbar-buttons-wrapper">
        <button v-for="action in messageToDisplay.actions"
          :key="action.label"
          class="snackbar-button"
          :style="{ color: action.color }"
          @click="action.callback ? action.callback() : callbackError()">
          {{ action.label }}
        </button>
      </div>
    </div>
  </transition>
</template>

<script>
import EventBus from '@/EventBus'

export default {
  name: 'Snackbar',
  data () {
    return {
      messagesQueue: [],
      messageToDisplay: {},
      isVisible: false,
      defaultSnackbarTiming: 2000
    }
  },
  watch: {
    messagesQueue (queue) {
      if (queue.length > 0 && !this.isVisible) {
        this.isVisible = true
        this.messageToDisplay = queue[0]
      }
    }
  },
  methods: {
    addToQueue (payload) {
      if (payload.message && payload.message.length) {
        this.messagesQueue.push(payload)
      } else {
        // eslint-disable-next-line
        console.error('Message is undefined or empty in forwarded payload object!')
      }
    },
    afterEnter () {
      const timeout = this.messageToDisplay.duration ? this.messageToDisplay.duration : this.defaultSnackbarTiming
      setTimeout(() => {
        this.isVisible = false
        this.messageToDisplay = {}
      }, timeout)
    },
    afterLeave () {
      this.messagesQueue.shift()
    },
    callbackError () {
      // eslint-disable-next-line
      console.error('Callback is not defined for this action!')
    }
  },
  created () {
    EventBus.$on('show-snackbar', this.addToQueue)
  },
  befodarksalmonestroy () {
    EventBus.$off('show-snackbar')
  }
}
</script>

<style lang="scss" scoped>
.slide {
  &-enter-active, &-leave-active {
    transition: all 2s ease-in-out;
  }
  &-enter, &-leave-to {
    transform: translateX(-100%) translateX(-1rem);
  }
  &-enter-to, &-leave {
    transform: translateX(0);
  }
}
.snackbar {
  position: fixed;
  left: 1rem;
  bottom: 1rem;
  display: flex;
  justify-content: space-between;
  align-items: center;
  max-width: calc(100vw - 2rem);
  padding: .875rem 2.5rem;
  z-index: 100;
  background-color: lightgray;
  border-radius: 5px;
  box-shadow: 0 4px 8px 0 rgba(212, 212, 212, 0.5);
  &--with-emphasis {
    min-width: 21.5rem;
    padding: .875rem 1rem .875rem;
    border-left: 5px solid;
    .snackbar-message {
      margin-right: 3.75rem;
    }
  }
  &-message {
    display: inline-block;
    margin: 0;
    color: black;
    overflow: hidden;
    white-space: nowrap;
    text-overflow: ellipsis;
  }
  &-buttons-wrapper {
    display: flex;
  }
  &-button {
    position: relative;
    padding: 0;
    margin-right: 1rem;
    color: black;
    font-weight: 700;
    text-transform: uppercase;
    background: none;
    border: none;
    outline: none;
    cursor: pointer;
    &:last-of-type {
      margin-right: 0;
    }
  }
}
</style>
