<template>
  <div class="nav-wrapper">
    <header class="header">
      <nav class="nav">
        <g-link class="nav-link" :class="isCurrentRoute('/')? 'underlined' : ''" to="/">Home</g-link>
        <g-link
          class="nav-link"
          :class="isCurrentRoute('/products/')? 'underlined' : ''"
          to="/products/"
        >Products</g-link>
      </nav>
      <nav class="nav snipcart-summary">
        <g-link class="nav-link snipcart-customer-signin" href="#">
          <ProfileLogo />
        </g-link>
        <g-link class="nav-link snipcart-checkout">
          <CartLogo />
          <span class="snipcart-total-price">{{this.totalPrice | formatMoney}}</span>
        </g-link>
      </nav>
    </header>
    <div>
      <slot />
    </div>
  </div>
</template>

<static-query>
query {
  metadata {
    siteName
  }
}
</static-query>

<style lang="scss">
@import "~/theme/_main.scss";

html,
body {
  background: $primary-color;
  background: $background-gradient;
  background-attachment: fixed;
  background-repeat: no-repeat;
  background-size: cover;
  height: 100%;
  width: 100%;
  margin: 0;
  padding: 0;
  line-height: 1.5;
}

.nav-wrapper {
  height: 100%;
  margin: 0 auto;
  padding-left: 20px;
  padding-right: 20px;
}

.header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 4%;
}

.nav {
  margin: 26px 0;
}

.nav-link {
  color: $custom-white;
  font-family: $primary-font;
  font-style: normal;
  font-size: 17px;
  font-weight: $normal-font-weight;
  line-height: 36px;
  margin: 0 34px;
  text-decoration: none;
}

.nav-link:hover {
  color: $accent-color;
  fill: $accent-color;
}

.underlined {
  border-bottom: 4px solid $accent-color;
}

@media only screen and (max-width: 768px) {
  .nav-link {
    margin: 17px;

    .snipcart-total-price {
      display: none;
    }
  }
}
</style>

<script>
import CartLogo from "~/assets/cart.svg";
import ProfileLogo from "~/assets/profile.svg";

export default {
  components: {
    CartLogo,
    ProfileLogo
  },
  data: function() {
    return {
      currentRoute: "",
      totalPrice: 0
    };
  },
  methods: {
    getTotalPrice: function() {
       return Snipcart.store.getState().cart.total;
    },
    isCurrentRoute: function(route) {
      return this.$route.fullPath === route;
    }
  },
  created: function() {
    this.activeTab = this.$route;
  },
  mounted: function() {
    this.totalPrice = this.getTotalPrice();
  }
};
</script>