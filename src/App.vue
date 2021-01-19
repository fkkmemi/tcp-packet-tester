<template>
  <v-app>
    <v-app-bar app color="primary" dark>
      <v-toolbar-title>tcp packet tester</v-toolbar-title>
    </v-app-bar>

    <v-main>
      <v-container fluid>
        <connection-form/>
      </v-container>
    </v-main>
  </v-app>
</template>

<script lang="ts">
import { Vue, Component } from 'vue-property-decorator'
import ConnectionForm from '@/components/connection/form.vue'

@Component({ components: { ConnectionForm } })
export default class App extends Vue {
}
</script>
