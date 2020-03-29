<template>
  <div id="app">
    <b-row class="text-left">
      <b-col cols="6">
        <pre>{{ fields }}</pre>
      </b-col>
      <b-col cols="6">
        <form-maker :fields="fields"></form-maker>
      </b-col>
    </b-row>
  </div>
</template>

<script>
import FormMaker from "./components/forms/FormMaker.vue";

export default {
  name: "App",
  components: {
    FormMaker
  },
  data() {
    return {
      fields: [
        {
          formType: "checkbox",
          key: "printed_out",
          label: "Printed Out?",
          value: true,
          required: false
        },
        {
          formType: "input",
          key: "name",
          type: "text",
          label: "Project Name",
          placeholder: "Enter project name...",
          value: "",
          required: true
        },
        {
          formType: "input",
          key: "order_date",
          label: "Order Date",
          type: "date",
          value: "",
          required: true
        },
        {
          formType: "select",
          key: "project_category",
          label: "Project Category",
          options: [
            { value: null, text: "Please select an option" },
            { value: 1, text: "This is First option" },
            { value: 2, text: "Selected Option" }
          ],
          value: null,
          required: true
        },
        {
          formType: "textarea",
          key: "note",
          label: "Note",
          placeholder: "Enter note...",
          rows: 3,
          value: "",
          required: false
        }
      ]
    };
  }
};
</script>


