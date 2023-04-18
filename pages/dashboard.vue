<template>
      <client-only placeholder="Loading...">
    <div class="calendar-container">
        <div id="app">
            <h1>Scheduling App</h1>
            <div>
                <input placeholder="Appoinment Title" type="text" name="appoinmentDetail" v-model="appoinmentDetail" />
                <input type="datetime-local" v-model="appoinmentStartTime">   
                <input type="datetime-local" v-model="appoinmentEndTime">
                <button class="submit-button" @click="handleBookAppoinment">{{ isEdit ? "Update Appointment" : 'Book Appointment' }}</button>
            </div>

            <div class="popup" v-if="showPopup && userType != 3">
                <button class="update" @click="handleUpdateAppoinment">Update Appointment</button>
                <button class="delete" v-if="userType != 2" @click="handleDeleteAppoinment">Delete Appointment</button>
                <button class="close" @click="showPopup = false">Cancel</button>
            </div>
            <CalendarView :items="appoinmentData" @click-item="onItemClick" :show-date="showDate"
                class="theme-default holiday-us-traditional holiday-us-official">
                <template #header="{ headerProps }">
                    <CalendarViewHeader :header-props="headerProps" @input="setShowDate" />
                </template>
            </CalendarView>
        </div>
    </div>
</client-only>
</template>
  
<script>
import { defineComponent, ref, watch } from 'vue'
import moment from 'moment'
import { CalendarView, CalendarViewHeader } from 'vue-simple-calendar'
import '../node_modules/vue-simple-calendar/dist/style.css'
import "../node_modules/vue-simple-calendar/dist/css/default.css"
import "../node_modules/vue-simple-calendar/dist/css/holidays-us.css"
export default defineComponent({
    setup() {
        const showDate = ref(new Date())
        const appoinmentDetail = ref('');
        const appoinmentStartTime = ref();
        const appoinmentEndTime = ref();
        const selectedObj = ref({})
        const showPopup = ref(false)
        const isEdit = ref(false)
        const userType = ref(localStorage.getItem('loginType'))
        let appoinmentData = ref(JSON.parse(window?.localStorage?.getItem('userData')) || [])


        const title = ref('Scheduling App')
        const description = ref('scheduling app for managing appointments')
        useHead({
            title,
            meta: [{
                name: 'description',
                content: description
            }]
        })

        function setShowDate(d) {
            showDate.value = d
        }

        function onItemClick(d) {
            showPopup.value = true
            selectedObj.value = JSON.parse(JSON.stringify(d.originalItem));
        }

        function handleBookAppoinment() {

            if(!appoinmentDetail.value || !appoinmentEndTime.value || !appoinmentStartTime.value){
                return alert('please fill required detail')              
            }

            if(moment(appoinmentEndTime.value).isBefore(appoinmentStartTime.value)){
                return alert('Start Date Should be lesser than End Date')
            }

            if (isEdit.value) {
                handleDeleteAppoinment()
            }

            appoinmentData.value = [...appoinmentData.value, {
                id: new Date().getMilliseconds(),
                startDate: appoinmentStartTime.value,
                endDate: appoinmentEndTime.value,
                title: appoinmentDetail.value,
            }]

            isEdit.value = false
            appoinmentStartTime.value = ''
            appoinmentEndTime.value = ''
            appoinmentDetail.value = ''

            localStorage.setItem('userData',JSON.stringify(appoinmentData.value))
        }

        function handleUpdateAppoinment() {

            appoinmentDetail.value = selectedObj.value.title
            appoinmentStartTime.value = moment(selectedObj.value.startDate).format('YYYY-MM-DDTHH:mm')
            appoinmentEndTime.value = moment(selectedObj.value.endDate).format('YYYY-MM-DDTHH:mm')
            showPopup.value = false
            isEdit.value = true

        }

        function handleDeleteAppoinment() {
            showPopup.value = false;
            appoinmentData.value = appoinmentData.value.filter((item) => item.id !== selectedObj.value.id)

           localStorage.setItem('userData',JSON.stringify(appoinmentData.value))
        }

        return {
            showDate,
            setShowDate,
            appoinmentDetail,
            handleDeleteAppoinment,
            appoinmentStartTime,
            appoinmentData,
            onItemClick,
            selectedObj,
            appoinmentEndTime,
            showPopup,
            userType,
            handleBookAppoinment,
            handleUpdateAppoinment,
            isEdit
        }
    },


    components: {
        CalendarView,
        CalendarViewHeader
    }
})
</script>
  
<style>
.calendar-container {
    height: 100%;
    display: flex !important;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    background-color: #f5f5f5;
    padding: 20px;
}

.cv-header-nav {
    display: flex !important;
}

#app {
    max-width: 800px;
    width: 100%;
}

h1 {
    text-align: center;
    margin-bottom: 20px;
}
.submit-button{
    background-color: #3e8e41;
}

input[type="text"],
input[type="datetime-local"],
button {
    display: block;
    margin: 20px auto;
    width: 100%;
    max-width: 400px;
    padding: 10px;
    font-size: 16px;
    border: none;
    border-radius: 4px;
    box-shadow: 0px 1px 3px rgba(0, 0, 0, 0.3);
    outline: none;
}

.popup {
    position: fixed;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    background-color: white;
    padding: 20px;
    z-index: 100;
    box-shadow: 0px 2px 6px rgba(0, 0, 0, 0.3);
}

button {
    color: white;
    cursor: pointer;
    transition: background-color 0.3s ease;
}

button:hover {
    background-color: 'white';
}

.theme-default {
    border-radius: 4px;
    box-shadow: 0px 1px 3px rgba(0, 0, 0, 0.3);
    background-color: white;
}

.holiday-us-traditional {
    background-color: #f6d9d9;
}

.holiday-us-official {
    background-color: #f6e7d9;
}

.update{
    background-color: yellowgreen;
}

.delete{
    background-color: lightcoral;
}

.close{
    background-color: lightblue;
}
</style>