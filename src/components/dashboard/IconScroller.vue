<template>
  <v-sheet>
    <v-card>
      <div class="scroller">
        <div class="d-flex">
          <v-btn
            v-for="(index) in 10"
            :key="index"
            rounded
            class="doc-btn"
          >
            <v-icon>fa-solid fa-file-pdf</v-icon>
          </v-btn>
        </div>
      </div>
      <div class="scroller">
        <div class="d-flex">
          <v-btn
            v-for="(index) in 10"
            :key="index"
            rounded
            class="doc-btn"
          >
            <v-icon>fa-solid fa-file-pdf</v-icon>
          </v-btn>
        </div>
      </div>
    </v-card>
  </v-sheet>
</template>

<script>
export default {
  data() {
    return {
      documents: [
        {
          title: 'test'
        }
      ]
    }
  }
}
</script>

<style scoped>
.doc-btn {
  width: 50px;
  height: 50px !important;
  margin: 10px 20px;
}
.scroller {
  overflow-x: auto;
}
</style>
