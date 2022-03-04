<template>
  <section class="home">
    <h1>LH's Quiz 🐢</h1>

    <p>Select the category</p>
    <select @change="changeCategory">
      <option :key="category.name" v-for="category in categories">
        {{ category.name }}
      </option>
    </select>

    <p>Choose the difficulty</p>
    <div class="difficulties">
      <div>
        <input
          @click="changeDifficulty($event)"
          name="difficulty"
          type="radio"
          id="easy"
        /><label class="button" for="easy" role="button">Easy</label>
      </div>
      <div>
        <input
          @click="changeDifficulty($event)"
          name="difficulty"
          type="radio"
          id="normal"
          checked
        /><label class="button" for="normal" role="button">Normal</label>
      </div>
      <div>
        <input
          @click="changeDifficulty($event)"
          name="difficulty"
          type="radio"
          id="hard"
        /><label class="button" for="hard" role="button">Hard</label>
      </div>
    </div>

    <p>Choose the number of questions</p>
    <input
      class="questions"
      type="number"
      min="10"
      max="50"
      v-model="questions"
    />

    <button>Start!</button>
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
      difficulty: "normal",
      questions: 15,
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
    changeCategory(event) {
      const options = event.target.options;
      const index = options.selectedIndex;

      if (index > -1) {
        this.selectedCategory = options[index].innerText;
      }
    },
    changeDifficulty(event) {
      this.difficulty = event.target.id;
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
  }

  p {
    text-align: center;
    margin: 10px 0 5px;
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

  .difficulties {
    @include App.flex(row);
    gap: 7px;

    div {
      @include App.flex(row);
      position: relative;
      margin-bottom: 5px;
      input {
        position: absolute;
        opacity: 0;
        cursor: pointer;
        height: 0;
        width: 0;
        &:checked ~ .button {
          background: App.$difficulty-checked;
          box-shadow: 0 0 2px #cfcfff;
        }
      }
      .button {
        transition: 0.3s;
        display: inline-block;
        width: 70px;
        text-align: center;
        color: black;
        background: App.$difficulty-btn;
      }
      &:hover input:not(:checked) ~ .button {
        background: App.$difficulty-hover;
      }
    }
  }

  .questions {
    width: 25%;
    padding: 2px 5px;
  }

  button {
    transition: .3s;
    padding: 10px 0;
    font-size: 1.3rem;
    margin: 30px;
    width: 200px;
    background: App.$start-btn;
    color: black;
    border: 0;
    box-shadow: 0 0 2px #cfcfff;
    border-radius: 5px;
    cursor: pointer;

    &:hover {
      background: App.$start-btn-hover;
      transform: scale(1.05);
    }
  }
}
</style>