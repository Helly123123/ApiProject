<template>
  <ErrorBlock v-if="errorBlock" :changeIncorrectPassword="chaneErrorBlock" />
  <section class="send-message">
    <div class="img-cont">
      <img class="file-img" src="/chats/file.svg" alt="" />
      <article class="smile-img-cont">
        <input
          class="send-message-input"
          placeholder="Введите сообщение"
          type="text"
          v-model="messageText"
        />
        <img class="smile-img" src="/chats/smile.svg" alt="" />
      </article>
      <img @click="sendMessage" class="send-img" src="/chats/send.svg" alt="" />
    </div>
  </section>
</template>

<script setup>
import { ref, toRefs, defineEmits } from "vue";
import axios from "axios";
import { useRouter } from "vue-router";
const router = useRouter();
import ErrorBlock from "@/components/ErrorBlock/ErrorBlock.vue";
const props = defineProps({
  chatInfo: {
    type: Object,
  },
  messages: {
    type: Array,
    default: () => [],
  },
});

const emit = defineEmits(["updateMessages"]);
const { chatInfo } = toRefs(props);
const errorBlock = ref(false);
const chaneErrorBlock = () => {
  errorBlock.value = errorBlock.value;
};
const messageText = ref("");

const sendMessage = async () => {
  console.log(22794591901);
  const message = {
    to: `${chatInfo.value.phone}`,
    text: messageText.value || null,
    content: messageText.value
      ? []
      : [{ type: "text", src: messageText.value }],
    time: Date.now() / 1000, // Установка времени сообщения
    outgoing: true, // Указывает, что это исходящее сообщение
  };

  try {
    const response = await axios.post(
      "https://b2288.apitter.com/instances/sendMessage",
      {
        source: "whatsapp",
        login: "helly",
        msg: message,
      },
      {
        headers: {
          "Content-Type": "application/json; charset=utf-8",
          Authorization: `Bearer ${localStorage.getItem("accountToken")}`,
        },
      }
    );
    if (response.data === 401) {
      errorBlock.value = true;
      setTimeout(() => {
        localStorage.removeItem("accountToken");
        router.push("/login");
      }, 2000);
    }
    console.log("Сообщение отправлено:", response.data);
    emit("updateMessages", message); // Обновляем список сообщений
    messageText.value = "";
  } catch (error) {
    console.error("Ошибка при отправке сообщения:", error);
    if (error.response) {
      console.error("Ошибка сервера:", error.response.data);
    }
  }
};
</script>

<style scoped>
.send-message {
  width: 100%;
  padding: 10px 0px;
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 11px;
}

.img-cont {
  display: flex;
  align-items: center;
  gap: 14px;
  flex: 1;
}

.smile-img-cont {
  display: flex;
  position: relative;
  flex: 1;
}

.file-img,
.smile-img,
.send-img {
  cursor: pointer;
}

.send-img {
  margin-right: 20px;
}

.file-img {
  margin-left: 20px;
}

.smile-img {
  position: absolute;
  top: 11px;
  right: 12px;
}

.send-message-input {
  border-radius: 233px;
  width: 100%;
  height: 36px;
  border: 1px solid #d4d4d4;
  padding-left: 10px;
  box-sizing: border-box;
}
</style>
