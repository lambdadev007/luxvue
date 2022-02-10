<template>
  <div>
    <!-- channel messages -->
    <div class="channel-page">
      <div id="messages" ref="messages" class="messages mx-2">
        <transition-group name="list">
          <channel-message
            v-for="message in messages"
            :key="message.id"
            :message="message"
            :user="user"
            class="my-4 d-flex"
          />
        </transition-group>
      </div>

      <div class="input-box pa-2">
        <input-box :channel="channel" @send-message="sendMessage" />
      </div>
    </div>
  </div>
</template>

<script>
import InputBox from './chat/InputBox'
import ChannelMessage from './chat/ChannelMessage'

// Demo messages and users
import getMessage, { users } from './chat/messages'

/*
|---------------------------------------------------------------------
| Chat Channel Page Component
|---------------------------------------------------------------------
|
| Layout to display the channel messages and users online
|
*/
export default {
  components: {
    InputBox,
    ChannelMessage
  },
  props: {
    // Current logged user
    user: {
      type: Object,
      default: () => ({})
    }
  },
  data() {
    return {
      // users online drawer
      usersDrawer: true,

      // channel information and messages
      channel: '',
      messages: [],

      // online users
      users: [
        this.user,
        ...users
      ],

      // demo random message timeout
      timeoutGenerator: null
    }
  },
  beforeDestroy() {
    clearTimeout(this.timeoutGenerator)
  },
  mounted() {
    this.startChannel()
  },
  methods: {
    startChannel() {
      clearTimeout(this.timeoutGenerator)

      this.messages = []

      // DEMO: generate random message to fill the channel
      this.messages.push(getMessage())
      this.messages.push(getMessage())
      this.messages.push(getMessage(this.user))
      this.messages.push(getMessage())

      this.channel = 'general'

      this.sendRandom()
    },
    // Random message generator
    sendRandom() {
      this.timeoutGenerator = setTimeout(() => {
        this.messages.push(getMessage())
        this.scrollToBottom()
        this.sendRandom()
      }, 1000 * (Math.floor(Math.random() * 15) + 1))
    },
    // Send message to channel
    sendMessage(message) {
      this.messages.push({
        id: '_' + Math.random().toString(36).substr(2, 9),
        user: this.user,
        text: message,
        timestamp: (new Date()).getTime()
      })

      this.scrollToBottom()
    },
    scrollToBottom() {
      this.$nextTick(() => {
        this.$refs.messages.scrollTop = this.$refs.messages.scrollHeight
      })
    }
  }
}
</script>
<style lang="scss" scoped>
// List Transition Animation
.list-enter-active {
  transition: all 0.3s;
}

.list-move {
  transition: transform 0.3s;
}

.list-enter,
.list-leave-to {
  opacity: 0;
  transform: translateX(-10px);
}
// -- End List Transition Animation

.channel-page {
  width: 100%;
  display: flex;
  flex-direction: column;
  background: url("/images/chat/chat-bg-2.png");
  position: relative;
  height: calc(100vh - 400px);
  margin-top: 15px;

  .messages {
    flex-grow: 1;
    margin-bottom: 68px;
    overflow-y: scroll;
    -webkit-overflow-scrolling: touch;
    min-height: 0;
  }

  .input-box {
    position: fixed;
    bottom: 12px;
    width: 100%;
  }

  .messages {
    padding-bottom: 0px;
  }

  .input-box {
    position: absolute;
    bottom: 12px;
  }
}

.theme--dark {
  .channel-page {
    background: none;
  }
}
</style>
