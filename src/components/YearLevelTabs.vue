<template>
  <v-container>
    <v-row class="text-center">
      <v-col cols="12">
        <template>
          <v-tabs v-model="tab">
            <v-tab>Year 9s</v-tab>
            <v-tab>Year 10s</v-tab>
            <v-tab>Seniors</v-tab>
          </v-tabs>
        </template>
        <v-tabs-items v-model="tab">

          <!-- Year 9s -->
          <v-tab-item>
            <v-card flat class="text-left">
              <!-- Table -->
              <StudentTable yearLevel="Year 9"></StudentTable>
            </v-card>
          </v-tab-item>


          <!-- Year 10s -->
          <v-tab-item>
            <v-card flat class="text-left">
              <StudentTable yearLevel="Year 10"></StudentTable>
            </v-card>
          </v-tab-item>

          <!-- Seniors -->
          <v-tab-item>
            <v-card flat class="text-left">
              <StudentTable yearLevel="Year 11"></StudentTable>
            </v-card>
          </v-tab-item>


        </v-tabs-items>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
import StudentTable from '../components/StudentTable'

export default {
  name: "YearLevelTabs",
  components: {
      StudentTable,
    },
  data: () => ({
    tab: null,

  }),
  methods: {
  },
};
</script>

