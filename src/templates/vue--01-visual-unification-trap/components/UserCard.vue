<template>
  <div class="card">
    <div class="avatar-container">
      <img :src="user.avatarUrl" :alt="user.name" class="avatar">
      <div v-if="user.isOnline" class="online-status"></div>
    </div>
    <div class="content">
      <h3>{{ user.name }}</h3>
      <p class="role">{{ user.role }}</p>
      <div class="stats">
        <div class="stat">
          <span class="stat-label">Posts</span>
          <span class="stat-value">{{ user.posts }}</span>
        </div>
        <div class="stat">
          <span class="stat-label">Followers</span>
          <span class="stat-value">{{ user.followers }}</span>
        </div>
      </div>
      <button
        @click="toggleFollow"
        :class="['action-button', { 'following': user.isFollowing }]"
      >
        {{ followButtonText }}
      </button>
    </div>
  </div>
</template>

<script lang="ts">
export default {
  name: 'UserCard',
  props: {
    user: {
      type: Object as () => {
        id: string;
        name: string;
        role: string;
        avatarUrl: string;
        isOnline: boolean;
        isFollowing: boolean;
        posts: number;
        followers: number;
      },
      required: true
    }
  },
  computed: {
    followButtonText(): string {
      return this.user.isFollowing ? 'Following' : 'Follow';
    }
  },
  methods: {
    toggleFollow(): void {
      this.$emit('toggle-follow', {
        userId: this.user.id,
        currentState: this.user.isFollowing
      });
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

.content {
  padding: 16px;
  text-align: center;
}

h3 {
  margin: 0 0 4px;
  font-size: 1.2rem;
  color: #333;
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

.action-button.following {
  background: #48bb78;
}

.action-button:hover {
  opacity: 0.9;
}
</style>