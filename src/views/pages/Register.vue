<template>
  <div class="auth-wrapper auth-v1">
    <div>
      <v-container>
        <v-row
          :align="center"
          no-gutters
        >
          <v-col
            cols="12"
            md="6"
          >
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
                        v-model="event.firstName"
                        :error-messages="nameErrors"
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
                        accept=".cer*"
                        label="Certificado (.cer)"
                        required
                      ></v-file-input>
                    </v-col>

                    <v-col
                      cols="12"
                      md="6"
                    >
                      <v-text-field
                        v-model="event.lastName"
                        :error-messages="lastNameErrors"
                        :counter="10"
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
                        accept="image/*"
                        label="Key"
                      ></v-file-input>
                    </v-col>

                    <v-col
                      cols="12"
                      md="6"
                    >
                      <v-text-field
                        v-model="event.email"
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
                        v-model="event.password"
                        label="Password"
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
                        v-model="event.modal"
                        :return-value.sync="event.date"
                        persistent
                        width="290px"
                      >
                        <template v-slot:activator="{ on, attrs }">
                          <v-text-field
                            v-model="event.date"
                            label="Ultimo Mes Descargado"
                            readonly
                            v-bind="attrs"
                            v-on="on"
                          ></v-text-field>
                        </template>
                        <v-date-picker
                          v-model="event.date"
                          scrollable
                        >
                          <v-spacer></v-spacer>
                          <v-btn
                            text
                            color="primary"
                            @click="event.modal = false"
                          >
                            Cancel
                          </v-btn>
                          <v-btn
                            text
                            color="primary"
                            @click="$refs.dialog.save(event.date)"
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
                      >
                        Register
                      </v-btn>
                      <p
                        v-if="$v.$anyError"
                        class="errorMessage"
                      >
                        please fill out the required form
                      </p>
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
                <router-link :to="{ name:'pages-login' }">
                  Sign in instead
                </router-link>
              </v-card-text>

              <!-- divider -->
              <v-card-text class="d-flex align-center mt-2">
                <v-divider></v-divider>
                <span class="mx-5">or</span>
                <v-divider></v-divider>
              </v-card-text>
            </v-card>
          </v-col>
        </v-row>
      </v-container>
    </div>

    <!-- tree -->
    <v-img
      class="auth-tree"
      width="247"
      height="185"
      src="@/assets/images/misc/tree.png"
    ></v-img>

    <!-- tree  -->
    <v-img
      class="auth-tree-3"
      width="377"
      height="289"
      src="@/assets/images/misc/tree-3.png"
    ></v-img>
  </div>
</template>

<script>
import { validationMixin } from 'vuelidate'
import { required, email, maxLength } from 'vuelidate/lib/validators'

export default {
  mixins: [validationMixin],

  validations: {
    firstName: { required },
    lastName: { required, maxLength: maxLength(10) },
    email: { required, email },
    password: { required },
  },

  data: () => ({
    event: {
      firstName: '',
      lastName: '',
      email: '',
      password: '',
      date: (new Date(Date.now() - (new Date()).getTimezoneOffset() * 60000)).toISOString().substr(0, 10),
      menu: false,
    },
  }),

  computed: {
    nameErrors() {
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
      !this.$v.lastName.maxLength && errors.push('Name must be at most 10 characters long')
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
  },

  methods: {
    sumbit() {
      this.$v.$touch()
      if (!this.$v.$invalid) {
        console.log('form Submitted', this.email)
      }
    },
    clear() {
      this.$v.$reset()
      this.lastName = ''
      this.lastName = ''
      this.email = ''
      this.password = ''
    },
  },
}
</script>

<style lang="scss">
@import '~@/plugins/vuetify/default-preset/preset/pages/auth.scss';
</style>
