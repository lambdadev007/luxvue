<template>
  <v-sheet elevation="1" class="px-2 py-2 past-task">
    <div class="text-center w-full d-flex align-center justify-center">
      <h3 class="headline">Vergangene Aufgaben</h3>
      <v-btn>
        <v-icon>fas fa-list</v-icon>
      </v-btn>
    </div>
    <div class="d-block past-task-wrapper overflow-auto">
      <v-card
        elevation="2"
        outlined
        v-for="index in 5"
        :key="index"
        class="my-1"
      >
        <v-row class="align-center flex-grow-0 mx-0">
          <v-col cols="6" lg="" xl="5" class="pa-0">
            <v-card-title class="text-sm-subtitle-1">Vergangene Aufgabe</v-card-title>
          </v-col>
          <v-col cols="2" lg="2" xl="2" class="pa-0">
            <v-chip color="pink" text-color="pink" outlined x-small>Delivery</v-chip>
          </v-col>
          <v-col cols="2" lg="2" xl="3" class="text-center pa-0">
            <v-chip class="mr-1 text-sm-12">05.11.2021-08.11.2021</v-chip>
          </v-col>
          <v-col cols="2" lg="2" xl="2" class="text-center pa-0">
            <v-chip
              rounded
              color="green"
              text-color="white"
              class="color-icon"
            ></v-chip>
          </v-col>
        </v-row>
      </v-card>
    </div>
  </v-sheet>
</template>

<style scoped>
.color-icon {
  width: 35px !important;
  height: 35px !important;
  border-radius: 50% !important;
}
.past-task {
  height: 100%;
}
.past-task-wrapper {
  height: calc(100% - 30px);
}
.text-sm-12 {
  font-size: 12px;
}
@media screen and (max-width: 1366px) {
  .past-task {
    height: 300px;
  }
}
</style>
