<template>
  <v-footer
    v-resize="storeFooterHeight"
    :class="`active-lang-${selectedLanguageCode}`"
    padless
    class="page-footer light--text position-relative"
    ref="pageFooter"
  >
    <portal to="page-footer-language-switcher">
      <nav>
        <div class="d-flex">
          <a
            @click="updateSelectedLanguageCode('en')"
            class="page-footer--flag-link pa-1 primary--text mr-3 lang-link-en"
          >
            <img
              loading="lazy"
              class="d-block page-footer--flag"
              height="16"
              src="https://res.cloudinary.com/die9ji2vn/image/upload/v1581961735/website-static/flags/usa_iv1clb.svg"
              alt=""
            />
          </a>
          <a
            :class="{ active: selectedLanguageCode === 'kh' }"
            @click="updateSelectedLanguageCode('kh')"
            class="page-footer--flag-link pa-1 primary--text lang-link-kh"
          >
            <img
              loading="lazy"
              class="d-block page-footer--flag"
              height="16"
              src="https://res.cloudinary.com/die9ji2vn/image/upload/v1581961735/website-static/flags/khm_gdmegl.svg"
              alt=""
            />
          </a>
        </div>
      </nav>
    </portal>

    <v-container fluid class="page-footer--container-1 px-0 py-8">
      <v-container class="is-limited align-start pt-0 pb-0">
        <v-row no-gutters justify-space-between>
          <v-col cols="6" sm="4" class="mb-4 mb-sm-0">
            <h3 class="h3 mb-6">Engage</h3>

            <nav>
              <ul class="pa-0 ma-0" style="list-style:none;">
                <li class="mb-2">
                  <router-link to="/Contact" class="link">
                    Contact
                  </router-link>
                </li>
                <li class="mb-2">
                  <a href="http://asiato.asia/" rel="noopener noreferrer" target="_blank" class="link">About</a>
                </li>
                <!-- <li class="mb-2">Book Now</li> -->
                <li class="mb-2">
                  <router-link to="/search/accommodations" class="link">
                    Book Now
                  </router-link>
                </li>
              </ul>
            </nav>

            <div class="page-footer--sm-language-switcher mt-4">
              <portal-target slim name="page-footer-language-switcher"></portal-target>
            </div>
          </v-col>
          <v-col cols="6" sm="4" class="mb-4 mb-sm-0">
            <h3 class="h3 mb-6">Invest</h3>
            <nav>
              <ul class="pa-0 ma-0" style="list-style:none;">
                <li class="mb-2">
                  <router-link to="/listing/nature-city-investment-cambodia-property" class="link">
                    Lease a Property
                  </router-link>
                </li>
                <!-- <li class="mb-2">Collaborate</li> -->
                <li class="mb-2">
                  <router-link to="/listing/partner" class="link">
                    Partner
                  </router-link>
                </li>
              </ul>
            </nav>
          </v-col>
          <v-col cols="12" sm="4" class="mb-4 mb-sm-0">
            <h3 class="h3 mb-6">Learn</h3>

            <nav>
              <ul class="pa-0 ma-0" style="list-style:none;">
                <!-- <li class="mb-2">Community Outreach</li> -->
                <li class="mb-2">
                  <router-link to="/listing/our-scholarship-program" class="link">
                    Our Scholarship Program
                  </router-link>
                </li>
                <li class="mb-2">
                  <router-link to="/listing/Proudly-Powered-by-KIT-Students" class="link">
                    Proudly Powered by KIT
                  </router-link>
                </li>
                <li class="mb-2">
                  <router-link to="/search/blog" class="link">
                    Blog
                  </router-link>
                </li>
              </ul>
            </nav>

            <div class="mt-8 hidden-sm-and-up">
              <portal-target slim name="page-footer-language-switcher"></portal-target>
            </div>
          </v-col>
        </v-row>
      </v-container>
    </v-container>
    <small class="footer--version position-absolute">v{{ appVersion }}</small>
  </v-footer>
</template>

<script>
import store from '@/store';
export default {
  name: 'page-footer',
  data() {
    return {
      appVersion: JSON.parse(unescape(process.env.APP_VERSION || 0))
    };
  },
  mounted() {
    this.storeFooterHeight();
  },
  methods: {
    updateSelectedLanguageCode(languageCode) {
      this.$store.dispatch('language/updateSelectedLanguageCode', languageCode);
    },
    storeFooterHeight() {
      const pageFooter = this.$refs.pageFooter;
      this.$store.dispatch('layout/updateFooterHeight', pageFooter.$el.clientHeight);
    }
  },
  computed: {
    selectedLanguageCode() {
      return this.$store.getters['language/config'].selectedLanguageCode;
    }
  }
};
</script>

<style lang="scss">
@import '@/styles/utility.scss';
</style>

<style lang="scss" scoped>
.link {
  text-decoration: none;
  color: map-get($grey, 'lighten-1');
}
.page-footer {
  background-color: map-get($grey, 'darken-4') !important;
  z-index: $zindex-page-footer;
}
.page-footer--flag-link {
  border: 1px solid transparent;
}
.footer--version {
  bottom: rem(8px);
  right: rem(8px);
  color: map-get($grey, 'darken-3');
  line-height: 1;
}
.page-footer--sm-language-switcher {
  @include media-breakpoint-down(xs) {
    display: none;
  }
}
.page-footer--flag {
  border-radius: $border-radius-root;
}
.active-lang-en {
  .lang-link-en {
    border: 1px solid;
    border-radius: $border-radius-root;
  }
}
.active-lang-kh {
  .lang-link-kh {
    border: 1px solid;
    border-radius: $border-radius-root;
  }
}
</style>
