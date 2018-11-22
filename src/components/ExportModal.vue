<template>
  <v-card>
    <v-card-text>
      <code style="width: 100%">
        {{ useNames ? "import colors from 'vuetify/es5/util/colors'" : null }}

        {
        primary: {{ useNames ? 'colors.' + palettes[selections[0]] + '.base': getHexValue(0) }},
        secondary: {{ useNames ? 'colors.' + palettes[selections[1]] + '.base': getHexValue(1) }},
        accent: {{ useNames ? 'colors.' + palettes[selections[2]] + '.base': getHexValue(2) }},
        error: {{ useNames ? 'colors.' + palettes[selections[3]] + '.base': getHexValue(3) }},
        warning: {{ useNames ? 'colors.' + palettes[selections[4]] + '.base': getHexValue(4) }},
        info: {{ useNames ? 'colors.' + palettes[selections[5]] + '.base': getHexValue(5) }},
        success: {{ useNames ? 'colors.' + palettes[selections[6]] + '.base': getHexValue(6) }}
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
      getHexValue (index) {
        return this.colors[camelCase(this.palettes[this.selections[index]])].base
      },
      close () {
        this.$emit('close')
      }
    }
  }
</script>
