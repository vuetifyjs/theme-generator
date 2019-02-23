<!-- eslint-disable vue/html-indent -->
<template>
  <v-card>
    <v-card-text>
      <code style="width: 100%">
      <span v-show="useNames">
      {{ "import colors from 'vuetify/es5/util/colors'" }}
      </span>
      {
        primary: {{ useNames ? getPaletteName(0) : getHexValue(0) }},
        secondary: {{ useNames ? getPaletteName(1) : getHexValue(1) }},
        accent: {{ useNames ? getPaletteName(2) : getHexValue(2) }},
        error: {{ useNames ? getPaletteName(3) : getHexValue(3) }},
        warning: {{ useNames ? getPaletteName(4) : getHexValue(4) }},
        info: {{ useNames ? getPaletteName(5) : getHexValue(5) }},
        success: {{ useNames ? getPaletteName(6) : getHexValue(6) }}
      }
      </code>
    </v-card-text>
    <v-card-actions>
      <v-btn
        flat
        class="ml-2 mb-1"
        @click="useNames = !useNames"
      >
        {{ useNames ? 'Use Hex Codes' : 'Use Names' }}
      </v-btn>

      <v-spacer />

      <v-btn
        flat
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
        type: Object,
        required: true,
        default: () => ({})
      },
      options: {
        type: Array,
        required: true,
        default: () => ([])
      }
    },
    data: () => ({
      useNames: false
    }),
    methods: {
      getPaletteName (index) {
        const color = this.options[index].color
        return color ? `colors.${camelCase(color)}.base` : `''`
      },
      getHexValue (index) {
        const color = this.colors[camelCase(this.options[index].color)]
        return color ? `'${color.base}'` : `''`
      },
      close () {
        this.$emit('close')
      }
    }
  }
</script>
