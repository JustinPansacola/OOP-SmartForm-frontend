<template>
    <a
        href="#"
        class="justify-content-between list-group-item list-group-item-action text-main-blue p-4 d-flex"
        aria-current="true"
        @click="toggleVendorPage(vendorDetails.name, vendorDetails.id)"
    >
        <span>
            <h3>{{ vendorDetails.name }}</h3>
            <h5>{{ vendorDetails.country }}</h5>
            <span>{{ vendorDetails.details }}</span
            ><br />
        </span>
        <div class="float-right">
            
            <span  class="badge mx-1 mt-2" style="background-color: #bc5090;"> {{filteredFormData[0]}} Awaiting Vendor </span>
            

            <span class="badge mx-1 mt-2" style="background-color: rgb(54, 162, 235);"> {{filteredFormData[1]}} Awaiting Admin </span>
            

            <span class="badge mx-1 mt-2" style="background-color: rgb(255, 128, 0);"> {{filteredFormData[2]}} Awaiting Approval </span>
            <span class="badge mx-1 mt-2" style="background-color: rgb(100, 202, 150);"> {{filteredFormData[3]}} Completed </span>
        </div>
    </a>

</template>



<script>
//SEANS CODE LOL
import { computed, ref, watchEffect } from 'vue';
import FormService from '../../../services/form/formService';
import { useRouter } from "vue-router";
import { useAuthStore } from "../../../stores/authStore";

export default {
    props: {
      vendorDetails: ""
    },
    setup(props) {
     
      const filteredFormData = computed(() => {
        
        let newValues = [0, 0, 0, 0];
        if (formData.value) {
          for (let i = 0; i < formData.value.length; i++) {
            let status = formData.value[i].status;
            if (status === "vendor_response") {
              newValues[0] += 1;
            } else if (status === "admin_response") {
              newValues[1] += 1;
            } else if (status === "approver_response") {
              newValues[2] += 1;
            } else if (status === "form_completed") {
              newValues[3] += 1;
            }
          }
        }
        return newValues;
      });
      var formData = ref(null);

      var displayRole = ref(null);
      var auth = useAuthStore();
      displayRole.value = auth.user;
      var role = displayRole.value.roles
  
      watchEffect(async () => {
        if (props.vendorDetails != null) {
          try {
            formData.value = await FormService.getVendorForms(props.vendorDetails.id).then((response) => {
                return response;
            })
          }
          catch (error) {
            console.log("error occured while getting forms for vendor", error);
          }
        }
      })
      const router = useRouter();

      const toggleVendorPage = (vendorName, vendorId) => {
      router.push({
        name: "AdminVendor",
        params: {
          name: vendorName,
        },
        query: {
          vendorId: vendorId,
        },
      });
    };
  
      return {
        props,
        filteredFormData,
        toggleVendorPage,
        role
      }
    }
  }
  </script>
  