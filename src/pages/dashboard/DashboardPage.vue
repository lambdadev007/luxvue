<template>
  <div class="d-flex flex-grow-1 flex-row">
    <v-navigation-drawer
      v-model="menuDrawer"
      floating
      :light="true"
      absolute
      class="elevation-1 rounded"
    >
      <div class="side-bar">
        <v-row class="column" dense>
          <v-col>
            <h3 class="headline font-weight-regular">Intern</h3>
            <v-divider style="border-color: #ccc"></v-divider>
            <div class="mx-0">
              <v-chip-group
                active-class="primary--text"
                column
              >
                <v-chip
                  v-for="( item, index ) in interns"
                  :key="index"
                >
                  {{ item }}
                </v-chip>
              </v-chip-group>
            </div>
          </v-col>
          <v-col>
            <h3 class="headline font-weight-regular">Subunternehmer</h3>
            <v-divider style="border-color: #ccc"></v-divider>
            <div class="mx-0">
              <v-chip-group
                active-class="primary--text"
                column
              >
                <v-chip
                  v-for="( item, index ) in subcontractors"
                  :key="index"
                >
                  {{ item }}
                </v-chip>
              </v-chip-group>
            </div>
          </v-col>
        </v-row>
      </div>
    </v-navigation-drawer>
    <v-expand-x-transition>
      <div ref="main" class="elevation-1 rounded calendar-wrapper">
        <usage-snippet @eventClicked="eventClickHandler"></usage-snippet>
        <task-modal :open="openTask" :taskMeta="taskMeta" @modal-closed="openTask = false"></task-modal>
      </div>
    </v-expand-x-transition>

  </div>
</template>

<script>
// DEMO Cards for dashboard
import { mapState } from 'vuex'
import UsageSnippet from '../ui/components/_examples/calendars/usage.vue'
import TaskModal from '../../components/dashboard/TaskModal.vue'

export default {
  components: {
    UsageSnippet,
    TaskModal
  },
  data() {
    return {
      menuDrawer: true,
      loadingInterval: null,

      isLoading1: true,
      isLoading2: true,
      isLoading3: true,
      isLoading4: true,

      ordersSeries: [{
        name: 'Orders',
        data: [
          ['2020-02-02', 34],
          ['2020-02-03', 43],
          ['2020-02-04', 40],
          ['2020-02-05', 43]
        ]
      }],

      customersSeries: [{
        name: 'Customers',
        data: [
          ['2020-02-02', 13],
          ['2020-02-03', 11],
          ['2020-02-04', 13],
          ['2020-02-05', 12]
        ]
      }],

      interns: [
        'Work',
        'Home Improvement',
        'Vacation',
        'Food',
        'Drawers',
        'Shopping',
        'Art',
        'Tech',
        'Creative Writing'
      ],

      subcontractors: [
        'Work',
        'Home Improvement',
        'Vacation',
        'Food',
        'Drawers',
        'Shopping',
        'Art',
        'Tech',
        'Creative Writing'
      ],
      openTask: false,
      taskMeta: null
    }
  },
  computed: {
    theme() {
      return this.$vuetify.theme.isDark
        ? this.$vuetify.theme.defaults.dark
        : this.$vuetify.theme.defaults.light
    },
    ...mapState('app', ['drawer'])
  },
  watch: {
    drawer(val) {
      if (val) {
        this.$refs.main.style.marginLeft = '280px'
        this.menuDrawer = true
      }
      else {
        this.$refs.main.style.marginLeft = '0'
        this.menuDrawer = false
      }
    }
  },
  mounted() {
    let count = 0

    // DEMO delay for loading graphics
    this.loadingInterval = setInterval(() => {
      this[`isLoading${count++}`] = false
      if (count === 4) this.clear()
    }, 400)

    if (this.menuDrawer) {
      this.$refs.main.style.marginLeft = '280px'
      this.menuDrawer = true
    }
    else {
      this.$refs.main.style.marginLeft = '0'
      this.menuDrawer = false
    }
  },
  beforeDestroy() {
    this.clear()
  },
  methods: {
    clear() {
      clearInterval(this.loadingInterval)
    },
    eventClickHandler(e) {
      console.log('[eventClickHandler]', e)
      this.openTask = true
      this.taskMeta = e.event
    }
  }
}
</script>

<style scoped>
aside.v-navigation-drawer {
  left: 24px !important;
  top: 24px !important;
  bottom: 24px !important;
  height: calc(100% - 48px) !important;
}
.side-bar {
  width: 100%;
  height: 100%;
  margin: 0;
  padding: 30px 10px;
}
.calendar-wrapper {
  background: #fff;
  width: 100%;
  height: 100%;
  margin-left: 280px;
}
</style>