<template>
  <v-card>
    <v-card-text>
      <code style="width: 100%">
        {{ useNames ? "import colors from 'vuetify/es5/util/colors'" : null }}

        {
        primary: {{ useNames ? 'colors.' + getPaletteName(0) + '.base': getHexValue(0) }},
        secondary: {{ useNames ? 'colors.' + getPaletteName(1) + '.base': getHexValue(1) }},
        accent: {{ useNames ? 'colors.' + getPaletteName(2) + '.base': getHexValue(2) }},
        error: {{ useNames ? 'colors.' + getPaletteName(3) + '.base': getHexValue(3) }},
        warning: {{ useNames ? 'colors.' + getPaletteName(4) + '.base': getHexValue(4) }},
        info: {{ useNames ? 'colors.' + getPaletteName(5) + '.base': getHexValue(5) }},
        success: {{ useNames ? 'colors.' + getPaletteName(6) + '.base': getHexValue(6) }}
        }
      </code>
    </v-card-text>
    <v-card-actions>
      <v-btn
        v-if="!useNames"
        flat
        class="ml-2 mb-1"
        @click="useNames = true"
      >
        Use Names
      </v-btn>

      <v-btn
        v-else
        flat
        dark
        class="ml-2 mb-1"
        @click="useNames = false"
      >
        Use Hex Codes
      </v-btn>

      <v-spacer />

      <v-btn
        flat
        dark
        color="primary"
        class="mr-2 mb-1"
        @click="close"
      >
        Close
      </v-btn>
    </v-card-actions>
  </v-card>
</template>
<script>
  import camelCase from 'lodash/camelCase'

  export default {
    name: 'ExportModal',

    props: {
      colors: {
        type: Array,
        default: () => ([])
      },
      palettes: {
        type: Array,
        default: () => ([])
      },
      selections: {
        type: Array,
        default: () => ([])
      }
    },

    data: () => ({
      useNames: false
    }),

    methods: {
      getPaletteName (index) {
        return camelCase(this.palettes[this.selections[index]])
      },
      getHexValue (index) {
        return `"${this.colors[this.getPaletteName(index)].base}"`
      },
      close () {
        this.$emit('close')
      }
    }
  }
</script>
