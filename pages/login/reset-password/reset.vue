<script setup lang="ts">
const router = useRouter()
</script>

<template>
  <div class="flex flex-column h-full justify-content-center">
    <div class="surface-card mb-2 login-card pt-6 sm:pt-8 pb-5 px-4 sm:px-5 shadow-2 border-round w-full lg:w-6 fadeinright animation-duration-1000 animation-iteration-1">
      <div class="mb-5  text-black">
        <h2 class="text-3xl sm:text-5xl font-semibold mb-1">
          Сбросьте пароль!
        </h2>
        <p class="text-base sm:text-xl font-medium ">
          сас
        </p>
      </div>

      <div>

        <div class="w-full mb-2">
          <span class="p-input-icon-left text-black w-full">
            <i class="pi pi-lock font-medium" style="margin-left: 20px; color: black" />
            <InputText
                type="password"
                toggle-mask
                placeholder="Пароль"
                class="pl-7 w-full mb-1 font-medium"
            />
          </span>
<!--          <small v-if="validations.password.show" class="p-error ml-3">{{ validations.password.message }}</small>-->
        </div>
        <div class="w-full mb-2">
          <span class="p-input-icon-left text-black w-full">
            <i class="pi pi-lock font-medium" style="margin-left: 20px; color: black" />
            <InputText
                type="password"
                toggle-mask
                placeholder="Подтвердить пароль"
                class="pl-7 w-full mb-1 font-medium"
            />
          </span>
          <!--          <small v-if="validations.password.show" class="p-error ml-3">{{ validations.password.message }}</small>-->
        </div>
        <div class="flex align-items-center justify-content-end mb-4 text-gray-500 flex-wrap">
          <span class="font-medium line-height-3">Нет аккаунт?</span>
          <a class="font-semibold no-underline ml-2 text-blue-500 cursor-pointer " @click="router.push('/login/register')" style="color: black !important;"><span class="hover-underline">Создать аккаунт</span></a>
        </div>
        <Button icon="pi pi-user " class="w-full login-button">
          Проверить
        </Button>
      </div>
    </div>
  </div>
</template>
