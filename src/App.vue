<template>
  <div :class="$style.App">
    <div :class="$style.tableBlock">
      <VButton @click="handleOpen"
               color="gray"
               :class="$style.btn"
      >
        Добавить
      </VButton>

      <UsersTable :users="users"/>
    </div>

    <VModal :is-open="isModalOpen"
            :class="$style.modal"
            @close="handleClose"
    >
      <VForm
        input-name
        input-phone
        :select-options="options"
        :class="$style.form"
        @close="handleClose"
        @set-data="handleSetData"
      />
    </VModal>
  </div>
</template>

<script>
import VModal from '@/components/ui/modal/VModal.vue';
import VButton from '@/components/ui/button/VButton.vue';
import VForm from '@/components/common/VForm';
import UsersTable from '@/components/common/table/UsersTable';

export default {
  name: 'App',

  components: {
    UsersTable,
    VForm,
    VModal,
    VButton,
  },

  data() {
    return {
      isModalOpen: false,
      users: [],
      options: [],
    };
  },

  methods: {
    handleClose() {
      this.isModalOpen = false;
    },

    handleOpen() {
      this.isModalOpen = true;
    },

    handleSetData(data) {
      this.options = [...this.options, data];
      if (data.parent) {
        this.users = [...this.addChildren(this.users, data)];
      } else {
        this.users = [...this.addChildren(this.users, data), data];
      }
    },

    addChildren(users, data) {
      return users.map((user) => {
        if (user.id === data.parent?.id) {
          const newChildren = [...user.children, data];
          return { ...user, children: newChildren };
        }
        return user;
      });
    },
  },
};
</script>

<style lang="scss" module>
  .App {
    position: relative;
      width: 60rem;

      @include respond-to(mobile) {
          width: 100%;
      }
  }

  .modal {
    position: absolute;
    top: 0;
    right: 0;
    bottom: 0;
    left: 0;
    z-index: 2;
  }

  .tableBlock {
    display: flex;
    flex-direction: column;
    align-items: flex-start;
    padding: 10rem 2rem 2rem 2rem;

    @include respond-to(mobile) {
      align-items: unset;
    }
  }

  .btn {
    margin-bottom: 3rem;
  }
</style>
