<template>
  <div :class="[$style.UserItem, { [$style._disabled]: !user.children.length }]"
       @click.stop="showChildren = !showChildren"
  >
    <div :class="$style.row">
      <div :class="$style.cell">
        <div v-if="user.children.length"
             :class="$style.icon"
        >
          {{ showChildren ? '-' : '+' }}
        </div>
        <p>{{ user.name }}</p>
      </div>
      <div :class="$style.cell">
        <p>{{ user.phone }}</p>
      </div>
    </div>

    <UsersList v-if="showChildren && user.children.length"
              :users="user.children"
    />
  </div>
</template>

<script>
import UsersList from '@/components/common/table/UsersList';

export default {
  name: 'UserItem',

  components: {
    UsersList,
  },

  props: {
    user: {
      type: Object,
      default: () => ({}),
    },
  },

  data() {
    return {
      showChildren: false,
    };
  },
};
</script>

<style lang="scss" module>
  .UserItem {
    display: flex;
    flex-direction: column;
    justify-content: flex-start;
    font-size: 2.4rem;
    line-height: 100%;
    cursor: pointer;
    background-color: rgba($bg-color, 1);

    @include respond-to(mobile) {
        font-size: 18px;
    }

    &._disabled {
      pointer-events: none;
    }

    p {
      margin-left: 2rem;
    }
  }

  .row {
    display: flex;
    justify-content: flex-start;
  }

  .cell {
    position: relative;
    display: flex;
    align-items: center;
    justify-content: flex-start;
    width: 100%;
    height: 5rem;
    box-shadow: inset 0 0 0 .25rem $base-color;

    @include respond-to(mobile) {
      height: 30px;
      box-shadow: inset 0 0 0 1px $base-color;
    }
  }

  .icon {
    position: absolute;
    left: 0;
    top: auto;
  }
</style>
