<!-- @vue-ignore -->
<script>
import { computed, reactive, toRefs, defineComponent, } from 'vue'
import SectionWrapper from './SectionWrapper.vue'
import PageWrapper from './PageWrapper.vue'

export default defineComponent({
  name: 'FreestyleDispenser',
  components: { SectionWrapper, PageWrapper },
  data() {
    return {
      selectedBeverage: null,
      slottedIngredientCartridges: [],
      ingredientCartridges: [
        {id: 100, name: 'Coke Part 1'},
        {id: 101, name: 'Coke Part 2'},
        {id: 102, name: 'Diet Coke Part 1'},
        {id: 103, name: 'Diet Coke Part 2'},
        {id: 104, name: 'Fanta'},
        {id: 105, name: 'Sprite'},
        {id: 106, name: 'Powerade'},
        {id: 107, name: 'Lemon'},
      ],
      slots: {
        S1: { item: null },
        S2: { item: null },
        S3: { item: null },
        S4: { item: null },
        S5: { item: null },
        S6: { item: null },
        S7: { item: null },
        S8: { item: null },
        S9: { item: null },
        S10: { item: null },
      },
    }
  },
  computed: {
    availableIngredientCartridges() {
      return this.ingredientCartridges
        .filter(ingredient => !this.slottedIngredientCartridges.includes(ingredient.id))
    },
  },
  methods: {
    select(beverage) {
      this.selectedBeverage = beverage
    },
    assign(slot) {
      this.slots[slot].item = this.selectedBeverage
      this.slottedIngredientCartridges.push(this.selectedBeverage.id)
      this.selectedBeverage = null
    },
    removeAssignment(slot, id) {
      const index = this.slottedIngredientCartridges.indexOf(id)
      this.slottedIngredientCartridges.splice(index, 1)
      this.slots[slot].item = null
    },
  },
})
</script>

<template>
  <PageWrapper class="mx-auto max-w-7xl p-8">
    <SectionWrapper class="grid grid-cols-2 gap-8">
      <div id="available-ingredients">
        <div class="grid grid-cols-2 gap-4">
          <button
            v-for="item in availableIngredientCartridges"
            :key="item.id"
            @click="select(item)"
            class="flex items-center justify-center text-white transition-colors duration-200 transform bg-gray-500 font-bold uppercase rounded-full shadow active:ring-blue-500 w-36 h-36 text-sm hover:scale-105 ring-2 ring-gray-500 hover:bg-gray-600 focus:ring-2 focus:ring-offset-4 focus:ring-offset-white focus:ring-gray-900 focus:outline-none"
          >
            {{ item.name }}
          </button>
        </div>
      </div>

      <div id="slots">
        <ul class="flex flex-col space-y-4">
          <li
            v-for="(ingredient, slot) in slots"
            :key="slot"
            class="flex items-center justify-between p-8 bg-gray-50 rounded shadow"
          >
            <span class="flex items-center space-x-2">
              <span>{{ slot }}</span>
              <span v-if="!ingredient.item">Unassigned</span>
            </span>

            <span v-if="ingredient.item" class="flex items-center space-x-2" >
              <span>{{ ingredient.item.name }}</span>
              <button
                @click="removeAssignment(slot, ingredient.item.id)"
                class="rounded-full w-4 h-4 p-4 border shadow transform hover:scale-105 flex items-center justify-center"
              >&times;</button>
            </span>

            <button
              :class="[
                {
                   'bg-red-500 text-white hover:bg-red-600 transform hover:scale-105': selectedBeverage,
                   'text-gray-300': !selectedBeverage,
                }, 'border py-1 px-4 rounded-lg' ]"
              v-else="ingredient.item" :disabled="!selectedBeverage" @click="assign(slot)">
              Assign
            </button>
          </li>
        </ul>
      </div>
    </SectionWrapper>
  </PageWrapper>
</template>
