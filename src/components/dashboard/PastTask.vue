<template>
  <v-sheet elevation="1" class="px-2 py-2">
    <div class="text-center w-full d-flex align-center justify-center">
      <h3 class="headline">Vergangene Aufgaben</h3>
      <v-btn>
        <v-icon>fas fa-list</v-icon>
      </v-btn>
    </div>
    <div class="d-block past-task-wrapper">
      <v-card
        elevation="2"
        outlined
        v-for="index in 5"
        :key="index"
        class="my-1"
      >
        <v-row class="align-center flex-grow-0 mx-0">
          <v-col cols="12" lg="8" class="pa-0">
            <v-card-title>Vergangene Aufgabe</v-card-title>
            <v-card-subtitle
              >Lorem ipsum dolor sit amet, consetetur sadipscing elitr, sed diam
              nonumy eirmod tempor invidunt ut labore et.</v-card-subtitle
            >
          </v-col>
          <v-col cols="12" lg="4" class="text-center pa-0">
            <v-chip class="mb-1 text-sm-12">05.11.2021-08.11.2021</v-chip>
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
  width: 50px !important;
  height: 50px !important;
  border-radius: 50% !important;
}
.past-task-wrapper {
    overflow-y: auto;
    height: calc(100vh - 280px);
}
.text-sm-12 {
    font-size: 12px;
}
</style>
