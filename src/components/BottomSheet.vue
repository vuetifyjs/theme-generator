<template>
  <v-bottom-sheet
    :value="true"
    hide-overlay
    persistent
    width="1024"
  >
    <v-expansion-panel
      :value="0"
      class="mx-auto"
    >
      <v-expansion-panel-content>
        <span slot="header">Palette Options</span>
        <v-card>
          <v-layout pa-3 wrap>
            <slot />
          </v-layout>
        </v-card>
      </v-expansion-panel-content>
    </v-expansion-panel>
  </v-bottom-sheet>
</template>

<style lang="stylus">
  .v-bottom-sheet
    min-width: 0 !important
    max-width: 700px !important
</style>
