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
                <a class="nav-link active" href="#">Home</a>
              </li>
              <li class="nav-item">
                <a class="nav-link" href="#">Vouchers/ Points</a>
              </li>
              <li class="nav-item">
                <a class="nav-link" href="localhost:3000">Leaderboard</a>
              </li>
              <li class="nav-item">
                <a class="nav-link" href="#">Log out</a>
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
</script>

<style lang="scss" scoped></style>
