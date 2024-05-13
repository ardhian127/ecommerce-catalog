<template>
  <div class="container bg-netral">

    <!-- isLoading Start-->
    <div v-if="isLoading" class="card">

      <!-- Loading Unavailable Product Start -->
      <div v-if="!isProductUnavailable" class="product-unavailable-container">
          <div class="product-text">
            <div class="cta">
              <div class="loader"></div>
            </div>
          </div>
      </div>
      <!-- Loading Unavailable Product End -->

      <!-- Loading Product Start-->
      <div v-else class="product-container load">
        <div class="load-thumbnail bg-load"></div>
        <div class="load-details">
          <div class="load-text-details">
            <div class="load-text-title bg-load"></div>
            <div class="load-text-sub-title">
              <div class="load-category bg-load"></div>
              <div class="load-rating bg-load"></div>
            </div>
            <div class="load-text-description bg-load"></div>
          </div>
          <div class="load-details-action">
            <div class="load-price bg-load"></div>
            <div class="load-cta">
              <div class="load-cta-buy bg-load"></div>
              <div class="load-cta-next bg-load"></div>
            </div>
          </div>
        </div>
      </div>
      <!-- Loading Product End -->

    </div>

    <!-- isLoading End -->

    <!-- Tampilan Berdasarkan Kategori -->
    <div v-else :class="{ container: true, 'bg-unavailable': !isProductUnavailable, 
        'bg-men-color': products.data && products.data.category === 'men\'s clothing', 
        'bg-women-color': products.data && products.data.category === 'women\'s clothing' }">

      <div class="overlay">
        <img src="../assets/images/bg-shape.svg" alt="bg-image" />
      </div>
      <div class="card">

        <!-- Unavailable Product Start-->
        <div v-if="!isProductUnavailable" class="product-unavailable-container">
          <div class="overlay">
            <img src="../assets/images/bg-unavailable-product.svg" alt="bg-unvailable-product" />
          </div>
          <div class="product-text">
            <p>This product is Unvailable to show</p>
            <div class="cta">
              <button class="cta-next" type="button" @click="getProductbyId()">
                Next Product
              </button>
            </div>
          </div>
        </div>
        <!-- Unavailable Product End -->

        <!-- Product by Category Start -->
        <div v-else class="product-container">
          <div class="product-image">
            <img :src="products.data.image" alt="image-product" />
          </div>
          <div class="product-text">
            <div class="details">
              <h4 :class="{ title: true, 'font-men': products.data.category === 'men\'s clothing', 'font-women': products.data.category === 'women\'s clothing' }" >
                {{ products.data.title }}
              </h4>
              <div class="sub-title">
                <span>{{ products.data.category }}</span>
                <div class="rating">
                  <span>{{ products.data.rating.rate }}/5 </span>
                  <div class="rating">
                    <div v-for="setRate in setRating" :key="setRate.id" :class="{ circle: true, 'bg-navy': products.data.category === 'men\'s clothing', 'bg-magenta': products.data.category === 'women\'s clothing', }"></div>
                    <div v-for="sisaRate in sisaRating" :key="sisaRate.id" :class="{ circleborder: true, 'border-men': products.data.category === 'men\'s clothing', 'border-women': products.data.category === 'women\'s clothing', }"></div>
                  </div>
                </div>
              </div>
              <div class="description">
                <p> {{ products.data.description }} </p>
              </div>
            </div>
            <div class="action">
              <span :class="{ price: true, 'font-men': products.data.category === 'men\'s clothing', 'font-women': products.data.category === 'women\'s clothing', }"> 
                $ {{ products.data.price }}</span>
              <div class="cta">
                <button :class="{ 
                    'cta-buy': true, 
                    'bg-navy btn-buy-men': products.data.category === 'men\'s clothing', 
                    'bg-magenta': products.data.category === 'women\'s clothing' 
                  }" type="button">
                  Buy Now
                </button>
                <button :class="{ 
                    'cta-next': true, 
                    'font-men border-men btn-men': products.data.category === 'men\'s clothing', 
                    'font-women border-women btn-women': products.data.category === 'women\'s clothing'
                  }" type="button" @click="getProductbyId()">
                  Next Product
                </button>
              </div>
            </div>
          </div>
        </div>
      </div>
      <!-- Product by Category -->

    </div>
    <!-- Tampilan Berdasarkan Kategori End -->

  </div>
</template>

<script>
export default {
  name: "ProductDisplay",
  data() {
    return {
      isLoading: false,
      products: {
        data:{
          category: ''
        },
      },
      index: 0,
      setRating: 0,
      sisaRating: 0,
      isProductUnavailable: false,
    };
  },
  methods: {
    async getData() {
      const res = await fetch(
        `https://fakestoreapi.com/products/${this.index}`
      );
      const result = await res.json();
      return result;
      
    },

    async getProductbyId() {
      this.isLoading = true;

      if (this.index !== 20) {
        this.index++;
      } else {
        this.index = 1;
      }

      let data = await this.getData();

      if ( data.category === "men's clothing" || data.category === "women's clothing") {
        
        this.products = {data};
        this.isProductUnavailable = true;

        this.setRating = Math.round(data.rating.rate);
        this.sisaRating = 5 - this.setRating;

      } else {
        this.products = {};
        this.isProductUnavailable = false;
      }
      this.isLoading = false;
    },
  },
  mounted() {
    this.getProductbyId();
  },
};
</script>

<style>
@import "../assets/style/page.css";
</style>
