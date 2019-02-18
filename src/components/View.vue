<template>
  <v-content>
    <v-container
      fluid
      pa-0
    >
      <v-layout>
        <v-flex class="xs12 sm10 md8 lg6 layout wrap flex">
          <template v-for="palette in palettes">
            <v-hover
              :key="palette"
              :class="$style.pointer"
            >
              <v-card
                slot-scope="{ hover }"
                :color="palette"
                :style="getStyle(hover, palette)"
                :class="`elevation-${isActive(palette) || hover ? '12' : '0'}`"
                class="pa-2 d-flex"
                tile
                height="300"
                max-width="125"
                width="125"
                @click.native="selectColor(palette)"
              >
                <v-layout
                  column
                  justify-space-between
                  align-space-between
                >
                  <span
                    class="subheading"
                    v-text="palette.toUpperCase()"
                  />
                  <v-slide-x-transition
                    class="text-xs-right"
                    hide-on-leave
                    group
                  >
                    <v-icon key="icon">mdi-check</v-icon>
                    <template v-for="option in options">
                      <span v-if="option.color === palette" :key="option.id" v-text="option.id" />
                    </template>
                  </v-slide-x-transition>
                </v-layout>
              </v-card>
            </v-hover>
          </template>
        </v-flex>
        <v-flex
          xs12
          sm10
          md8
          lg6
          pa-5
        >
          <v-card
            class="elevation-16 mx-auto"
            color="white"
            dark
            height="500"
            max-width="500"
            tile
          >
            <v-card
              :color="primary"
              flat
              height="275"
              tile
            >
              <v-system-bar
                :class="primary"
                class="darken-2"
                status
              />
              <v-toolbar
                color="transparent"
                flat
              >
                <v-btn icon>
                  <v-icon>mdi-arrow-left</v-icon>
                </v-btn>
                <v-spacer />
                <v-btn icon>
                  <v-icon>mdi-dots-vertical</v-icon>
                </v-btn>
              </v-toolbar>
              <div
                class="pl-5 py-4"
              >
                <span class="pl-5 font-weight-light">Theme Preview</span>
                <span class="pl-5 font-weight-light">Export your theme below</span>
              </div>
              <v-btn
                :color="secondary"
                absolute
                bottom
                fab
                right
              >
                <v-icon>mdi-share-variant</v-icon>
              </v-btn>
            </v-card>
          </v-card>
        </v-flex>
      </v-layout>
    </v-container>

    <theme-bottom-sheet>
      <v-btn
        v-for="(option, index) in options"
        :key="option.id"
        :color="currentOption !== index ? (option.color || 'grey darken-1') : 'grey darken-4'"
        depressed
        @click="currentOption = index"
        v-text="option.id"
      />

      <v-flex xs12>
        <v-divider />
      </v-flex>

      <v-flex xs12 class="text-xs-right">
        <v-dialog
          v-model="exportModal"
          max-width="370"
          lazy
        >
          <v-btn
            slot="activator"
            dark
            color="primary"
          >
            Export
          </v-btn>

          <export-modal
            :colors="colors"
            :options="options"
            @close="exportModal = false"
          />
        </v-dialog>
      </v-flex>
    </theme-bottom-sheet>
  </v-content>
</template>

<script>
  // Utilities
  import colors from 'vuetify/es5/util/colors'
  import kebabCase from 'lodash/kebabCase'

  export default {
    components: {
      ThemeBottomSheet: () => import('./BottomSheet'),
      ExportModal: () => import('./ExportModal')
    },
    data: () => ({
      colors,
      selections: [],
      exportModal: false,
      currentOption: 0,
      options: [
        { id: 'primary', color: '' },
        { id: 'secondary', color: '' },
        { id: 'accent', color: '' },
        { id: 'error', color: '' },
        { id: 'warning', color: '' },
        { id: 'info', color: '' },
        { id: 'success', color: '' }
      ]
    }),
    computed: {
      palettes () {
        const keys = Object.keys(this.colors)
        return keys.map(kebabCase).slice(0, keys.length - 2)
      },
      nextOption () {
        return this.options.findIndex(option => !option.color)
      },
      primary () {
        return this.options[0].color
      },
      secondary () {
        return this.options[1].color
      }
    },
    methods: {
      isActive (color) {
        return this.options.findIndex(option => option.color === color) > -1
      },
      getStyle (hover, palette) {
        return {
          transform: this.isActive(palette) || hover ? 'scale(.9, .85)' : 'none'
        }
      },
      selectColor (color) {
        // check if color is already selected, else set color and change currentOption
        const index = this.options.findIndex(option => option.color === color)
        if (index > -1) {
          this.options[index].color = ''
          this.currentOption = index
        } else if (this.currentOption >= 0 && this.currentOption < 7) {
          this.options[this.currentOption].color = color
          this.currentOption = this.nextOption
        }
      }
    }
  }
</script>

<style lang="stylus" module>
.pointer
  cursor: pointer
</style>
