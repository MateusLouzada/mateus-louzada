<template>
  <div class="container">
    <div class="filters-and-ordenations">
      <ordenation
        :products="products"
        :ordenationCallback="changeOrdenationProducts"
      />
      <button
        class="btn-default"
        v-for="(category, index) in categorys"
        :key="index"
        @click="filterCategory($event)"
      >
        {{ capitalize(category) }}
      </button>
      <rating
        :products="products"
        :productsNotChangeable="productsNotChangeable"
        :ratingCallback="changeOrdenationProducts"
      />
    </div>
    <div class="container-cards">
      <card
        v-for="product in products"
        :key="product.id"
        :product="product"
        :handleAddCartAndQuantity="handleAddCartAndQuantity"
      />
    </div>
  </div>
</template>

<script>
import Card from "../components/Card.vue";
import Ordenation from "../components/Ordenation.vue";
import Rating from "../components/Rating.vue";

export default {
  name: "HomeView",
  strict: true,

  data() {
    return {
      products: [],
      productsNotChangeable: [],
      cart: [],
      categorys: [],
    };
  },
  components: {
    card: Card,
    ordenation: Ordenation,
    rating: Rating,
  },
  watch: {
    products: {
      handler() {
        this.productsNotChangeable.forEach((product) => {
          const existingCategory = this.categorys.find((category) => {
            return category === product.category;
          });
          if (!existingCategory) this.categorys.push(product.category);
        });
      },
      deep: true,
    },
  },
  created() {
    this.products = this.$store.getters.getProducts;
    this.productsNotChangeable = this.$store.getters.getProducts;
    this.cart = this.$store.getters.getCart;
  },
  methods: {
    handleAddCartAndQuantity(value) {
      this.$store.commit("addCart", value);
      console.log(this.products);
    },
    changeOrdenationProducts(newProducts) {
      this.products = newProducts;
    },
    filterCategory(event) {
      const isClassActive = event.target.classList.value.includes("active");
      if (!isClassActive) {
        event.target.classList.toggle("active");
        const allCategorysSelects = [
          ...document.getElementsByClassName("active"),
        ];
        const categorysSelecteds = [];
        allCategorysSelects.forEach((categorySelect) => {
          this.productsNotChangeable.forEach((product) => {
            if (
              product.category.toUpperCase() ===
              categorySelect.outerText.toUpperCase()
            ) {
              categorysSelecteds.push(product);
            }
          });
        });
        this.products = categorysSelecteds;
      } else {
        event.target.classList.remove("active");
        const allCategorysSelects = [
          ...document.getElementsByClassName("active"),
        ];
        const categorysSelecteds = [];
        allCategorysSelects.forEach((categorySelect) => {
          this.productsNotChangeable.forEach((product) => {
            if (
              product.category.toUpperCase() ===
              categorySelect.outerText.toUpperCase()
            ) {
              categorysSelecteds.push(product);
            }
          });
        });
        this.products = categorysSelecteds;
        if (allCategorysSelects.length === 0)
          this.products = this.productsNotChangeable;
      }
    },
    capitalize(value) {
      return value.charAt(0).toUpperCase() + value.slice(1);
    },
  },
};
</script>

<style scoped>
.container {
  display: flex;
  flex-direction: column;
  flex-wrap: wrap;
  width: 80%;
  margin: 20px auto 0;
}
.filters-and-ordenations {
  display: flex;
  align-items: center;
  margin-left: 5rem;
}
.btn-default {
  background-color: #4954db;
  color: bisque;
  padding: 16px;
  font-size: 1.2rem;
  border: none;
  cursor: pointer;
  margin-right: 20px;
  border-radius: 20px;
}
.btn-default:hover {
  background-color: #4480f2;
}
.active {
  background-color: #4480f2;
}
.container-cards {
  display: flex;
  justify-content: center;
  flex-wrap: wrap;
}
@media only screen and (max-width: 1185px) {
  .btn-default {
    font-size: 1rem;
    margin: 0.8rem 1rem;
    width: 12rem;
    }
  .filters-and-ordenations {
    display: flex;
    align-items: center;
    justify-content: center;
    flex-wrap: wrap;
    margin: 0;
  }
}
</style>
