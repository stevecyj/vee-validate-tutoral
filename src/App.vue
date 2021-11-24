<script>
import { useForm, useField } from 'vee-validate';
import * as yup from 'yup';

export default {
  name: 'App',

  setup() {
    const onSubmit = () => {
      console.log('submit here');
    };

    // 定義規則內容
    // function isRequired(value) {
    //   if (value && value.trim()) {
    //     return true;
    //   }
    //   return "This is required";
    // }

    // {錯誤訊息，驗證內容}
    const { errorMessage, value } = useField('fieldInput', yup.string().required().min(8));

    const simpleSchema = yup.object({
      name: yup.string().required('請輸入帳號'),
      email: yup.string().required('請輸入email').email('請輸入正確的email格式'),
    });
    useForm({ validationSchema: simpleSchema });

    const { value: name, errorMessage: nameError } = useField('name');
    const { value: email, errorMessage: emailError } = useField('email');

    return { onSubmit, errorMessage, value, name, nameError, email, emailError };
  },
};
</script>

<template>
  <div id="app">
    <form @submit.prevent="onSubmit">
      <label for="name">name</label>
      <input id="name" type="text" name="text" v-model="name" />
      <span display:inline-block>{{ nameError }}</span>
      <!-- separate -->
      <label for="email">email</label>
      <input id="email" name="email" v-model="email" />
      <span>{{ emailError }}</span>
      <button>Sign up for newsletter</button>
    </form>
  </div>
</template>

<style>
span {
  display: block;
}
</style>
