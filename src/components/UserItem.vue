<template >
  <ul class="users__content_row">
    <li>{{ user.username }}</li>
    <li>{{ user.email }}</li>
    <li>{{
      new Date(user.registration_date).getDay() + '.' + new Date(user.registration_date).getMonth() + '.' + new
        Date(user.registration_date).getFullYear()
    }}</li>
    <li>{{ user.rating }}</li>
    <li class="remove"><img class="remove__img" src="../assets/close.svg" alt="Delete" @click="onOpen"></li>
  </ul>

  <div class="dialog" v-if="isOn" @click="hideDialog">
    <div class="dialog__content" @click.stop>
      <h2>Вы уверены, что хотите удалить пользователя?</h2>
      <div class="btns">
        <button class="btn" @click="yesDelete">Да</button>
        <button class="btn active" @click="noDelete">Нет</button>
      </div>
    </div>
  </div>
</template>
<script>
export default {
  name: 'user-item',
  data() {
    return {
      isOn: false
    }
  },
  props: {
    user: {
      type: Object,
      required: true
    }
  },
  methods: {
    onOpen() {
      this.isOn = true
    },
    hideDialog() {
      this.isOn = false
    },
    noDelete() {
      this.isOn = false
    },
    yesDelete() {
      this.isOn = false
      this.$emit('del', this.user)
    }
  }
}
</script>
<style scoped>
li {
  font-family: 'Inter';
  font-weight: 700;
  font-size: 12px;
  line-height: 16px;
  color: #ACACAC;
  width: 200px;
  /* width: 100%; */
  position: relative;
}

.users__content_row {
  border-top: 1px solid #F7F7F7;
  padding-top: 14px;
  padding-bottom: 20px;
  display: flex;
  justify-content: space-between;
  list-style: none;
  width: 927px;
  align-items: center;
}

.users__content_row li:first-child {
  color: #0CB4F1;
}

.users__content_row li:last-child {
  display: flex;
  justify-content: start;
  align-items: center;
  width: 36px;
  cursor: pointer;
}

/* .remove {
 
} */

.remove__img {
  width: 10px;
  height: 10px;
}

/* dialog */
.dialog {
  top: 0;
  right: 0;
  bottom: 0;
  left: 0;
  position: fixed;
  background: rgba(0, 0, 0, 0.5);
  display: flex;
  justify-content: center;
  align-items: center;
  z-index: 2;
}

.dialog__content {
  position: absolute;
  margin: auto;
  background: #fff;
  padding: 20px;
  width: 353px;
  height: 134px;
  border-radius: 8px;
  z-index: 3;
}

h2 {
  margin: 24px auto;
  font-family: 'Inter';
  font-weight: 400;
  font-size: 12px;
  line-height: 15px;
  color: #000000;
  text-align: center;
}

.btns {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding-left: 60px;
  padding-right: 60px;
}

.btn {
  padding: 6px 36px;
  width: 88px;
  height: 27px;
  background: #E0E0E0;
  border-radius: 3px;
  font-family: 'Inter';
  font-weight: 500;
  font-size: 12px;
  line-height: 15px;
  color: #828282;
  border: none;
  outline: none;
  cursor: pointer;
  transition: all .3s linear;
}

.btn:hover {
  background: #0CB4F1;
  color: #fff;
}

.btn.active {
  background: #0CB4F1;
  color: #fff;
}
</style>