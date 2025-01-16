<template>
  <v-container class="menu-container">
    <v-row no-gutters align="center">
      <v-col cols="1" order="1">
        <v-sheet class="pa-1 ma-1">
          <v-img  height="64" src="@/assets/logo.svg" />
        </v-sheet>
      </v-col>
      <v-col  order="2">
        <v-sheet class="pt-4 mt-2">
          <v-autocomplete
            class="mx-auto"
            density="comfortable"
            menu-icon=""
            placeholder="Find places and things to do"
            prepend-inner-icon="mdi-magnify"
            style="max-width: 350px"
            theme="light"
            variant="solo"
            auto-select-first
            item-props
            rounded
          ></v-autocomplete>
        </v-sheet>
      </v-col>
      <v-col cols="5" order="3" >
        <div class="pa-1 ma-1 d-flux  justify-center items-center" >
          <a href="#" class="link">
            Become a supplier
          </a>
          <a href="#" class="link">
              <v-icon>mdi-heart</v-icon> 
              <p>Wishlist</p>
          </a>
          <a href="#" class="link">
            <v-icon>mdi-cart</v-icon> 
            <p>Cart</p>
          </a>
          <a href="#" class="link">
            <v-icon>mdi-account</v-icon> 
            <p>People</p>
          </a>
        </div>
      </v-col>
    </v-row>

  </v-container>
</template>

<script setup>
import { VAppBar, VAppBarTitle, VImg, VSpacer, VTextField, VMenu, VBtn, VIcon, VRow, VCol, VContainer } from 'vuetify/components'
</script>

<style scoped>
.menu-container {
  max-width: 1200px;
  margin: 0 auto;
  padding: 0 20px;
}
  .link {
    text-decoration: none;
    color: black;
    display: inline-block;
    padding:7px;
    text-align: center;
    margin-left:5px;
  }

  .link:hover {
    border-bottom: 2px solid #f53;
  }
</style>
