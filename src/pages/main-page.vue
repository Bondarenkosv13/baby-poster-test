<template>
  <div class="main_page">
    <div
      ref="workPanel"
      class="work_panel"
    >
      <button name="button" class="primary_button question_button">
        <img class="question_button_image" src="/images/icon-tipp.svg"/>
      </button>
      <div ref="frame" class="frame">
        <div class="fabric"></div>
      </div>
      <canvas id="canvas"></canvas>
    </div>

    <div class="left_sidebar">
      <h1 class="title">PLATZIERUNG, FARBE UND VORLAGE</h1>
      <!-- Reset-->
      <div>
        <div class="position_title">Position der Zeichnung</div>
        <v-btn small class="primary_button reset-position">Zurücksetzen</v-btn>
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
            v-for="item in colors"
            :key="item.color"
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
      <!-- Select template-->
      <div>
        <div class="position_title">VORLAGE</div>
        <v-btn small class="primary_button reset-position">Auswählen</v-btn>
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

const TITLE_POSITION_MARGIN = 50;
const DEFAULT_IMAGE_PATH = document.URL + 'images/test.svg';
const COLORS = [
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

export default {
  name: "MainPages",
  data() {
    return {
      posterTitle: '',
      imageSize: 250,
      colors: COLORS,
      color: null,
      canvas: null,
      img: null,
      test: null,
    }
  },
  methods: {
    canvasInit() {
      const workPanel = this.$refs.workPanel;

      this.canvas = new fabric.Canvas('canvas');
      this.canvas.setWidth(workPanel.offsetWidth);
      this.canvas.setHeight(workPanel.offsetHeight);
    },
    fabricInit() {
      fabric.Image.fromURL(DEFAULT_IMAGE_PATH, (img) => {
        img.scale(1.0).set({
          top: this.getTopPositionImage(),
          left: this.getLeftPositionImage(),
          selectable: true,
        });
        img.scaleToWidth(this.imageSize);
        img.filters = []
        this.img = img
        this.canvas.add(img);
      });
    },
    posterTitleInit() {
      this.posterTitle = new fabric.Text('', {
          fontFamily: 'Montserrat',
          fontSize: 20,
          top: this.getTopPositionPosterTitle(),
          left: this.getLeftPositionPosterTitle()
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

      this.img.applyFilters();
      this.canvas.requestRenderAll();
    },
    getTopPositionImage() {
      return this.$refs.workPanel.offsetHeight / 2 - (this.imageSize / 2)
    },
    getLeftPositionImage() {
      return this.$refs.workPanel.offsetWidth / 2 - (this.imageSize / 2)
    },
    getTopPositionPosterTitle() {
      return (this.$refs.workPanel.offsetHeight / 2) + (this.$refs.frame.offsetHeight / 2) - TITLE_POSITION_MARGIN
    },
    getLeftPositionPosterTitle() {
      return this.$refs.workPanel.offsetWidth / 2 - (this.$refs.frame.offsetWidth / 2) + TITLE_POSITION_MARGIN
    }
  },
  mounted() {
    this.$nextTick(() => {
      this.canvasInit()
      this.fabricInit()
      this.posterTitleInit()
    })
  }
}
</script>
