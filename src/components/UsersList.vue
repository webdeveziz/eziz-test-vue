<template >
  <div class="users__list">
    <h2 class="list__title">Список пользователей</h2>
    <div class="search__block">
      <div class="input__wrapper">
        <input type="text" class="input" :value="modelValue" @input="changing" placeholder="Поиск по имени или e-mail">
        <img class="search__img" src="../assets/search.svg" alt="Search">
      </div>
      <div class="clear" v-if="isTrue" @click="onClear">
        <img class="clear__img" src="../assets/clear.svg" alt="Clear">
        <p class="clear__text">Очистить фильтр</p>
      </div>
    </div>
    <div class="filters__block">
      <div class="sort">
        Сортировка: <span :class="{
          filter: true,
          active: isDate
        }" @click="onOpenClearTo">Дата регистрации</span>
        <span :class="{
          filter: true,
          active: isRating
        }" @click="onOpenClear">Рейтинг</span>
      </div>
    </div>
    <!-- users -->
    <div v-if="!loading" class="users">
      <div class="users__header">
        <ul class="users__header_nav">
          <li>Имя пользователя</li>
          <li>E-mail</li>
          <li>Дата регистрации</li>
          <li>Рейтинг</li>
          <li class="remove"></li>
        </ul>
      </div>
      <div v-if="users.length">
        <div class="users__content" v-for="user in users " :key="user.id">
          <!-- <transition-group name="user-list"> -->
          <UserItem :user="user" @del="$emit('del', user)" />
          <!-- </transition-group> -->
        </div>
      </div>
      <div v-else>
        <h3>Пользователей отсутствуют. Требуется перезагрузка страницы!</h3>
      </div>

    </div>
    <div v-else class="loading">
      <img src="../assets/loading.gif" alt="Loading...">
    </div>
  </div>
</template>

<script>
import UserItem from './UserItem.vue';

export default {
  name: "users-list",
  components: { UserItem },
  data() {
    return {
      isTrue: false,
      isRating: false,
      isDate: false,

    }
  },
  props: {
    users: {
      type: Array,
      default() {
        return [];
      }
    },
    modelValue: [String, Number],
    loading: {
      type: Boolean,
      default: () => false
    }
  },
  methods: {
    onOpenClear() {
      this.isTrue = true
      this.isRating = true
      this.isDate = false
      this.$emit('selrating', 'rating')
    },
    onOpenClearTo() {
      this.isTrue = true
      this.isRating = false
      this.isDate = true
      this.$emit('selrating', 'registration_date')
    },
    onClear() {
      this.isTrue = false
      this.isRating = false
      this.isDate = false
      this.$emit('selrating', '')
    },
    changing(event) {
      this.$emit('update:modelValue', event.target.value)
    }
  },
}
</script>

<style scoped>
.list__title {
  font-family: 'Inter';
  font-weight: 700;
  font-size: 24px;
  line-height: 28px;
  color: #333333;
}

.search__block {
  width: 961px;
  height: 102px;
  background: #FFFFFF;
  box-shadow: 0px 18px 15px rgba(148, 148, 148, 0.15);
  border-radius: 7px;
  margin-top: 20px;
  padding: 12px 12px 12px 16px;
  margin-bottom: 72px;
}

/* start input */
.input__wrapper {
  width: 901px;
  height: 34px;
  background: #ECEFF0;
  border-radius: 4px;
  position: relative;
}

.search__img {
  position: absolute;
  left: 0;
  top: 5px;
}

.input {
  width: 100%;
  height: 100%;
  background: inherit;
  border: none;
  border-radius: 4px;
  padding: 9px 31px;
  font-family: 'Inter';
  font-weight: 500;
  font-size: 12px;
  line-height: 16px;
  color: #9EAAB4;
}

.input:focus {
  outline: none;
}

.input::placeholder {
  font-family: 'Inter';
  font-weight: 500;
  font-size: 12px;
  color: #9EAAB4;
}

/* end input */
/* start clear */
.clear {
  position: relative;
  margin-top: 24px;
  cursor: pointer;
}

.clear__img {
  position: absolute;
  left: 0;
}

.clear__text {
  position: absolute;
  left: 15px;
  font-family: 'Inter';
  font-weight: 500;
  font-size: 12px;
  line-height: 16px;
  color: #4F4F4F;
}

/* end clear */

/* start sort */
.filters__block {
  margin-bottom: 15px;
}

.sort {
  display: flex;
  gap: 10px;
  font-family: 'Inter';
  font-weight: 500;
  font-size: 9px;
  line-height: 14px;
  color: #9EAAB4;
}

.filter {
  border-bottom: 1px dashed #9EAAB4;
  cursor: pointer;
}

.filter.active {
  border-bottom: 1px dashed #333333;
  color: #333333;
}

/* end sort */

/* start users */
.users {
  width: 961px;
  height: 384px;
  background: #FFFFFF;
  padding: 16px;
}

.users__header {
  width: 927px;
  height: 30px;
}

ul {
  list-style: none;
  width: 927px;
  display: flex;
  justify-content: space-between;
  align-items: center;
}

ul li:last-child {
  display: flex;
  justify-content: start;
  align-items: center;
  width: 36px;
  cursor: pointer;
}

li {
  font-family: 'Inter';
  font-weight: 700;
  font-size: 12px;
  line-height: 16px;
  color: #ACACAC;
  width: 200px;
}



.users__content {
  display: flex;
  flex-direction: column;
}



.loading {
  width: 30px;
}

.loading img {
  width: 100%;
}

/* end users */

/* Animation */
.user-list-item {
  display: inline-block;
  margin-right: 10px;
}

.user-list-enter-active,
.user-list-leave-active {
  transition: all 0.6s ease;
}

.user-list-enter-from,
.user-list-leave-to {
  opacity: 0;
  transform: translateY(30px);
}

.user-list-move {
  transition: transform 0.8s ease;
}

/* end Animation */
</style>