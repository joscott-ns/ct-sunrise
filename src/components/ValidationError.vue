<template>
  <div v-if="vuelidate.$error"
       class="error">
    <div v-for="validation in validations"
         :key="validation">
      <div v-if="!vuelidate[validation]"
           :validation="validation"
           data-test="validation-error">
        {{ getErrorMessage(validation) }}
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    vuelidate: {
      type: Object,
      required: true,
    },
    customMessages: {
      type: Object,
      default() { return {}; },
    },
  },

  computed: {
    validations() {
      return Object.keys(this.vuelidate.$params);
    },
  },

  methods: {
    getErrorMessage(validation) {
      const customMessage = this.customMessages[validation];
      if (customMessage) {
        return customMessage;
      }
      const { type, ...args } = this.vuelidate.$params[validation];
      return this.$te(type) ? this.$t(type, args) : this.$t('unknownValidation');
    },
  },
};
</script>

<!-- eslint-disable -->
<i18n>
{
  "en": {
    "unknownValidation": "Invalid field",
    "required": "Required field",
    "email": "A valid email is required",
    "minLength": "It should contain at least {min} characters"
  },
  "de": {
    "unknownValidation": "Ungültiger Feldwert",
    "required": "Pflichtfeld",
    "email": "Eine gültige E-Mail ist erforderlich",
    "minLength": "Es sollte mindestens {min} Zeichen enthalten"
  }
}
</i18n>