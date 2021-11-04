<template>
  <section>
    <div class="container">
      <div class="field is-grouped">
        <p class="control is-expanded">
          <input
            class="input"
            v-model="searchValue"
            id="search-input"
            type="text"
            placeholder="Search carrier"
          />
        </p>

        <div class="select">
          <select v-model="sortBy">
            <option :key="option" v-for="option in filterOptions">
              {{ option }}
            </option>
          </select>
        </div>
        <Button @click="ascending = !ascending" class="button is-info ml-3">
          <h3 v-if="ascending">Descending</h3>
          <h3 v-else>Ascending</h3>
        </Button>
      </div>
      <Spinner v-if="isLoading" />

      <p class="pt-6" v-if="!products || products.length === 0">
        No products found
      </p>

      <div v-else class="card-container">
        <Product
          v-for="product in filteredProducts"
          :expiry="product.plan.expiry_type"
          :size="product.plan.size"
          :unit="product.plan.unit"
          :name="product.carrier.name"
          :image="product.carrier.imageUrl"
          :country="product.carrier.country_code_iso3"
          :link="product.plan.tnc_url"
          :key="product.id"
        />
      </div>
    </div>
  </section>
</template>

<script>
import Product from "./Product";
import Button from "./Button";
import productData from "../api/productData";
import Spinner from "./Spinner";

export default {
  components: {
    Product,
    Button,
    Spinner,
  },
  data() {
    return {
      isLoading: true,
      products: [],
      ascending: true,
      searchValue: "",
      filterOptions: [
        "Filter By",
        "By Country Code",
        "By carrier name",
        "By plan size",
      ],
      sortBy: "Filter By",
    };
  },

  computed: {
    filteredProducts() {
      let tempProducts = [...this.products];

      // Handling search input
      if (this.searchValue != "" && this.searchValue) {
        tempProducts = tempProducts.filter((item) => {
          return item.carrier.name
            .toUpperCase()
            .includes(this.searchValue.toUpperCase());
        });
      }

      // Sort by By Country Code
      tempProducts = tempProducts.sort((a, b) => {
        if (this.sortBy == "By Country Code") {
          let fa = a.carrier.country_code_iso3.toLowerCase(),
            fb = b.carrier.country_code_iso3.toLowerCase();

          if (fa < fb) {
            return -1;
          }
          if (fa > fb) {
            return 1;
          }
          return 0;

          // Sort by By carrier name
        } else if (this.sortBy == "By carrier name") {
          let fa = a.carrier.name.toLowerCase(),
            fb = b.carrier.name.toLowerCase();

          if (fa < fb) {
            return -1;
          }
          if (fa > fb) {
            return 1;
          }
          return 0;

          // Sort by By plan size
        } else if (this.sortBy == "By plan size") {
          if (a.plan.unit === "MB" && b.plan.unit === "MB") {
            return a.plan.size - b.plan.size;
          }
        }
      });

      // Show products in descending or ascending order
      if (!this.ascending) {
        tempProducts.reverse();
      }

      return tempProducts;
    },
  },

  methods: {
    async FetchProductList() {
      const res = await productData.post("/package/search", {
        msisdn: "",
      });
      for (const item of res.data) {
        if (item.plan.unit === "GB") {
          item.plan.size = item.plan.size * 1024;
          item.plan.unit = "MB";
        }
      }

      this.products = res.data;
      this.isLoading = false;
    },
  },
  mounted() {
    this.FetchProductList();
  },
};
</script>

<style scoped>
.container {
  padding-top: 50px;
}
.card-container {
  display: grid;
  grid-gap: 20px;
  grid-template-columns: repeat(auto-fill, minmax(300px, 1fr));
  padding-top: 50px;
}
</style>
