<template>
  <section v-if="!station.phone" class="qr-whatsapp-section">
    <LoadingModal :stationLoading="stationLoading" />
    <article v-if="qrCodeData.station">
      <qrcode-vue :value="qrCodeData.link" :size="256" />
      <h2 @click="getPhone" class="title">Связать через ввод кода</h2>
    </article>
  </section>
  <section v-if="station.phone" class="number-section">
    <input
      :class="station.inpPhone ? 'num-input-error' : 'num-input'"
      placeholder="7 (900) 000-000"
      @input="formatPhone"
      class="num-input"
      type="text"
      id="phone"
      v-model="phone"
      required
    />
    <button @click="getCode" class="next-button">Далее</button>
  </section>
</template>

<script setup>
import { inject, ref, reactive, onMounted, onBeforeUnmount } from "vue";
import axios from "axios";
import QrcodeVue from "qrcode.vue";
import LoadingModal from "../LoadingModal.vue";
const { changeEnableStation } = inject("changeEnableStation");
const { selectedItem, startFunc, offQrCodeStation } = inject("accountItems");
const { source, login } = selectedItem.value;

const station = reactive({
  phone: false,
});

const stationLoading = ref(false);
const qrCodeData = reactive({
  link: "",
  station: false,
});

let intervalId = null;
let previousLink = "";

const phone = ref("7 ");

const formatPhone = () => {
  const cleaned = phone.value.replace(/\D/g, "");

  if (cleaned.length === 0) {
    phone.value = "7 ";
    return;
  }

  const match = cleaned.match(/^(7)?(\d{0,3})(\d{0,3})(\d{0,4})$/);

  if (match) {
    phone.value = `7 (${match[2]}) ${match[3]}${
      match[4] ? "-" + match[4] : ""
    }`;
  }
};

const getInternationalFormat = () => {
  return phone.value.replace(/\D/g, "");
};

const forceStop = async () => {
  stationLoading.value = true;
  try {
    const response = await axios.post(
      `https://b2288.apitter.com/instances/forceStop`,
      {
        source: source,
        login: login,
      },
      {
        headers: {
          "Content-Type": "application/json; charset=utf-8",
          Authorization: "Bearer 342b63fd-6017-446f-adf8-d1b8e0b7bfc6",
        },
      }
    );
    if (response.data.ok === true) {
      console.log(response.data);
    } else {
      console.log(response.data.ok);
    }
  } catch (error) {
    console.error(`${request} - Ошибка`, error);
    if (error.response) {
      console.error("Ошибка сервера:", error.response.data);
    }
  }
};

const disablePhoneAuth = async () => {
  const internationalPhone = getInternationalFormat();
  try {
    const response = await axios.post(
      "https://b2288.apitter.com/instances/disablePhoneAuth",
      {
        source: source,
        login: login,
        phone: internationalPhone,
      },
      {
        headers: {
          "Content-Type": "application/json; charset=utf-8",
          Authorization: "Bearer 342b63fd-6017-446f-adf8-d1b8e0b7bfc6",
        },
      }
    );

    if (response.data.ok === true) {
      console.log("Аунтефикация 0ff");
      console.log(response.data);
    } else {
      console.log(response.data.ok);
    }
  } catch (error) {
    console.error("Ошибка при создании аккаунта:", error);
    if (error.response) {
      console.error("Ошибка сервера:", error.response.data);
    }
  }
};

const setState = async () => {
  try {
    const response = await axios.post(
      "https://b2288.apitter.com/instances/setState",
      {
        source: source,
        login: login,
        setState: true,
      },
      {
        headers: {
          "Content-Type": "application/json; charset=utf-8",
          Authorization: "Bearer 342b63fd-6017-446f-adf8-d1b8e0b7bfc6",
        },
      }
    );

    if (response.data.ok === true) {
      console.log("Состояние установлено");
      console.log(response.data);
    } else {
      console.log(response.data.ok);
    }
  } catch (error) {
    console.error("Ошибка при создании аккаунта:", error);
    if (error.response) {
      console.error("Ошибка сервера:", error.response.data);
    }
  }
};

const enablePhoneAuth = async () => {
  stationLoading.value = true;
  try {
    const response = await axios.post(
      `https://b2288.apitter.com/instances/enablePhoneAuth`,
      {
        source: source,
        login: login,
        phone: "79228556998",
      },
      {
        headers: {
          "Content-Type": "application/json; charset=utf-8",
          Authorization: "Bearer 342b63fd-6017-446f-adf8-d1b8e0b7bfc6",
        },
      }
    );
    if (response.data.ok === true) {
      console.log(response.data);
    } else {
      console.log(response.data.ok);
    }
  } catch (error) {
    console.error(`${request} - Ошибка`, error);
    if (error.response) {
      console.error("Ошибка сервера:", error.response.data);
    }
  }
};

const getQr = async () => {
  try {
    const response = await axios.post(
      "https://b2288.apitter.com/instances/getQr",
      {
        source: source,
        login: login,
      },
      {
        headers: {
          "Content-Type": "application/json; charset=utf-8",
          Authorization: "Bearer 342b63fd-6017-446f-adf8-d1b8e0b7bfc6",
        },
      }
    );

    if (response.data.data.status === "ok") {
      previousLink = qrCodeData.link; // Сохраняем предыдущую ссылку
      qrCodeData.link = response.data.data.value;
      qrCodeData.station = true;
      stationLoading.value = false;
    } else {
      // Если значение пустое, останавливаем запросы
      if (!response.data.data.value) {
        clearInterval(intervalId);
        qrCodeData.link = previousLink; // Отображаем предыдущую ссылку
        changeEnableStation();
      }
    }
  } catch (error) {
    console.error("Ошибка при создании аккаунта:", error);
    if (error.response) {
      console.error("Ошибка сервера:", error.response.data);
    }
  }
};

const EnablebyQR = async () => {
  console.log(source);
  await forceStop();
  await disablePhoneAuth();
  await setState();
  await getQr();

  let count = 0;
  intervalId = setInterval(async () => {
    await getQr();
    count++;
    if (count >= 6) {
      clearInterval(intervalId);
      changeEnableStation();
    }
  }, 20000); // 20 секунд
};

const closeModal = () => {
  // Логика для закрытия модального окна
  qrCodeData.station = false; // Сброс состояния QR-кода
};

const getPhone = async () => {
  station.phone = true;
};

const getCode = async () => {
  await offQrCodeStation();
  await enablePhoneAuth();
  await startFunc();
};

onMounted(() => {
  EnablebyQR();
});

onBeforeUnmount(() => {
  clearInterval(intervalId);
});
</script>

<style scoped>
.title {
  margin-top: 16px;
  font-weight: 500;
  font-size: 18px;
  text-align: center;
  padding: 4px;
  background-color: rgb(243, 243, 243);
  border-radius: 5px;
}

.number-section {
  display: flex;
  flex-direction: column;
}

.num-input {
  border-radius: 5px;
  padding-left: 10px;
  width: 350px;
  height: 45px;
  font-weight: 400;
  font-size: 14px;
  color: #000;
  border: 0.5px solid #c1c1c1;
  background: #fcfcfc;
}

.num-input-error {
  border-radius: 5px;
  padding-left: 10px;
  width: 350px;
  height: 45px;
  font-weight: 400;
  font-size: 14px;
  color: #000;
  border: 0.5px solid #be2424;
  background: #ffeaea;
}

.next-button {
  width: 365px;
  height: 35px;
  border-radius: 5px;
  background-color: #4950ca;
  font-size: 14px;
  color: rgb(255, 255, 255);
  font-weight: 600;
  margin-top: 20px;
}
</style>
