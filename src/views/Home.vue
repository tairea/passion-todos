<template>
  <v-container>
      <h1 class="title mb-2">Passion Projects</h1>
    <v-row>
      <v-col>
        
        <YearLevelTabs />
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
import YearLevelTabs from "../components/YearLevelTabs";

export default {
  name: "Home",

  components: {
    YearLevelTabs,
  },
};
</script>
