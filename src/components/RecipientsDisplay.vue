
<script setup lang="ts">

import { method } from 'lodash-es';
import RecipientsBadge from './RecipientsBadge.vue'
 import { ref, onMounted , onUnmounted} from 'vue';

      // Create a reactive reference to store the window width
      const windowWidth = ref(window.innerWidth);
      const use_space = ref((window.innerWidth-208)/2-76);
      // Function to update the window width
      const updateWindowWidth = () => {
        windowWidth.value = window.innerWidth;
        use_space.value = (windowWidth.value-208)/2-76;
        
      };
      // const getDispalyemail = () => {

      // }
      // Add event listeners when the component is mounted
      onMounted(() => {
        window.addEventListener('resize', updateWindowWidth);
        // window.addEventListener('resize', getDispalyemail);
      });

      // Remove event listeners when the component is unmounted
      onUnmounted(() => {
        window.removeEventListener('resize', updateWindowWidth);
      });
 
      
      const props = defineProps<{
        recipients: string[]
      }>()
      const tp_show = ref(true);
    
      const recipientsObject = props.recipients
     // const dataDisplay = recipientsObject[0];
      let tooltipDisplay :string = recipientsObject[0];
      let num: number = 0;
      let count:number = recipientsObject[0].length;
      let email_num :number = recipientsObject.length - 1;
      let dataDisplayMulti:string = recipientsObject[0];

    for( num = 1; num < recipientsObject.length; num++)
          {    
              tooltipDisplay +=", " + (recipientsObject[num]);
    }

    if(email_num > 0 )
      {
        for( num = 1; num < recipientsObject.length; num++)
          {
            if(count+recipientsObject[num].length < use_space.value/8)
            {
              count =count + recipientsObject[num].length;
              dataDisplayMulti +=", " + (recipientsObject[num]);
              email_num -= 1;
            }
            else{
              
            }
            
          }
          //dataDisplayMulti += count; 
          if( email_num != 0 )
            { 
              dataDisplayMulti +=", ...";
            }
      } 
      else{
        dataDisplayMulti = recipientsObject[0];
        email_num = 0;
      }


</script>

<template>
  <!-- <span>{{ recipients }}</span> -->
  <div id="app1">
    <div class="badge-div" ref="cell">
      <!-- <button @click="changeVal">get</button> -->
        <span v-if="email_num <= 1 " class="ellipsis">{{ use_space }}</span>
     
      
        <span v-else-if="email_num > 1" class="ellipsis">{{ windowWidth }}</span>
       
      
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