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
            lg="10"
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
                <v-form class="multi-col-validation">
                  <v-row>
                    <v-col
                      cols="12"
                      md="6"
                    >
                      <v-text-field
                        v-model="firstName"
                        label="First Name"
                        outlined
                        dense
                        placeholder="First Name"
                        hide-details
                      ></v-text-field>
                    </v-col>

                    <v-col
                      cols="12"
                      md="6"
                    >
                      <v-file-input
                        accept="image/*"
                        label="Certificado (.cer)"
                      ></v-file-input>
                    </v-col>

                    <v-col
                      cols="12"
                      md="6"
                    >
                      <v-text-field
                        v-model="lastName"
                        label="Last Name"
                        outlined
                        dense
                        placeholder="Last Name"
                        hide-details
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
                        v-model="email"
                        label="Email"
                        outlined
                        dense
                        placeholder="Email  "
                        hide-details
                      ></v-text-field>
                    </v-col>

                    <v-col
                      cols="12"
                      md="6"
                    >
                      <v-text-field
                        v-model="password"
                        label="Password"
                        outlined
                        dense
                        placeholder="....."
                        hide-details
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
                            :prepend-icon="icons.mdiCalendar"
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
                      <v-btn color="primary">
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

              <!-- social link -->
              <v-card-actions class="d-flex justify-center">
                <v-btn
                  v-for="link in socialLink"
                  :key="link.icon"
                  icon
                  class="ms-1"
                >
                  <v-icon :color="$vuetify.theme.dark ? link.colorInDark:link.color">
                    {{ link.icon }}
                  </v-icon>
                </v-btn>
              </v-card-actions>
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
import {
  mdiFacebook, mdiTwitter, mdiGithub, mdiGoogle, mdiEyeOutline, mdiEyeOffOutline, mdiCalendar,
} from '@mdi/js'
import { ref } from '@vue/composition-api'

export default {
  data: () => ({
    modal: false,
    event: {
      firstName: '',
      lastName: '',
      Email: '',
      password: '',
    },
  }),
  setup() {
    const isPasswordVisible = ref(false)
    const username = ref('')
    const email = ref('')
    const password = ref('')
    const socialLink = [
      {
        icon: mdiFacebook,
        color: '#4267b2',
        colorInDark: '#4267b2',
      },
      {
        icon: mdiTwitter,
        color: '#1da1f2',
        colorInDark: '#1da1f2',
      },
      {
        icon: mdiGithub,
        color: '#272727',
        colorInDark: '#fff',
      },
      {
        icon: mdiGoogle,
        color: '#db4437',
        colorInDark: '#db4437',
      },
    ]

    return {
      isPasswordVisible,
      username,
      email,
      password,
      socialLink,

      icons: {
        mdiEyeOutline,
        mdiEyeOffOutline,
        mdiCalendar,
      },
    }
  },
}
</script>

<style lang="scss">
@import '~@/plugins/vuetify/default-preset/preset/pages/auth.scss';
</style>
