<template>
  <div class="auth-page">
    <ShadowBlock>
      <h1 class="auth-page__title">To Manage</h1>
      <form class="auth-form">
        <vs-input
          v-model="email"
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
          name="password"
          class="auth-form__input"
          type="password"
          placeholder="Пароль"
        >
          <template #icon>
            <LockIcon class="svg-icon" />
          </template>
        </vs-input>

        <vs-input
          v-model="rePassword"
          name="rePassword"
          class="auth-form__input"
          type="password"
          placeholder="Повторный пароль"
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

        <vs-button size="large">Зарегестрироваться</vs-button>

        <div class="auth-form__dialog">
          Есть акаунт?
          <nuxt-link to="/" class="auth-form__link">Войти</nuxt-link>
        </div>
      </form>
    </ShadowBlock>
  </div>
</template>

<script>
import ShadowBlock from '@/components/ShadowBlock'

import LockIcon from '@/assets/icons/lock.svg'
import AtIcon from '@/assets/icons/at.svg'
import '@/styles/pages/auth.sass'

export default {
  layout: 'auth',
  components: { ShadowBlock, LockIcon, AtIcon },

  data: () => ({
    email: '',
    password: '',
    rePassword: '',
    remember: false
  })
}
</script>
