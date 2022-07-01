<template>
  <form :class="$style.VForm">
    <div :class="$style.cells">
      <div v-if="inputName"
           :class="$style.row"
      >
        <VFormCell :class="$style.cell"
                   required
        >
          <template #label>
            Имя
          </template>

          <template v-if="$v.name.$error"
                    #error
          >
            {{ !$v.name.required ? errorMsg.required : errorMsg.alpha }}
          </template>

          <VInput v-model="name"
                  type="text"
                  placeholder="Введите имя"
                  :error="$v.name.$error"
                  :max-length="maxLengthName"
                  :class="$style.input"
          />
        </VFormCell>
      </div>

      <div v-if="inputPhone"
           :class="$style.row"
      >
        <VFormCell :class="$style.cell"
                   required
        >
          <template #label>
            Tелефон
          </template>

          <template v-if="$v.phone.$error"
                    #error
          >
            {{ !$v.phone.phone ? errorMsg.phone : errorMsg.required }}
          </template>

          <VMaskedInput v-model="phone"
                        type="tel"
                        placeholder="Введите телефон"
                        :error="$v.phone.$error"
                        :class="$style.input"
          />
        </VFormCell>
      </div>

      <div v-if="selectOptions.length"
           :class="$style.row">
        <VFormCell :class="$style.cell">
          <template #label>
            Начальник
          </template>

          <VSelect :class="[$style.select, $style.input]"
                   :options="selectOptions"
                   :value="option"
                   @change="handleChangeOption"
          />

        </VFormCell>
      </div>
    </div>

    <VButton size="medium"
             color="gray"
             :class="$style.button"
             @click="handleSubmit"
    >
      Сохранить
    </VButton>
  </form>
</template>

<script>
// library imports
import { validationMixin } from 'vuelidate';
import {
  required,
  minLength,
  maxLength,
  helpers,
} from 'vuelidate/lib/validators';
import _ from 'lodash';

// components
import VButton from '@/components/ui/button/VButton';
import VMaskedInput from '@/components/ui/input/VMaskedInput';
import VFormCell from '@/components/ui/formcell/VFormCell';
import VInput from '@/components/ui/input/VInput';
import VSelect from '@/components/ui/select/VSelect';

export default {
  name: 'VForm',

  components: {
    VInput,
    VMaskedInput,
    VButton,
    VFormCell,
    VSelect,
  },

  mixins: [validationMixin],

  props: {
    inputName: {
      type: Boolean,
      default: false,
    },

    inputPhone: {
      type: Boolean,
      default: false,
    },

    selectOptions: {
      type: Array,
      default: () => [],
    },
  },

  data() {
    return {
      name: null,
      phone: null,
      option: null,
      errorMsg: {
        required: 'Обязательное поле',
        email: 'Введите верный email',
        alpha: 'Заполните поле корректно',
        phone: 'Проверьте корректность номера',
      },
      maxLengthName: 15,
    };
  },

  beforeDestroy() {
    this.resetForm();
  },

  validations() {
    let validations = {};
    const alpha = helpers.regex('alpha', /^[^-][-\sa-zA-Zа-яА-ЯЁё]+$/);
    const phone = helpers.regex('phone', /^(\+7)(\s)([0-9]{3})(\s)([0-9]{3})(\s)([0-9]{2})(\s)([0-9]{2})$/);

    if (this.inputName) {
      validations = {
        ...validations,
        name: {
          required,
          alpha,
          minLength: minLength(2),
          maxLength: maxLength(this.maxLengthName),
        },
      };
    }

    if (this.inputPhone) {
      validations = { ...validations, phone: { required, phone } };
    }

    return validations;
  },

  methods: {
    handleSubmit() {
      this.$v.$touch();
      if (!this.$v.$invalid) {
        const data = {
          name: this.name,
          phone: this.phone,
          id: _.uniqueId(),
          parent: this.option,
          children: [],
        };

        this.$emit('set-data', data);
        this.$emit('close');
      }
    },

    handleChangeOption(value) {
      this.option = value;
    },

    resetForm() {
      this.name = null;
      this.phone = null;
      this.option = null;

      this.$v.$reset();
    },
  },
};
</script>

<style lang="scss" module>
  .VForm {
    display: flex;
    flex-direction: column;
    align-items: flex-start;
    justify-content: space-between;
    overflow: hidden;
    height: 100%;
    padding: 2rem;
  }

  .top {
    margin-bottom: 1.6rem;
  }

  .row {
    display: flex;
    margin-bottom: 3rem;

    @include respond-to(mobile) {
      margin-bottom: 24px;
    }
  }

  .cell {
    flex-grow: 1;
  }

  .button {
    flex-shrink: 0;

    @include respond-to(tablet) {
      width: 100%;
    }
  }

  .input {
    width: 20rem;
    height: 3rem;
    padding: .5rem;
    border: 1px solid #e9eff4;
  }

  .select {
    z-index: 2;
    margin-bottom: 4rem;
    padding: 0;
  }
</style>
