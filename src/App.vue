<script>
import { useField } from "vee-validate";
import * as yup from "yup";

export default {
  name: "App",

  setup() {
    const onSubmit = () => {
      console.log("submit here");
    };

    // 定義規則內容
    // function isRequired(value) {
    //   if (value && value.trim()) {
    //     return true;
    //   }
    //   return "This is required";
    // }

    // {錯誤訊息，驗證內容}
    const { errorMessage, value } = useField(
      "fieldInput",
      yup.string().required().min(8)
    );

    return { onSubmit, errorMessage, value };
  },
};
</script>

<template>
  <div id="app">
    <form @submit.prevent="onSubmit">
      <input type="text" name="text" v-model="value" />
      <span display:inline-block>{{ errorMessage }}</span>
      <button>Sign up for newsletter</button>
    </form>
  </div>
</template>

<style>
span {
  display: block;
}
</style>
