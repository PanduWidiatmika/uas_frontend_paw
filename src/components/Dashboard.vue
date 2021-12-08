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
          Start
        </h2>
        <div>
          open as food stand
        </div>
      </div>
    </v-timeline-item>

    <v-timeline-item
      small
    >
      <template v-slot:opposite>
        <span
          :class="`headline font-weight-bold green--text`"
          v-text="2002"
        ></span>
      </template>
      <div class="py-4">
        <h2 :class="`headline font-weight-light mb-4 green--text`">
          Move
        </h2>
        <div>
          rent a building for the restaurant
        </div>
      </div>
    </v-timeline-item>

    <v-timeline-item
      small
    >
      <template v-slot:opposite>
        <span
          :class="`headline font-weight-bold pink--text`"
          v-text="2004"
        ></span>
      </template>
      <div class="py-4">
        <h2 :class="`headline font-weight-light mb-4 pink--text`">
          Move
        </h2>
        <div>
          has our own restaurant building
        </div>
      </div>
    </v-timeline-item>

    <v-timeline-item
      small
    >
      <template v-slot:opposite>
        <span
          :class="`headline font-weight-bold amber--text`"
          v-text="2005"
        ></span>
      </template>
      <div class="py-4">
        <h2 :class="`headline font-weight-light mb-4 amber--text`">
          Updates
        </h2>
        <div>
          Updating our logo & restaurant
        </div>
      </div>
    </v-timeline-item>

    <v-timeline-item
      small
    >
      <template v-slot:opposite>
        <span
          :class="`headline font-weight-bold orange--text`"
          v-text="2007"
        ></span>
      </template>
      <div class="py-4">
        <h2 :class="`headline font-weight-light mb-4 orange--text`">
          Goes International
        </h2>
        <div>
          
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
            src: 'https://images.unsplash.com/photo-1504754524776-8f4f37790ca0?ixlib=rb-1.2.1&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=870&q=80',
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