<template>
  <div :class="['product-display', bgClass]">
    <div v-if="loading" class="loader"></div>

    <!-- Available product: Women's or Men's Clothing -->
    <div v-else-if="product && (product.category === 'men\'s clothing' || product.category === 'women\'s clothing')">
      <div class="card">
        <div class="image">
          <img :src="product.image" alt="product" class="product-image" />
        </div>
        <div class="description">
          <h2 :class="textClass">{{ product.title }}</h2>
          <div class="desc2">
            <div class="category">
              <p>{{ product.category }}</p>
            </div>
            <div class="rating">
              2.9/5
              <span class="dot" v-for="n in 5" :key="n" :class="{ filled: n <= 3, purple: isWomen, blue: isMen }">●</span>
            </div>
          </div>
          <hr class="custom-line" />
          <p class="desc">{{ product.description }}</p>
          <hr class="custom-line" />
          <p :class="['price', textClass]"><strong>${{ product.price }}</strong></p>
          <div class="btn-group">
            <button class="btn" :class="btnClass">Buy now</button>
            <button :class="['btn-outline', nextBtnClass]" @click="nextProduct">Next product</button>
          </div>
        </div>
      </div>
    </div>

    <!-- Unavailable product -->
    <div v-else class="empty bg-unavailable">
      <div class="card2">
        <div class="sad">
          <img src="@/assets/style/sad-face.png" alt="sad face" class="sad-img" />
        </div>
        <div class="empty-content">
          <p>This product is unavailable to show</p>
          <button class="btn-outline" @click="nextProduct">Next product</button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "ProductDisplay",
  data() {
    return {
      index: 1,
      product: null,
      loading: false,
    };
  },
  mounted() {
    this.fetchProduct();
  },
  methods: {
    async fetchProduct() {
      this.loading = true;
      try {
        const res = await fetch(`https://fakestoreapi.com/products/${this.index}`);
        const data = await res.json();
        console.log('Fetched product:', data); // 🔍 DEBUG
        if (data.category === "men's clothing" || data.category === "women's clothing") {
        this.product = data;
        } else {
          this.product = null;
        }
      } catch (error) {
        console.error("Error fetching product:", error);
        this.product = null;
      } finally {
        this.loading = false;
      }
    },
    nextProduct() {
      this.index = this.index <= 20 ? this.index + 1 : 1;
      this.fetchProduct();
    }
  },
  computed: {
    isWomen() {
      return this.product?.category === "women's clothing";
    },
    isMen() {
      return this.product?.category === "men's clothing";
    },
    bgClass() {
       if (!this.product || !this.product.category) return '';
       if (this.product.category === "men's clothing") return 'bg-men';
       if (this.product.category === "women's clothing") return 'bg-women';
       return '';
    },
    btnClass() {
      if (this.isWomen) return 'btn-purple';
      if (this.isMen) return 'btn-blue';
      return '';
    },
    nextBtnClass() {
      if (this.isWomen) return 'btn-outline-women';
      if (this.isMen) return 'btn-outline-men';
      return '';
    },
    textClass() {
      if (this.isWomen) return 'text-purple';
      if (this.isMen) return 'text-blue';
      return '';
    }
  }
};
</script>
