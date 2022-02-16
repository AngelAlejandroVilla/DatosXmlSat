<template v-slot:default="dialog">
  <v-card
    class="auth-card"
  >
    <!-- logo -->
    <v-card-title class="d-flex align-center justify-center py-7">
      <router-link
        to="/"
        class="d-flex align-center"
      >
        <v-img
          :src="require('@/assets/images/logos/logo.svg')"
          max-height="30px"
          max-width="30px"
          alt="logo"
          contain
          class="me-3 "
        ></v-img>

        <h2 class="text-2xl font-weight-semibold">
          VillaSol
        </h2>
      </router-link>
    </v-card-title>

    <!-- title -->
    <v-card-text>
      <p class="text-2xl font-weight-semibold text--primary mb-2">
        Registarte Ahora!  🚀
      </p>
      <p class="mb-2">
        Registrate Para Obtener Todos Los Beneficios De Nuestro Servicio!
      </p>
    </v-card-text>

    <!-- login form -->
    <v-card-text>
      <v-form
        @submit.prevent="submit"
      >
        <v-row>
          <v-col
            cols="12"
            md="6"
          >
            <v-text-field
              v-model="firstName"
              :error-messages="firstNameErrors"
              label="First Name"
              placeholder="First Name"
              required
              @input="$v.firstName.$touch()"
              @blur="$v.firstName.$touch()"
            ></v-text-field>
          </v-col>

          <v-col
            cols="12"
            md="6"
          >
            <v-file-input
              v-model="certificado"
              accept=".cer*"
              label="Certificado (.cer)"
              required
              :error-messages="certificadoErrors"
            ></v-file-input>
          </v-col>

          <v-col
            cols="12"
            md="6"
          >
            <v-text-field
              v-model="lastName"
              :error-messages="lastNameErrors"
              label="Last Name"
              placeholder="Last Name"
              required
              @input="$v.lastName.$touch()"
              @blur="$v.lastName.$touch()"
            ></v-text-field>
          </v-col>

          <v-col
            cols="12"
            md="6"
          >
            <v-file-input
              v-model="key"
              accept=".key*"
              label="Key"
              required
              :error-messages="keyErrors"
            ></v-file-input>
          </v-col>

          <v-col
            cols="12"
            md="6"
          >
            <v-text-field
              v-model="email"
              :error-messages="emailErrors"
              label="E-mail"
              required
              @input="$v.email.$touch()"
              @blur="$v.email.$touch()"
            ></v-text-field>
          </v-col>

          <v-col
            cols="12"
            md="6"
          >
            <v-text-field
              v-model="password"
              label="Password"
              type="password"
              :error-messages="passwordErrors"
              placeholder="....."
              required
              @input="$v.password.$touch()"
              @blur="$v.password.$touch()"
            ></v-text-field>
          </v-col>

          <v-col>
          </v-col>

          <v-col
            cols="12"
            sm="6"
            md="4"
          >
            <v-dialog
              ref="dialog"
              v-model="modal"
              :return-value.sync="date"
              persistent
              width="290px"
            >
              <template v-slot:activator="{ on, attrs }">
                <v-text-field
                  v-model="date"
                  label="Ultimo Mes Descargado"
                  readonly
                  v-bind="attrs"
                  v-on="on"
                ></v-text-field>
              </template>
              <v-date-picker
                v-model="date"
                scrollable
              >
                <v-spacer></v-spacer>
                <v-btn
                  text
                  color="primary"
                  @click="modal = false"
                >
                  Cancel
                </v-btn>
                <v-btn
                  text
                  color="primary"
                  @click="$refs.dialog.save(date)"
                >
                  OK
                </v-btn>
              </v-date-picker>
            </v-dialog>
          </v-col>

          <v-col cols="12">
            <v-btn
              color="primary"
              :disabled="$v.$invalid"
              @click.prevent="submit"
            >
              Register
            </v-btn>
            <v-btn
              type="reset"
              outlined
              class="mx-2"
            >
              Reset
            </v-btn>
          </v-col>
        </v-row>
      </v-form>
    </v-card-text>

    <!-- create new account  -->
    <v-card-text class="d-flex align-center justify-center flex-wrap mt-2">
      <span class="me-2">
        Already have an account?
      </span>
      <v-card-actions>
        <v-btn
          text
          color="primary"
          @click="reveal = true"
        >
          Login
        </v-btn>
      </v-card-actions>
    </v-card-text>
    <v-alert
      border="top"
      color="error"
      :value="error"
      icon="close"
      dark
    >
      Please Complete The Form Register
    </v-alert>
  </v-card>
</template>

<script>

// validators
import { validationMixin } from 'vuelidate'
import { required, email, minLength } from 'vuelidate/lib/validators'

// import axios from 'axios'

export default {
  mixins: [validationMixin],

  validations: {
    firstName: { required },
    lastName: { required },
    email: { required, email },
    certificado: { required },
    key: { required },
    date: { required },
    password: { required, minLength: minLength(8) },
  },
  data: () => ({
    firstName: '',
    lastName: '',
    email: '',
    password: '',
    certificado: null,
    key: null,
    date: (new Date(Date.now() - (new Date()).getTimezoneOffset() * 60000)).toISOString().substr(0, 10),
    modal: false,
    error: false,
  }),

  computed: {
    firstNameErrors() {
      const errors = []
      if (!this.$v.firstName.$dirty) return errors
      // eslint-disable-next-line no-unused-expressions
      !this.$v.firstName.required && errors.push('Name is required.')

      return errors
    },
    lastNameErrors() {
      const errors = []
      if (!this.$v.lastName.$dirty) return errors
      // eslint-disable-next-line no-unused-expressions
      !this.$v.lastName.required && errors.push('Name is required.')

      return errors
    },
    emailErrors() {
      const errors = []

      if (!this.$v.email.$dirty) return errors
      // eslint-disable-next-line no-unused-expressions
      !this.$v.email.email && errors.push('Must be valid e-mail')
      // eslint-disable-next-line no-unused-expressions
      !this.$v.email.required && errors.push('E-mail is required')

      return errors
    },
    passwordErrors() {
      const errors = []
      if (!this.$v.password.$dirty) return errors
      // eslint-disable-next-line no-unused-expressions
      !this.$v.password.required && errors.push('The Password Is Required')
      // eslint-disable-next-line no-unused-expressions
      !this.$v.password.minLength && errors.push('password must have more than 8 characters')

      return errors
    },
    certificadoErrors() {
      const errors = []
      if (!this.$v.certificado.$dirty) return errors
      // eslint-disable-next-line no-unused-expressions
      !this.$v.certificado.required && errors.push('Plaese add a certificed')

      return errors
    },
    keyErrors() {
      const errors = []
      if (!this.$v.key.$dirty) return errors
      // eslint-disable-next-line no-unused-expressions
      !this.$v.key.required && errors.push('Plaese add a key')

      return errors
    },
  },

  methods: {
    submit() {
      this.$v.$touch()
    },
  },
  clear() {
    this.$v.$reset()
    this.lastName = ''
    this.lastName = ''
    this.email = ''
    this.password = ''
  },
}
</script>

<style lang="scss">
@import '~@/plugins/vuetify/default-preset/preset/pages/auth.scss';
</style>
