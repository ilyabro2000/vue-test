<template>
  <div :class="$style.UsersList">
    <UserItem v-for="user in users"
              :key="user.id"
              :user="user"
              :children="user.children"
    />
  </div>
</template>

<script>

export default {
  name: 'UserList',

  components: {
    UserItem: () => import('@/components/common/table/UserItem'),
  },

  props: {
    users: {
      type: Array,
      default: () => [],
    },
  },
};
</script>

<style lang="scss" module>
  .UsersList {
    width: 100%;
    margin-left: 2rem;
  }
</style>
