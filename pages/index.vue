<template>
  <AuthFormWrapper>
    <form class="auth-form">
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
import AuthFormWrapper from '@/components/AuthFormWrapper'

import LockIcon from '@/assets/icons/lock.svg'
import AtIcon from '@/assets/icons/at.svg'

export default {
  layout: 'auth',
  components: { AuthFormWrapper, LockIcon, AtIcon },

  data: () => ({
    email: '',
    password: '',
    remember: false
  })
}
</script>
