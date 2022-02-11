<template>
  <v-card class="min-w-0 h-full">
    <h3 class="headline font-weight-regular text-center px-2 py-2">Unteraufgaben</h3>

    <v-divider></v-divider>

    <div v-if="tasks.length === 0">
      <div class="px-1 py-6 text-center">All done! No more tasks!</div>
    </div>

    <v-slide-y-transition
      v-else
      group
      tag="div"
      class="tasks-wrapper"
    >
      <div v-for="task in visibleTasks" :key="task.id" class="d-flex pa-2 task-item align-center" @click="$emit('edit-task', task)">
        <v-checkbox
          :input-value="task.completed"
          class="mt-0 pt-0"
          hide-details
          off-icon="mdi-checkbox-blank-circle-outline"
          on-icon="mdi-checkbox-marked-circle"
          @click.stop="task.completed ? setIncomplete(task) : setComplete(task)"
        ></v-checkbox>

        <div class="task-item-content flex-grow-1" :class="{ 'complete': task.completed }">
          <div class="font-weight-bold" v-text="task.title"></div>
          <div v-text="task.description"></div>
          <div>
            <v-chip
              v-for="label in task.labels"
              :key="label"
              :color="getLabelColor(label)"
              class="font-weight-bold mt-1 mr-1"
              outlined
              x-small
            >
              {{ getLabelTitle(label) }}
            </v-chip>
          </div>
        </div>

        <div class="d-flex align-center">
          <v-btn icon @click.stop="deleteTask(task)">
            <v-icon>mdi-delete-outline</v-icon>
          </v-btn>
        </div>
      </div>
    </v-slide-y-transition>

    <div class="pa-2">
        <v-btn
            block
            large
            color="primary"
            class="mt-2"
        >
        {{ $t('todo.addTask') }}
        </v-btn> 
    </div>
  </v-card>
</template>

<script>
import { mapState, mapMutations } from 'vuex'

/*
|---------------------------------------------------------------------
| ToDo List Component
|---------------------------------------------------------------------
|
| Task lister
|
*/
export default {
  props: {
    // task list
    tasks: {
      type: Array,
      default: () => []
    }
  },
  data() {
    return {
      filter: ''
    }
  },
  computed: {
    ...mapState('todo-app', ['labels']),
    visibleTasks() {
      if (!this.filter) return this.tasks

      return this.tasks.filter((t) => {
        return Boolean(Object.values(t).filter((prop) => typeof prop === 'string').find((item) => item.includes(this.filter)))
      })
    }
  },
  methods: {
    ...mapMutations('todo-app', {
      setComplete: 'taskCompleted',
      setIncomplete: 'taskIncomplete',
      deleteTask: 'deleteTask'
    }),
    getLabelColor(id) {
      const label = this.labels.find((l) => l.id === id)

      return label ? label.color : ''
    },
    getLabelTitle(id) {
      const label = this.labels.find((l) => l.id === id)

      return label ? label.title : ''
    }
  }
}
</script>

<style lang="scss" scoped>
.todo-filter {
  position: sticky;
  background-color: var(--v-background-base) !important;
  z-index: 2;
  top: 0;
}

.task-item {
  cursor: pointer;
  border-bottom: 1px solid rgba(100, 100, 100, 0.1);

  &:hover {
    background-color: rgba(100, 100, 100, 0.1);
  }

  .task-item-content {
    &.complete {
      text-decoration: line-through;
    }
  }
}
.tasks-wrapper {
    overflow-y: auto;
    height: calc(100% - 155px);
}
</style>
