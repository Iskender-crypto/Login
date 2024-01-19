<script setup lang="ts">
import axios from 'axios'
import { reactive } from 'vue'
import { useToast } from 'primevue/usetoast'
import { useUserStore } from '~/stores/useUserStore'
const router = useRouter()
const userStore = useUserStore()
const loginUser = reactive({
  login: '',
  password: ''
})
const validations = reactive({
  login: {
    show: false,
    message: 'логин обязательно!',
    validate: () => {
      if (loginUser.login === '') {
        validations.login.show = true
      } else {
        validations.login.show = false
      }
      return !validations.login.show
    }
  },
  password: {
    show: false,
    message: 'пароль обязательно!',
    validate: () => {
      if (loginUser.password === '') {
        validations.password.show = true
      } else {
        validations.password.show = false
      }
      return !validations.password.show
    }
  }
})

const validate = () => {
  for (const validationItem of Object.values(validations)) {
    const valid = validationItem.validate()
    if (!valid) { return false }
  }
  return true
}
const toast = useToast()
const signIn = async () => {
  const valid = validate()
  if (!valid) {
    showError('Форма содержит ошибки. Пожалуйста, проверьте введенные данные.')
    return
  }
  try {
    const { data } = await axios.post('https://localhost:5232/User/Authenticate', loginUser)
    userStore.user = data
    await router.push('/')
    await showSuccess()
  } catch (error) {
    console.log('catch error')
    showError(error as string)
  }
}
const showSuccess = () => {
  toast.add({ severity: 'success', summary: 'Success Message', detail: 'Message Content', life: 3000 })
}
const showError = (error:string) => {
  toast.add({ severity: 'error', summary: 'Ошибка', detail: `${error}`, life: 3000 })
}
</script>
<template>
  <div class="flex flex-column h-full align-items-baseline">
    <div class="surface-card mb-2 login-card pt-6 sm:pt-8 pb-5 px-4 sm:px-5 shadow-2 border-round w-full lg:w-6 fadeinright animation-duration-1000 animation-iteration-1">
      <div class="mb-5  text-black">
        <h2 class="text-3xl sm:text-5xl font-semibold mb-1">
          C возвращением!
        </h2>
        <p class="text-base sm:text-xl font-medium ">
          Как поживаете?
        </p>
      </div>

      <div>
        <div class="w-full mb-3">
          <span class="p-input-icon-left text-black w-full">
            <i class="pi pi-user font-medium" style="margin-left: 20px; color: black" />
            <InputText v-model="loginUser.login" placeholder="Логин" class="pl-7 w-full mb-1 font-medium" @input="validations.login.validate()" />
          </span>
          <small v-if="validations.login.show" class="p-error ml-3">{{ validations.login.message }}</small>
        </div>
        <div class="w-full mb-2">
          <span class="p-input-icon-left text-black w-full">
            <i class="pi pi-lock font-medium" style="margin-left: 20px; color: black" />
            <InputText
              v-model="loginUser.password"
              type="password"
              toggle-mask
              placeholder="Пароль"
              class="pl-7 w-full mb-1 font-medium"
              @input="validations.password.validate()"
            />
          </span>
          <small v-if="validations.password.show" class="p-error ml-3">{{ validations.password.message }}</small>
        </div>

        <div class="flex align-items-center justify-content-end mb-5 ">
          <span
            class="text-black font-medium text-sm line-height-3 cursor-pointer hover-underline"
            @click="router.push('/login/reset-password/check')"
          >Забыли пароль?
          </span>
        </div>
        <Button icon="pi pi-user " class="w-full login-button" @click="signIn">
          Войти
        </Button>
      </div>
    </div>
    <div class="surface-card pt-6 pb-5 px-5 h-full flex justify-content-center align-items-center flex-column fadeinright animation-duration-1000 animation-delay-200 animation-iteration-1">
      <div class="flex align-items-center justify-content-center mb-2 text-gray-500 flex-wrap">
        <span class="font-medium line-height-3">Нет аккаунт?</span>
        <a class="font-semibold no-underline ml-2 text-blue-500 cursor-pointer " style="color: black !important;" @click="router.push('/login/register')"><span class="hover-underline">Создать аккаунт</span></a>
      </div>

      <p class="font-medium line-height-3 text-gray-500 text-center">
        Создавая учетную запись или подписывая контракт, вы соглашаетесь с нашими <span class="text-black cursor-pointer hover-underline">Условиями использования.</span>
      </p>
    </div>
  </div>
</template>
