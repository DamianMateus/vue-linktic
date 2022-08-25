

<template>
  <section class="container">
    <section class="dropdown-wrapper">
      <div @click="isVisible = !isVisible" class="selected-item">
        <span v-if="selectedFruit">{{ selectedFruit }}</span>
        <span v-else>Select an item</span>
        <Icon name="iconDown"></Icon>
      </div>
      <div v-if="isVisible" class="dropdown-open">
        <Icon name="iconUp"></Icon>
        <input type="text" v-model="searchQuery"  placeholder="This is a search input"/>
        <div class="options">
          <ul>
            <li @click="selectFruit()" v-for="(fruit, index) in filteredFruit" :key="`${index}`"> {{ fruit }}
            </li>
          </ul>
        </div>
      </div>
    </section>
  </section>
</template>

<script>

import Icon from '../shared/Icon.vue'



export default {
  data() {
    return {
      fruitsArray: [],
      searchQuery: '',
      selectedItem: null,
      isVisible: false,
    }
  },
  computed: {
    filteredFruit() {
      return this.fruitsArray.filter((fruit) => {
        return fruit.match(this.searchQuery);
      })
    }
  },
  methods: {
    selectFruit(fruit) {
      this.selectedItem = fruit;
      this.isVisible = false;
    },
  },

  mounted() {
    fetch('http://127.0.0.1:8888/api/fruits')
      .then(res => res.json())
      .then(json => {
        this.fruitsArray = json.data.fruits
      })
  },
  components: {
    Icon
  }
}
</script>

<style scoped lang="scss">
.container {
  width: 320px;
  height: 120px;
  left: 0;
  background-color: #EDF2F7;
}

.dropdown-wrapper {
  max-width: 256px;
  height: 56px;
  padding: 32px;
  position: relative;
  margin: 0 auto;

  .selected-item {
    height: 40px;
    background-color: #FFFFFF;
    border-radius: 5px;
    padding: 5px 10px;
    display: flex;
    justify-content: space-between;
    align-items: center;
    font-size: 16px;
    font-weight: 400;
    color: #A0AEC0;
    box-shadow: 0px 10px 15px rgba(35, 78, 82, 0.1);
  }

  .dropdown-open {
    position: absolute;
    background-color: #fff;
    max-width: 256px;
    left: 32px;
    top: 32px;
    right: 0;
    border-radius: 8px;

    input {
      width: 94%;
      height: 30px;
      border: none;
      font-size: 16px;
      padding-left: 8px;
      color: #A0AEC0;

      &::placeholder {
        color: #A0AEC0;
      }

      &:focus-visible {
        outline-color: white;
        outline-width: 0px;
      }
    }

    .options {
      width: 100%;

      ul {
        list-style: none;
        text-align: left;
        padding-left: 8px;
        max-height: 180px;
        overflow-y: scroll;
        overflow-x: hidden;

        li {
          width: 100%;
          padding: 10px;
          background-color: #FFFFFF;
          color: black;
          font-size: 18px;
          cursor: pointer;

          &:hover {
            background: #FFFFFF;
            color: #4299E1;
          }
        }
      }
    }
  }
}
</style>

