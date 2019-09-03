<template>
  <div>
    <div class="characterFilter">
      <div class="row">
        <div class="col" v-for="(n, i) in 2" v-bind:key="i">
          <button type="button" class="btn btn-dark" v-on:click="toggleMenu(i)">
            {{ selected[i].house !== '' && selected[i].index !== -1 ? $t(`character.${characters[selected[i].house][selected[i].index].firstName}`) : 'Select' }}
          </button>
        </div>
      </div>
      <div class="selector" v-bind:class="selectIndex !== -1 ? 'active' : ''">
        <div class="row">
          <div class="col">
            <button type="button" class="btn btn-light" v-on:click="clear()">
              Clear
            </button>
          </div>
        </div>
        <div class="row" v-for="(n, i) in houseCharacterMax" v-bind:key="i">
          <div class="col m-1 p-1 alert character"
            v-for="(house, houseIndex) in houses"
            v-bind:key="houseIndex"
            v-bind:class="typeof characters[house][i] !== 'undefined' ? houseClass[house] : ''"
            v-on:click="selectCharacter(house, i)">
            {{ typeof characters[house][i] !== 'undefined' ? $t(`character.${characters[house][i]['firstName']}`) : ''}}
          </div>
        </div>
      </div>
    </div>
    <table class="table">
      <tbody>
        <tr v-for="(meal, i) in meals" v-bind:key="i">
          <td>{{ $t(`meal.${meal}.name`) }}</td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
import Characters from '../data/characters/';

export default {
  name: 'MealFilter',
  data() {
    return {
      houseCharacterMax: 0,
      selectIndex: -1,
      characters: Characters,
      houses: [],
      houseClass: {
        'Black Eagles': 'alert-danger',
        'Blue Lions': 'alert-info',
        'Golden Deer': 'alert-warning',
        'Church Of Seiros': 'alert-secondary',
      },
      meals: [],
      selected: [
        {
          house: '',
          index: -1,
        },
        {
          house: '',
          index: -1,
        },
      ],
    };
  },
  methods: {
    initData() {
      this.selected = [
        {
          house: '',
          index: -1,
        },
        {
          house: '',
          index: -1,
        },
      ];
      this.selectIndex = -1;
    },
    clear() {
      this.selected[this.selectIndex].house = '';
      this.selected[this.selectIndex].index = -1;
      this.selectIndex = -1;
      this.serveMeal();
    },
    toggleMenu(index) {
      this.selectIndex = this.selectIndex === index ? -1 : index;
    },
    selectCharacter(house, index) {
      this.selected[this.selectIndex].house = house;
      this.selected[this.selectIndex].index = index;
      this.selectIndex = -1;
      this.serveMeal();
    },
    serveMeal() {
      this.meals = [];
      for(let index = 0; index < this.selected.length; index++) {
        if(this.selected[index].house !== '' && this.selected[index].index !== -1) {
          if(this.meals.length === 0) {
            this.meals = this.characters[this.selected[index].house][this.selected[index].index].meal.like;
          }
          else {
            this.meals = this.meals.filter(value => this.characters[this.selected[index].house][this.selected[index].index].meal.like.indexOf(value) !== -1);
          }
        }
      }
    },
  },
  mounted() {
    this.initData();
    this.houses = Object.keys(Characters);
    this.houseCharacterMax = Object.keys(Characters).reduce((accumulator, currentValue) => {
      const characterCount = Characters[currentValue].length;
      return accumulator > characterCount ? accumulator : characterCount;
    }, 0);
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.characterFilter {
  margin: 7px 0px;
}
.character {
  display: flex;
  justify-content: center;
  align-items: center;
  cursor: pointer;
  user-select: none;
}
.selector {
  display: none;
}
.selector.active {
  display: block;
}
</style>
