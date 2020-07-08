<template>
    <b-modal centered v-bind:id="'delete-form-modal-'+contact.id" hide-footer>
      <template v-slot:modal-title>Confirm Delete Modal</template>
      <div class="d-block text-center">
        <h3 class="breakLongTxt">Are you sure you want to delete {{contact.name}}?</h3>
      </div>
      <b-button variant="danger" class="mt-3" block @click="handleDelete">Delete</b-button>
    </b-modal>
</template>

<script>
export default {
  name: "ConfirmDeleteModal",
  props: ["contact"],
  data() {
    return {
    };
  },
  methods: {
    hideModal() {
      this.$bvModal.hide(`delete-form-modal-${this.contact.id}`);
    },
    handleDelete() {
      this.$emit("remove-contact", this.contact.id, this.hideModal);
    }
  }
};
</script>