<template lang="">
<div class="farben">
            <div v-for="(variant, index) in varianten" 
            :key="variant.id" class="color-circle"
            :style="{ backgroundColor: variant.color }"
            @click="updateVarianteFromComp(varianten[index].image)"
            >
            </div>
      </div>
</template>
<script>
const imagePath_tshirt_green = require("@/assets/img/tshirt-green.jpg");
const imagePath_tshirt_rot = require("@/assets/img/tshirt-rot.jpeg");
const imagePath_tshirt_blau = require("@/assets/img/tshirt-blau.jpg");

export default {
  name: "VariantComp",
  props: { artikelvariantArray: Object },
  data() {
    return {
      gesamtArtikelMenge: 0,
      varianten: [
        {
          id: 66,
          color: "rgb(58, 148, 190)",
          image: imagePath_tshirt_blau,
          menge: 0,
        },
        {
          id: 33,
          color: "rgb(255, 69, 52)",
          image: imagePath_tshirt_rot,
          menge: 0,
        },
        {
          id: 99,
          color: "rgb(66, 149, 52)",
          image: imagePath_tshirt_green,
          menge: 0,
        },
      ],
    };
  },
  mounted: function () {
    console.log("Mounted Varianten: ", this.artikelvariantArray);
    for (let v = 0; v < this.artikelvariantArray.length; v++) {
      this.varianten[v].menge = this.artikelvariantArray[1];
      
    }
  },
  methods: {
    updateVarianteFromComp(img) {
      this.$emit("update-image-comp", img);
    },
  },
  computed: {},
};
</script>
<style>
</style>