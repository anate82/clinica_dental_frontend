<template>
  <v-card color="#B2DFDB" height="500px">
    <v-card-title class="headline teal--text">
      <strong>Próximas citas</strong>
      <v-spacer></v-spacer>
      <v-btn
        color="teal darken-2 white--text"
        grow
        :to="{ name: 'Home', params: { patient } }"
        >Crear Cita
      </v-btn>
    </v-card-title>
    <v-divider></v-divider
    ><v-card-text>
      <v-col class="d-flex justify-center">
        <v-timeline align-top dense reverse>
          <v-timeline-item
            v-for="(appointment, idx) in appointments"
            :key="idx"
            small
            color="teal"
          >
            <div>
              <div class="font-weight-medium">
                <strong>
                  {{ appointment.intervention }}
                </strong>
              </div>
              <div>
                {{ appointment.start }}
              </div>
              <div>
                <strong>
                  {{ appointment.employees }}
                </strong>
              </div>
            </div>
          </v-timeline-item>
        </v-timeline>
      </v-col>
    </v-card-text>
  </v-card>
</template>

<script>
import AppointmentService from '@/services/appointmentService'

import PatientService from '../services/patientService'
export default {
  name: 'PatientDates',
  data() {
    return {
      appointments: [],
      patient: {}
    }
  },
  mounted() {
    AppointmentService.getAppointmentsByPatient(this.$route.params.patientId)
      .then(appointments => {
        this.appointments = []
        if (appointments.data.length < 1) {
          let today = new Date(Date.now())
          this.appointments.push({
            employees: '',
            start: `Fecha: ${today.getFullYear()}-${today.getMonth() +
              1}-${today.getDate()} ${today.getHours()}:${today.getMinutes()}`,
            intervention: 'No hay proximas citas',
            color: 'teal'
          })
        } else {
          this.sortByStart(appointments.data)
          for (let i = 0; i < 3; i++) {
            if (appointments.data[i]) {
              this.formatAppointments(appointments.data[i])
            }
          }
        }
      })
      .catch(err => console.log(err))

    PatientService.getPatientById(this.$route.params.patientId)
      .then(request => {
        this.patient = request.data
      })
      .catch(err => {
        console.log(err)
      })
  },
  methods: {
    sortByStart(appointments) {
      appointments.sort(function(a, b) {
        return new Date(a.start) - new Date(b.start)
      })
    },
    formatAppointments(appointment) {
      let employees = appointment.employees
        .map(employee => {
          return `${employee.firstName} ${employee.lastName.slice(0, 3)}`
        })
        .join('. -')
      this.appointments.push({
        employees: employees,
        start: appointment.start,
        intervention: appointment.intervention,
        color: 'teal'
      })
    }
  }
}
</script>

<style lang="scss" scoped></style>
