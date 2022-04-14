<template>
  <div class="product-display">
    <!-- ////////////////////// 2.1 Sub-Container EINKAUFKORB /////////////////////////////// -->
    <!-- Der Einkaufskorb bleibt in der App da er ein einmalig und ein Root-Element ist -->
    <div class="cart">
      <h5 class="cart-title">{{ cartprops.title }}</h5>
      <!-- v-if="artikelprops.checkAnzahlWarenkorb" Prüfen ob schon ein Artikel im Warenkorb ist. -->
      <span class="cart_info" v-if="cartprops.showWarenkorb">{{
        cartprops.emptyCart
      }}</span>
      <table v-else>
        <tr>
          <th class="table_left">{{ cartprops.tableProps[0].article }}</th>
          <th class="table_right">{{ cartprops.tableProps[0].preis }}</th>
        </tr>
        <tr v-for="(newArtikel, index) in rowArtikels" v-bind:key="index">
          <!-- {{ getAllArtikelCounter }}/{{ cartprops.artikelCartAnzahl }} -->
          <td class="table_left">
            {{ newArtikel.addTyp }}
          </td>
          <td class="table_right">{{ newArtikel.addPreis.toFixed(2) }} €</td>
          <td class="table_right">
            <button :id="index" class="deleteBtn" @click="deleteItem(index)">
              -
            </button>
          </td>
        </tr>
        <tr>
          <td class="table_left">{{ cartprops.tableProps[0].versand }}</td>
          <td class="table_right">{{ getLieferPreis().toFixed(2) }} €</td>
        </tr>
        <tr>
          <td class="last_td table_left">
            {{ cartprops.tableProps[0].sum }} ({{ cartprops.artikelCartAnzahl }}
            Artikel)
          </td>
          <td class="last_td table_right">
            {{ (zwischenSumme + getLieferPreis()).toFixed(2) }} €
          </td>
        </tr>
      </table>
      <div class="premiumUser">
        <button class="toggleBtn" @click="checkPremiumUser">X</button>
        <span class="Btn-label" v-if="premiumUser">Sie sind Premium-User.</span>
        <span class="Btn-label" v-else>Sie sind kein Premium-User.</span>
      </div>
    </div>
    <!-- ////////////////////// 2.2 Sub-Container ARTIKEL /////////////////////////////// -->
    <div class="artikels">
      <!-- Einfügen der Artikel-Componente mit allen Attribut-Werten die den Artikel spezifizieren -->
      <ArtikelComp
        :artikelpreis="5.0"
        artikelname="Trikots"
        artikelfirma="Bleed"
        :artikelvariante="1"
        :artikelvariantmenge="[2, 4, 3]"
        :artikeldetails="[
          'Regular Fit',
          'Rundhalsausschnitt',
          'Kurzarm',
          '100% Baumwolle',
          'Modern',
          'Kragenform: crew neck',
        ]"
        artikelbeschreibung="Im Bundesliga-Qualität."
        @remove-from-cart="removeFromCart"
        @addto-cart="addToCart"
      >
      </ArtikelComp>
      <ArtikelComp
        :artikelpreis="10.0"
        artikelname="Sport Shirts"
        artikelfirma="Jakko"
        :artikelvariante="2"
        :artikelvariantmenge="[2, 1, 1]"
        :artikeldetails="[
          'Ringspinn-Baumwolle',
          'Single-Jersey (Piqué-Look)',
          'Nackenband',
          'Zwei Ton-in-Ton-Knöpfe',
          'Ohne Ärmelbündchen',
          'Seitennähte',
        ]"
        artikelbeschreibung="Der elegantere Look: Poloshirt."
        @remove-from-cart="removeFromCart"
        @addto-cart="addToCart"
      >
      </ArtikelComp>
      <ArtikelComp
        :artikelpreis="2.0"
        artikelname="T-Shirts"
        artikelfirma="Lacoste"
        :artikelvariante="0"
        :artikelvariantmenge="[1, 4, 2]"
        :artikeldetails="[
          'Kragen in Kontrastfarbe',
          'Belcoro® Garn',
          'langarmzarm',
          'Höhere Maschendichte',
          'Doppelnaht an Halsausschnitt',
        ]"
        artikelbeschreibung="Klamotten für jede Jahreszeit."
        @remove-from-cart="removeFromCart"
        @addto-cart="addToCart"
      >
      </ArtikelComp>
    </div>
  </div>
</template>

<script>
import ArtikelComp from "./Artikel-Comp.vue";

export default {
  name: "ShopComp",
  components: {
    ArtikelComp,
  },
  props: {},
  data() {
    return {
      zwischenSumme: 0.0,
      premiumUser: true,
      max: 0,
      rowArtikels: [],
      cartprops: {
        title: "Einkaufskorb 1",
        emptyCart: "Dein Einkaufkorb ist leer.",
        artikel: "Artikel",
        artikelCartAnzahl: 0,
        showWarenkorb: true,
        tableProps: [
          {
            article: "Artikel",
            preis: "Preis",
            versand: "Versandkosten",
            sum: "Summe",
          },
        ],
      },
    };
  },
  mounted: function () {
    //this.ArtikelComp.art
  },
  methods: {
    /// Neue Funktion die vom Artikel durchgereicht werden
    addToCart(preis, typ) {
      if (this.cartprops.artikelCartAnzahl <= this.getAllArtikelCounter) {
        this.cartprops.artikelCartAnzahl += 1;
        //this.$emit("addcart", this.cartprops.artikelCartAnzahl);
        console.log("addCart: ", preis, typ);
        this.addItem(preis, typ);
        this.zwischenSumme += parseFloat(preis);
        this.checkAnzahlWarenkorb();
      }
    },
    /// Neue Funktion die vom Einkaufkorb verwendet werden
    removeFromCart(preis) {
      this.cartprops.artikelCartAnzahl -= 1;
      this.zwischenSumme -= parseFloat(preis);
      this.checkAnzahlWarenkorb();
    },
    checkAnzahlWarenkorb() {
      if (this.cartprops.artikelCartAnzahl <= 0) {
        this.cartprops.showWarenkorb = true;
      } else {
        this.cartprops.showWarenkorb = false;
      }
    },
    getZwischenSumme() {
      return this.zwischenSumme;
    },
    getLieferPreis() {
      if (this.cartprops.artikelCartAnzahl > 0) {
        return this.premiumUser ? 0.0 : 6.99;
      } else {
        return 0.0;
      }
    },
    checkPremiumUser() {
      this.premiumUser = !this.premiumUser;
    },
    addItem(currPreis, currTyp) {
      var newArtikelInEinkaufsKorb = {
        addTyp: currTyp,
        addPreis: currPreis,
      };
      this.rowArtikels.push(newArtikelInEinkaufsKorb);
    },
    deleteItem(who) {
      this.cartprops.artikelCartAnzahl -= 1;
      this.zwischenSumme -= parseFloat(this.rowArtikels[who].addPreis);
      this.rowArtikels.splice(who, 1);
      this.checkAnzahlWarenkorb();
    },
  },
  computed: {
    getAllArtikelCounter() {
      return 20;
    },
  },
};
</script>
<style>
.toggleBtn {
  margin: 10px 10px;
  width: 29px;
  height: 29px;
  font-size: 1.2rem;
  cursor: pointer;
}
.premiumUser {
  display: flex;
  justify-content: flex-start;
  align-items: center;
}
.deleteBtn {
  width: 20px;
  height: 20px;
}
</style>