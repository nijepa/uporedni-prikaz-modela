<template>
  <main id="app">
    <div v-if="loading">Loading...</div>
    <section v-else class="section-packages" id="section-packages">
      <dropdown :options="arrayOfObjects"
                :selected="object"
                v-on:updateOption="methodToRunOnSelect">
      </dropdown>
      <Packages :packageName="object.name" />
    </section>
  </main>
</template>

<script>
    import Packages from './components/Packages.vue'
    import Dropdown from './components/PackagesDropdown'
    import axios from "axios";

    export default {
      name: 'App',

      components: {
        Packages,
        Dropdown
      },

      data: function () {
        return {
          loading: true,
          arrayOfObjects: [],
          object: {
            name: 'Object Name',
          }
        }
      },

      mounted() {
        axios.get('http://www.mocky.io/v2/5ed511c53300005f00f7a790')
          .then(response => {
              this.arrayOfObjects = response.data.contract_length.contract_length_options.map(function(row) {
                  return { name : row }
              })
              this.object.name = response.data.contract_length.preselected_contract_length;
              this.loading = false;
              console.log(this.object.name);
          })
          .catch(error => {
              this.loading = false;
              if (error.response === 404) {
                  console.log('errore');
              }
          });
      },

      methods: {
        methodToRunOnSelect(payload) {
          this.object = payload;
        },
      }
    }
</script>

<style lang="scss">
  @import "./css/variables";
  @import "./css/button";
  @import "./css/mixins";
  @import "./css/base";
  @import "./css/grid";
  @import "./css/card";
  @import "./css/utilities";
  @import "./css/typography";
  //@import "./css/animations";
  .section-packages {
    background-color: $color-grey-light-1;
    padding: 25rem 0 15rem 0;
    margin-top: -10rem;

    @include respond(tab-port) {
      padding: 20rem 0 10rem 0;
    }
  }
</style>
