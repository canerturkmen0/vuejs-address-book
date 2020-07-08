<template>
  <div>
    <b-button
      v-b-modal.contact-form-modal
      squared
      block
      variant="success"
      @click="activateAddMode"
    >ADD NEW</b-button>
    <ContactFormModal
      v-bind:isEditMode="isEditMode"
      v-bind:contact="selectedContact"
      v-on:add-contact="addContact"
      v-on:update-contact="updateContact"
    />
    <b-table class="breakLongTxt" stacked="md" striped hover :items="contacts" :fields="fields">
      <template v-slot:cell(actions)="row" class="d-flex align-items-center">
        <b-button
          v-b-modal.contact-form-modal
          variant="warning"
          size="sm"
          @click="activateEditMode(row.item)"
        >Update</b-button>

        <b-button
          v-b-modal="'delete-form-modal-'+row.item.id"
          variant="danger"
          size="sm"
        >Delete</b-button>

        <ConfirmDeleteModal v-bind:contact="row.item" v-on:remove-contact="removeContact" />
      </template>
    </b-table>
  </div>
</template>

<script>
import axios from "axios";
import ContactFormModal from "../components/ContactFormModal";
import ConfirmDeleteModal from "../components/ConfirmDeleteModal";
import { API_URL } from "../consts";

export default {
  head() {
    return {
      title: "Contacts | Address Book",
      meta: [
        {
          hid: "description",
          name: "description",
          content: "Add page of Address Book application developed with Vue.js"
        }
      ]
    };
  },
  components: {
    ContactFormModal,
    ConfirmDeleteModal
  },
  data() {
    return {
      contacts: [],
      fields: [
        { key: "name", label: "Name", sortable: true },
        { key: "phone", label: "Phone", sortable: true },
        { key: "email", label: "Email", sortable: true },
        { key: "city", label: "City", sortable: true },
        { key: "address", label: "Address", sortable: true },
        { key: "actions", label: "Actions" }
      ],
      selectedContact: {},
      isEditMode: false
    };
  },
  async mounted() {
    try {
      const res = await axios.get(API_URL);
      this.contacts = res.data;
    } catch (error) {
      this.$router.push("/err");
    }
  },
  methods: {
    activateEditMode(contact) {
      this.isEditMode = true;
      this.selectedContact = contact;
    },
    activateAddMode() {
      this.isEditMode = false;
      this.selectedContact = {};
    },
    async removeContact(id, hideModal) {
      try {
        await axios.delete(`${API_URL}/${id}`);
        this.contacts = this.contacts.filter(contact => contact.id !== id);
        hideModal();
      } catch (error) {
        this.$router.push("/err");
      }
    },
    async addContact(contact, hideModal) {
      const newContact = { ...contact, id: Date.now() };
      try {
        await axios.post(API_URL, newContact);
        this.contacts = [...this.contacts, newContact];
        hideModal();
      } catch (error) {
        this.$router.push("/err");
      }
    },
    async updateContact(contact, hideModal) {
      try {
        await axios.put(`${API_URL}/${contact.id}`, contact);
        const idx = this.contacts.findIndex(ct => ct.id === contact.id);
        const newContacts = [...this.contacts];
        newContacts[idx] = contact;
        this.contacts = newContacts;
        hideModal();
      } catch (error) {
        this.$router.push("/err");
      }
    }
  }
};
</script>