
<script setup lang="ts">


import RecipientsBadge from './RecipientsBadge.vue'
 import { ref, onMounted , onUnmounted} from 'vue';

      // Create a reactive reference to store the window width
      const windowWidth = ref(window.innerWidth);
      const use_space = ref(((window.innerWidth-208)/2-76)/8);
      
      
      const props = defineProps<{
        recipients: string[]
      }>()
      const tp_show = ref(true);
    
      const recipientsObject = props.recipients
     // const dataDisplay = recipientsObject[0];
      let tooltipDisplay :string = recipientsObject[0];
      let num: number = 0;


       for( num = 1; num < recipientsObject.length; num++)
          {    
              tooltipDisplay +=", " + (recipientsObject[num]);
          }

      let dataDisplayMulti = ref(recipientsObject[0]);
      let email_num  = ref(0);
      let count:number = recipientsObject[0].length;
      
      function getDispalyemail(){
        console.log("create")
        email_num.value  = recipientsObject.length - 1;
          
          if(email_num.value > 0 ) 
            {
              for( num = 1; num < recipientsObject.length; num++ )
                {
                  if((count + recipientsObject[num].length)  < (use_space.value-6))
                  {
                    count = count + recipientsObject[num].length;
                    dataDisplayMulti.value += ", " + (recipientsObject[num]);
                    email_num.value -= 1;
                  }
                  else{
                    break;
                  }
                  
                }
                //dataDisplayMulti += count; 
                if( email_num.value != 0 )
                  { 
                    dataDisplayMulti.value +=", ...";
                  }
            } 
          else{
              dataDisplayMulti.value = recipientsObject[0];
              email_num.value = 0;
            }
      }
      getDispalyemail();

      // Function to update the window width

      const updateWindowWidth = () => {
        if( windowWidth.value !=window.innerWidth)
            windowWidth.value = window.innerWidth;
            use_space.value = ((windowWidth.value-208)/2-76)/8;       
            window.location.reload();

      };

      // Add event listeners when the component is mounted
      onMounted(() => {
        window.addEventListener('resize', updateWindowWidth);
       
      });

      // Remove event listeners when the component is unmounted
      onUnmounted(() => {
        window.removeEventListener('resize', updateWindowWidth);
       
      });
      


</script>

<template>
  <!-- <span>{{ recipients }}</span> -->
  <div id="app1">
    <div class="badge-div" ref="cell">
      <!-- <button @click="changeVal">get</button> -->
        <span v-if="email_num <= 1 " class="ellipsis">{{ dataDisplayMulti }}</span>
     
      
        <span v-else-if="email_num > 1" class="ellipsis">{{ dataDisplayMulti }}</span>
       
      
      <div class="badge-left" v-if="email_num >= 1" @mouseover="tp_show = false" @mouseleave=" tp_show = true">
          <RecipientsBadge :numTruncated=email_num></RecipientsBadge>
       
      </div>
  </div>

<!-- tooltip -->
  <span v-if = "tp_show" class="tooltip" :style="{ display: 'none' }">{{ tooltipDisplay }}</span>

  <span v-else  class="tooltip" :style="{ display: 'block' }" >{{ tooltipDisplay }}</span>
  </div>
      
  
</template>

<style>
.badge-div{
  display: flex;
  position: relative;
  justify-content: space-between;
}
.badge-left{
  /* position: absolute;
  right: 0px; */
  position: relative;

}
.text {
min-width: 10px;
text-overflow: ellipsis;
}

.ellipsis {
  display: inline-block;
  max-width: 100%;
  overflow: hidden;
  white-space: nowrap;
  text-overflow: ellipsis;
}

.tooltip{

  z-index: 1000;
  /* display: none; */
  /* left: 300px; */
  align-items: center;
  position: absolute;
  /* top: 8.3em; */
  right:1.4em;
  
  /* left: 150px; */
  /* Margin-top:8px; */
  margin-top: -4em;
  Padding : 8px 16px;
  Background-color : #666;
  color : #f0f0f0;
  border-radius:  24px;
  overflow-y: auto;
  overflow-x: show;
}

</style>