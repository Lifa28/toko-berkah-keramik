<!-- landing page -->

<script setup lang="ts">
import { ref } from "vue";
import AppHeader from "~/components/AppHeader.vue";
import AppHero from "~/components/AppHero.vue";
import CategorySection from "~/components/CategorySection.vue";
import FlooringSection from "~/components/FlooringSection.vue";
import CatalogSection from "~/components/CatalogSection.vue";
import BrandSection from "~/components/BrandSection.vue";
import StoreInfoSection from "~/components/StoreInfoSection.vue";
import ProductDetailView from "~/components/ProductDetailView.vue";
import BrandProductListView from "~/components/BrandProductListView.vue";
import AppFooter from "~/components/AppFooter.vue"; 
import SubcategoryTab from "~/components/SubcategoryTab.vue"; 
import FlooringProductListingView from "~/components/FlooringProductListingView.vue";
import AdminView from "~/components/AdminView.vue";

const selectedProduct = ref<any | null>(null);
const selectedBrand = ref<string | null>(null);
const selectedCategory = ref<number | null>(null); // ✅ TAMBAHAN
const showFlooringListing = ref(false);
const isAdminView = ref(false);

const handleSelectProduct = (product: any) => {
  selectedProduct.value = product;
  window.scrollTo(0, 0);
};

const handleBrandSelect = (brand: string) => {
  selectedBrand.value = brand;
  window.scrollTo(0, 0);
};

const handleBackToLanding = () => {
  selectedProduct.value = null;
  selectedBrand.value = null;
  showFlooringListing.value = false;
  window.scrollTo(0, 0);
};

const handleViewAllFlooring = () => {
  showFlooringListing.value = true;
  window.scrollTo(0, 0);
};

const handleEnterAdmin = () => {
  isAdminView.value = true;
  window.scrollTo(0, 0);
};

const handleExitAdmin = () => {
  isAdminView.value = false;
  window.scrollTo(0, 0);
};
</script>

<template>
  <div class="app-layout" :class="{ 'is-admin': isAdminView }">
    <AppHeader v-if="!isAdminView" />

    <main>
      <div v-if="!selectedProduct && !selectedBrand && !showFlooringListing && !selectedCategory">
        <AppHero />
        <CatalogSection @selectProduct="handleSelectProduct" />
        <CategorySection @category-selected="selectedCategory = $event" />
        <FlooringSection @viewAll="handleViewAllFlooring" />
        <BrandSection @brand-selected="handleBrandSelect" />
        <StoreInfoSection />
      </div>

      <div v-else-if="showFlooringListing">
        <FlooringProductListingView @back="handleBackToLanding" />
      </div>

      <div v-else-if="selectedProduct">
        <ProductDetailView
          :productPreview="selectedProduct"
          @back="handleBackToLanding"
        />
      </div>

      <div v-else-if="selectedCategory">
        <SubcategoryTab
          :categoryId="selectedCategory"
          @back="handleBackToLanding"
        />
      </div>

      <div v-else-if="selectedBrand">
        <BrandProductListView
          :brand="selectedBrand"
          @back="handleBackToLanding"
        />
      </div>

      <div v-else-if="isAdminView">
        <AdminView @exit="handleExitAdmin" />
      </div>
    </main>

  </div>
</template>

<style scoped>
.admin-toggle-btn {
  position: fixed;
  bottom: 20px;
  right: 20px;
  width: 50px;
  height: 50px;
  border-radius: 50%;
  background: white;
  border: 1px solid #eee;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
  cursor: pointer;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 24px;
  z-index: 9999;
  transition: all 0.3s ease;
}

.admin-toggle-btn:hover {
  transform: scale(1.1);
  box-shadow: 0 6px 16px rgba(0, 0, 0, 0.15);
}

.is-admin {
  background-color: #f8fafc;
  min-height: 100vh;
}
</style>

<style>
@import url("https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@300;400;500;600;700;800&display=swap");

:root {
  --primary-blue: #236d90;
  --primary-red: #ee2d24;
  --primary-yellow: #ffcc00;
  --bg-light: #f8fafc;
  --text-dark: #1e293b;
  --text-muted: #64748b;
  --accent-blue: #2563eb;
}

* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  font-family: "Plus Jakarta Sans", sans-serif;
  color: var(--text-dark);
  background-color: white;
  -webkit-font-smoothing: antialiased;
}

.app-layout {
  display: flex;
  flex-direction: column;
  min-height: 100vh;
}

main {
  flex: 1;
}

/* Global utility classes */
.container {
  max-width: 1600px;
  margin: 0 auto;
  padding: 0 1rem;
}

/* Custom scrollbar */
::-webkit-scrollbar {
  width: 8px;
}

::-webkit-scrollbar-track {
  background: #f1f5f9;
}

::-webkit-scrollbar-thumb {
  background: #cbd5e1;
  border-radius: 4px;
}

::-webkit-scrollbar-thumb:hover {
  background: #94a3b8;
}
</style>
