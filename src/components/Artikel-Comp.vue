<template>
  <div class="product-container">
    <div class="product-image">
      <!-- Das Bild muss über eine spezifische SCR-Binding (v-bind:src) an an die Quelle gebunden werden -->
      <img :src="variantimage" alt="Artikel-Bild" class="image" />
      <!-- <img :src="artikelprops.image" alt="" class="image"> -->
    </div>
    <div class="product-info">
      <!-- Über data()-Funktion könne wir nun die Eigenschaften implementieren -->
      <h2 class="info">
        {{ artikelname }} <span class="firma">({{ artikelfirma }})</span>
      </h2>
      <div class="beschreibung">{{ artikelbeschreibung }}</div>
      <!-- Über Eigenschaften maxAnzahl und anzahl können wir abfagen ob noch Artikel auf Lager sind -->
      <!-- <p v-if="artikelprops.anzahl == artikelprops.maxAnzahl">Zur Zeit nicht lieferbar.</p> -->
      <p v-if="artikelprops.lagerbestand">
        Auf Lager
        <span class="artCount"
          >{{ maxAnzahlArtikel }} / {{ artikelprops.anzahl }}</span
        >
      </p>
      <p v-else>Zur Zeit nicht auf Lager.</p>
      <ul>
        <!-- Über das Eigenschafts-Array können wir eine Liste generieren -->
        <li v-for="(detail, index) in artikeldetails" :key="index">
          {{ detail }}
        </li>
      </ul>
      <!-- // Einfügen der Componente VARIANTEN  //// Durchreichen des Click-Events -->
      <VariantComp
        @update-image-comp="updateVariante"
        :artikelvariantArray="artikelvariantmenge"
      ></VariantComp>
      <!-- Der Preis wird über index in den Artikel injiziert-->
      <div class="preis">{{ artikelpreis.toFixed(2) }} €</div>
      <!-- Button zu hinzufügen in den Einkaufwagen -->
      <div class="buttons">
        <button
          class="buttonAdd"
          @click="addToEinkaufswagen"
          :disabled="!aufLager"
          :class="{ disabledButton: !aufLager }"
        >
          In den Einkaufwagen
        </button>
        <!-- Button um aus den Einkaufwagen zu entfernen !inEinkauf-->
        <button
          class="buttonDel"
          @click="removeFromEinkaufwagen"
          :disabled="!inEinkauf"
          :class="{ disabledButton: !inEinkauf }"
        ></button>
      </div>
    </div>
  </div>
</template>

<script>
import VariantComp from "./Variant-Comp.vue";
const imagePath_tshirt_green = require("@/assets/img/tshirt-green.jpg");
const imagePath_tshirt_rot = require("@/assets/img/tshirt-rot.jpeg");
const imagePath_tshirt_blau = require("@/assets/img/tshirt-blau.jpg");

export default {
  name: "ArtikelComp",
  components: {
    VariantComp,
  },
  props: {
    artikelpreis: Number,
    artikelname: String,
    artikelfirma: String,
    artikelbeschreibung: String,
    artikeldetails: Object,
    artikelvariante: Number,
    artikelvariantmenge: Object,
  },
  setup() {
    return {};
  },
  data() {
    return {
      artikelImg: [
        imagePath_tshirt_green,
        imagePath_tshirt_rot,
        imagePath_tshirt_blau,
      ],
      publicPath: process.env.BASE_URL,
      maxAnzahlArtikel: 0,
      variantimage: "",
      artikelprops: {
        anzahl: 0,
        lagerbestand: true,
      },
    };
  },
  mounted: function () {
    // Das gewünschte Startbild setzen
    this.variantimage = this.artikelImg[this.artikelvariante];
    // Summe aller Artikel inkl. Varianten
    this.maxAnzahlArtikel = this.artikelvariantmenge.reduce((a, b) => a + b);
  },
  methods: {
    addToEinkaufswagen() {
      if (this.artikelprops.anzahl < this.maxAnzahlArtikel) {
        this.artikelprops.anzahl += 1;
        this.$emit("addto-cart", this.artikelpreis, this.artikelname);
        this.checkLagerbestand();
      }
    },
    removeFromEinkaufwagen() {
      if (this.artikelprops.anzahl >= 1) {
        this.artikelprops.anzahl -= 1;
        this.$emit("remove-from-cart", this.artikelpreis);
        this.checkLagerbestand();
      }
    },
    checkLagerbestand() {
      if (this.artikelprops.anzahl >= this.maxAnzahlArtikel) {
        this.artikelprops.lagerbestand = false;
      } else {
        this.artikelprops.lagerbestand = true;
      }
    },
    updateVariante(img) {
      this.variantimage = img;
    },
    setMaxAnzahlArtikel(max) {
      this.maxAnzahlArtikel = max;
    },
  },
  computed: {
    aufLager() {
      return this.artikelprops.lagerbestand ? true : false;
    },
    inEinkauf() {
      return this.artikelprops.anzahl;
    },
  },
};
</script>

<style lang="">
</style>