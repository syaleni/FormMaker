<script>
export default {
  props: {
    fields: { type: Array, required: true }
  },
  data() {
    // make the form object from the fiedls prop
    let form = [];
    this.fields.forEach(field => {
      form = Object.assign({}, form, {
        [field.key]: field.value
      });
    });
    return {
      form
    };
  },
  methods: {
    createFormGroup(createElement, self, field) {
      let elements = [];
      switch (field.formType) {
        case "input":
          elements.push(this.createInputField(createElement, self, field));
          break;
        case "select":
          elements.push(this.createSelectField(createElement, self, field));
          break;
        case "textarea":
          elements.push(this.createTextAreaField(createElement, self, field));
          break;
        case "checkbox":
          elements.push(this.createCheckboxField(createElement, self, field));
          break;
        default:
        // code block
      }
      return createElement(
        "b-form-group",
        {
          attrs: {
            id: field.id,
            description: field.description,
            label: field.label
          },
          props: {
            "label-cols-sm": 4,
            "label-size": "sm"
          }
        },
        elements
      );
    },
    // key: text value of the key in the form object that holds the value of a field
    createInputField(
      createElement,
      self,
      { id, key, type, placeholder, required }
    ) {
      return createElement("b-form-input", {
        attrs: {
          id,
          type,
          placeholder,
          required,
          value: self.form[key],
          size: "sm"
        },
        on: {
          input: function(event) {
            self.form[key] = event;
            self.$emit("input", event);
          }
        }
      });
    },
    createSelectField(createElement, self, { id, key, options, required }) {
      return createElement("b-form-select", {
        attrs: {
          id,
          options,
          required,
          value: self.form[key],
          size: "sm"
        },
        on: {
          change: function(event) {
            self.form[key] = event;
            self.$emit("input", event);
          }
        }
      });
    },
    createTextAreaField(
      createElement,
      self,
      { id, key, placeholder, rows, required }
    ) {
      return createElement("b-form-textarea", {
        attrs: {
          id,
          placeholder,
          rows,
          required,
          value: self.form[key],
          size: "sm"
        },
        on: {
          input: function(event) {
            self.form[key] = event;
            self.$emit("input", event);
          }
        }
      });
    },
    createCheckboxField(createElement, self, { id, key, required }) {
      return createElement("b-form-checkbox", {
        attrs: {
          id,
          required,
          checked: self.form[key],
          size: "sm"
        },
        on: {
          change: function(event) {
            self.form[key] = event;
            self.$emit("input", event);
          }
        }
      });
    },
    onSubmit(evt) {
      evt.preventDefault();
      // pass data to api
    },
    onReset(evt) {
      evt.preventDefault();
      this.resetForm();
    },
    resetForm() {
      let form = [];
      this.fields.forEach(field => {
        form = Object.assign({}, form, {
          [field.key]: field.value
        });
      });
      this.form = form;
    }
  },
  render(createElement) {
    var self = this;
    const formTreeView = createElement("pre", JSON.stringify(this.form));
    const elements = [];
    this.fields.forEach(field => {
      let formGroup = this.createFormGroup(createElement, self, field);
      elements.push(formGroup);
    });
    const submitButton = createElement(
      "b-button",
      {
        props: {
          type: "submit",
          variant: "primary",
          size: "sm"
        }
      },
      "Submit"
    );

    const resetButton = createElement(
      "b-button",
      {
        props: {
          type: "reset",
          variant: "danger",
          size: "sm"
        }
      },
      "Reset"
    );

    const form = createElement(
      "b-form",
      {
        on: {
          submit: this.onSubmit,
          reset: this.onReset
        }
      },
      [...elements, submitButton, resetButton]
    );

    // Add form validation using vee-validate?
    return form;
  }
};
</script>
