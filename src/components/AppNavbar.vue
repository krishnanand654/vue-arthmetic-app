<template>
  <nav class="navbar">
    <div>
      <button @click="logout">
        Log out <img class="logout-icon" src="../assets/logout.png" />
      </button>
    </div>
  </nav>
</template>

<script>
export default {
  name: 'AppNavbar',
  methods: {
    logout() {
      localStorage.clear()
      this.$router.push('/')
    }
  }
}
</script>

<style scoped>
.navbar {
  background-color: #333;
  color: white;
  padding: 10px;
}

.navbar div {
  display: flex;
  justify-content: end;
  flex-direction: row;
  margin: 0 20px 0;
  align-items: center;
}

.navbar div h1 {
  font-weight: 700;
}

.navbar button {
  border: none;
  padding: 5px 10px 5px;
  background: none;
  color: white;
  font-weight: 600;
  cursor: pointer;
  display: flex;
  align-items: center;
  gap: 5px;
}

.logout-icon {
  width: auto;
  height: 20px;
  position: relative;
}
</style>
