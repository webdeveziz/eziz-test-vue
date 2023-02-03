<template>
  <div class="app">
    <UsersList :users="cropedUsers" :loading="isLoading" @del="onDelete" @selrating="onRating" v-model="search" />
    <ul class="pagination">
      <li :class="{
        pag: true,
        active: currentPage === page
      }" @click="onChangePage(page)" v-for="page in pageArr" :key="page">{{ page }}</li>
    </ul>
  </div>
</template>

<script>
import UsersList from './components/UsersList.vue';
import axios from 'axios'
import _ from 'lodash'

export default {
  name: 'App',
  components: {
    UsersList
  },

  data() {
    return {
      users: [],
      isLoading: false,
      selectedSort: '',
      search: '',
      page: 1,
      pageSize: 5,
      sum: 25,
      pageArr: [],
      currentPage: 1
    }
  },

  methods: {
    onRating(val) {
      this.selectedSort = val
    },
    async fetchUsers() {
      try {
        this.isLoading = true
        const { data } = await axios.get('https://5ebbb8e5f2cfeb001697d05c.mockapi.io/users')
        this.users = data
      } catch (error) {
        console.log('Ошибка!!!');
      } finally {
        this.isLoading = false
      }

    },
    onDelete(user) {
      this.users = this.users.filter(usr => usr.id !== user.id)
    },
    onChangePage(indexPage) {
      console.log(indexPage);
      this.currentPage = indexPage
      this.page = indexPage
      console.log(this.page);
    }

  },
  mounted() {
    this.fetchUsers()
  },
  updated() {
    this.sum = this.sortedSearchedUsers.length
    let count = Math.ceil(this.sum / this.pageSize)
    if (count <= 1) {
      this.pageArr = []
    } else {
      let arr = _.range(1, count + 1)
      this.pageArr = arr
    }



  },
  computed: {
    selectedUsers() {
      return [...this.users].sort((user1, user2) => {
        if (user1[this.selectedSort] > user2[this.selectedSort]) {
          return 1
        }
        else if (user2[this.selectedSort] > user1[this.selectedSort]) {
          return -1
        }
        else {
          return 0
        }
      })
    },
    sortedSearchedUsers() {
      return this.selectedUsers.filter(user => (user.username.toLowerCase().includes(this.search.toLowerCase())) || (user.email.toLowerCase().includes(this.search.toLowerCase())))

    },

    cropedUsers() {
      const startIndex = (this.currentPage - 1) * this.pageSize
      return [...this.sortedSearchedUsers].splice(startIndex, this.pageSize)
    }

  },


}
</script>

<style>
* {
  box-sizing: border-box;
  padding: 0;
  margin: 0;
}

.app {
  margin: 0 auto;
  width: 1000px;
  min-height: 647px;
  padding: 12px 12px 12px 27px;
  background: #F7F7F7;
}

.pagination {
  display: flex;
  align-items: center;
  margin-top: 20px;
}

.pag {
  display: flex;
  justify-content: center;
  align-items: center;
  width: 20px;
  height: 20px;
  border: 1px solid #0CB4F1;
  cursor: pointer;

}

.pag.active {
  background: #0CB4F1;
  color: #fff;
}
</style>
