<template>
  <div class="card">
    <!-- Complex conditional rendering for different types -->
    <template v-if="type === 'product'">
      <img :src="(data as ProductData).imageUrl" :alt="data.name" class="product-image">
    </template>
    <template v-else-if="type === 'user'">
      <div class="avatar-container">
        <img :src="(data as UserData).avatarUrl" :alt="data.name" class="avatar">
        <div v-if="(data as UserData).isOnline" class="online-status"></div>
      </div>
    </template>

    <div :class="['content', { 'text-center': type === 'user' }]">
      <h3>{{ data.name }}</h3>

      <!-- Product-specific content -->
      <template v-if="type === 'product'">
        <p class="price">${{ (data as any).price?.toLocaleString() }}</p>
        <div class="stock-info">
          <span :class="['stock-status', stockStatusClass]">
            {{ stockStatusText }}
          </span>
        </div>
      </template>

      <!-- User-specific content -->
      <template v-else-if="type === 'user'">
        <p class="role">{{ (data as any).role }}</p>
        <div class="stats">
          <div class="stat">
            <span class="stat-label">Posts</span>
            <span class="stat-value">{{ (data as any).posts }}</span>
          </div>
          <div class="stat">
            <span class="stat-label">Followers</span>
            <span class="stat-value">{{ (data as any).followers }}</span>
          </div>
        </div>
      </template>

      <!-- Generic action button with complex conditional logic -->
      <button
        @click="handleAction"
        :disabled="type === 'product' && !(data as any).inStock"
        :class="['action-button', {
          'following': type === 'user' && (data as any).isFollowing
        }]"
      >
        {{ buttonText }}
      </button>
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue'

type ProductData = {
  id: string
  name: string
  price: number
  imageUrl: string
  inStock: boolean
  stockCount: number
}

type UserData = {
  id: string
  name: string
  role: string
  avatarUrl: string
  isOnline: boolean
  isFollowing: boolean
  posts: number
  followers: number
}

export default defineComponent({
  name: 'BaseCard',
  props: {
    type: {
      type: String as () => 'product' | 'user',
      required: true,
    },
    data: {
      type: Object as () => ProductData | UserData,
      required: true,
    },
  },
  emits: {
    'add-to-cart': (_productId: string) => true,
    'toggle-follow': (_selectedUser: { id: string; currentState: boolean }) => true,
  },
  methods: {
    handleAction(): void {
      if (this.type === 'product') {
        const product = this.data as ProductData
        if (!product.inStock) return
        this.$emit('add-to-cart', product.id)
      } else {
        const user = this.data as UserData
        this.$emit('toggle-follow', {
          id: user.id,
          currentState: user.isFollowing,
        })
      }
    },
  },
  computed: {
    buttonText(): string {
      if (this.type === 'product') {
        return 'Add to Cart'
      }
      return (this.data as UserData).isFollowing ? 'Following' : 'Follow'
    },
    stockStatusText(): string {
      const product = this.data as ProductData
      if (!product.inStock) return 'Out of Stock'
      if (product.stockCount < 5) return 'Low Stock'
      return 'In Stock'
    },
    stockStatusClass(): string {
      const product = this.data as ProductData
      if (!product.inStock) return 'out-of-stock'
      if (product.stockCount < 5) return 'low-stock'
      return 'in-stock'
    },
  },
})
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

/* Product-specific styles */
.product-image {
  width: 100%;
  height: 200px;
  object-fit: cover;
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

/* User-specific styles */
.avatar-container {
  position: relative;
  width: 120px;
  height: 120px;
  margin: 20px auto;
}

.avatar {
  width: 100%;
  height: 100%;
  border-radius: 50%;
  object-fit: cover;
  border: 3px solid white;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

.online-status {
  position: absolute;
  bottom: 5px;
  right: 5px;
  width: 12px;
  height: 12px;
  background: #48bb78;
  border: 2px solid white;
  border-radius: 50%;
}

.role {
  color: #666;
  font-size: 0.9rem;
  margin: 0 0 16px;
}

.stats {
  display: flex;
  justify-content: center;
  gap: 24px;
  margin-bottom: 16px;
}

.stat {
  display: flex;
  flex-direction: column;
  align-items: center;
}

.stat-label {
  font-size: 0.8rem;
  color: #666;
}

.stat-value {
  font-size: 1.1rem;
  font-weight: bold;
  color: #333;
}

/* Common styles */
.content {
  padding: 16px;
}

.text-center {
  text-align: center;
}

h3 {
  margin: 0 0 8px;
  font-size: 1.2rem;
  color: #333;
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
  transition: all 0.2s;
}

.action-button:disabled {
  background: #a0aec0;
  cursor: not-allowed;
}

.action-button.following {
  background: #48bb78;
}

.action-button:hover:not(:disabled) {
  opacity: 0.9;
}
</style>