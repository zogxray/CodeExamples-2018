<template>
  <div class="page-container">
    <md-app>
      <md-app-drawer md-permanent="full">
        <md-toolbar class="md-transparent" md-elevation="0">
          <h2>{{ 'title' | trans }}</h2>
        </md-toolbar>

        <md-list>
          <router-link :to="{ name: 'IncomingSms'}" tag="md-list-item">
            <md-icon>move_to_inbox</md-icon>
            <span class="md-list-item-text">{{ 'incomingSms' | trans }}</span>
          </router-link>

          <router-link :to="{ name: 'OutgoingSms'}" tag="md-list-item">
            <md-icon>move_to_inbox</md-icon>
            <span class="md-list-item-text">{{ 'outgoingSms' | trans }}</span>
          </router-link>

          <router-link :to="{ name: 'SendSms'}" tag="md-list-item">
            <md-icon>send</md-icon>
            <span class="md-list-item-text">{{ 'sendSms' | trans }}</span>
          </router-link>

          <router-link :to="{ name: 'Ussd'}" tag="md-list-item">
            <md-icon>move_to_inbox</md-icon>
            <span class="md-list-item-text">{{ 'ussd' | trans }}</span>
          </router-link>

          <router-link :to="{ name: 'SendUssd'}" tag="md-list-item">
            <md-icon>send</md-icon>
            <span class="md-list-item-text">{{ 'sendUssd' | trans }}</span>
          </router-link>

          <router-link :to="{ name: 'Channels'}" tag="md-list-item">
            <md-icon>menu</md-icon>
            <span class="md-list-item-text">{{ 'simCarts' | trans }}</span>
          </router-link>

          <router-link :to="{ name: 'AddChannel'}" tag="md-list-item">
            <md-icon>add</md-icon>
            <span class="md-list-item-text">{{ 'addSim' | trans }}</span>
          </router-link>

          <md-list-item v-if="$root.auth.token" tag="md-list-item" v-on:click.prevent="logout()">
            <md-icon>remove</md-icon>
            <span class="md-list-item-text">{{ 'logout' | trans }}</span>
          </md-list-item>
        </md-list>

        <md-list>
          <md-list-item tag="md-list-item" v-on:click.prevent="setLang('ru')">
            <md-icon>flag</md-icon>
            <span class="md-list-item-text">Ru</span>
          </md-list-item>
          <md-list-item tag="md-list-item" v-on:click.prevent="setLang('en')">
            <md-icon>flag</md-icon>
            <span class="md-list-item-text">En</span>
          </md-list-item>
        </md-list>

      </md-app-drawer>

      <md-app-content>
        <router-view :key="$route.fullPath"></router-view>
      </md-app-content>
    </md-app>
  </div>
</template>

<style lang="scss">
  @import '~bootstrap/scss/bootstrap';

  .md-app {
    border: 1px solid rgba(#000, .12);
  }

  // Demo purposes only
  .md-drawer {
    width: 280px;
    max-width: calc(100vw - 125px);
  }
</style>

<script>
import {trans} from './main'
export default {
  name: 'App',
  data: () => ({
    locale: 'ru',
    translations: {}
  }),
  created: function () {
    this.getLang()

    let locale = localStorage.getItem('locale')

    if (locale !== null) {
      this.locale = locale
    }
  },
  methods: {
    logout: function () {
      this.$root.auth.token = null
      this.$router.push({name: 'Login'})
    },
    getLang: function () {
      let self = this
      self.$axios.get('lang.js')
        .then(function (response) {
          self.translations = response.data
          trans.translation = self.translations[self.locale]
        })
        .catch(function (error) {
          self.error = error
        })
    },
    setLang: function (lang) {
      this.locale = lang
      trans.translation = this.translations[this.locale]
      localStorage.setItem('locale', lang)
    }
  }
}
</script>
