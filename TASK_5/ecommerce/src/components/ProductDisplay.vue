<template>
  <div class="container">
    <div v-if="isLoading" class="loader"></div>
    <div
      v-else
      class="container"
      :class="
        !isProductAvailable
          ? 'bg-gray'
          : product.data.category === category
          ? 'bg-blue'
          : 'bg-pink'
      "
    >
      <div class="overlay">
        <img src="../assets/images/bg-shape.svg" alt="background overlay" />
      </div>
      <div class="card">
        <div v-if="isProductAvailable" class="product-container">
          <!-- asd -->
          <div class="product-thumbnail">
            <img :src="product.data.image" alt="" />
          </div>
          <div class="product-details">
            <div class="top">
              <h3
                :class="
                  product.data.category === category
                    ? 'text-navy'
                    : 'text-magenta'
                "
                class="title"
              >
                {{ product.data.title }}
              </h3>
              <div class="sub-title">
                <span>{{ product.data.category }}</span>
                <div class="rating">
                  <span>{{ product.data.rating.rate }}/5</span>
                  <div class="rating" v-for="item in 5" :key="item">
                    <span
                      v-if="item <= rating.countRate"
                      :class="
                        product.data.category === category
                          ? 'bg-navy'
                          : 'bg-magenta'
                      "
                      class="circle"
                    ></span>
                    <span
                      v-else
                      :class="
                        product.data.category === category
                          ? 'border-navy'
                          : 'border-magenta'
                      "
                      class="circle"
                    ></span>
                  </div>
                </div>
              </div>
              <div class="description">
                <p>{{ product.data.description }}</p>
              </div>
            </div>
            <div class="bottom">
              <span
                :class="
                  product.data.category === category
                    ? 'text-navy'
                    : 'text-magenta'
                "
                class="price"
                >${{ product.data.price }}</span
              >
              <div class="cta">
                <button
                  type="button"
                  :class="
                    product.data.category === category
                      ? 'bg-navy'
                      : 'bg-magenta'
                  "
                  class="cta-buy"
                >
                  Buy Now
                </button>
                <button
                  type="button"
                  @click="getSingleProduct()"
                  :class="
                    product.data.category === category
                      ? 'border-navy text-navy'
                      : 'border-magenta text-magenta'
                  "
                  class="cta-next"
                >
                  Next Product
                </button>
              </div>
            </div>
          </div>
        </div>
        <div v-else class="product-unavailable-container">
          <div class="overlay">
            <img
              src="../assets/images/bg-sad.svg"
              alt="background unavailable product"
              srcset=""
            />
          </div>
          <div class="product-details">
            <p>This product is unavailable to show</p>
            <div class="cta">
              <button
                type="button"
                @click="getSingleProduct()"
                class="cta-next"
              >
                Next Product
              </button>
            </div>
          </div>
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
      isLoading: false,
      index: 0,
      isProductAvailable: false,
      product: {},
      rating: {},
      category: "men's clothing",
    };
  },
  methods: {
    async fetchAPI() {
      const response = await fetch(
        `https://fakestoreapi.com/products/${this.index}`
      );
      const result = await response.json();
      return result;
    },

    async getSingleProduct() {
      this.isLoading = true;
      // console.log(this.isLoading);
      if (this.index !== 20) {
        this.index++;
      } else {
        this.index = 1;
      }

      let data = await this.fetchAPI();
      let countRate = Math.round(data.rating.rate);
      this.rating = { countRate };
      // console.log(countRate);
      if (
        data.category === "men's clothing" ||
        data.category === "women's clothing"
      ) {
        this.product = { data };
        this.isProductAvailable = true;
      } else {
        this.isProductAvailable = false;
      }

      this.isLoading = false;
    },
  },

  mounted() {
    this.getSingleProduct();
  },
};
</script>
