<template>
  <div
    :class="sectionColor"
  >
    <div class="row">
      <div class="col-6 px-3 py-1 bg fw-bold text-light">
          {{ assignTo }} Section
      </div>
      <div class="col-6 text-end">
        <button
        class="text-end borderless-button mx-2 "
        @click="removeSection(order)"
        >
        Remove Section <font-awesome-icon icon="fa-solid fa-xmark" />
      </button>
      </div>
      <div class="row">
        <div v-for="(item, index) in formItems" :key="index">
          <FormComponent
            :itemInfo="item"
            :idx="index"
            @updateQuestion="update"
            @remove="removeQuestion"
          />
        </div>
      </div>
    </div>
    <div
      class="col-8 offset-2 sticky-bottom d-flex justify-content-center border rounded border-light border-1 bg-light-grey shadow-lg"
    >
      <div class="row m-1 p-1">
        <button
          @click="addHeaderText"
          :class="buttonColor"
          data-bs-toggle="tooltip"
          data-bs-placement="right"
          data-bs-animation="true"
          title="Add Header"
        >
          <font-awesome-icon icon="fa-solid fa-h" />
        </button>
        
        <button
          @click="addTextInput"
          :class="buttonColor"
          data-bs-toggle="tooltip"
          data-bs-placement="right"
          title="Add Text Input"
        >
          <font-awesome-icon icon="fa-solid fa-t" />
        </button>
        <button
          @click="addNumberInput"
          :class="buttonColor"
          data-bs-toggle="tooltip"
          data-bs-placement="right"
          title="Add Number Input"
        >
          <font-awesome-icon icon="fa-solid fa-1" />
        </button>
        <button
          @click="addDateInput"
          :class="buttonColor"
          data-bs-toggle="tooltip"
          data-bs-placement="right"
          title="Add Date"
        >
          <font-awesome-icon icon="fa-solid fa-calendar" />
        </button>
        <button
          @click="addBooleanInput"
          :class="buttonColor"
          data-bs-toggle="tooltip"
          data-bs-placement="right"
          title="Add True False"
        >
          <font-awesome-icon icon="fa-solid fa-t" />
          <font-awesome-icon icon="fa-solid fa-f" />
        </button>
        <button
          @click="addCheckboxInput"
          :class="buttonColor"
          data-bs-toggle="tooltip"
          data-bs-placement="right"
          title="Add Checkbox Group"
        >
          <font-awesome-icon icon="fa-solid fa-circle-check" />
        </button>
        <button
          @click="addRadioInput"
          :class="buttonColor"
          data-bs-toggle="tooltip"
          data-bs-placement="right"
          title="Add Radio Group"
        >
          <font-awesome-icon icon="fa-solid fa-radio" />
        </button>
        <button
          @click="addLikertGroupInput"
          :class="buttonColor"
          data-bs-toggle="tooltip"
          data-bs-placement="right"
          title="Add Likert Group"
        >
          <font-awesome-icon icon="fa-solid fa-object-group" />
        </button>
        
        <button
          v-if="assignTo == 'Vendor'"
          @click="addAcknowledgementInput"
          :class="buttonColor"
          data-bs-toggle="tooltip"
          data-bs-placement="right"
          title="Add Acknowledgement"
        >
        <font-awesome-icon icon="fa-solid fa-signature" />
        </button>
        <button
          v-if="assignTo == 'Admin'"
          @click="addApprovalInput"
          :class="buttonColor"
          data-bs-toggle="tooltip"
          data-bs-placement="right"
          title="Add Approval"
        >
        <font-awesome-icon icon="fa-solid fa-signature" />
        </button>
      </div>
    </div>
  </div>
</template>

<script>
import FormComponent from "../form/FormComponent.vue";
import { ref, computed } from "vue";
export default {
  components: { FormComponent },
  props: ["sectionInfo", "order"],
  setup(props, context) {
    var formItems = ref(props.sectionInfo[Object.keys(props.sectionInfo)[0]]);
    const assignTo = computed(() => {
      if (Object.keys(props.sectionInfo)[0] == "admin") {
        return "Admin";
      } else {
        return "Vendor";
      }
    });

    //Computed style for admin // vendor
    const sectionColor = computed(()=>{
      if (Object.keys(props.sectionInfo)[0] == "admin") {
        return "mt-2 p-2 border rounded border-light border-1 bg-dark-purple shadow-sm";
      } else {
        return "mt-2 p-2 border rounded border-light border-1 bg-main-blue shadow-sm"
      }
    })

    const buttonColor = computed(()=>{
      if (Object.keys(props.sectionInfo)[0] == "admin") {
        return "m-2 btn btn-main-dark-purple btn-circle";
      } else {
        return "m-2 btn btn-main-blue btn-circle";
      }

    })

    function addTextInput() {
      formItems.value.push({
        type: "text",
        order: formItems.value.length,
        text: "",
        required: true,
      });
    }
    function addCheckboxInput() {
      formItems.value.push({
        type: "checkbox",
        order: formItems.value.length,
        text: "",
        options: [],
        required: true,
        shortAnswer: false,
      });
    }
    function addRadioInput() {
      formItems.value.push({
        type: "radio",
        order: formItems.value.length,
        text: "",
        options: [],
        required: true,
      });
    }
    function addHeaderText() {
      formItems.value.push({
        type: "header",
        order: formItems.value.length,
        text: "",
        style: "h1",
        required: true,
      });
    }
    function addBooleanInput() {
      formItems.value.push({
        type: "boolean",
        order: formItems.value.length,
        text: "",
        options: ["Yes", "No"],
        required: true,
      });
    }
    function addDateInput() {
      formItems.value.push({
        type: "date",
        order: formItems.value.length,
        text: "",
        required: true,
      });
    }
    function addNumberInput() {
      formItems.value.push({
        type: "number",
        order: formItems.value.length,
        text: "",
        required: true,
      });
    }
    function addLikertGroupInput() {
      formItems.value.push({
        type: "likertGroup",
        order: formItems.value.length,
        text: "",
        options: [],
        required: true,
      });
    }
    function addAcknowledgementInput() {
      formItems.value.push({
        type: "acknowledgement",
        required: true,
      });
    }

    function addApprovalInput() {
      formItems.value.push({
        type: "approval",
        required: true,
      });
    }

    function update() {
      //Uncomment this out to check
      context.emit("updateSection");
    }

    function removeQuestion(questionKey) {
      //Remove from formItems first
      formItems.value.splice(questionKey, 1);
    }

    function removeSection(toRemove) {
      context.emit("removeSection", toRemove);
    }

    return {
      formItems,
      assignTo,
      sectionColor,
      buttonColor,
      addCheckboxInput,
      addRadioInput,
      addHeaderText,
      addBooleanInput,
      addDateInput,
      addNumberInput,
      addLikertGroupInput,
      addTextInput,
      addApprovalInput,
      addAcknowledgementInput,
      removeQuestion,
      removeSection,
      update,
    };
  },
};
</script>
