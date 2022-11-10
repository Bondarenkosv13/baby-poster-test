<template>
  <div class="main_page">
    <div class="work_panel">
      <button name="button" class="btn question_button">
        <img class="question_button_image" src="/images/icon-tipp.svg"/>
      </button>

      <div class="frame">
        <div class="fabric"></div>
      </div>
      <canvas id="canvas"></canvas>

    </div>
    <div class="left_sidebar">
      <h1 class="title">PLATZIERUNG, FARBE UND VORLAGE</h1>

      <!-- Reset-->
      <div>
        <div class="position_title">Position der Zeichnung</div>
        <v-btn small class="btn reset-position">Zurücksetzen</v-btn>
      </div>

      <!-- Colors-->
      <div>
        <div class="position_title">Farbe</div>
        <v-btn-toggle
            v-model="color"
            color="primary"

            group
        >
          <v-tooltip
              v-for="(item, index) in colors"
              :key="index"
              top
              color="#e69a9a"
          >
            <template v-slot:activator="{ on }">
              <v-btn
                  class="color_avatar"
                  active-class="active_button"
                  :value="index"
                  @click="changeColor(item)"
                  text
                  v-on="on"
              >
                <v-avatar
                    v-on="on"
                    size="30"
                    :color="item.color"
                ></v-avatar>
              </v-btn>
            </template>
            <span>{{ item.name }}</span>
          </v-tooltip>
        </v-btn-toggle>
      </div>

      <!-- Vorlage-->
      <div>
        <div class="position_title">VORLAGE</div>
        <v-btn small class="btn reset-position">Auswählen</v-btn>
      </div>

      <!-- Text-->
      <div>
        <div class="position_title">BESCHRIFTUNG & TEXT</div>
        <v-text-field
            class="poster_input"
            placeholder="Titel des Posters"
            background-color="#f6f2f1"
            filled
            rounded
            dense
            @input="addText"
        ></v-text-field>
      </div>

    </div>
  </div>
</template>

<script>
import {fabric} from "fabric";

export default {
  name: "MainPages",
  data() {
    return {
      posterTitle: '',
      imageSize: 250,
      color: null,
      canvas: null,
      img: null,
      test: null,
    }
  },
  computed: {
    colors() {
      return [
        {
          name: 'Blue',
          color: '#7a8e95',
          active: true
        },
        {
          name: 'Rosa',
          color: '#c59a9a',
          active: false
        },
        {
          name: 'Senf',
          color: '#d09c50',
          active: false
        },
        {
          name: 'Grau',
          color: '#6f6f6f',
          active: false
        },
        {
          name: 'Lila',
          color: '#a099a3',
          active: false
        },
        {
          name: 'Beige',
          color: '#a38e89',
          active: false
        }
      ]
    },
  },
  methods: {
    canvasInit() {
      this.canvas = new fabric.Canvas('canvas');

      const worPanel = document.querySelector('.work_panel');
      const frame = document.querySelector('.frame');

      this.canvas.setWidth(worPanel.offsetWidth);
      this.canvas.setHeight(worPanel.offsetHeight);
      fabric.Image.fromURL(document.URL + 'images/test.svg', (img) => {
        img.scale(1.0).set({
          top: worPanel.offsetHeight / 2 - (this.imageSize / 2),
          left: worPanel.offsetWidth / 2 - (this.imageSize / 2),
          selectable: true,
        });
        img.filters = []
        img.scaleToWidth(this.imageSize);
        this.img = img
        this.canvas.add(img);
      });

      this.posterTitle = new fabric.Text('', {
            top: worPanel.offsetHeight / 2 + (frame.offsetHeight / 2 - 70),
            left: worPanel.offsetWidth / 2 - (frame.offsetWidth / 2 - 70)
          }
      );
      this.canvas.add(this.posterTitle);
    },
    addText(text) {
      this.posterTitle.set('text', text)
      this.canvas.requestRenderAll();
    },
    changeColor(item) {
      this.img.filters.push(new fabric.Image.filters.BlendColor({
        color: item.color,
        mode: 'tint',
        opacity: 0
      }))
      this.$nextTick(() => {
        this.img.applyFilters();
        this.canvas.requestRenderAll();
      })
    }
  },
  mounted() {
    this.$nextTick(() => {
      this.canvasInit()
    })
  }
}
</script>
