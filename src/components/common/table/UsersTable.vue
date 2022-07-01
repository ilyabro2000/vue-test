<template>
  <div :class="$style.UsersTable">
    <div :class="[$style.row, $style.header]">
      <div :class="$style.cell">
        <p>Имя</p>
      </div>
      <div :class="$style.cell">
        <p>Телефон</p>
      </div>
    </div>

    <UserItem v-for="user in users"
              :key="user.id"
              :user="user"
    />
  </div>
</template>

<script>
import UserItem from '@/components/common/table/UserItem';

export default {
  name: 'UsersTable',

  components: {
    UserItem,
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
  .UsersTable {
      width: 100%;
  }

  .row {
    display: flex;
  }

  .header {
    font-size: 2.8rem;
    line-height: 100%;

    @include respond-to(mobile) {
        font-size: 20px;
    }
  }

  .cell {
    display: flex;
    align-items: center;
    justify-content: center;
    width: 100%;
    height: 5rem;
    box-shadow: inset 0 0 0 .25rem $base-color;
    background-color: rgba($bg-color, .8);

    @include respond-to(mobile) {
      height: 30px;
      box-shadow: inset 0 0 0 1px $base-color;
    }
  }
</style>
