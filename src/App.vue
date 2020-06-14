<template>
  <div class="min-h-screen">
    <nav class="navbar">
      <ul>
        <li v-for="(city, idx) in cities" :key="idx">
          <a ref="navItem" href="#" @click="selectedCity = city">{{
            city.label
          }}</a>
        </li>
      </ul>
    </nav>

    <div class="underline" ref="underline"></div>

    <div v-if="selectedCity" class="current-time">{{ currentTime }}</div>
  </div>
</template>

<script>
import { cities } from "@/navigation";

export default {
  data: () => ({
    cities,
    selectedCity: null,
  }),
  computed: {
    currentTime() {
      if (!this.selectedCity) return;
      const { timeZone } = this.selectedCity;

      return new Date().toLocaleString("en-US", { timeZone });
    },
  },
  mounted() {
    const underline = this.$refs.underline;
    const navItems = this.$refs.navItem;

    function moveUnderline() {
      if (!this.parentNode.classList.contains("active")) {
        for (let i = 0; i < navItems.length; i++) {
          if (navItems[i].parentNode.classList.contains("active")) {
            navItems[i].parentNode.classList.remove("active");
          }
        }

        this.parentNode.classList.add("active");

        const width = this.getBoundingClientRect().width;
        const height = this.getBoundingClientRect().height;
        const left = this.getBoundingClientRect().left;
        const top = this.getBoundingClientRect().top;

        underline.style.width = `${width}px`;
        underline.style.height = `${height}px`;
        underline.style.left = `${left}px`;
        underline.style.top = `${top}px`;
      }
    }

    for (let i = 0; i < navItems.length; i++) {
      navItems[i].addEventListener("click", moveUnderline);
    }

    window.addEventListener("resize", this.resizeWindow);
  },
  methods: {
    resizeWindow() {
      const underline = this.$refs.underline;
      const active = document.querySelector(".navbar li.active");

      if (active) {
        const left = active.getBoundingClientRect().left + window.pageXOffset;
        const top = active.getBoundingClientRect().top + window.pageYOffset;

        underline.style.left = `${left}px`;
        underline.style.top = `${top}px`;
      }
    },
  },
};
</script>

<style lang="scss">
.navbar ul {
  display: flex;
  justify-content: center;
  flex-wrap: wrap;
  list-style-type: none;
  padding: 0;
  margin-top: 8px;
  border-bottom: 1px solid lightgray;

  a {
    display: block;
    color: gray;
    text-decoration: none;
    padding: 10px 15px;

    &:hover {
      color: cornflowerblue;
    }
  }

  li.active {
    a {
      color: black;
    }
  }
}

.underline {
  position: absolute;
  border-bottom: 1px solid black;
  transition: all 0.25s ease-in-out;
}

.current-time {
  text-align: center;
  margin-top: 20px;
  font-size: 20px;
}
</style>
