<template>
  <div>
    <button @click="type = isSignUp ? 'signIn' : 'signUp'">
      {{
        isSignUp
          ?"Already keeping track of your goals? Sign in here."
          :"Got some goals you want to track? Click here to sign up."
      }}
    </button>
    <pre>{{ error }}</pre>
    <form @submit.prevent="handleSubmit">
      <FormControl label="email">
        <input v-model="credentials.email">
      </FormControl>
      <FormControl label="password">
        <input
          :type="show ? 'text' : 'password'"
          v-model="credentials.password">
        <button
          @click="show = !show"
          type="button"
        >
          {{ show ? 'Hide' : 'Show' }}
        </button>
      </FormControl>
      <FormControl>
        <button type="submit">
          {{ label }}
        </button>
      </FormControl>
    </form>
  </div>
</template>

<script>
import { signUp, signIn } from '../services/api';
import FormControl from './FormControl';

export default {
  props: {
    onUser: Function
  },
  components: {
    FormControl
  },
  data() {
    return {
      credentials: {
        email: '',
        password: ''
      },
      show: false,
      type: 'signIn',
      error: null
    };
  },
  computed: {
    isSignUp() {
      return this.type === 'signUp';
    },
    label() {
      return this.isSignUp ? 'Sign Up' : 'Sign In';
    }
  },
  watch: {
    type(newType, oldType) {
      if(newType !== oldType) {
        this.error = null;
      }
    }
  },
  methods: {
    handleSubmit() {
      this.error = null;
      const action = this.isSignUp ? signUp : signIn;
      action(this.credentials)
        .then(user => {
          this.onUser(user);
          this.$router.push('/');
        })
        .catch(err => {
          this.error = err;
        });
    }
  }
};
</script>

<style>

</style>
