<template>
<v-main>
  <!-- Carousel -->
  <v-carousel cycle hide-delimiter-background
    show-arrows-on-hover>
    <v-carousel-item
      v-for="(item,i) in items"
      :key="i"
      :src="item.src"
      reverse-transition="fade-transition"
      transition="fade-transition"
    ></v-carousel-item>
  </v-carousel>
  
  <v-divider></v-divider>

  <!-- Timeline -->
  <h5 class="text-h5" font-weight-medium mb-5>Sejarah Kita</h5>
  <v-timeline>
    <v-timeline-item
      small
    >
      <template v-slot:opposite>
        <span
          :class="`headline font-weight-bold cyan--text`"
          v-text="1999"
        ></span>
      </template>
      <div class="py-4">
        <h2 :class="`headline font-weight-light mb-4 cyan--text`">
          Halo
        </h2>
        <div>
          aada
        </div>
      </div>
    </v-timeline-item>

    <v-timeline-item
      small
    >
      <template v-slot:opposite>
        <span
          :class="`headline font-weight-bold green--text`"
          v-text="1999"
        ></span>
      </template>
      <div class="py-4">
        <h2 :class="`headline font-weight-light mb-4 green--text`">
          Halo
        </h2>
        <div>
          aada
        </div>
      </div>
    </v-timeline-item>

    <v-timeline-item
      small
    >
      <template v-slot:opposite>
        <span
          :class="`headline font-weight-bold pink--text`"
          v-text="1999"
        ></span>
      </template>
      <div class="py-4">
        <h2 :class="`headline font-weight-light mb-4 pink--text`">
          Halo
        </h2>
        <div>
          aada
        </div>
      </div>
    </v-timeline-item>

    <v-timeline-item
      small
    >
      <template v-slot:opposite>
        <span
          :class="`headline font-weight-bold amber--text`"
          v-text="1999"
        ></span>
      </template>
      <div class="py-4">
        <h2 :class="`headline font-weight-light mb-4 amber--text`">
          Halo
        </h2>
        <div>
          aada
        </div>
      </div>
    </v-timeline-item>

    <v-timeline-item
      small
    >
      <template v-slot:opposite>
        <span
          :class="`headline font-weight-bold orange--text`"
          v-text="1999"
        ></span>
      </template>
      <div class="py-4">
        <h2 :class="`headline font-weight-light mb-4 orange--text`">
          Halo
        </h2>
        <div>
          aada
        </div>
      </div>
    </v-timeline-item>
  </v-timeline>
  
  <v-divider></v-divider>

  <!-- Footer -->
  <v-footer
      padless
    >
      <v-card
        flat
        tile
        width="100%"
        class="lighten-1 text-center"
        dark
      >
        <v-card-text>
          <v-btn
            v-for="icon in icons"
            :key="icon"
            class="mx-4"
            icon
          >
            <v-icon size="24px">
              {{ icon }}
            </v-icon>
          </v-btn>
        </v-card-text>

        <v-divider></v-divider>

        <v-card-text class="white--text">
          {{ new Date().getFullYear() }} — <strong>Vuetify</strong>
        </v-card-text>
      </v-card>
    </v-footer>
  
</v-main>
</template>
<script>
  export default {
    data () {
      return {
        items: [
          {
            src: 'https://static.thehoneycombers.com/wp-content/uploads/sites/4/2019/08/Hyatt-Regency-Bali-Omang-Omang-at-Night-restaurant-in-Sanur-Bali-Indonesia.jpg', 
          },
          {
            src: 'https://hallobro.com/wp-content/uploads/2017/04/bakso-malang.jpg',
          },
          {
            src: 'https://i.ytimg.com/vi/kHVRI5-hjaM/maxresdefault.jpg',
          },
          {
            src: 'https://cdn.vuetifyjs.com/images/carousel/planet.jpg',
          },
        ],

        icons: [
          'mdi-facebook',
          'mdi-twitter',
          'mdi-instagram'
        ],
      }
    },
  }
</script>