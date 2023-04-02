<template>
  <header>
    <div>
      <nav class="navbar navbar-expand-lg bg-light" style="width: 100vw">
        <div class="container-fluid py-3">
          <router-link class="navbar-brand nav-margin-left nav-brand" to="/">
            <div style="height: 30px">
              <img src="../assets/img/Logo.png" alt="Light mode logo" />
              <span class="fw-bold">GreenVenture</span>
            </div>
          </router-link>
          <button
            class="navbar-toggler"
            type="button"
            data-bs-toggle="collapse"
            data-bs-target="#navbarNav"
            aria-controls="navbarNav"
            aria-expanded="false"
            aria-label="Toggle navigation"
          >
            <span class="navbar-toggler-icon"></span>
          </button>
          <div id="navbarNav" class="collapse navbar-collapse">
            <ul class="navbar-nav">
              <li class="nav-item">
                <router-link class="nav-link active" to="/">Home</router-link>
              </li>
              <li class="nav-item">
                <router-link class="nav-link" to="/wallet">Vouchers/ Points</router-link>
              </li>
              <li class="nav-item">
                <a class="nav-link" :href="'http://localhost:3000/leaderboards/' + getUserID()"
                  >Leaderboard</a
                >
              </li>
              <li class="nav-item">
                <router-link class="nav-link" to="/community">Community</router-link>
              </li>
              <li class="nav-item">
                <router-link class="nav-link" to="/login">Community</router-link>
              </li>
              <li>
                <router-link to="/login" class="nav-link" @click="logout()">Log out</router-link>
              </li>
            </ul>
          </div>
        </div>
      </nav>
    </div>
  </header>
</template>

<script setup>
// Script Portion

function logout() {
  localStorage.clear()
}
function getUserID() {
  return localStorage.getItem('userID')
}

// const isLoggedin = localStorage.getItem('userID')
</script>

<style lang="scss" scoped></style>
