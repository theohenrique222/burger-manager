<template>
  <nav class="bg-neutral-900 border-b-2 border-neutral-600">
    <div id="nav" class="lg:max-w-7xl max-w-96 m-auto flex items-center justify-between">
      <router-link
        class="flex items-center font-bold space-x-1"
        to="/"
        id="logo-url"
        aria-label="Home"
      >
        <img :src="logo" :alt="alt" id="logo" />
        <span class="text-base">Burger Manager</span>
      </router-link>
      <div class="relative inline-block" ref="dropdown">
        <button
          @click="toggleDropdown"
          class="text-yellow-500 hover:text-yellow-100 text-xl border-none cursor-pointer flex items-center gap-2"
          aria-label="Menu"
        >
          <span class="flex text-4xl">
            <ion-icon name="menu-outline"></ion-icon>
          </span>
        </button>
        <div
          v-show="isDropdownOpen"
          class="absolute bg-neutral-900 shadow-lg z-10 mt-2 right-px rounded-md min-w-[160px] max-h-[200px] overflow-y-auto"
          role="menu"
        >
          <router-link
            v-for="(item, index) in menuItems"
            :key="index"
            :to="item.id"
            @click="closeDropdown"
            class="flex items-center p-4 text-neutral-100 no-underline hover:bg-neutral-600 transition-colors duration-300 border-b-2 border-neutral-600"
            role="menuitem"
          >
            <ion-icon :name="item.icon" class="mr-2"></ion-icon>
            {{ item.label }}
          </router-link>
        </div>
      </div>
    </div>
  </nav>
</template>

<style>
#logo {
  width: 60px;
  height: 60px;
}
#nav a {
  color: #fcba03;
  text-decoration: none;
  margin: 12px;
  transition: 0.5s;
}
#nav a:hover {
  color: #fff;
}
@media (max-width: 450px) {
  #nav a {
    font-size: 0.9rem;
  }
  #logo {
    width: 40px;
    height: 40px;
  }
}
</style>

<script>
export default {
  name: "Navbar",
  props: ["logo", "alt"],
  data() {
    return {
      isDropdownOpen: false,
      menuItems: [
        { id: "/", icon: "home-outline", label: "Home" },
        { id: "/solicitations", icon: "cart-outline", label: "Pedidos" },
        // Adicione outros itens aqui
      ],
    };
  },
  methods: {
    toggleDropdown() {
      this.isDropdownOpen = !this.isDropdownOpen;
    },
    closeDropdown() {
      this.isDropdownOpen = false;
    },
    handleClickOutside(event) {
      if (this.$refs.dropdown && !this.$refs.dropdown.contains(event.target)) {
        this.closeDropdown();
      }
    },
  },
  mounted() {
    document.addEventListener("click", this.handleClickOutside);
  },
  beforeDestroy() {
    document.removeEventListener("click", this.handleClickOutside);
  },
};
</script>

