<template>
  <md-card>
    <md-card-content>
      <!--Error-->
      <p class="error">
        {{ err }}
      </p>
      <!--/Error-->

      <div class="md-layout md-gutter">
        <!--Name-->
        <div class="md-layout-item md-small-size-100">
          <md-field>
            <label>Name</label>
            <md-input v-model="name" />
          </md-field>
        </div>
        <!--/Name-->

        <!--Quantity-->
        <div class="md-layout-item md-small-size-50">
          <md-field>
            <label>Quantity</label>
            <md-input v-model="quantity" type="number" />
          </md-field>
        </div>
        <!--/Quantity-->

        <!--Measurement-->
        <div class="md-layout-item md-small-size-50">
          <md-field>
            <label>Measurement</label>
            <md-select v-model="measurement" md-dense>
              <md-option v-for="options in measurementOptions" :key="options" :value="options">
                {{ options }}
              </md-option>
            </md-select>
          </md-field>
        </div>
        <!--/Measurement-->

        <!--Add button-->
        <div v-if="!edit" class="md-layout-item md-small-size-100">
          <md-button class="md-icon-button md-raised md-accent" @click="addIngredient">
            <md-icon>
              add
            </md-icon>
          </md-button>
        </div>
        <!--/Add button-->

        <!--Edit buttons-->
        <div v-else class="md-layout-item md-small-size-100">
          <md-button class="md-icon-button md-raised md-accent" @click="editIngredient">
            <md-icon>
              create
            </md-icon>
          </md-button>
          <md-button class="md-icon-button md-raised md-accent" @click="deleteIngredient">
            <md-icon>
              remove_circle
            </md-icon>
          </md-button>
        </div>
        <!--/Edit buttons-->
      </div>
    </md-card-content>
  </md-card>
</template>

<script>
export default {
  props: {
    ingredient: {
      type: Object,
      required: false,
      default: () => {}
    },
    edit: Boolean
  },
  data () {
    return {
      // measurement options
      measurementOptions: ['Liter', 'Centiliter', 'Kilo', 'Gram', 'Unit', 'Tablespoons'],

      // ingredient props
      name: '',
      quantity: '',
      measurement: '',

      // error
      err: ''
    }
  },
  created () {
    // if prop ingredient is not undifined set prop
    if (this.ingredient) {
      this.name = this.ingredient.name
      this.quantity = this.ingredient.quantity
      this.measurement = this.ingredient.measurement
    }
  },
  methods: {
    /**
     * add ingredient
     */
    addIngredient () {
      if (this.checkIngredient()) {
        this.$store.commit('addIngredientSelectedMenu', { name: this.name, quantity: this.quantity, measurement: this.measurement })
        this.resetIntredient()
      }
    },

    /**
     * edit the ingredient
     */
    editIngredient () {
      if (this.checkIngredient()) {
        this.$store.commit('editIngredientSelectedMenu', { newIngredient: { name: this.name, quantity: this.quantity, measurement: this.measurement }, oldIngredient: this.ingredient })
      }
    },

    /**
     * delete ingredient
     */
    deleteIngredient () {
      this.$store.commit('deleteIngredientSelectedMenu', this.ingredient)
    },

    /**
     * check if everthing is filled in
     */
    checkIngredient () {
      if (!this.name || !this.quantity || !this.measurement) {
        this.err = 'Fill in all items before adding'
        return false
      } else {
        return true
      }
    },

    /**
    * reset the ingredient props
    */
    resetIntredient () {
      this.name = ''
      this.quantity = ''
      this.measurement = ''
      this.err = ''
    }
  }
}
</script>
