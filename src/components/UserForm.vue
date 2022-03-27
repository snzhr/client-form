<template>
  <div class="user-form">
    <transition name="slide-fade">
      <Toast :valid="$v.$invalid" v-show="showToast">
        <p>{{ message }}</p>
      </Toast>
    </transition>
    <form class="form" @submit.prevent="send">
      <div class="user-details">
        <UserInput
          label="Фамилия"
          type="text"
          id="lastname"
          v-model="person.lastname"
          :isInvalid="$v.person.lastname.$dirty && !$v.person.lastname.required"
          ><span
            v-if="$v.person.lastname.$dirty && !$v.person.lastname.required"
            class="error-text"
            >Поле обязательное для заполнения</span
          >
          <span
            v-if="$v.person.lastname.$dirty && !$v.person.lastname.alpha"
            class="error-text"
            >Фамилия не может содержать цифры</span
          >
        </UserInput>
        <UserInput
          v-model="person.name"
          :isInvalid="$v.person.name.$dirty && !$v.person.name.required"
          label="Имя"
          type="text"
          id="name"
        >
          <span
            v-if="$v.person.name.$dirty && !$v.person.name.required"
            class="error-text"
            >Поле обязательное для заполнения</span
          >
          <span
            v-if="$v.person.name.$dirty && !$v.person.name.alpha"
            class="error-text"
            >Имя не может содержать цифры</span
          >
        </UserInput>
        <UserInput
          v-model="person.middlename"
          label="Отчество"
          type="text"
          id="middlename"
        />
        <UserInput
          v-model="person.birth_date"
          label="Дата рождения"
          type="date"
          id="birthdate"
          :isInvalid="
            $v.person.birth_date.$dirty && !$v.person.birth_date.required
          "
        >
          <span
            v-if="$v.person.birth_date.$dirty && !$v.person.birth_date.required"
            class="error-text"
            >Поле обязательное для заполнения</span
          >
        </UserInput>
        <UserInput
          v-model="person.phone"
          label="Номер телефона"
          type="number"
          id="phone"
          :isInvalid="$v.person.phone.$dirty && !$v.person.phone.required"
        >
          <span
            v-if="$v.person.phone.$dirty && !$v.person.phone.required"
            class="error-text"
            >Поле обязательное для заполнения</span
          >
          <span v-if="!$v.person.phone.maxLength" class="error-text">
            Максимум 11 цифр.</span
          >
          <span v-if="!$v.person.phone.minLength" class="error-text">
            Осталось
            {{ $v.person.phone.$params.minLength.min - person.phone.length }}
            цифр.</span
          >
          <span
            v-if="$v.person.phone.$dirty && !$v.person.phone.startsWith"
            class="error-text"
            >Номер телефона должен начинаться с цифры "7"</span
          >
        </UserInput>
        <UserSelect
          label="Пол"
          :data="['Муж.', 'Жен.']"
          v-model="person.sex"
          id="sex"
        />
        <UserSelect
          label="Лечащий врач"
          :data="['Иванов', 'Захаров', 'Чернышева']"
          v-model="person.personal_doctor"
          id="personal_doctor"
        />
        <label class="multi-select" for="client_group"
          ><span>Группа клиентов</span>
          <select
            :class="{
              error:
                $v.person.client_group.$dirty &&
                !$v.person.client_group.required,
            }"
            id="client_group"
            multiple
            v-model="person.client_group"
          >
            <option value="vip">VIP</option>
            <option value="problem">Проблемные</option>
            <option value="oms">ОМС</option>
          </select>
          <span
            v-if="
              $v.person.client_group.$dirty && !$v.person.client_group.required
            "
            class="error-text"
            >Поле обязательное для заполнения</span
          >
        </label>
        <UserCheckbox
          v-model="person.sms"
          label="Не отправлять СМС"
          id="send_sms"
        />
      </div>
      <div class="user-details">
        <UserInput
          v-model="person.address.index"
          label="Индекс"
          type="number"
          id="index"
        />
        <UserInput
          v-model="person.address.country"
          label="Страна"
          type="text"
          id="country"
        />
        <UserInput
          v-model="person.address.region"
          label="Область"
          type="text"
          id="region"
        />
        <UserInput
          v-model="person.address.city"
          label="Город"
          type="text"
          id="city"
          :isInvalid="
            $v.person.address.city.$dirty && !$v.person.address.city.required
          "
        >
          <span
            v-if="
              $v.person.address.city.$dirty && !$v.person.address.city.required
            "
            class="error-text"
            >Поле обязательное для заполнения</span
          >
        </UserInput>
        <UserInput
          v-model="person.address.street"
          label="Улица"
          type="text"
          id="street"
        />
        <UserInput
          v-model="person.address.apartment"
          label="Дом"
          type="text"
          id="apartment"
        />
      </div>
      <div class="user-details passport">
        <UserSelect
          label="Тип документа"
          :data="['Паспорт', 'Свидетельство о рождении', 'Вод. удостоверение']"
          v-model="person.passport.document_type"
          id="document_type"
          :isInvalid="
            $v.person.passport.document_type.$dirty &&
            !$v.person.passport.document_type.required
          "
        >
          <span
            v-if="
              $v.person.passport.document_type.$dirty &&
              !$v.person.passport.document_type.required
            "
            class="error-text"
            >Поле обязательное для заполнения</span
          >
        </UserSelect>
        <UserInput
          v-model="person.passport.series"
          label="Серия"
          type="text"
          id="passport_series"
        />
        <UserInput
          v-model="person.passport.id"
          label="Номер"
          type="text"
          id="passport_id"
        />
        <UserInput
          v-model="person.passport.issued_by"
          label="Кем выдан"
          type="text"
          id="issued_by"
        />
        <UserInput
          v-model="person.passport.date_of_issue"
          label="Дата выдачи"
          type="date"
          id="date_of_issue"
          :isInvalid="
            $v.person.passport.date_of_issue.$dirty &&
            !$v.person.passport.date_of_issue.required
          "
        >
          <span
            v-if="
              $v.person.passport.date_of_issue.$dirty &&
              !$v.person.passport.date_of_issue.required
            "
            class="error-text"
            >Поле обязательное для заполнения</span
          >
        </UserInput>
      </div>
      <div>
        <input type="submit" value="Отправить" />
      </div>
    </form>
  </div>
