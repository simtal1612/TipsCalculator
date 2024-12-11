<template>
  
  <div>
    <h1 class="mb-2">Tips Calculator</h1>

    <v-text-field
      v-model="totalAmountTips"
      label="Enter total amount tips"
      type="number"
    ></v-text-field>

    <v-btn class="mb-5" @click="addWaiter">Add Waiter</v-btn>
    <br>

    <v-btn @click="calculateTips">Calculate Tips</v-btn>

    <v-dialog v-model="addWaiterClicked" persistent>
      <v-card>
        <v-card-title>Add Waiter</v-card-title>
        <v-card-text>
          <v-text-field
            v-model="workerDataCurrentShift.waiterName"
            label="Waiter's Name"
            type="text"
          ></v-text-field>

          <v-text-field
            v-model="workerDataCurrentShift.hoursWorked"
            label="Hours Worked"
            type="number"
            @input="convertToNumber" 
          ></v-text-field>

          <v-text-field
            v-model="workerDataCurrentShift.HourlyWage"
            label="Hourly Wage"
            type="number"
            @input="convertToNumber" 
          ></v-text-field>
        </v-card-text>
        <v-card-actions>
          <v-btn @click="addWaiterClicked = false">Cancel</v-btn>
          <v-btn @click="submitWaiter">Submit</v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>

    <div v-if="workersDataCurrentShift.length">
      <h3 class="text-h5 mb-4">Waiters</h3>
      <v-list>
        <v-list-item-group v-for="(worker, index) in workersDataCurrentShift" :key="index">
          <v-list-item>
            <v-list-item-content>
              <v-list-item-title class="font-weight-bold">{{ worker.waiterName }}</v-list-item-title>
              <v-list-item-subtitle>
                <span><strong>Hours:</strong> {{ worker.hoursWorked }}</span> | 
                <span><strong>Hourly Wage:</strong> {{ worker.HourlyWage }}</span>
              </v-list-item-subtitle>
            </v-list-item-content>
          </v-list-item>
        </v-list-item-group>
      </v-list>
      </div>
    </div>

   
    <TipsResults v-if="waiterTips.length" :waiterTips="waiterTips" />
  
</template>

<script>
import TipsResults from '../components/TipsResults.vue'

export default {
  components: {
    TipsResults
  },
  data() {
    return {
      totalAmountTips: null,
      addWaiterClicked: false,
      workerDataCurrentShift: {
        waiterName: '',
        hoursWorked: null,
        HourlyWage: null,
        waiterTotalCost: null,
      },
      workersDataCurrentShift: [],
      totalAmountHours: null,
      totalCostOfWaiters: 0,
      tipsForWaitersToShare: 0,
      TAX: 0.17,
      waiterTips: [],
    };
  },
  methods: {
    addWaiter() {
      this.addWaiterClicked = true;
    },
    convertToNumber() {
      this.workerDataCurrentShift.hoursWorked = parseFloat(this.workerDataCurrentShift.hoursWorked) || 0;
      this.workerDataCurrentShift.HourlyWage = parseFloat(this.workerDataCurrentShift.HourlyWage) || 0;
    },
    submitWaiter() {
      this.waiterTotalCost = this.workerDataCurrentShift.hoursWorked * this.workerDataCurrentShift.HourlyWage;
      this.totalCostOfWaiters += this.waiterTotalCost;
      this.totalAmountHours += this.workerDataCurrentShift.hoursWorked;

      this.workersDataCurrentShift.push({ ...this.workerDataCurrentShift });

      this.workerDataCurrentShift.waiterName = '';
      this.workerDataCurrentShift.hoursWorked = 0;
      this.workerDataCurrentShift.HourlyWage = 0;

      this.addWaiterClicked = false;
    },
    calculateTips() {
  
  this.tipsForWaitersToShare = this.totalAmountTips - this.totalCostOfWaiters;
  

  this.tipsForWaitersToShare = this.tipsForWaitersToShare - Math.round(this.tipsForWaitersToShare * this.TAX)
  

  this.tipsForWaitersToShare = Math.round(this.tipsForWaitersToShare);
  
  console.log('this.tipsForWaitersToShare', this.tipsForWaitersToShare);
  console.log('after TAX:', this.tipsForWaitersToShare);
  

  const totalHoursWorked = this.workersDataCurrentShift.reduce((acc, worker) => acc + worker.hoursWorked, 0);
  

  const TipsForHour = this.tipsForWaitersToShare / totalHoursWorked;
  
 

  this.waiterTips = this.workersDataCurrentShift.map(worker => {

    const theShareOfEachWaiter = Math.ceil(worker.hoursWorked * TipsForHour);

  
    return {
      waiterName: worker.waiterName,
      calculatedTip: theShareOfEachWaiter
    };
  });
 this.dialogVisible = true
  // Log the new array containing the waiter's name and calculated tip
  console.log('Waiter tips:', this.waiterTips);
}


  },
  
};
</script>
