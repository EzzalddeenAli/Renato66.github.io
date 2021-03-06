<template>
  <v-container>
    <v-layout mt-4 style="height:44px">
      <v-flex shrink>
        <v-menu offset-y content-class="br-8">
          <template v-slot:activator="{ on }">
            <v-btn
              aria-label="Menu"
              icon
              :outlined="!followScroll"
              :color="followScroll ? 'black' : 'white'"
              large
              :class="{'follow-scroll': followScroll}"
              class="ml-0"
              v-on="on"
            >
              <v-icon>
                mdi-menu
              </v-icon>
            </v-btn>
          </template>
          <v-list dense>
            <v-list-item
              v-for="item in menu"
              :key="item"
              @click="$vuetify.goTo(`#${item}`)"
            >
              <v-list-item-title>{{ $t(`menu.${item}`) }}</v-list-item-title>
            </v-list-item>
          </v-list>
        </v-menu>
      </v-flex>
      <v-spacer />
      <v-flex shrink>
        <LocaleChange />
      </v-flex>
    </v-layout>
    <v-layout class="mt-12">
      <v-flex class="text-center">
        <v-lazy
            :options="{
              threshold: .5
            }"
            min-height="236"
            transition="fade-transition"
          >
        <v-avatar size="236">
          <v-img
            :src="`https://images.weserv.nl/?url=${contact.avatar}%3Fsize=236&output=${imageOutput}`"
            :lazy-src="`https://images.weserv.nl/?url=${contact.avatar}%3Fsize=236&q=10&output=webp`"
            :alt="contact.shortName"
            @error="SET_WEBP_SUPPORT(false)"
          />
        </v-avatar>
        </v-lazy>
      </v-flex>
    </v-layout>
    <v-layout class="mt-6 pb-6" row wrap>
      <v-flex xs12 class="text-center">
        <h1 class="font-weight-thin" :class="{'display-4': !mobile, 'display-2': mobile}">
          {{ contact.shortName }}
        </h1>
      </v-flex>
      <v-flex xs12 class="text-center mt-4">
        <h2 :class="{'display-1': !mobile, 'headline': mobile}">
          {{ $t('description') }}
        </h2>
      </v-flex>
      <v-flex class="text-center mt-4">
        <h3 class="grey--text font-weight-light">
          {{ contact.city }} - {{ contact.birthday | age }} {{ $t('age') }} {{ $t('aditional') }}
        </h3>
      </v-flex>
    </v-layout>
    <v-layout align-center>
      <v-spacer />
      <v-flex shrink class="text-center floating-btn">
        <v-dialog width="320" content-class="br-8">
          <template v-slot:activator="{ on }">
            <v-btn
              :aria-label="$t('getInTouch')"
              large
              rounded
              class="text-none"
              color="primary"
              v-on="on"
            >
              {{ $t('getInTouch') }}
            </v-btn>
          </template>
          <v-card>
            <v-card-title
              class="headline primary py-6"
              primary-title
            >
              <v-spacer />
              <span class="text-uppercase">
                {{ $t('getInTouch') }}
              </span>
              <v-spacer />
            </v-card-title>

            <v-card-text style="background: #424242">
              <v-container grid-list-md pa-0>
                <v-layout align-center pt-4>
                  <v-flex shrink>
                    <v-icon small color="grey">
                      mdi-phone
                    </v-icon>
                  </v-flex>
                  <v-flex>
                    {{ $t('phone') }}:
                  </v-flex>
                  <v-spacer />
                  <v-flex shrink>
                    +{{ contact.phone.country }}
                    ({{ contact.phone.area }})
                    {{ contact.phone.number }}
                  </v-flex>
                </v-layout>
                <v-layout align-center>
                  <v-flex shrink>
                    <v-icon small color="grey">
                      mdi-mail
                    </v-icon>
                  </v-flex>
                  <v-flex>
                    {{ $t('email') }}:
                  </v-flex>
                  <v-spacer />
                  <v-flex shrink>
                    {{ contact.mail }}
                  </v-flex>
                </v-layout>
                <v-layout align-center>
                  <v-flex shrink>
                    <v-icon small color="grey">
                      mdi-telegram
                    </v-icon>
                  </v-flex>
                  <v-flex>
                    Telegram:
                  </v-flex>
                  <v-spacer />
                  <v-flex shrink>
                    @{{ contact.telegram }}
                  </v-flex>
                </v-layout>
                <v-layout pt-4>
                  <v-flex>
                    <v-divider />
                  </v-flex>
                </v-layout>
                <v-layout>
                  <v-spacer />
                  <v-flex v-for="network in socialNetworks" :key="network.name" shrink>
                    <v-btn icon :href="network.url" rel="noopener" target="_blank" :aria-label="`Open ${network.name}`">
                      <v-icon>
                        {{ network.icon }}
                      </v-icon>
                    </v-btn>
                  </v-flex>
                  <v-spacer />
                </v-layout>
                <v-layout>
                  <v-flex>
                    <v-divider />
                  </v-flex>
                </v-layout>
                <v-layout pt-2>
                  <v-spacer />
                  <v-flex shrink>
                    <v-btn aria-label="Open WhatsApp" rounded class="text-none" color="#25d366">
                      WhatsApp
                      <v-icon class="ml-2">
                        mdi-whatsapp
                      </v-icon>
                    </v-btn>
                  </v-flex>
                  <v-spacer />
                </v-layout>
              </v-container>
            </v-card-text>
          </v-card>
        </v-dialog>
      </v-flex>
      <v-spacer />
    </v-layout>
  </v-container>
