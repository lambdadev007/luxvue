<template>
  <div
    v-shortkey="['ctrl', '/']"
    class="d-flex flex-grow-1"
    @shortkey="onKeyup"
  >

    <!-- Toolbar -->
    <v-app-bar
      app
      :color="isToolbarDetached ? 'surface' : undefined"
      :flat="isToolbarDetached"
      :light="toolbarTheme === 'light'"
      :dark="toolbarTheme === 'dark'"
    >
      <v-card class="flex-grow-1 d-flex" :class="[isToolbarDetached ? 'pa-1 mt-3 mx-1' : 'pa-0 ma-0']" :flat="!isToolbarDetached">
        <div class="d-flex flex-grow-1 align-center">

          <div class="d-flex flex-grow-1 align-center">
            <v-btn elevation="0">
              <v-icon small>mdi-logout-variant</v-icon>
            </v-btn>

            <v-spacer class="d-block d-lg-block"></v-spacer>

            <v-app-bar-nav-icon @click.stop="toggleDrawer()"></v-app-bar-nav-icon>

            <v-spacer class="d-block d-sm-none"></v-spacer>

            <toolbar-apps />

            <div :class="[$vuetify.rtl ? 'ml-1' : 'mr-1']">
              <toolbar-notifications />
            </div>

            <toolbar-user />
          </div>
        </div>
      </v-card>
    </v-app-bar>

    <v-main>
      <v-container class="fill-height" :fluid="!isContentBoxed">
        <v-layout>
          <slot></slot>
        </v-layout>
      </v-container>

      <v-footer app inset>
        <v-spacer></v-spacer>
      </v-footer>
    </v-main>
  </div>
</template>

<script>
import { mapState } from 'vuex'

// navigation menu configurations
import config from '../configs'

import ToolbarUser from '../components/toolbar/ToolbarUser'
import ToolbarApps from '../components/toolbar/ToolbarApps'
import ToolbarNotifications from '../components/toolbar/ToolbarNotifications'

export default {
  components: {
    ToolbarUser,
    ToolbarApps,
    ToolbarNotifications
  },
  data() {
    return {
      showSearch: false,

      navigation: config.navigation
    }
  },
  computed: {
    ...mapState('app', ['product', 'isContentBoxed', 'menuTheme', 'toolbarTheme', 'isToolbarDetached', 'drawer'])
  },
  methods: {
    onKeyup(e) {
      this.$refs.search.focus()
    },
    toggleDrawer() {
      this.$store.dispatch('app/toggleDrawer')
    }
  }
}
</script>

<style scoped>
.buy-button {
  box-shadow: 1px 1px 18px #ee44aa;
}
</style>
