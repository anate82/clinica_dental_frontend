<template>
  <v-container fluid id="hero" ma-0 pa-0>
    <v-img
      :min-height="'calc(100vh - ' + $vuetify.application.top + 'px)'"
      :aspect-ratio="16 / 9"
      gradient="to top right, rgba(0,0,0,0.6), rgba(0,0,0,0.6)"
      src="https://images.unsplash.com/photo-1598256989800-fe5f95da9787?ixid=MXwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHw%3D&ixlib=rb-1.2.1&auto=format&fit=crop&w=1950&q=80"
    >
      <v-container fill-height>
        <v-row
          align="center"
          class=" d-flex white--text mx-auto"
          justify="center"
        >
          <v-col
            class="d-flex flex-row justify-center align-center white--text text-center"
            cols="12"
            md="12"
            lg="6"
            xl="4"
          >
            <h1>
              Clinica Dental Marrero
            </h1>

            <br />
          </v-col>
          <v-col
            class="d-flex flex-row justify-center align-center white--text text-center"
            cols="12"
            sm="8"
            lg="6"
            xl="4"
          >
            <p
              class="text-left text-body-2 text-sm-body-2 text-md-body-1 text-lg-body-1 text-xl-body-1 mb-2"
            >
              La clínica dental Marrero fue creada en Las Palmas de Gran Canaria
              en el año 1997 por el Dr. Juan Marrero. Contamos con los últimos
              avances tecnológicos, entre los que se incluyen invisalign,
              radiografías 3D...
            </p></v-col
          >

          <v-col class="white--text text-center" cols="12" md="6" xl="12">
            <v-btn color="white" fab outlined @click="$vuetify.goTo('#team')">
              <v-icon>mdi-chevron-double-down</v-icon>
            </v-btn>
          </v-col>
        </v-row>
      </v-container>
    </v-img>
  </v-container>
</template>

<script>
export default {}
</script>

<style lang="scss" scoped>
.bottom-gradient {
  background-image: linear-gradient(
    to top,
    rgba(0, 0, 0, 0.4) 0%,
    transparent 72px
  );
}
.image-gradient {
  background-image: repeating-linear-gradient(
    -45deg,
    rgba(255, 0, 0, 0.25),
    rgba(255, 0, 0, 0.25) 5px,
    rgba(0, 0, 255, 0.25) 5px,
    rgba(0, 0, 255, 0.25) 10px
  );
}
</style>
