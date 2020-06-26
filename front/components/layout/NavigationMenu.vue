<template>
  <v-navigation-drawer
    v-model="showSideMenu"
    class="sidebarMenu"
    absolute
    temporary
  >
    <v-list dense dark>
      <!-- Back button -->

      <div class="logo-container">
        <v-icon class="logo-container__icon" @click.native="close">fa-times</v-icon>
        <span class="logo-container__text">D A I L Y I D E A</span>
        <img
          src="~/assets/images/bulb_white_on.png"
          class="logo-container__image"/>

      </div>
      <v-list-item @click.native="openPage('/ideas/all')" v-bind:class="getClass('/ideas/all')">
        <v-list-item-content>
          <v-list-item-title>All Ideas</v-list-item-title>
        </v-list-item-content>
      </v-list-item>
      <v-list-item v-if="isAuthenticated" @click.native="openPage('/ideas/me')" v-bind:class="getClass('/ideas/me')">
        <v-list-item-content>
          <v-list-item-title>My Ideas</v-list-item-title>
        </v-list-item-content>
      </v-list-item>
      <v-list-item
        v-if="isAuthenticated"
        @click.native="openPage('/ideas/liked')"
        v-bind:class="getClass('/ideas/liked')"
      >
        <v-list-item-content>
          <v-list-item-title>Saved Ideas</v-list-item-title>
        </v-list-item-content>
      </v-list-item>
      <v-list-item v-if="isAuthenticated" @click.native="openPage('/profile')" v-bind:class="getClass('/profile')">
        <v-list-item-content>
          <v-list-item-title>My Profile</v-list-item-title>
        </v-list-item-content>
      </v-list-item>
      <v-list-item v-if="isAuthenticated" @click.native="openPage('/settings')" v-bind:class="getClass('settings')">
        <v-list-item-content>
          <v-list-item-title>Settings</v-list-item-title>
        </v-list-item-content>
      </v-list-item>
      <v-list-item v-if="isAuthenticated" @click.native="signOut">
        <v-list-item-content>
          <v-list-item-title>Sign Out</v-list-item-title>
        </v-list-item-content>
      </v-list-item>
      <v-list-item
        v-if="!isAuthenticated"
        @click.native="openPage('/auth/login')"
      >
        <v-list-item-content>
          <v-list-item-title>Log In</v-list-item-title>
        </v-list-item-content>
      </v-list-item>
      <v-list-item
        v-if="!isAuthenticated"
        @click.native="openPage('/auth/signup')"
      >
        <v-list-item-content>
          <v-list-item-title>Sign Up</v-list-item-title>
        </v-list-item-content>
      </v-list-item>
    </v-list>
  </v-navigation-drawer>
</template>

<script>
export default {
  name: 'SiteNavigation',
  data() {
    return {
      showSideMenu: false
    }
  },
  computed: {
    isAuthenticated() {
      return this.$store.state.userData.isAuthenticated
    }
  },
  methods: {
    close() {
      this.showSideMenu = false
    },
    openPage(to) {
      this.showSideMenu = false
      this.$router.push(to)
    },
    signOut() {
      this.showSideMenu = false
      this.$emit('signOut')
    },
    show() {
      this.showSideMenu = true
    },
    getClass(property){
      return {
        'active': this.$router.currentRoute.path === property,
      }
    }
  }
}
</script>

<style scoped lang="scss">
.active{
  border-left: 5px solid;
}
.sidebarMenu {
  background: $primary-color !important;
  z-index: 999;

  .logo-container {
    margin-top: 3vh;
    margin-bottom: 4vh;
    padding-left: 16px;
    /*text-align: center;*/

    &__image {
      height: 20px;
      float: right;
      vertical-align: top;
      margin-right: 16px;
    }

    &__text {
      vertical-align: top;
      line-height: 22px;
      font-size: 14px;
    }

    &__icon {
      margin-right: 3px;
      /*float: right;*/
      display: inline-block;
      font-size: 21px;
      vertical-align: top;
      cursor: pointer;
    }
  }

  // display: none;
  .v-list-item {
    cursor: pointer;

    &:hover,
    &:active {
      background-color: #ffbd26 !important;
    }

    .v-list-item__content {
      margin: 10px 0 !important;
      .nuxt-link-exact-active.nuxt-link-active {
        text-decoration: none;
      }

      .v-list-item__title {
        font-size: 16px;
        line-height: normal !important;
        color: white;
      }
    }
  }
}
</style>