</template>

<script>
import { mapMutations, mapGetters } from 'vuex'
import LocaleChange from '@/components/LocaleChange'
export default {
  components: {
    LocaleChange
  },
  filters: {
    age (value) {
      const currentDate = new Date()
      const age = currentDate.getFullYear() - value.getFullYear()
      const month = currentDate.getMonth() - value.getMonth()
      const day = currentDate.getDate() - value.getDate()
      return (month < 0 || month === 0) && day < 0 ? age - 1 : age
    }
  },
  props: {
    socialNetworks: {
      type: Array,
      default: () => {
        return []
      }
    },
    contact: {
      type: Object,
      default: () => {
        return {}
      }
    },
    menu: {
      type: Array,
      default: () => {
        return []
      }
    }
  },
  data () {
    return {
      mobile: false,
      followScroll: false
    }
  },
  computed: {
    ...mapGetters(['imageOutput']),
    phone () {
      return `${this.contact.phone.country}${this.contact.phone.area}${this.contact.phone.number}`.replace(/[^0-9]/g, '')
    }
  },
  beforeMount () {
    if (process.browser) {
      this.mobile = this.$vuetify.breakpoint.xsOnly
      window.addEventListener('scroll', this.handleScroll)
    }
  },
  beforeDestroy () {
    if (process.browser) {
      window.removeEventListener('scroll', this.handleScroll)
    }
  },
  methods: {
    ...mapMutations(['SET_WEBP_SUPPORT']),
    handleScroll () {
      if (process.browser) {
        this.followScroll = window.scrollY > 400
      }
    },
    openWhats () {
      if (process.browser) {
        if (navigator.userAgent.match(/Android/i) ||
          navigator.userAgent.match(/webOS/i) ||
          navigator.userAgent.match(/iPhone/i) ||
          navigator.userAgent.match(/iPad/i) ||
          navigator.userAgent.match(/iPod/i) ||
          navigator.userAgent.match(/BlackBerry/i) ||
          navigator.userAgent.match(/Windows Phone/i)) {
          window.open(`https://api.whatsapp.com/send?phone=${this.phone}`, '_blank')
        } else {
          window.open(`https://web.whatsapp.com/send?phone=${this.phone}`, '_blank')
        }
      }
    }
  }
}
</script>

<style>
.floating-btn {
  position: relative;
  top: 45px;
}
.follow-scroll {
  position: fixed!important;
  z-index: 2;
  /* transition: .3s cubic-bezier(.25,.8,.5,1),color 1ms; */
  transform: rotate(360deg)
}
</style>
