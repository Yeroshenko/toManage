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
          <span v-if="!$v.email.email">Email не коректный</span>
          <span v-else-if="!$v.email.required">Введите email</span>
        </template>
      </vs-input>
      <vs-input
        v-model="password"
        primary
        name="password"
        class="auth-form__input"
        type="password"
        placeholder="Пароль"
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
          <span v-else-if="!$v.email.required">Введите пароль</span>
        </template>
      </vs-input>

      <vs-input
        v-model="rePassword"
        primary
        name="rePassword"
        class="auth-form__input"
        type="password"
        placeholder="Повторный пароль"
      >
        <template #icon>
          <LockIcon class="svg-icon" />
        </template>
        <!-- validation messages -->
        <template
          v-if="$v.rePassword.$dirty && $v.rePassword.$error"
          #message-danger
        >
          <span v-if="!$v.password.sameAsPassword">Пароли не совпадают</span>
        </template>
      </vs-input>

      <vs-checkbox
        v-model="remember"
        class="auth-form__checkbox"
        name="remember"
      >
        Запомнить меня
      </vs-checkbox>

      <vs-button size="large">Зарегистрироваться</vs-button>

      <div class="auth-form__dialog">
        Есть акаунт?
        <nuxt-link to="/" class="auth-form__link">Войти</nuxt-link>
      </div>
    </form>
  </AuthFormWrapper>
</template>

<script>
import { required, email, minLength, sameAs } from 'vuelidate/lib/validators'

import AuthFormWrapper from '@/components/AuthFormWrapper'
import LockIcon from '@/assets/icons/lock.svg'
import AtIcon from '@/assets/icons/at.svg'

export default {
  layout: 'auth',
  components: { AuthFormWrapper, LockIcon, AtIcon },

  data: () => ({
    email: '',
    password: '',
    rePassword: '',
    remember: true
  }),

  validations: {
    email: { required, email },
    password: { required, minLength: minLength(6) },
    rePassword: { required, sameAsPassword: sameAs('password') }
  },

  methods: {
    handleSubmit(e) {
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
