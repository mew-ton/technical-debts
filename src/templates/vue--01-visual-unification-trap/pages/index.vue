<template>
  <main class="container">
    <h1>Technical Debt Demo: Component Abstraction</h1>

    <section class="demo-section">
      <h2>Product Cards (Using Common Component)</h2>
      <div class="cards-container">
        <BaseCard
          v-for="product in products"
          :key="product.id"
          type="product"
          :data="product"
          @add-to-cart="handleAddToCart"
        />
      </div>
    </section>

    <section class="demo-section">
      <h2>User Cards (Using Common Component)</h2>
      <div class="cards-container">
        <BaseCard
          v-for="user in users"
          :key="user.id"
          type="user"
          :data="user"
          @toggle-follow="handleToggleFollow"
        />
      </div>
    </section>
  </main>
</template>

<script lang="ts">
export default {
  name: 'IndexPage',
  data() {
    return {
      products: [
        {
          id: 'p1',
          name: 'Luxury Watch',
          price: 2500,
          imageUrl: 'https://picsum.photos/300/200?1',
          inStock: true,
          stockCount: 3,
        },
        {
          id: 'p2',
          name: 'Smartphone',
          price: 980,
          imageUrl: 'https://picsum.photos/300/200?2',
          inStock: true,
          stockCount: 15,
        },
        {
          id: 'p3',
          name: 'Wireless Earbuds',
          price: 350,
          imageUrl: 'https://picsum.photos/300/200?3',
          inStock: false,
          stockCount: 0,
        },
      ],
      users: [
        {
          id: 'u1',
          name: 'John Smith',
          role: 'Senior Engineer',
          avatarUrl: 'https://picsum.photos/120/120?1',
          isOnline: true,
          isFollowing: false,
          posts: 142,
          followers: 1234,
        },
        {
          id: 'u2',
          name: 'Sarah Johnson',
          role: 'Product Manager',
          avatarUrl: 'https://picsum.photos/120/120?2',
          isOnline: false,
          isFollowing: true,
          posts: 89,
          followers: 892,
        },
      ],
    }
  },
  methods: {
    handleAddToCart(productId: string): void {
      console.log(`Added product to cart: ${productId}`)
    },
    handleToggleFollow({ id, currentState }: { id: string; currentState: boolean }): void {
      console.log(`Toggled follow - User: ${id}, Current state: ${currentState}`)
      this.users = this.users.map((user) => {
        if (user.id === id) {
          return { ...user, isFollowing: !currentState }
        }
        return user
      })
    },
  },
}
</script>

<style scoped>
.container {
  max-width: 1200px;
  margin: 0 auto;
  padding: 20px;
}

h1 {
  text-align: center;
  color: #2d3748;
  margin-bottom: 40px;
}

.demo-section {
  margin-bottom: 40px;
}

h2 {
  color: #4a5568;
  margin-bottom: 20px;
}

.cards-container {
  display: flex;
  gap: 20px;
  flex-wrap: wrap;
  justify-content: center;
}
</style>