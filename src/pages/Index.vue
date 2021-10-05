<template>
  <div>
    Battery status is: <strong>{{ batteryStatus }}</strong>
  </div>
  <div>
      <div>IMEI:</div>{{imei}}
      <h5>
        <div>
     <q-btn @click="actionButton1()"
     class="glossy" color="teal" label="Objective-C"  />
      <br>
      <q-btn @click="actionButton2()"
     class="glossy" color="orange" label="Swift"  />
      <br>
      <q-btn @click="actionButton3()"
     class="glossy" color="yellow" label="Present"  />
      <br>
      <q-btn @click="actionButton4()"
     class="glossy" color="green" label="Others"  />
      <br>
    </div>

     </h5>
      <div class="q-pa-md" style="max-width: 400px">

    <q-form
      @submit="onSubmit"
      @reset="onReset"
      class="q-gutter-md"
    >
      <q-input
        filled
        v-model="name"
        label="Your name *"
        hint="Name and surname"
        lazy-rules
        :rules="[ val => val && val.length > 0 || 'Please type something']"
      />

      <q-input
        filled
        type="number"
        v-model="age"
        label="Your age *"
        lazy-rules
        :rules="[
          val => val !== null && val !== '' || 'Please type your age',
          val => val > 0 && val < 100 || 'Please type a real age'
        ]"
      />
      <q-toggle v-model="accept" label="I accept the license and terms" />

      <div>
        <q-btn label="Submit" type="submit" color="primary"/>
        <q-btn label="Reset" type="reset" color="primary" flat class="q-ml-sm" />
      </div>
    </q-form>
    <div>RESULTT:</div>{{resultNative}}
  </div>
  </div>
</template>

<script>
import { ref, onBeforeUnmount } from 'vue'
import { useQuasar } from 'quasar'


export default {
  data() {
    return {
      resultNative: 'No Result yet'
    }
  }, 
  setup () {
    const $q = useQuasar()
    const resultNative = ref(null)
    const strResult = ref(null)
    const name = ref(null)
    const age = ref(null)
    const accept = ref(false)

    const functionCallback = function() { 
        this.resultNative = 'hahahahah';
      }
    const batteryStatus = ref('determining...')
    const imei = ref(
      window.device === void 0
        ? 'Run this on a mobile/tablet device'
        : window.device
    )

    function updateBatteryStatus (status) {
      batteryStatus.value = `Level: ${status.level}, plugged: ${status.isPlugged}`
    }
    
    // we register the event like on plugin's doc page
    window.addEventListener('batterystatus', updateBatteryStatus, false)
    
    onBeforeUnmount(() => {
      // we do some cleanup;
      // we need to remove the event listener
      window.removeEventListener('batterystatus', updateBatteryStatus, false)
    })


    return {
      batteryStatus, 
      imei,
      name,
      age,
      accept,
      onSubmit () {
        if (accept.value !== true) {
          $q.notify({
            color: 'red-5',
            textColor: 'white',
            icon: 'warning',
            message: 'You need to accept the license and terms first'
          })
        }
        else {
          $q.notify({
            color: 'green-4',
            textColor: 'white',
            icon: 'cloud_done',
            message: 'Submitted'
          })
        }
      },

      onReset () {
        name.value = null
        age.value = null
        accept.value = false
      }
    }
  },
  methods: {
    actionButton1() {
      //modusecho.echo('Testing Alert');
       this.resultNative = strResult;
    },
    actionButton2() {
      var dict = {
        name: this.name,
        age: this.age
      };    
      var success = function(message) {
        var delayInMilliseconds = 1000; //1 second
        setTimeout(function() {
          printSuccess(message)
          this.strResult = message
        }, delayInMilliseconds);
      }
      var failure = function(message) {
        var delayInMilliseconds = 1000; //1 second
        setTimeout(function() {
          printSuccess(message)
          this.strResult = message
        }, delayInMilliseconds);
      }

    
      cordova.plugins.cyplugin.add(dict, success, failure);
      
    },
    actionButton3(){
       var success = function(message) {
         this.strResult = message
         printSuccess(message)
      }
      var failure = function(message) {
        printError(message)
      }
       cordova.plugins.cyplugin.coolMethod("dict", success, failure);
    },
    
    actionButton4(){
      var success = function(message) {
          //alert(message);//this.resultNative = message;//alert(message);
         //this.resultNative = message;
        
        var delayInMilliseconds = 2000; //1 second
        setTimeout(function() {
          printSuccess(message)
          this.strResult = message
        }, delayInMilliseconds);
          
      }

      var failure = function(message) {
          alert("Error calling Hello Plugin");
          printError(message)
      }

     
      hello.greet("World", success, failure);
      
      

    },
    

  },
  mounted() {
      //modusecho.echo('Testing Alert');
      //cyplugin.coolMethod('CYPLUGIN ECHO');
     
  },
  
}

  function printSuccess(message) {
    console.log("SUCCESS", message)
  }

  function printError(message) {
    console.log("ERROR", message)
  }
</script>