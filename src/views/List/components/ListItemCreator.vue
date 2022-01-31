<template>
   <div class="modal is-active">
      <div class="modal-background has-background-dark"></div>
      <div class="modal-card">
         <header class="modal-card-head">
            <p class="modal-card-title">Edit</p>
            <button
               class="delete"
               aria-label="close"
               @click="closeModal"
            ></button>
         </header>
         <section class="modal-card-body">
            <form @submit.prevent="changeRecordData" class="columns">
               <!-- INPUT -->
               <div class="field column is-three-fifths">
                  <label class="label">Name</label>
                  <div class="control">
                     <input
                        class="input"
                        type="text"
                        v-model="name"
                        :placeholder="name"
                     />
                  </div>
               </div>

               <!-- SELECT -->

               <div class="field column is-two-fifths">
                  <label class="label">Status</label>
                  <div class="select is-fullwidth">
                     <select v-model="isVerified" class="is-12">
                        <option>verified</option>
                        <option>unverified</option>
                     </select>
                  </div>
               </div>
            </form>
         </section>
         <footer class="modal-card-foot">
            <div class="container">
               <button
                  class="button is-success"
                  type="submit"
                  @click="changeRecordData"
               >
                  Save changes
               </button>
               <button class="button" @click="closeModal">Cancel</button>
            </div>
         </footer>
      </div>
   </div>
</template>
<script>
import { ref } from 'vue'
export default {
   props: ['isModalActive', 'modalData'],
   setup(props, { emit }) {
      const name = ref(props.modalData.name)
      const isVerified = ref(props.modalData.status)
      function closeModal() {
         name.value = ''
         emit('closeModal')
      }

      function changeRecordData() {
         emit('changeRecordData', {
            name: name.value,
            isVerified: isVerified.value,
            id: props.modalData.id
         })
         emit('closeModal')
      }

      return {
         closeModal,
         name,
         isVerified,
         changeRecordData
      }
   }
}
</script>
<style lang="scss" scoped>
</style>
