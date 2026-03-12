<script setup lang="ts">
import { navigateTo } from "#app";
import { ref, onMounted } from "vue";
import {
  Search,
  User,
  Heart,
  ShoppingBag,
  MapPin,
  Menu,
  ChevronDown,
} from "lucide-vue-next";

const isLogin = ref(false);
const userEmail = ref("");
const userName = ref("");

const checkLogin = () => {
  if (typeof window !== "undefined") {
    const token = localStorage.getItem("token");
    isLogin.value = !!token;
    if (isLogin.value) {
      userName.value = localStorage.getItem("userName") || "User";
    }
  }
};

onMounted(() => {
  checkLogin();
});

const goLogin = () => {
  checkLogin();
  if (isLogin.value) {
    navigateTo("/admin");
  } else {
    navigateTo("/login");
  }
};

const logout = () => {
  localStorage.removeItem("token");
  localStorage.removeItem("userName");
  checkLogin();
  navigateTo("/login");
};

const navLinks = [
  { name: "Beranda", href: "#" },
  { name: "Shop by Brands", href: "#" },
  { name: "Shop by Rooms", href: "#" },
  { name: "House Brand Berkah Keramik", href: "#" },
];
</script>

<template>
  <header class="main-header">
    <!-- Top Bar -->
    <div class="top-bar">
      <div class="container container-flex">
        <div class="top-info">
          <div class="info-item">
            <span class="flag-icon">🇮🇩</span>
            <span>Indonesia</span>
            <ChevronDown :size="20" />
          </div>
        </div>
        <div class="top-links">
          <a href="#">Membership</a>
          <a href="#">Tentang Kami</a>
          <a href="#">Bantuan</a>
          <div class="info-item">
            <MapPin :size="16" />
            <span>Lokasi Toko</span>
          </div>
        </div>
      </div>
    </div>

    <!-- Middle Header -->
    <div class="middle-header">
      <div class="container container-flex">
        <div class="logo">
          <img src="/LOGO.png" alt="Logo" width="70" height="70"/>
        </div>

        <div class="search-container">
          <input
            type="text"
            placeholder="Cari produk dengan kata kunci, model atau SKU"
          />
          <button class="search-btn">
            <Search :size="20" />
          </button>
        </div>

        <div class="header-actions">
          <!-- LOGIN -->
          <div class="action-item" @click="goLogin" v-if="!isLogin">
            <User :size="24" />
            <div class="action-text">
              <span class="small-label">Masuk / Daftar</span>
            </div>
          </div>

          <!-- LOGGED IN (Show Email) -->
          <div class="action-item" v-else @click="logout" title="Klik untuk Logout">
            <User :size="24" />
            <div class="action-text">
              <span class="small-label">{{ userName }}</span>
            </div>
          </div>

          <div class="action-item">
            <Heart :size="24" />
            <div class="action-text">
              <span class="small-label">Favorit</span>
            </div>
          </div>

          <div class="action-item cart">
            <ShoppingBag :size="24" />
            <div class="action-text">
              <span class="small-label">Keranjang</span>
            </div>
          </div>
        </div>
      </div>
    </div>

    <!-- Bottom Nav -->
    <nav class="bottom-nav">
      <div class="container container-flex">
        <div class="all-categories-btn">
          <Menu :size="20" />
          <span>Semua Kategori</span>
        </div>
        <ul class="nav-menu">
          <li v-for="link in navLinks" :key="link.name">
            <a :href="link.href">{{ link.name }}</a>
          </li>
        </ul>
      </div>
    </nav>
  </header>
</template>

<style scoped>
.container {
  max-width: 1600px;
  margin: 0 auto;
  padding: 0 1rem;
}

.container-flex {
  display: flex;
  align-items: center;
  justify-content: space-between;
}

.main-header {
  width: 100%;
  background: white;
}

/* Top Bar */
.top-bar {
  background-color: #1e3a8a;
  color: white;
  padding: 6px 0;
  font-size: 0.75rem;
}

.top-links {
  display: flex;
  gap: 1.5rem;
  align-items: center;
}

.top-links a {
  color: white;
  text-decoration: none;
}

.info-item {
  display: flex;
  align-items: center;
  gap: 0.25rem;
}

/* Middle Header */
.middle-header {
  padding: 0.75rem 0;
  border-bottom: 1px solid #f0f0f0;
}

.brand-red {
  color: red;
  font-size: 1.5rem;
  font-weight: 900;
}

.brand-blue {
  color: blue;
  font-size: 1.5rem;
  font-weight: 900;
  margin-left: 4px;
}

.sub-logo {
  font-size: 0.6rem;
  font-weight: 700;
  color: blue;
}

.search-container {
  flex: 1;
  max-width: 600px;
  margin: 0 2rem;
  position: relative;
  display: flex;
}

.search-container input {
  width: 100%;
  padding: 0.75rem 1rem;
  border: 1px solid #e0e0e0;
  border-radius: 2rem;
}

.search-btn {
  position: absolute;
  right: 5px;
  top: 50%;
  transform: translateY(-50%);
  background: none;
  border: none;
  cursor: pointer;
}

.header-actions {
  display: flex;
  gap: 1.5rem;
}

.action-item {
  display: flex;
  flex-direction: column;
  align-items: center;
  cursor: pointer;
}

.small-label {
  font-size: 0.7rem;
  font-weight: 600;
  margin-top: 4px;
}

/* Bottom Nav */
.bottom-nav {
  background: white;
  border-bottom: 1px solid #f0f0f0;
}

.all-categories-btn {
  background: #ffcc00;
  padding: 0.75rem 1.5rem;
  display: flex;
  align-items: center;
  gap: 1rem;
  font-weight: 700;
  cursor: pointer;
}

.nav-menu {
  display: flex;
  list-style: none;
  gap: 1.5rem;
  flex: 1;
  margin-left: 2rem;
}

.nav-menu a {
  text-decoration: none;
  color: #333;
  font-weight: 600;
}

.nav-menu a:hover {
  color: blue;
}
</style>
