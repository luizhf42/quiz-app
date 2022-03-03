<template>
  <section class="home">
    <h1>LH's Quiz 🐢</h1>

    <p>Select the category</p>
    <select @change="changeCategoty">
      <option :key="category.name" v-for="category in categories">
        {{ category.name }}
      </option>
    </select>
  </section>
</template>

<script>
import axios from "../services/axios";

export default {
  name: "Home",
  data() {
    return {
      categories: [],
      selectedCategory: "",
    };
  },

  async mounted() {
    try {
      const { data } = await axios.get("api_category.php");
      this.categories = data.trivia_categories;
      this.selectedCategory = data.trivia_categories[0].name;
    } catch (error) {
      console.error(error);
    }
  },

  methods: {
    changeCategoty(event) {
      const options = event.target.options;
      const index = options.selectedIndex;
      if (index > -1) {
        this.selectedCategory = options[index].innerText;
      }
    },
  },
};
</script>

<style lang="scss" scoped>
@use "../App.scss";
.home {
  width: min(95%, 400px);
  @include App.flex();

  h1 {
    font-size: 2.5rem;
    margin-bottom: 5px;
  }
  p {
    margin-bottom: 5px;
    font-size: 1.3rem;
  }
  select {
    width: 80%;
    padding: 3px;
    font-size: 1rem;
    cursor: pointer;

    &:focus {
      outline: 0;
    }
  }
}
</style>