</template>

<script>
import {
  required,
  minLength,
  maxLength,
  alpha,
} from "vuelidate/lib/validators";
const startsWith = (param) => (value) =>
  value.toString().split("")[0] === param;
import UserInput from "./ui/UserInput.vue";
import UserSelect from "./ui/UserSelect.vue";
import UserCheckbox from "./ui/UserCheckbox.vue";
import Toast from "./ui/Toast.vue";
export default {
  components: { UserInput, UserSelect, UserCheckbox, Toast },
  data() {
    return {
      message: "",
      showToast: false,
      person: {
        lastname: "",
        name: "",
        middlename: "",
        birth_date: "",
        phone: "",
        sex: "",
        client_group: [],
        personal_doctor: "",
        sms: false,
        address: {
          index: "",
          country: "",
          region: "",
          city: "",
          street: "",
          apartment: "",
        },
        passport: {
          document_type: "",
          series: "",
          id: "",
          issued_by: "",
          date_of_issue: "",
        },
      },
    };
  },
  validations: {
    person: {
      lastname: { required, alpha },
      name: { required, alpha },
      birth_date: { required },
      phone: {
        required,
        minLength: minLength(11),
        maxLength: maxLength(11),
        startsWith: startsWith("7"),
      },
      client_group: { required },
      address: { city: { required } },
      passport: { document_type: { required }, date_of_issue: { required } },
    },
  },
  watch: {
    showToast(val) {
      if (val) {
        setTimeout(() => {
          this.showToast = false;
        }, 3000);
      }
    },
  },
  methods: {
    send() {
      if (this.$v.$invalid) {
        this.$v.$touch();
        this.message = "Заполните форму корректно.";
        // return;
      } else {
        this.message = "Новый клиент успешно создан.";
      }
      this.showToast = true;
    },
  },
};
</script>
<style lang="scss" scoped>
@import "@/assets/_common.scss";

.user-form {
  padding: 2em;
  background-color: #5f9ea0;
  .user-details {
    background-color: white;
    display: flex;
    flex-wrap: wrap;
    justify-content: space-between;
    width: 70%;
    margin: 1em auto;
    padding: 2em;
    border-radius: 0.2em;
  }

  .multi-select {
    @include form-item-style;

    select {
      @include select-styles;
      @include font-styles;
      margin-top: 0.5em;

      &:focus {
        @include field-focus;
      }
      option {
        margin: 0.2em 0;

        &:checked {
          background-color: #b0c4de;
        }
      }
    }
  }
  .error-text {
    color: red !important;
    font-size: 14px;
    margin-top: 0.4em;
  }
  .error {
    box-shadow: tomato 0px 0px 0px 2px !important;
  }

  input[type="submit"] {
    background-color: white;
    border: none;
    padding: 1em 2em;
    display: block;
    margin: 0 auto;
    border-radius: 0.3em;
    cursor: pointer;
  }
}

.slide-fade-enter-active {
  transition: all 0.3s ease;
}
.slide-fade-leave-active {
  transition: all 0.3s ease;
}
.slide-fade-enter,
.slide-fade-leave-to {
  transform: translateX(100px);
  opacity: 0;
}

@media screen and (max-width: 767px) {
  .user-form {
    padding: 1em;

    .user-details {
      width: 100%;
      padding: 1em;
    }
  }
}
@media screen and (min-width: 768px) and (max-width: 1280px) {
  .user-form {
    padding: 1em;

    .user-details {
      width: 100%;
      padding: 1.5em;
    }
  }
}
</style>
