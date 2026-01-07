<template>
  <div class="container">

    <h2 class="title">Products Table</h2>

    <!-- Controls Row -->
    <div class="controls-row">
      <input
        v-model="searchText"
        type="text"
        placeholder="Global Search…"
        class="search-input"
      />

      <div class="right-controls">
        <label>
          Per Page:
          <select v-model.number="perPage">
            <option v-for="opt in perPageOptions" :key="opt" :value="opt">
              {{ opt }}
            </option>
          </select>
        </label>
      </div>
    </div>

    <!-- Table -->
    <table class="products-table">
      <thead>
        <tr>
          <th @click="sortTable('id')">ID</th>
          <th @click="sortTable('title')">Title</th>
          <th @click="sortTable('price')">Price</th>
          <th @click="sortTable('discountPercentage')">Discount %</th>
          <th @click="sortTable('rating')">Rating</th>
          <th @click="sortTable('stock')">Stock</th>
          <th @click="sortTable('brand')">Brand</th>
          <th @click="sortTable('category')">Category</th>
          <th @click="sortTable('availabilityStatus')">Availability</th>
        </tr>

        <tr class="filters-row">
          <th><input v-model="columnFilters.id" placeholder="Filter..." /></th>
          <th><input v-model="columnFilters.title" placeholder="Filter..." /></th>
          <th><input v-model="columnFilters.price" placeholder="Filter..." /></th>
          <th><input v-model="columnFilters.discountPercentage" placeholder="Filter..." /></th>
          <th><input v-model="columnFilters.rating" placeholder="Filter..." /></th>
          <th><input v-model="columnFilters.stock" placeholder="Filter..." /></th>
          <th><input v-model="columnFilters.brand" placeholder="Filter..." /></th>
          <th><input v-model="columnFilters.category" placeholder="Filter..." /></th>
          <th><input v-model="columnFilters.availabilityStatus" placeholder="Filter..." /></th>
        </tr>
      </thead>

      <tbody>
        <tr v-for="item in paginatedData" :key="item.id">
          <td>{{ item.id }}</td>
          <td>{{ item.title }}</td>
          <td>{{ item.price }}</td>
          <td>{{ item.discountPercentage }}</td>
          <td>{{ item.rating }}</td>
          <td>{{ item.stock }}</td>
          <td>{{ item.brand }}</td>
          <td>{{ item.category }}</td>
          <td>{{ item.availabilityStatus }}</td>
        </tr>
      </tbody>
    </table>

    <!-- Pagination -->
    <div class="pagination">
      <button :disabled="currentPage === 1" @click="changePage(currentPage - 1)">
        Prev
      </button>

      <span>Page {{ currentPage }} of {{ totalPages }}</span>

      <button :disabled="currentPage === totalPages" @click="changePage(currentPage + 1)">
        Next
      </button>
    </div>

  </div>
</template>



<script>
  export default {
  name: "ProductsTable",
  data() {
    return {
      products: [],        // raw products from API
      cache: null,         // caching store
      searchText: "",
      currentPage: 1,
      perPage: 10,
      sortKey: "",
      sortOrder: "asc",
      perPageOptions: [5, 10, 15, 20, 50],
      perPage: 10, // default
      columnFilters: {
        id: "",
        title: "",
        price: "",
        discountPercentage: "",
        rating: "",
        stock: "",
        brand: "",
        category: "",
        availabilityStatus: "",
      },
    };
  },
  computed: {
    filteredProducts() {
      let data = this.cache || this.products;

      // APPLY all column filters
      Object.keys(this.columnFilters).forEach((key) => {
        const term = this.columnFilters[key].toString().toLowerCase();
        if (term)
          data = data.filter((item) =>
            (item[key] + "").toLowerCase().includes(term)
          );
      });

      // GLOBAL SEARCH (optional)
      if (this.searchText.trim()) {
        const q = this.searchText.toLowerCase();
        data = data.filter((p) =>
          p.title.toLowerCase().includes(q)
        );
      }

      // SORT
      if (this.sortKey) {
        data = [...data].sort((a, b) => {
          if (a[this.sortKey] < b[this.sortKey]) return this.sortOrder === "asc" ? -1 : 1;
          if (a[this.sortKey] > b[this.sortKey]) return this.sortOrder === "asc" ? 1 : -1;
          return 0;
        });
      }

      return data;
    },
    paginatedData() {
      const start = (this.currentPage - 1) * this.perPage;

      //slice logic What you were asking about
      return this.filteredProducts.slice(start, start + this.perPage); 
    },
    totalPages() {
      return Math.ceil(this.filteredProducts.length / this.perPage);
    },
  },
  mounted() {
    this.fetchProducts();
  },
  methods: {
    async fetchProducts() {
      try {
        // USE CACHE if present
        if (this.cache) return;

        const response = await fetch(
          "https://dummyjson.com/products?limit=100"
        );
        const data = await response.json();

        this.products = data.products || [];

        // Storing in cache
        this.cache = this.products;
      } catch (e) {
        console.error("Fetch Error:", e);
      }
    },
    changePage(page) {
      if (page < 1 || page > this.totalPages) return;
      this.currentPage = page;
    },
    sortTable(key) {
      if (this.sortKey === key) {
        this.sortOrder = this.sortOrder === "asc" ? "desc" : "asc";
      } else {
        this.sortKey = key;
        this.sortOrder = "asc";
      }
    },
  }
};
</script>


<style scoped>
.container {
  padding: 20px;
  width: 100%;
  box-sizing: border-box;
}

.title {
  text-align: center;
  margin-bottom: 15px;
}

.controls-row {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 15px;
  flex-wrap: wrap;
}

.search-input {
  flex: 1;
  max-width: 300px;
  margin-right: 10px;
  padding: 6px;
}

.right-controls {
  white-space: nowrap;
  margin-top: 10px;
}

.products-table {
  width: 100%;
  border-collapse: collapse;
  table-layout: fixed; /* forces wrap on overflow */
}

.products-table th,
.products-table td {
  padding: 8px;
  border: 1px solid #ddd;
  word-wrap: break-word; /* wrap text */
  white-space: normal;   /* don’t force single line */
  text-align: left;
}

.products-table th {
  background: #f5f5f5;
  cursor: pointer;
}

.filters-row input {
  width: 100%;
  padding: 4px;
  box-sizing: border-box;
}

.pagination {
  display: flex;
  justify-content: center;
  margin-top: 15px;
  gap: 12px;
}

button:disabled {
  opacity: 0.5;
  cursor: not-allowed;
}

</style>