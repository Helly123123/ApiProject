<template>
  <div>
    <table>
      <thead>
        <tr>
          <th class="table-login">ЛОГИН</th>
          <th class="table-step">ШАГ</th>
          <th class="table-action">ДЕЙСТВИЕ</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(item, index) in data" :key="index">
          <td class="table-text-number">{{ item.login }}</td>
          <td class="table-text">{{ item.step }}</td>
          <td class="table-action-text">
            <button
              class="action-table-button"
              @click="openModal($event, item)"
            >
              <img
                src="/telegramAccount/menu_table_button.svg"
                alt=""
              />Действия
            </button>
          </td>
        </tr>
      </tbody>
    </table>

    <div v-if="isModalOpen" class="black-fon" @click="closeModal">
      <div
        class="action-list"
        :style="{
          top: modalPosition.top + 'px',
          left: modalPosition.left + 'px',
        }"
      >
        <span class="action" @click="performAction('Action 1')"
          >Действие 1</span
        >
        <span class="action" @click="performAction('Action 2')"
          >Действие 2</span
        >
        <span class="action" @click="performAction('Action 3')"
          >Действие 3</span
        >
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      data: [
        { login: "+7 (922) 855-69-98", step: "" },
        { login: "+7 (922) 855-69-98", step: "" },
        { login: "+7 (922) 855-69-98", step: "" },
      ],
      isModalOpen: false,
      modalPosition: { top: 0, left: 0 },
      selectedItem: null,
    };
  },
  methods: {
    openModal(event, item) {
      this.selectedItem = item;
      this.isModalOpen = true;
      const rect = event.target.getBoundingClientRect();
      this.modalPosition = {
        top: rect.bottom + window.scrollY,
        left: rect.left + window.scrollX,
      };
    },
    closeModal() {
      this.isModalOpen = false;
    },
    performAction(action) {
      alert(`Вы выбрали ${action} для ${this.selectedItem.login}`);
      this.closeModal();
    },
  },
};
</script>

<style scoped>
table {
  width: 100%;
  border-collapse: collapse;
}

.table-login {
  text-align: left;
  padding: 8px 8px 8px 20px;
  width: 200px;
}

.table-step {
  text-align: left;
  padding: 8px;
  width: 200px;
}

.table-action {
  text-align: right;
  padding: 8px;
}

.table-text-number {
  padding: 20px;
}

.table-text {
  padding: 8px;
}

.table-action-text {
  padding: 8px;
  text-align: right;
}

.action-table-button {
  border-radius: 5px;
  padding: 9px 11px;
  background: rgba(73, 80, 202, 0.2);
  font-weight: 600;
  font-size: 14px;
  color: #4047ca;
}

.action-table-button img {
  margin-right: 10px;
}

th,
td {
  padding: 8px;
  font-weight: 500;
  font-size: 14px;
  color: #989898;
}

td {
  font-weight: 600;
  font-size: 14px;
  color: #000;
  text-align: left;
}

tbody {
  width: 300px;
  overflow-x: auto;
  white-space: nowrap;
}

thead {
  border: 1px solid #d9d9d9;
  height: 50px;
  background: #efefef;
}

.black-fon {
  position: fixed;
  z-index: 5;
  width: 100%;
  height: 100vh;
  opacity: 0.3;
  background: rgba(3, 3, 3, 0.3);
  top: 0;
  left: 0;
}

.action-list {
  border-radius: 10px;
  width: 170px;
  height: auto;
  background: #ffffff;
  position: absolute;
  z-index: 10;
  display: flex;
  align-items: flex-start;
  justify-content: center;
  flex-direction: column;
  gap: 10px;
  padding-left: 20px;
}

.action {
  font-weight: 500;
  font-size: 15px;
  color: #000;
  cursor: pointer;
}
</style>

Найти еще
