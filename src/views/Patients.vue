<template>
  <v-container fluid pa-5>
    <v-row class="ma-xs-0 ma-sm-0 ma-md-5">
      <v-col cols="12" sm="6" lg="3" class="d-flex justify-start align-center">
        <v-text-field
          color="teal lighten-2"
          v-model="search"
          append-icon="mdi-magnify"
          label="Buscar"
          single-line
          hide-details
          @keyup="doSearch"
        ></v-text-field>
      </v-col>
      <v-spacer></v-spacer>
      <v-col
        cols="12"
        sm="6"
        lg="3"
        class="d-flex order-first order-sm-0 justify-end align-center"
      >
        <CreatePatient></CreatePatient>
      </v-col>
    </v-row>
    <v-row class="ma-xs-0 ma-sm-0 ma-md-5">
      <v-col>
        <v-card>
          <v-card-title class="teal--text">
            <strong>Pacientes</strong>
            <v-spacer></v-spacer>
          </v-card-title>
          <v-data-table
            :headers="headers"
            :items="patients"
            :page="page"
            :items-per-page="itemsPerPage"
            :options.sync="options"
            :server-items-length="totalPatients"
            :search="search"
            :footer-props="{
              'items-per-page-text': 'Pacientes por página'
            }"
            @click:row="showPatient"
          ></v-data-table>
        </v-card>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
import PatientService from '../services/patientService'
import CreatePatient from '../components/CreatePatient'
export default {
  name: 'Patients',
  components: { CreatePatient },
  data: () => ({
    patients: [],
    search: '',
    totalPatients: 0,
    options: {},
    dialog: false,
    page: 1,
    itemsPerPage: 10,
    headers: [
      {
        text: 'Apellidos',
        align: 'start',
        sortable: true,
        value: 'lastName',
        class: 'teal darken-2 white--text'
      },
      {
        text: 'Nombre',
        value: 'firstName',
        class: 'teal darken-2 white--text'
      },
      { text: 'DNI', value: 'dni', class: 'teal darken-2 white--text' },
      {
        text: 'Telefono',
        value: 'contact.mobilephone',
        class: 'teal darken-2 white--text'
      }
    ]
  }),
  created() {
    if (!localStorage.token) {
      this.$router.push('/')
    }

    PatientService.getPatients()
      .then(response => {
        console.log('created', response.data.patients)
        this.patients = response.data.patients
      })
      .catch(err => console.log(err))
  },
  watch: {
    options: function() {
      const { page, itemsPerPage } = this.options
      this.page = page
      this.itemsPerPage = itemsPerPage
      this.doSearch()
    }
  },
  methods: {
    showPatient: function(item) {
      console.log(item)
      this.$router.push(`${item._id}`)
    },
    doSearch: function() {
      console.log(
        'doSeasrch',
        'itemsperpage',
        this.itemsPerPage,
        'page',
        this.page
      )
      PatientService.getPatientsByQuery(
        this.itemsPerPage,
        this.page,
        this.search
      )
        .then(response => {
          this.patients = response.data.patients
          this.totalPatients = response.data.totalPatients

          const { sortBy, sortDesc } = this.options
          if (sortBy.length === 1 && sortDesc.length === 1) {
            this.patients = this.patients.sort((a, b) => {
              const sortA = a[sortBy[0]]
              const sortB = b[sortBy[0]]

              if (sortDesc[0]) {
                if (sortA < sortB) return 1
                if (sortA > sortB) return -1
                return 0
              } else {
                if (sortA < sortB) return -1
                if (sortA > sortB) return 1
                return 0
              }
            })
          }
        })
        .catch(err => console.log(err))
    }
  }
}
</script>

<style lang="scss" scoped></style>
