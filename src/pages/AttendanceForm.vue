<template>
  <v-container style="margin-top: 50px;">
    <v-flex xs12>
      <v-row>
        <h1 class="page-title">Attendance</h1>
        <!-- {{ eventData }} -->
          <br><br>
          <v-spacer></v-spacer>
          <!-- <pre>{{attendance}}</pre> -->
            <v-btn tile outlined 
              to="/calendar"
              color="primary">
                <v-icon left>mdi-calendar</v-icon>Calendar
            </v-btn>    
          <br><br>
      </v-row>
      <v-row>
        <!-- RENDER FOR GROUPS -->
        <v-col v-if="eventData.grpScheduleID">
          <v-card class="flex-wrap"
            min-height="200" style="padding: 20px;margin-bottom: 30px;" elevation="2">  
            <small>group</small>
            <div class="mdl-card__title">
              <h2 class="mdl-card__title-text">{{ eventData.name }} - Group {{ eventData.groupNo }}</h2>
            </div>
            <span>DATE: {{ eventData.classDate }}</span>
          </v-card>
        </v-col>
        <!-- RENDER FOR PRIVATE -->
        <v-col v-if="eventData.grpScheduleID === ''">
          <v-card class="flex-wrap"
            min-height="200" style="padding: 20px;margin-bottom: 30px;" elevation="2">    
            <div class="mdl-card__title">
              <small>private</small>
              <h2 class="mdl-card__title-text">{{ eventData.fname }} {{ eventData.sname }}</h2>
            </div>
            <span>DATE: {{ eventData.classDate }}</span>
          </v-card>
        </v-col>
      </v-row>
      <v-col class="d-flex" cols="12" sm="6" align="right" justify="end">
        <v-select
          :items="outcomeOptions"
          label="Please make a selection for this class"
          solo
          v-model="selectedOption"
          outlined
          @change="otherStatusSelected()"
        ></v-select>
      </v-col>
      <v-card v-if="showForm" class="flex-wrap"
              min-height="200" style="padding: 20px;">
        <v-simple-table>
          <!-- ATTENDANCE FORM TEMPLATE FOR GROUP CLASS -->
          <template v-if="eventData.grpScheduleID">
            <thead>
              <tr>
                <th class="text-left">
                  Student
                </th>
                <th class="text-left">
                  Present
                </th>
                <th class="text-left">
                  Remarks <small>(Optional)</small>
                </th>
              </tr>
            </thead>
            <tbody>
              <tr  v-for="(student,index) in eventData.students" :key="index">
                <td> 
                  <p class="font-weight-bold student_name">
                  {{student["Student classmates startup::First Name"]}}
                  {{student["Student classmates startup::Last Name"]}} 
                  </p>   
                </td>  
                <td>
                  <v-layout wrap align-center>
                    <v-flex xs12 sm6 d-flex>
                      <v-checkbox            
                          v-model="attendance[index].present"
                          @change="updateAttendance(index)"                    
                      ></v-checkbox>
                    </v-flex>
                  </v-layout>  
                </td>
                <td>
                  <v-layout wrap align-center>
                  <!-- <v-flex xs12 sm6 d-flex> -->
                    <v-textarea
                      label="Note"
                      rows="1"
                      row-height="15"
                      class="remarks"
                      @input="remark => updateRemarks(index,remark)"           
                    ></v-textarea>
                  <!-- </v-flex> -->
                  </v-layout>
                </td>
              </tr>
            </tbody>
          </template>

          <!-- ATTENDANCE FORM TEMPLATE FOR PRIVATE CLASS -->
          <template v-if="eventData.grpScheduleID === ''">
            <thead>
              <tr>
                <th class="text-left">
                  Student
                </th>
                <th class="text-left">
                  Present
                </th>
                <th class="text-left">
                  Remarks <small>(Optional)</small>
                </th>
              </tr>
            </thead>
            <tbody>
              <tr>
                <td> 
                  <p class="font-weight-bold student_name">
                    {{ eventData.fname }} {{eventData.sname}}
                  </p>        
                </td>  
                <td>
                  <v-layout wrap align-center>
                    <v-flex xs12 sm6 d-flex>
                      <v-checkbox            
                          v-model="attendance[0].present"
                          @change="updateAttendance(0)"                    
                      ></v-checkbox>
                    </v-flex>
                  </v-layout>  
                </td>
                <td>
                  <v-layout wrap align-center>
                    <v-textarea
                      label="Note"
                      rows="1"
                      row-height="15"
                      class="remarks"
                      @input="remark => updateRemarks(0,remark)"           
                    ></v-textarea>
                  </v-layout>
                </td>
              </tr>
            </tbody>   
          </template>
        </v-simple-table>

        <!-- Attendance form actions -->
        <v-row
          align="center"
          justify="space-around"
        >
          <v-spacer></v-spacer>
    
          <v-card-actions>
            <v-btn color="error" @click="cancel">Cancel</v-btn>
            <v-btn v-show="eventData.outcome !== 'done'" class="btn btn-primary" @click="submit" color="primary">
              Submit
            </v-btn> 
          </v-card-actions>
        </v-row>
      </v-card>
    </v-flex>
  </v-container>
</template>

<script>

import privateClassData from '../data/private-class.json' 

export default  {
  components: {
    // 'confirm-dialog': ConfirmDialog, ? 
  },
  data() {
    return {
      attendance:[],
      eventData: [],
      outcomeOptions: ['Attendance Form', 'LMC', 'PS', 'PT', 'NS'], // TODO: dynamic info box for each option
      selectedOption: null,
      showForm: false,
    }
  },
  async mounted () {
    this.eventData = await this.createStudentAttendance()
    console.log('CLASS EVENT : ', this.eventData)
  },
  methods: {
    submit() {
      // TODD
      // UI/UX ideas 
      // User feedback that attendance is being uploaded
      // Dialog modal to notify submit success/failure after upload has completed

      // Fake API call
      setTimeout(function(){ alert("Attendance Submitted. Thank you!"); }, 2000);
    },
    cancel(){
      console.log('Cancelled...')
    },
    async createStudentAttendance() {

      const attendanceRecord = {
        fname: privateClassData[0]['portalData']['Student'][0]['Student::First Name'],
        sname: privateClassData[0]['portalData']['Student'][0]['Student::Last Name'],
        sID: privateClassData[0]['portalData']['Student'][0]['Student::_kp_student_id'],
        sessionRegId: '99999988888',
        classDate: '12/05/2020',
        classID: '8888777777',
        grpScheduleID: ''
      }

      await this.attendance.push(attendanceRecord)
      return attendanceRecord
    },
    updateAttendance(index){
      if(index === 0) {
        console.log('A PRIVATE FORM...', this.attendance) 
      } else {
        console.log('INDEX of group student attendance record : ', this.attendance[index])
      }
    },
    updateRemarks(i, t) {
      this.attendance[i].remarks = t
      console.log(this.attendance[i].remarks)
    }, 
  }, 
  watch: {  
    selectedOption: function(){
      if(this.selectedOption === 'Attendance Form') {
        console.log('STATUS SELECTED : DONE = ', this.selectedOption)
        this.showForm = true

      } else if(this.selectedOption  !== null && this.selectedOption !== 'Attendance Form'){
        console.log('STATUS SELECTED : DONE = ', this.selected)
        this.showForm = false
      }
    },  
  }
}
</script>
