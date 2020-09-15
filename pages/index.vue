<template>
  <AuthFormWrapper>
    <form class="auth-form" @submit.prevent="handleSubmit">
      <vs-input
        v-model="email"
        primary
        name="email"
        class="auth-form__input"
        placeholder="E-mail"
      >
        <template #icon>
          <AtIcon class="svg-icon" />
        </template>
        <!-- validation messages -->
        <template v-if="$v.email.$dirty && $v.email.$error" #message-danger>
          <span v-if="!$v.email.email"> Email не коректный </span>
          <span v-else-if="!$v.email.required"> Введите email </span>
        </template>
      </vs-input>
      <vs-input
        v-model="password"
        primary
        name="password"
        class="auth-form__input"
        placeholder="Пароль"
        type="password"
      >
        <template #icon>
          <LockIcon class="svg-icon" />
        </template>
        <!-- validation messages -->
        <template
          v-if="$v.password.$dirty && $v.password.$error"
          #message-danger
        >
          <span v-if="!$v.password.minLength">
            Минимальная длина {{ $v.password.$params.minLength.min }} символов
          </span>
          <span v-else-if="!$v.email.required"> Введите пароль </span>
        </template>
      </vs-input>

      <vs-checkbox
        v-model="remember"
        class="auth-form__checkbox"
        name="remember"
      >
        Запомнить меня
      </vs-checkbox>

      <vs-button size="large">Войти</vs-button>

      <div class="auth-form__dialog">
        Впервые здесь?
        <nuxt-link to="/register" class="auth-form__link">
          Зарегестрироавться
        </nuxt-link>
      </div>
    </form>
  </AuthFormWrapper>
</template>

<script>
import { required, email, minLength } from 'vuelidate/lib/validators'

import AuthFormWrapper from '@/components/AuthFormWrapper'
import LockIcon from '@/assets/icons/lock.svg'
import AtIcon from '@/assets/icons/at.svg'

export default {
  layout: 'auth',
  components: { AuthFormWrapper, LockIcon, AtIcon },

  data: () => ({
    email: '',
    password: '',
    remember: true
  }),

  validations: {
    email: { required, email },
    password: { required, minLength: minLength(6) }
  },

  methods: {
    handleSubmit(e) {
      console.log(this.$v)
      // stop here if form is invalid
      this.$v.$touch()
      if (this.$v.$invalid) {
        return
      }

      alert('OK')
    }
  }
}
</script>
