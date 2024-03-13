<!-- Inside your Vue component template -->

<template>
  <v-app>
    <v-container :style="{
      backgroundColor: 'rgba(255, 255, 255, 0)',
    }" fluid>
      <v-col cols="12" class="text-center" style="
      font-family: 'Roboto, Helvetica, sans-serif, Open Sans, Arial';
      margin-top: 2rem;
    ">
        <v-row no-gutters>
          <v-col cols="12" sm="6" class="pr-4" v-for="(field, index) in leadFormFields" :key="index">
            <template v-if="index === text_after" >
              <v-col :color="text_color"
                :style="{ fontSize: text_size + 'px', fontWeight: '700', marginBottom: '5px', marginTop: 0 }">
                {{ text }}
              </v-col>
            </template>
           
            <!-- Render TextFields based on question_type -->
            <template v-if="field.question_type === 'shortanswer' || field.question_type === 'longanswer'">
              <v-text-field :label="`${field.question_text}${field.required ? '*' : ''}`"
                :placeholder="`${field.question_text}${field.required ? '*' : ''}`" :color="`${captionColor}`"
                variant="outlined" :style="styles" @blur="handleBlur"></v-text-field>

            </template>

            <!-- Render Select boxes based on question_type -->

            <template v-else-if="field.question_type === 'multiplechoice'">
              <v-select v-model="fieldValue[index]" :label="`${field.question_text}${field.required ? '*' : ''}`"
                :style="styles" :items="field.options" :outlined="true" :color="`${captionColor}`" :height="24"
                :full-width="true" variant="outlined" @change="handleChange(index, $event)"></v-select>
            </template>
          </v-col>
        </v-row>
      </v-col>
    </v-container>
  </v-app>
</template>


<script>
import { defineComponent, ref } from 'vue';

import { useVuelidate } from '@vuelidate/core';
import { required } from '@vuelidate/validators';


export default defineComponent({
  setup() {

    const fieldValue = ref([]);
    const inputFontSizePx = 14;
    const inputBorderPx = 2;
    const inputBorderSolid = 'solid';
    const inputBorderColor = '#CCCCFF';
    const buttonHoverColor = '#CCCCFF'; // Define buttonHoverColor if needed
    const captionColor = '#6366F1';
    const captionBg = 'white'; // Define captionBg if needed
    const text_after = 4
    const text_color = "#263238"
    const text_size = '18 px'
    const text = "Inform my result to"


    const styles = {
      '--v-field-border-color': inputBorderColor,
      '--v-field-caption-color': captionColor,
      '--v-field-border-width-size': `${inputBorderPx}px`
    };


    const value = ref('');

    const validations = {
      value: { required },
    };

    const v$ = useVuelidate(validations, { value });

    const handleBlur = () => {
      v$.value.$touch();
    };

    return {
      fieldValue,
      inputFontSizePx,
      inputBorderPx,
      inputBorderSolid,
      inputBorderColor,
      buttonHoverColor,
      captionColor,
      captionBg,
      styles,
      value,
      handleBlur,
      text_after,
      text_color,
      text,
      text_size
    };
  },
  data() {
    return {
      // Sample data for leadFormFields array
      leadFormFields: [
        { question_text: 'Full Name', required: true, question_type: 'shortanswer' },
        { question_text: 'FD Amount', required: true, question_type: 'shortanswer' },
        { question_text: 'Residential Status', required: true, question_type: 'multiplechoice', options: ['Singaporean', 'Singaporean PR', 'Singapore Pass Holder', 'Foreigner'] },
        { question_text: 'Deposit Tenure', required: true, question_type: 'multiplechoice', options: ['3 months', '4 months', '5 months', '6 months', '7 months', '8 months', '9 months', '10 months', 'More than 10 months'] },
        { question_text: 'Singapore Mobile', required: true, question_type: 'shortanswer' },
        { question_text: 'Email', required: true, question_type: 'shortanswer' },

      ],
      fieldValue: [] // Array to store field values
    };
  },
  methods: {
    handleChange(index, value) {
      // Handle change event
      console.log(`Field ${index + 1} changed:`, value);
    }
  }
});
</script>



<style>
.v-field--variant-outlined .v-field__outline__start,
.v-field--variant-outlined .v-field__outline__notch::before,
.v-field--variant-outlined .v-field__outline__notch::after,
.v-field--variant-outlined .v-field__outline__end,
.v-field-label {
  border-color: var(--v-field-border-color) !important;
  --v-field-border-opacity: 1;
  --v-field-border-width: var(--v-field-border-width-size);
}

.v-field--variant-outlined .v-label.v-field-label--floating {
  color: var(--v-field-caption-color) !important;
}
</style>