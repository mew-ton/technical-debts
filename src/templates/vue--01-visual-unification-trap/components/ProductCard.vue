<template>
  <div class="card">
    <img :src="product.imageUrl" :alt="product.name" class="product-image">
    <div class="content">
      <h3>{{ product.name }}</h3>
      <p class="price">${{ product.price.toLocaleString() }}</p>
      <div class="stock-info">
        <span :class="['stock-status', stockStatusClass]">
          {{ stockStatusText }}
        </span>
      </div>
      <button 
        @click="addToCart"
        :disabled="!product.inStock"
        class="action-button"
      >
        Add to Cart
      </button>
    </div>
  </div>
</template>

<script lang="ts">
export default {
  name: 'ProductCard',
  props: {
    product: {
      type: Object as () => {
        id: string;
        name: string;
        price: number;
        imageUrl: string;
        inStock: boolean;
        stockCount: number;
      },
      required: true
    }
  },
  computed: {
    stockStatusText(): string {
      if (!this.product.inStock) return 'Out of Stock';
      if (this.product.stockCount < 5) return 'Low Stock';
      return 'In Stock';
    },
    stockStatusClass(): string {
      if (!this.product.inStock) return 'out-of-stock';
      if (this.product.stockCount < 5) return 'low-stock';
      return 'in-stock';
    }
  },
  methods: {
    addToCart(): void {
      if (!this.product.inStock) return;
      this.$emit('add-to-cart', this.product.id);
    }
  }
}
</script>

<style scoped>
.card {
  border: 1px solid #e2e2e2;
  border-radius: 8px;
  overflow: hidden;
  width: 300px;
  background: white;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

.product-image {
  width: 100%;
  height: 200px;
  object-fit: cover;
}

.content {
  padding: 16px;
}

h3 {
  margin: 0 0 8px;
  font-size: 1.2rem;
  color: #333;
}

.price {
  font-size: 1.4rem;
  font-weight: bold;
  color: #e53e3e;
  margin: 8px 0;
}

.stock-info {
  margin: 8px 0;
}

.stock-status {
  padding: 4px 8px;
  border-radius: 4px;
  font-size: 0.9rem;
}

.in-stock {
  background: #c6f6d5;
  color: #22543d;
}

.low-stock {
  background: #feebc8;
  color: #744210;
}

.out-of-stock {
  background: #fed7d7;
  color: #822727;
}

.action-button {
  width: 100%;
  padding: 8px;
  background: #4299e1;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  font-size: 1rem;
}

.action-button:disabled {
  background: #a0aec0;
  cursor: not-allowed;
}

.action-button:hover:not(:disabled) {
  background: #3182ce;
}
</style>