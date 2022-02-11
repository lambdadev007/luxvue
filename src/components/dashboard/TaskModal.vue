<template>
  <v-row justify="center">
    <v-dialog v-model="dialog" persistent max-width="100%">
      <v-card class="h-full">
        <v-toolbar
          elevation="2"
        >
          <div class="w-full d-flex align-center justify-space-between ml-4">
            <div class="d-flex align-center justify-space-between">
              <v-chip class="color-status"></v-chip>
              <v-spacer class="mx-2"></v-spacer>
              <v-btn class="icon-btn" rounded>
                <v-icon>fas fa-list</v-icon>
              </v-btn>
            </div>
            <div class="d-flex-column flex-grow-1 align-center">
              <v-card-title class="d-block headline font-weight-bold text-center pt-1 pb-1">{{ taskName }}</v-card-title>
              <p class="text-center mb-1">{{ startDate }} - {{ endDate }}</p>
            </div>
            <div class="d-flex align-center justify-space-between">
              <v-btn class="icon-btn" rounded>
                <v-icon>fas fa-edit</v-icon>
              </v-btn>
              <v-spacer class="mx-1"></v-spacer>
              <v-btn class="icon-btn" rounded>
                <v-icon>fas fa-edit</v-icon>
              </v-btn>
              <v-spacer class="mx-2"></v-spacer>
              <v-btn class="icon-btn" rounded @click="dialog = false">
                <v-icon>mdi-close</v-icon>
              </v-btn>
            </div>
          </div>
        </v-toolbar>
        
        <v-sheet color="#f2f2f2" class="modal-body">
          <v-row class="flex-grow-0 mx-2 h-full">
            <v-col
              cols="12"
              md="4"
              lg="4"
              xl="3"
              class="h-full"
            >
              <todo-list :tasks="incompleteTasks" @edit-task="$emit('edit-task', $event)" />
            </v-col>
            <v-col
              cols="12"
              md="8"
              lg="8"
              xl="5"
              class="h-full"
            >
              <div class="h-full">
                <div class="icon-scroller-wrapper overflow-auto">
                  <icon-scroller></icon-scroller>
                </div>
                <div class="past-task-wrapper mt-2">
                  <past-task-alt class="d-xl-none"></past-task-alt>
                </div>
                <div class="chat-box-wrapper overflow-auto mt-2">
                  <chat-box></chat-box>
                </div>
              </div>
            </v-col>
            <v-col
              cols="0"
              lg="0"
              xl="4"
              class="d-sm-none d-lg-none d-xl-block"
            >
              <past-task-alt></past-task-alt>
            </v-col>  
          </v-row>
        </v-sheet>
      </v-card>
    </v-dialog>
  </v-row>
</template>

<script>
import TodoList from './TodoList.vue'
import IconScroller from './IconScroller.vue'
import PastTask from './PastTask'
import PastTaskAlt from './PastTaskAlt'
import ChatBox from './ChatBox'
import { mapGetters } from 'vuex'
import moment from 'moment'

export default {
  components: {
    TodoList,
    IconScroller,
    PastTaskAlt,
    ChatBox
  },
  props: {
    open: {
      type: Boolean,
      default: false
    },
    taskMeta: {
      type: Object,
      default: null
    }
  },
  data () {
    return {
      dialog: false
    }
  },
  computed: {
    startDate() {
      return moment(this.taskMeta?.start).format('MM.DD.YYYY')
    },
    endDate() {
      return moment(this.taskMeta?.end).format('MM.DD.YYYY')
    },
    taskName() { return this.taskMeta?.name },
    statusColor() { return this.taskMeta?.color },
    ...mapGetters('todo-app', ['incompleteTasks'])
  },
  watch: {
    open(val) {
      this.dialog = val
    },
    dialog(val) {
      if (!val) this.$emit('modal-closed')
    }
  }
}
</script>

<style scoped>
.color-status {
  width: 24px;
  height: 24px;
  border-radius: 50%;
  background-color: #673ab7 !important;
}
.icon-btn {
  width: 50px !important;
  height: 50px !important;
}
.modal-body {
  height: calc(100% - 64px);
}
.chat-box-wrapper {
  height: calc(100% - 205px);
}
@media screen and (max-width: 1366px) {
  .chat-box-wrapper {
    height: calc(100% - 300px - 160px);
  }
}
</style>
<style>
.v-dialog.v-dialog--active.v-dialog--persistent {
  height: 90% !important;
}
</style>
