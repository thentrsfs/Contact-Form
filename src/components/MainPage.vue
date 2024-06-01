<template>
    <v-container class="container" style="margin-top: 3rem;" :class="isMobile ? 'mobile-style' : ''">

        <v-form ref="form" v-model="valid" @submit.prevent="submitForm" class="px-5 py-3" style="font-size: 16px;">
            <v-alert
      v-if="showSuccessMessage && !isMobile"
      class="success-message"
      type="success"
      closable
      dismissible
      @input="showSuccessMessage = false"
    >
     <h4> Message Sent!</h4>
      <p style="font-size: 14px;">Thanks for completing the form.We'll be in touch soon!</p>
    </v-alert>
            <h1 class="pb-8">Contact Us</h1>
            <v-row>
                <v-col :cols="isMobile ? 12 : 6" class="custom-height">
                    <label>First Name <span>*</span></label>
                    <v-hover>
                    <v-text-field color="hsl(169, 82%, 27%)" base-color="hsl(170, 80%, 27%)" class="custom-inputs" v-model="firstName" style="margin-top: 8px;" variant="outlined" density="comfortable"
                        required></v-text-field>
                    </v-hover>
                    <div style="height: 5px; margin-top: -12px; margin-bottom: 10px; font-size: 14px; color:hsl(0, 67%, 59%);"
                        v-if="firstNameErrors.length !== 0">
                        This
                        field is
                        required</div>
                </v-col>
                <v-col :cols="isMobile ? 12 : 6">
                    <label>Last Name <span>*</span></label>
                    <v-hover>
                    <v-text-field color="hsl(169, 82%, 27%)" base-color="hsl(170, 80%, 27%)" v-model="lastName" style="margin-top: 8px;" variant="outlined" density="comfortable"
                        required></v-text-field>
                    </v-hover>
                    <div style="height: 5px; margin-top: -12px; margin-bottom: 10px; font-size: 14px; color:hsl(0, 67%, 59%);"
                        v-if="lastNameErrors.length !== 0">
                        This
                        field is
                        required</div>
                </v-col>
            </v-row>
            <v-row style="margin-top: -20px;">
                <v-col cols="12">
                    <label>Email Address <span>*</span></label>
                    <v-text-field color="hsl(169, 82%, 27%)" base-color="hsl(170, 80%, 27%)" class="custom-inputs" v-model="email" style="margin-top: 8px;" variant="outlined" density="comfortable"
                        required></v-text-field>
                    <div style=" margin-top: -8px;margin-bottom: 10px ; font-size: 14px; color:hsl(0, 67%, 59%);"
                        v-if="emailErrors.includes('Email is required')">
                        This
                        field is
                        required</div>
                    <div style=" margin-top: -8px;margin-bottom: 10px ; font-size: 14px; color:hsl(0, 67%, 59%);"
                        v-else-if="emailErrors.includes('Email must be valid')">Email must be valid</div>
                </v-col>
            </v-row>
            <label>Query Type <span>*</span></label>
            <v-row style=" margin-top: 1px;">
                <v-radio-group v-model="selectedRadio" inline required>
                    <v-col :cols="isMobile ? 12 : 6">
                        <div class="radio-button first-radio" :class="{ 'selected-radio': selectedRadio === 'first' }">
                            <v-radio class="radio" :class="{ 'text-color': selectedRadio === 'first' }" value="first"
                                label="General Enquiry" color="teal-darken-2"></v-radio>
                        </div>
                    </v-col>
                    <v-col :cols="isMobile ? 12 : 6">
                        <div class="radio-button second-radio"
                            :class="{ 'selected-radio': selectedRadio === 'second' }">
                            <v-radio class="radio" :class="{ 'text-color': selectedRadio === 'second' }" value="second"
                                label="Support Request" color="teal-darken-2"></v-radio>
                        </div>
                    </v-col>
                </v-radio-group>
            </v-row>
            <div style=" margin-top: -20px;margin-bottom: 20px ; font-size: 14px; color:hsl(0, 67%, 59%);"
                v-if="radioErrors.length !== 0">
                Please select a query type</div>
            <v-row style="margin-top: -20px;">
                <v-col cols="12">
                    <label>Message <span>*</span></label>
                    <v-textarea color="hsl(169, 82%, 27%)" base-color="hsl(170, 80%, 27%)" class="custom-inputs" v-model="message" auto-grow variant="outlined"></v-textarea>
                </v-col>
            </v-row>
            <div style="height: 5px; margin-top: -22px; margin-bottom: 22px; font-size: 14px; color:hsl(0, 67%, 59%);"
                v-if="messageErrors.length !== 0">
                This
                field is
                required</div>
            <v-row style="margin-top: -16px; margin-left: -20px;" align="center">
                <v-col cols="1">
                    <v-checkbox v-model="consent" required></v-checkbox>
                </v-col>
                <v-col cols="11">
                    <p style="margin-bottom: 20px;" :style="[isMobile ? {'margin-left': '20px'} : {'margin-left':'0px'}]">I consent to being
                        contacted by the team
                        <span>*</span>
                    </p>
                </v-col>

            </v-row>
            <div style="height: 5px; margin-top: -40px; margin-bottom: 40px; font-size: 14px; color:hsl(0, 67%, 59%);"
                v-if="consentErrors.length !== 0">
                To submit this form, please consent to being contacted</div>
                <v-alert
      v-if="showSuccessMessage && isMobile"
      type="success"
      closable
      dismissible
      @input="showSuccessMessage = false"
    >
    <h4> Message Sent!</h4>
      <p style="font-size: 14px;">Thanks for completing the form.We'll be in touch soon!</p>
    </v-alert>
            <v-btn @click="validateForm" block height="52">
                Submit
            </v-btn>

            

        </v-form>
    </v-container>
