<template>
  <v-container grid-list-md>
    <v-layout row wrap align-center>
      <v-spacer class="hidden-sm-and-up" />
      <v-flex v-for="(social, index) in socialNetworks" :key="index" shrink style="min-width:86px;">
        <a class="social-icon" rel="noopener" target="_blank" :href="social.url" :aria-label="`Open ${social.name}`">
          <v-lazy
            :options="{
              threshold: .5
            }"
            min-height="40"
            transition="fade-transition"
          >
            <v-img
              width="40"
              height="40"
              class="mx-auto"
              :src="`https://images.weserv.nl/?url=${baseUrl + social.img}.webp&output=${imageOutput}`"
              :lazy-src="`https://images.weserv.nl/?url=${baseUrl + social.img}.webp&q=10&output=webp`"
              :alt="social.name"
            />
          </v-lazy>
        </a>
      </v-flex>
      <v-spacer />
      <v-flex xs12 sm6 class="text-center text-sm-right py-6">
        <v-btn
          download="Renato-CV.pdf"
          aria-label="download Renato-CV.pdf"
          rounded
          large
          color="white"
          light
          class="text-none"
          @click="download"
        >
          {{ $t('downloadResume') }}
        </v-btn>
      </v-flex>
    </v-layout>
  </v-container>
</template>

<script>
import { mapGetters } from 'vuex'

export default {
  props: {
    socialNetworks: {
      type: Array,
      default: () => {
        return []
      }
    }
  },
  computed: {
    ...mapGetters(['baseUrl', 'imageOutput'])
  },
  methods: {
    download () {
      this.$ga.event('resume', 'download', 'bar')
      this.$emit('print-resume')
    }
  }
}
</script>

<style>
.social-icon {
  display: block;
  padding: 18px;
  border: 1px solid #fff;
  border-radius: 100%;
  transition: all .3s;
}
.social-icon:hover {
  border-color: #2D373D;
  background-color: #2D373D;
}
.social-icon img {
  width: 40px;
}
</style>
