<template>
  <b-modal id="contact-form-modal" hide-footer>
    <template v-slot:modal-title>{{isEditMode ? "Edit" : "Add"}} Contact</template>
    <b-container>
      <validation-observer ref="observer" v-slot="{ handleSubmit }">
        <b-form @submit.stop.prevent="handleSubmit(onSubmit)">
          <b-row>
            <b-col sm>
              <!-------------------- NAME ----------------------->
              <validation-provider
                name="Name"
                :rules="{ required: true, max: 75 }"
                v-slot="validationContext"
              >
                <b-form-group label="Name" label-for="name">
                  <b-form-input
                    id="name"
                    name="name"
                    v-model="selectedContact.name"
                    :state="getValidationState(validationContext)"
                    aria-describedby="name-feedback"
                  ></b-form-input>

                  <b-form-invalid-feedback id="name-feedback">{{ validationContext.errors[0] }}</b-form-invalid-feedback>
                </b-form-group>
              </validation-provider>
            </b-col>
            <b-col sm>
              <!-------------------- PHONE ----------------------->
              <validation-provider
                name="Phone"
                :rules="{ required: true, regex: /^[+](?!.*-.*-.*-)(?=(?:\d{8,13}$)|(?:(?=.{9,14}$)[^-]*-[^-]*$)|(?:(?=.{10,15}$)[^-]*-[^-]*-[^-]*$)  )[\d-]+$/ }"
                v-slot="validationContext"
              >
                <b-form-group label="Phone" label-for="phone">
                  <b-form-input
                    id="phone"
                    name="phone"
                    placeholder="+905437838614"
                    v-model="selectedContact.phone"
                    :state="getValidationState(validationContext)"
                    aria-describedby="phone-feedback"
                  ></b-form-input>

                  <b-form-invalid-feedback id="phone-feedback">{{ validationContext.errors[0] }}</b-form-invalid-feedback>
                </b-form-group>
              </validation-provider>
            </b-col>
          </b-row>
          <b-row>
            <b-col sm>
              <!-------------------- EMAIL ----------------------->
              <validation-provider name="Email" :rules="{ email: true }" v-slot="validationContext">
                <b-form-group label="Email" label-for="email">
                  <b-form-input
                    id="email"
                    name="email"
                    v-model="selectedContact.email"
                    :state="selectedContact.email ? getValidationState(validationContext) : getValidationState('')"
                    aria-describedby="email-feedback"
                  ></b-form-input>
                  <b-form-invalid-feedback id="email-feedback">{{ validationContext.errors[0] }}</b-form-invalid-feedback>
                </b-form-group>
              </validation-provider>
            </b-col>
            <b-col sm>
              <!-------------------- CITY ----------------------->
              <validation-provider
                name="City"
                :rules="{ alpha_spaces: true, max: 30 }"
                v-slot="validationContext"
              >
                <b-form-group label="City" label-for="city">
                  <b-form-input
                    id="city"
                    name="city"
                    v-model="selectedContact.city"
                    :state="selectedContact.city ? getValidationState(validationContext) : getValidationState('')"
                    aria-describedby="city-feedback"
                  ></b-form-input>

                  <b-form-invalid-feedback id="city-feedback">{{ validationContext.errors[0] }}</b-form-invalid-feedback>
                </b-form-group>
              </validation-provider>
            </b-col>
          </b-row>
          <b-row>
            <b-col>
              <!-------------------- ADDRESS ----------------------->
              <validation-provider name="Address" :rules="{ max: 100 }" v-slot="validationContext">
                <b-form-group label="Address" label-for="address">
                  <b-form-textarea
                    id="address"
                    name="address"
                    v-model="selectedContact.address"
                    :state="selectedContact.address ? getValidationState(validationContext) : getValidationState('')"
                  ></b-form-textarea>

                  <b-form-invalid-feedback id="address-feedback">{{ validationContext.errors[0] }}</b-form-invalid-feedback>
                </b-form-group>
              </validation-provider>
              <!---------------------------------------------->
            </b-col>
          </b-row>
          <b-button type="submit" variant="primary">Submit</b-button>
          <b-button class="ml-2" @click="$bvModal.hide('contact-form-modal')">Cancel</b-button>
        </b-form>
      </validation-observer>
    </b-container>
  </b-modal>
</template>

<script>
import axios from "axios";

export default {
  name: "ContactFormModal",
  props: ["contact", "isEditMode"],
  data() {
    return {
      selectedContact: {}
    };
  },
  watch: {
    contact: function() {
      this.selectedContact = { ...this.contact };
    }
  },
  methods: {
    getValidationState({ dirty, validated, valid = null }) {
      return dirty || validated ? valid : null;
    },
    hideModal() {
      this.$bvModal.hide("contact-form-modal");
    },
    onSubmit() {
      if (!this.isEditMode)
        this.$emit("add-contact", this.selectedContact, this.hideModal);
      else this.$emit("update-contact", this.selectedContact, this.hideModal);
    }
  }
};
</script>