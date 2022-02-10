<template>
  <v-row justify="center">
    <v-dialog v-model="dialog" persistent max-width="100%">
      <v-card>
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
        
        <v-sheet color="#f2f2f2">
          <v-row class="flex-grow-0 mx-2">
            <v-col cols="12" lg="3">
              <todo-list :tasks="incompleteTasks" @edit-task="$emit('edit-task', $event)" />
            </v-col>
            <v-col cols="12" lg="5">
              <icon-scroller></icon-scroller>
              <chat-box></chat-box>
            </v-col>
            <v-col cols="12" lg="4">
              <past-task></past-task>
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
import ChatBox from './ChatBox'
import { mapGetters } from 'vuex'
import moment from 'moment'

export default {
  components: {
    TodoList,
    IconScroller,
    PastTask,
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
</style>
