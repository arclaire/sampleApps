<template>
  <div>
    Battery status is: <strong>{{ batteryStatus }}</strong>
  </div>
  <div>
    <q-page class="flex flex-center">
      <div>IMEI:</div>
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
     class="glossy" color="green" label="Battery"  />
      <br>
    </div>

     </h5>
    </q-page>
    
  </div>
</template>

<script>
import { ref, onBeforeUnmount } from 'vue'

export default {
  setup () {
   
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
      batteryStatus
    }
  },
  methods: {
    actionButton1() {
      modusecho.echo('Testing Alert');
    },
    actionButton2() {
      cordova.plugins.cyplugin.add('CYPLUGIN ECHO');
    },
    actionButton3(){
       cordova.plugins.cyplugin.coolMethod('HELLO');
    },
    actionButton4(){
       
    },
  },
  mounted() {
      //modusecho.echo('Testing Alert');
      //cyplugin.coolMethod('CYPLUGIN ECHO');
     
  },
  
}
</script>