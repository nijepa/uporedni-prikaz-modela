<template>
  <div class="">
    <div v-if="loading">Loading...</div>

    <div  v-else>
      <div class="row">
        <div class="col-1-of-3" v-for="(packageSingle, id) in packages" :key="id">
          <div class="card__side card__side--front">

            <div class="fav" v-if="packageSingle.is_featured">
              <p class="favorit" >{{ promoText }}</p>
            </div>

            <div class="card__heading fav">
              <p>{{ packageSingle.name }}</p>
            </div>

            <div class="card__details">

              <PackagesCat :packages="packageSingle.included"
                           :img-src="imgTV"
                           :category-name="'tv'">
              </PackagesCat>
              <PackagesCat :packages="packageSingle.included"
                           :img-src="imgNET"
                           :category-name="'net'">
              </PackagesCat>

              <Promotions v-if="packageSingle.promotions[0].discount_variations[0] === packageName"
                          :promotions="packageSingle.promotions">
              </Promotions>

              <div class="row card__price-box">
                <p v-if="packageSingle.prices.old_price_recurring[packageName]"
                   class="col-1-of-2 card__price-only"
                   style="text-decoration: line-through;">{{
                     numberFormated(parseInt(packageSingle.prices.old_price_recurring[packageName]))
                   }} rsd/mes.
                </p>
                <p class="col-1-of-2 card__price-value">{{
                     numberFormated(parseInt(packageSingle.prices.price_recurring[packageName]))
                     }} rsd/mes.
                </p>
              </div>
              <p class="promo-date"
                 v-if="packageSingle.prices.old_price_recurring[packageName]"
                 v-html="packageSingle.prices.old_price_promo_text">
              </p>

              <br />
              <div class="container  btn btn--white">Naručite</div>

            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
  import axios from 'axios';
  import PackagesCat from "./PackagesCategories";
  import Promotions from "./Promotions";

  export default {
    name: 'Packages',

    components: {
      PackagesCat,
      Promotions
    },

    props: {
      packageName: {
        type: String
      },
    },

    data: function () {
        return {
            loading: true,
            imgTV: '',
            imgNET: '',
            packages: null,
            promoText: ''
        }
    },

    mounted() {
      axios.get('http://www.mocky.io/v2/5ed511c53300005f00f7a790')
        .then(response => {
          this.packages = response.data.items;
          this.promoText = response.data.promo_text;
          this.imgTV = response.data.assets.tv_category;
          this.imgNET = response.data.assets.net_category;
          this.loading = false;
        })
        .catch(error => {
          this.loading = false;
          if (error.response === 404) {
            console.log('errore');
          }
      });
    },

    methods: {
      numberFormated(x) {
        return x.toString().replace(/\B(?=(\d{3})+(?!\d))/g, ".");
      }
    }
}
</script>

<style scoped lang="scss">
  img {
    width: 40%;
    height: auto;
    text-align: center;
  }
</style>
