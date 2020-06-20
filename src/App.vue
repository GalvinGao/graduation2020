<template>
  <v-app>
    <v-content class="transparent">
      <div class="locale-switcher">
        <LocaleSwitcher />
      </div>
      <Background style="position: fixed;" />

      <v-container class="transparent">
        <v-row align="start" justify="center">
          <v-col cols="12">
            <v-img
              position="center center"
              src="https://memories.upyun.galvincdn.com/kinglee-logo.png"
              aspect-ratio="1"
              height="64px"
              contain
              class="my-6"
            />

            <h2 class="overline font-weight-bold text-center z-index-fix">
              {{ $t('app.name') }}
            </h2>
            <h1 class="heading text-center z-index-fix">
              {{ $t('app.title') }}
            </h1>

            <v-alert border="left" type="info" color="blue lighten-2" class="mt-4 mb-2" outlined>
              {{ $t('app.notice') }}
            </v-alert>
          </v-col>

          <v-col
            v-for="(project, i) in projects"
            :key="i"
            cols="12"
            :md="project.featured ? 12 : 6"
          >
            <v-card hover :href="(project.hide || project.lock) ? (beforeValid ? project.url.href : '') : project.url.href" v-if="!project.hide || (project.hide && beforeValid)" class="card-background">
              <v-overlay :opacity="1" absolute v-if="project.lock && !beforeValid" z-index="10" class="slash-strip">
                <v-row
                  align="center"
                  justify="center"
                  class="fill-height text-center"
                >
                  <v-icon
                    large
                    class="mb-3"
                  >
                    mdi-lock
                  </v-icon>

                  <span class="title mx-6">
                    {{$t('invalidTime')}}
                  </span>
                </v-row>
              </v-overlay>
              <v-img
                class="white--text align-center text-center"
                :height="picHeight"
                :src="`https://memories.upyun.galvincdn.com/projects/${project.id}.jpg`"
                gradient="to top right, rgba(100,115,201,.33), rgba(25,32,72,.0)"
              />
              <v-divider/>
              <v-card-title
                class="darken-1"
              >
                <v-icon left>
                  {{ project.url.type === "bilibili" ? "mdi-video" : "mdi-earth" }}
                </v-icon>
                {{ $t(['projects', project.id, 'title'].join('.')) }}
                <v-icon v-if="project.featured" right>
                  mdi-alert-decagram
                </v-icon>
              </v-card-title>
              <v-card-subtitle
                class="darken-1"
              >
                {{ $t(['projects', project.id, 'subtitle'].join('.')) }}
              </v-card-subtitle>
              <v-card-text
                class="darken-1 text-right"
              >
                {{ $t('credits') }}
                {{ $t(['projects', project.id, 'credits'].join('.')) }}
              </v-card-text>
            </v-card>
            <v-card v-else height="256px">
              <v-row
                align="center"
                justify="center"
                class="fill-height text-center"
              >
                <v-icon
                  large
                >
                  mdi-lock
                </v-icon>
              </v-row>

            </v-card>

            <v-divider v-if="project.featured" class="mt-6" />
          </v-col>
        </v-row>
      </v-container>
    </v-content>

    <v-footer color="transparent" class="white--text text-center">
      <v-card-text class="white--text">
        <strong>Kinglee × Galvin Gao</strong>
        —
        <strong>{{ year }}</strong>
      </v-card-text>
    </v-footer>
  </v-app>
</template>

<script>

import strings from "./utils/strings";
import I18n from "./mixins/I18n";
import LocaleSwitcher from "./components/LocaleSwitcher";
import Background from "./components/Background";

export default {
  name: 'App',
  components: {Background, LocaleSwitcher},
  mixins: [I18n],

  data: () => ({
    projects: [
      {
        id: "album",
        url: {
          type: "link",
          href: "https://memories.galvincdn.com"
        },
        featured: true,
        lock: false
      },
      {
        id: "fun",
        url: {
          type: "bilibili",
          href: "https://www.bilibili.com/video/BV1Pz4y1R7Bc/"
        },
        lock: true
      },
      {
        id: "mv",
        url: {
          type: "bilibili",
          href: "https://www.bilibili.com/video/BV1PT4y1J7cV/"
        },
        lock: true
      },
      {
        id: "bye",
        url: {
          type: "bilibili",
          href: "https://www.bilibili.com/video/BV1nz411i7RK/"
        },
        lock: true
      },
      {
        id: "documentation",
        url: {
          type: "bilibili",
          href: "https://www.bilibili.com/video/BV13V411k7fU/"
        },
        lock: false,
        hide: true
      },
    ],
    beforeValid: Date.now() > 1592618400000
  }),

  created () {
    const language = strings.getFirstBrowserLanguage();
    console.info("i18n", "detected language", language);
    if (language) this.changeLocale(language)

    setInterval(() => {
      if (Date.now() > 1592618400000 && this.beforeValid === false) window.location.reload()
    }, 15000)
  },

  computed: {
    year() {
      return new Date().getFullYear();
    },
    picHeight () {
      return this.$vuetify.breakpoint.xs ? "256px" : "316px"
    }
  },
}
</script>

<style>
  .monospace {
    font-family: "Roboto Mono", "Fira Code", "Roboto", sans-serif;
    font-weight: 500;
  }
  .monospace .normal {
    font-size: 20px
  }
  .monospace .emphasize {
    letter-spacing: -.15rem;
    text-shadow: 0 0 10px rgba(0, 0, 0, .5);
  }
  .monospace .wide {
    letter-spacing: 0 !important;
  }

  .locale-switcher {
    position: fixed !important;
    right: 1em !important;
    top: 1em !important;
    z-index: 99999999;
  }

  .z-index-fix {
    z-index: 5 !important;
  }

  .v-application, .v-application--wrap {
    background: transparent !important;
  }

  .card-background {
    background: rgba(20, 20, 20, .9) !important;
  }

  .theme--dark.v-sheet.v-alert {
    background: rgba(0, 0, 0, .7) !important;
  }

  .slash-strip {
    background: repeating-linear-gradient(
      -45deg,
      rgba(237, 144, 53, 0.5),
      rgba(237, 144, 53, 0.5) 45px,
      rgba(0, 0, 0, 0.5) 45px,
      rgba(0, 0, 0, 0.5) 90px
    );
  }
</style>