</template>

<script setup>
import { ref, onMounted, onUnmounted } from 'vue'

const valid = ref(false);
const selectedRadio = ref(null);
const firstName = ref('');
const lastName = ref('');
const email = ref('');
const message = ref('');
const consent = ref(false);
const showSuccessMessage = ref(false);

// Error messages

const firstNameErrors = ref([])
const lastNameErrors = ref([])
const emailErrors = ref([])
const radioErrors = ref([])
const messageErrors = ref([])
const consentErrors = ref([])

const validateForm = () => {
    firstNameErrors.value = []
    lastNameErrors.value = []
    emailErrors.value = []
    radioErrors.value = []
    messageErrors.value = []
    consentErrors.value = []

    if (!firstName.value) {
        firstNameErrors.value.push('First name is required')
    }
    if (!lastName.value) {
        lastNameErrors.value.push('Last name is required')
    }
    if (!email.value) {
        emailErrors.value.push('Email is required')
    } else if (!/.+@.+\..+/.test(email.value)) {
        emailErrors.value.push('Email must be valid')
    }

    if (selectedRadio.value === null) {
        radioErrors.value.push('Please select a radio');
    }

    if (!message.value) {
        messageErrors.value.push('Please write a message');
    }

    if (!consent.value) {
        consentErrors.value.push('Please consent to this');
    }

    if (!firstNameErrors.value.length && !lastNameErrors.value.length && !emailErrors.value.length) {
        submitForm()
    }
}

const submitForm = () => {
    console.log('Form submitted:', { firstName: firstName.value, lastName: lastName.value, email: email.value });
    showSuccessMessage.value = true
}

const isMobile = ref(window.innerWidth < 600)

// Function to handle window resize
const handleResize = () => {
  isMobile.value = window.innerWidth < 600
}

onMounted(() => {
  window.addEventListener('resize', handleResize)
})

onUnmounted(() => {
  window.removeEventListener('resize', handleResize)
})

const form = ref(null);

</script>


<style scoped>
.container {
    max-width: 700px;
    background-color: white;
    border-radius: 20px
}

.radio-button {
    border: 1px solid hsl(146, 26%, 60%);
    border-radius: 5px;
    height: 50px;

}

.radio {
    padding-top: 4px;
}

.selected-radio {
    background-color: hsl(148, 38%, 91%);

}

span {
    color: hsl(169, 82%, 27%)
}

button {
    background-color: hsl(169, 82%, 27%);
    color: white;
}

.v-checkbox {
    color: hsl(169, 82%, 27%);
}

.mobile-style{
    max-width: 90%;
    margin-bottom: 2rem
    
}

.success-message{
    position: absolute;
    top: 0.5rem;
    left: 720px

}


</style>
