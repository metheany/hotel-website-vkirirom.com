<template>
  <v-form name="Login" v-model="valid" @submit.prevent="">
    <v-container fluid class="pa-0">
      <v-row no-gutters>
        <v-flex xs12 class="text-center">
          <p class="light--text body-2">
            Looks like you already have an account. Please log in instead.
          </p>
          <v-divider class="is-small light-border mx-auto"></v-divider>
        </v-flex>
        <div class="d-flex justify-center align-center pa-6 w-100">
          <v-avatar color="primary user-select-none">
            <span class="white--text headline">BG</span>
          </v-avatar>
        </div>
        <v-flex xs12 class="light--text text-center body-2">
          <p class="mb-2">Bill</p>
          <p class="mb-6">bill.gates@outlook.com</p>
        </v-flex>
        <v-flex xs12>
          <v-text-field
            dark
            class="mb-1"
            v-model="password"
            outlined
            label="Password"
            name="Password"
            color="light"
            required
            :type="isPasswordVisible ? 'text' : 'password'"
            :rules="rules.password"
          >
            <v-icon slot="append">$vuetify.icons.lock</v-icon>
          </v-text-field>
        </v-flex>
        <v-flex xs12>
          <v-btn
            x-large
            block
            color="primary"
            style="font-size:16px;"
            class="py-3 text-capitalize mb-3 dark--text"
            dark
            @click="login()"
            type="submit"
            :disabled="!valid"
            :loading="loading"
          >
            <v-spacer></v-spacer>
            <span>Log in</span>
            <v-spacer></v-spacer>
            <v-icon>keyboard_arrow_right</v-icon>
          </v-btn>
        </v-flex>
        <v-flex xs12 class="my-3">
          <v-divider class="light-border"></v-divider>
        </v-flex>

        <v-flex xs12 sm6 md12 class="text-center text-sm-left text-md-center light--text body-2">
          <p class="my-3 my-sm-0 my-md-3">
            <a @click="updateActiveState('auth-login')">Login with a different account</a>
          </p>
        </v-flex>

        <v-flex xs12 sm6 md12 class="text-center body-2 text-sm-right text-md-center">
          <p class="mb-0">
            <a @click="updateActiveState('auth-forgot-password')">Forgot password?</a>
          </p>
        </v-flex>
      </v-row>
    </v-container>
  </v-form>
</template>

<script lang="ts">
// import SeparatorOr from '@/components/SeparatorOr.vue'
import store from '@/store';

export default {
  name: 'auth-login',
  components: {
    // SeparatorOr
  },
  data() {
    return {
      valid: false,
      isPasswordVisible: false,
      rules: {
        password: [v => !!v || 'Password is required']
      }
    };
  },
  methods: {
    updateActiveState(value) {
      (this as any).$store.dispatch('auth/updateActiveState', value);
    },
    async login() {
      try {
        await (this as any).$store.dispatch('auth/loginStandard');
        (this as any).$store.dispatch('snackbar/show', {
          color: 'success',
          text: 'Logging in was successful',
          class: 'dark--text'
        });
      } catch (error) {
        console.log('wrong credentials');
      }
    }
  },
  computed: {
    email: {
      get() {
        return (this as any).$store.getters['auth/email'];
      },
      set(value) {
        (this as any).$store.commit('auth/updateEmail', value);
      }
    },
    password: {
      get() {
        return (this as any).$store.getters['auth/password'];
      },
      set(value) {
        (this as any).$store.commit('auth/updatePassword', value);
      }
    },
    loading() {
      return (this as any).$store.getters['auth/loading'];
    },
    loginError() {
      return (this as any).$store.getters['auth/loginError'];
    }
  }
};
</script>

<style lang="scss">
@import '@/styles/utility.scss';
</style>
