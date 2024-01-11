<script setup lang="ts">
import { reactive } from 'vue'
import axios from 'axios'
import { useToast } from 'primevue/usetoast'
import { useUserStore } from '~/stores/useUserStore'

const router = useRouter()
const model = reactive({
  userLogin: {
    login: '',
    password: ''
  },
  user: {
    firstName: '',
    lastName: '',
    age: null
  }
})
const userStore = useUserStore()
const validations = reactive({
  firstName: {
    show: false,
    message: 'Имя обязательно!',
    validate: () => {
      if (model.user.firstName === '') {
        validations.firstName.show = true
      } else {
        validations.firstName.show = false
      }
      return !validations.firstName.show
    }
  },
  lastName: {
    show: false,
    message: 'Фамилия обязательно!',
    validate: () => {
      if (model.user.lastName === '') {
        validations.lastName.show = true
      } else {
        validations.lastName.show = false
      }
      return !validations.lastName.show
    }
  },
  age: {
    show: false,
    message: 'Возраст обязательно!',
    messageReal: 'Введите реальный возраст',
    validate: () => {
      if (model.user.age ! >= 130 || model.user.age ! <= 1) {
        showError(validations.age.messageReal)
        // eslint-disable-next-line eqeqeq
      } else if (model.user.age == 0 || model.user.age == null) {
        validations.age.show = true
      } else {
        validations.age.show = false
      }

      return !validations.age.show
    }
  },
  login: {
    show: false,
    message: 'Логин обязательно!',
    validate: () => {
      if (model.userLogin.login === '') {
        validations.login.show = true
      } else {
        validations.login.show = false
      }
      return !validations.login.show
    }
  },
  password: {
    show: false,
    message: 'Пароль обязательно!',
    validate: () => {
      if (model.userLogin.password === '') {
        validations.password.show = true
      } else {
        validations.password.show = false
      }
      return !validations.password.show
    }
  }
})
const toast = useToast()
const validate = () => {
  for (const validationItem of Object.values(validations)) {
    const valid = validationItem.validate()
    if (!valid) { return false }
  }
  return true
}
const submit = async () => {
  const valid = validate()
  if (!valid) {
    showError('Форма содержит ошибки. Пожалуйста, проверьте введенные данные.')

    return
  }
  try {
    const { data } = await axios.post('https://localhost:7123/User/Registration', model)
    userStore.user = data
    router.push('/')
    showSuccess()
  } catch (error) {
    showError(error as string)
  }
}
const showSuccess = () => {
  toast.add({ severity: 'success', summary: 'Success Message', detail: 'Message Content', life: 3000 })
  return true
}
const showError = (error:string) => {
  toast.add({ severity: 'error', summary: 'Ошибка', detail: `${error}`, life: 3000 })
}
</script>
<template>
  <div class="flex flex-column h-full align-items-baseline">
    <div class="surface-card mb-2 relative login-card pt-6 lg:pt-8 pb-5 px-4 sm:px-5 shadow-2 border-round w-full lg:w-6 fadeinright animation-duration-1000 animation-iteration-1">
      <div class="mb-5  text-black">
        <h2 class="text-3xl sm:text-5xl font-semibold mb-1">
          Добро пожаловать
        </h2>
        <p class="text-base sm:text-xl font-medium ">
          давайте познакомим как вам обращатся?
        </p>
      </div>

      <div>
        <div class="w-full mb-3">
          <span class="p-input-icon-left text-black w-full">
            <i class="pi pi-user font-medium" style="margin-left: 20px; color: black" />
            <InputText
              v-model="model.user.firstName"
              type="text"
              placeholder="Имя"
              class="pl-7 w-full mb-1 font-medium"
              :class="{'p-invalid': !validations.firstName.message}"
              @input="validations.firstName.validate()"
            />
          </span>
          <small v-if="validations.firstName.show" class="p-error ml-3">{{ validations.firstName.message }}</small>
        </div>
        <div class="flex justify-content-between align-items-start sm:gap-3 flex-wrap sm:flex-nowrap ">
          <div class="w-8 mb-3">
            <div class="p-input-icon-left text-black w-full">
              <i class="pi pi-user font-medium" style="margin-left: 20px; color: black" />
              <InputText
                v-model="model.user.lastName"
                type="text"
                placeholder="Фамилия"
                class="pl-7 w-full mb-1 font-medium"
                :class="{'p-invalid': !validations.lastName.message}"
                @input="validations.lastName.validate()"
              />
            </div>
            <small v-if="validations.lastName.show" class="p-error ml-3 ">{{ validations.lastName.message }}</small>
          </div>
          <div class="w-4 mb-3">
            <div class="p-input-icon-left text-black w-full">
              <i class="pi pi-user font-medium" style="margin-left: 20px; color: black" />
              <InputText
                id="age"
                v-model="model.user.age"
                type="number"
                placeholder="Возраст"
                class="pl-7 w-full mb-1 font-medium"
                min="0"
                max="130"
                :class="{'p-invalid': !validations.age.message}"
                @input="validations.age.validate()"
              />
            </div>
            <small v-if="validations.age.show" class="p-error ml-3">{{ validations.age.message }}</small>
          </div>
        </div>
        <div class="w-full mb-3">
          <div class="p-input-icon-left text-black w-full">
            <i class="pi pi-user font-medium" style="margin-left: 20px; color: black" />
            <InputText
              v-model="model.userLogin.login"
              type="email"
              placeholder="email"
              class="pl-7 w-full mb-1 font-medium"
              :class="{'p-invalid': !validations.login.message}"
              @input="validations.login.validate()"
            />
          </div>
          <small v-if="validations.login.show" class="p-error ml-3">{{ validations.login.message }}</small>
        </div>
        <div class="w-full mb-4">
          <span class="p-input-icon-left text-black w-full">
            <i class="pi pi-lock font-medium" style="margin-left: 20px; color: black" />
            <InputText
              v-model="model.userLogin.password"
              type="password"
              toggle-mask
              placeholder="Пароль"
              class="pl-7 w-full mb-1 font-medium"
              @input="validations.password.validate()"
            />
          </span>
          <small v-if="validations.password.show" class="p-error ml-3">{{ validations.password.message }}</small>
        </div>

        <Button icon="pi pi-user " class="w-full login-button" @click="submit">
          Регистрация
        </Button>
        <div class="h-1rem bg-white absolute left-50 top-100 px-2" style="transform: translate(-50%,-50%)">
          <span>или</span>
        </div>
      </div>
    </div>
    <div class="surface-card btn-account pt-6 pb-5 px-5 h-full flex justify-content-center align-items-center flex-column fadeinright animation-duration-1000 animation-delay-200 animation-iteration-1">
      <div class="flex justify-content-between gap-3 w-full mb-4 px-0 sm:px-8 md:px-0 lg:px-6">
        <Button class="flex justify-content-center align-items-center w-6 font-semibold">
          <i class="pi pi-google mr-3" /> Google
        </Button>
        <Button class="flex justify-content-center align-items-center w-6 font-semibold">
          <i class="pi pi-facebook mr-3" /> Facebook
        </Button>
      </div>
      <div class="flex align-items-center justify-content-center mb-2 text-gray-500 flex-wrap">
        <span class=" font-medium line-height-3">Есть аккаунт?</span>
        <a class="font-medium no-underline ml-2 text-blue-500 cursor-pointer" @click="router.push('/login/auth')"><span class="hover-underline text-black">Войти в аккаунт</span></a>
      </div>

      <p class="font-medium line-height-3 text-gray-500 text-center">
        Создавая учетную запись или подписывая контракт, вы соглашаетесь с нашими <span class="text-black cursor-pointer hover-underline">Условиями использования.</span>
      </p>
    </div>
    <div>
      <!--      <form-->
      <!--          class="glass-card surface-card p-4 shadow-2 border-round w-full lg:w-6 fadeindown animation-duration-1000 animation-iteration-1"-->
      <!--      >-->
      <!--        <div class="text-center mb-5">-->
      <!--          <h2 class="text-3xl font-medium mb-3 text-white">-->
      <!--            Регистрация-->
      <!--          </h2>-->
      <!--        </div>-->

      <!--        <div>-->
      <!--          <div class="w-full mb-3">-->
      <!--            <label for="name" class="block  font-medium mb-2 text-white">Имя</label>-->
      <!--            <InputText-->
      <!--                id="name"-->
      <!--                v-model="model.user.firstName"-->
      <!--                type="text"-->
      <!--                class="w-full"-->
      <!--                :class="{'p-invalid': !validations.firstName.message}"-->
      <!--                @input="validations.firstName.validate()"-->
      <!--            />-->
      <!--            <small v-if="validations.firstName.show" class="p-error">{{ validations.firstName.message }}</small>-->
      <!--          </div>-->
      <!--          <div class="flex justify-content-between align-items-start gap-3">-->
      <!--            <div class="w-full mb-3">-->
      <!--              <label for="suname" class="block  font-medium mb-2 text-white">Фамилия</label>-->
      <!--              <InputText-->
      <!--                  id="suname"-->
      <!--                  v-model="model.user.lastName"-->
      <!--                  type="text"-->
      <!--                  class="w-full "-->
      <!--                  :class="{'p-invalid': !validations.lastName.message}"-->
      <!--                  @input="validations.lastName.validate()"-->
      <!--              />-->
      <!--              <small v-if="validations.lastName.show" class="p-error">{{ validations.lastName.message }}</small>-->
      <!--            </div>-->

      <!--            <div class="w-full mb-3">-->
      <!--              <label for="age" class="block  font-medium mb-2 text-white">Возраст</label>-->
      <!--              <InputText-->
      <!--                  id="age"-->
      <!--                  v-model="model.user.age"-->
      <!--                  type="number"-->
      <!--                  class="w-full"-->
      <!--                  min="1"-->
      <!--                  max="130"-->
      <!--                  :class="{'p-invalid': !validations.age.message}"-->
      <!--                  @input="validations.age.validate()"-->
      <!--              />-->
      <!--              <small v-if="validations.age.show" class="p-error">{{ validations.age.message }}</small>-->
      <!--            </div>-->
      <!--          </div>-->

      <!--          <div class="w-full mb-3">-->
      <!--            <label for="login" class="block  font-medium mb-2 text-white">Логин</label>-->
      <!--            <InputText-->
      <!--                id="login"-->
      <!--                v-model="model.userLogin.login"-->
      <!--                type="text"-->
      <!--                class="w-full"-->
      <!--                :class="{'p-invalid': !validations.login.message}"-->
      <!--                @input="validations.login.validate()"-->
      <!--            />-->
      <!--            <small v-if="validations.login.show" class="p-error">{{ validations.login.message }}</small>-->
      <!--          </div>-->
      <!--          <div class="w-full mb-3">-->
      <!--            <label for="password" class="block  font-medium mb-2 text-white">Пароль</label>-->
      <!--            <Password-->
      <!--                id="password"-->
      <!--                v-model="model.userLogin.password"-->
      <!--                type="password"-->
      <!--                class="w-full"-->
      <!--                :class="{'p-invalid': !validations.password.message}"-->
      <!--                toggle-mask-->
      <!--                @input="validations.password.validate()"-->
      <!--            />-->
      <!--            <small v-if="validations.password.show" class="p-error">{{ validations.password.message }}</small>-->
      <!--          </div>-->

      <!--          <div class="flex align-items-center justify-content-center mb-6">-->
      <!--            <span class=" font-medium line-height-3">Есть аккаунт?</span>-->
      <!--            <a class="font-medium no-underline ml-2 text-blue-500 cursor-pointer" @click="router.push('/login/auth')">Войти-->
      <!--              в аккаунт</a>-->
      <!--          </div>-->

      <!--          <Button label="Регистрация" icon="pi pi-user" class="w-full" @click="submit" />-->
      <!--        </div>-->
      <!--      </form>-->
      <Toast />
    </div>
  </div>
</template>
<style>
input[type="number"] {
  -moz-appearance: textfield;
  appearance: textfield;
  margin: 0; /* Убираем отступы, которые могут создавать счётчик */
}

/* Скрываем стрелочки счётчика */
input[type="number"]::-webkit-inner-spin-button,
input[type="number"]::-webkit-outer-spin-button {
  -webkit-appearance: none;
  appearance: none;
  margin: 0;
}
</style>
