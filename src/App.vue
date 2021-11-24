<script>
import { computed } from "vue";
import { useForm, useField } from "vee-validate";
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

    const simpleSchema = yup.object({
      name: yup.string().required("請輸入帳號"),
      email: yup
        .string()
        .required("請輸入email")
        .email("請輸入正確的email格式"),
    });
    useForm({ validationSchema: simpleSchema });

    // 加入 handleBlur
    const validationListenersName = computed(() => {
      // If the field is valid or have not been validated yet
      // lazy
      if (!errorMessage.value) {
        return {
          blur: handleChangeName,
          change: handleChangeName,
          // disable `shouldValidate` to avoid validating on input
          input: (e) => handleChangeName(e, false),
        };
      }
      // Aggressive
      return {
        blur: handleChangeName,
        change: handleChangeName,
        input: handleChangeName, // only switched this
      };
    });
    const validationListenersEmail = computed(() => {
      // If the field is valid or have not been validated yet
      // lazy
      if (!errorMessage.value) {
        return {
          blur: handleChangeEmail,
          change: handleChangeEmail,
          // disable `shouldValidate` to avoid validating on input
          input: (e) => handleChangeEmail(e, false),
        };
      }
      // Aggressive
      return {
        blur: handleChangeEmail,
        change: handleChangeEmail,
        input: handleChangeEmail, // only switched this
      };
    });

    const {
      value: name,
      errorMessage: nameError,
      handleChange: handleChangeName,
      meta: metaName,
    } = useField("name");
    const {
      value: email,
      errorMessage: emailError,
      handleChange: handleChangeEmail,
    } = useField("email");

    return {
      onSubmit,
      errorMessage,
      value,
      name,
      nameError,
      metaName,
      email,
      emailError,
      validationListenersName,
      validationListenersEmail,
    };
  },
};
</script>

<template>
  <div id="app">
    <form @submit.prevent="onSubmit">
      <label for="name">name</label>
      <input
        id="name"
        type="text"
        name="text"
        v-model="name"
        v-on="validationListenersName"
      />
      <span display:inline-block>{{ nameError }}</span>
      <!-- separate -->
      <label for="email">email</label>
      <input
        id="email"
        name="email"
        v-model="email"
        v-on="validationListenersEmail"
      />
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
