<template>
   <div class="p-2">
      <div class="field">
         <div class="control">
            <input
               @input="onInput($event)"
               placeholder="Type here..."
               type="text"
               class="input"
            />
         </div>
      </div>
      <Table
         :content="tableContent"
         :config="tableConfig"
         @select="selectRecord"
      />
      <ListItemCreator
         v-if="isModalActive"
         :isModalActive="isModalActive"
         :modalData="modalData"
         @closeModal="closeModal"
         @changeRecordData="changeRecordData"
      />
   </div>
</template>
<script>
import Table from '@/components/Table.vue'
import { computed, onMounted, reactive, ref,  } from 'vue'
import { filterList, mapList } from './listHelper'
import dummy from '@/assets/dummy.json'
import timeout from 'q'
import ListItemCreator from './components/ListItemCreator.vue'
export default {
   components: { Table, ListItemCreator },
   setup() {
      const isModalActive = ref(false)
      const modalData = ref('')
      const tableConfig = {
         columns: [
            { key: 'id', header: 'ID' },
            { key: 'name', header: 'Name' },
            { key: 'cords_display', header: 'Cords' },
            { key: 'tags_display', header: 'Tags' },
            { key: 'status', header: 'Status' }
         ]
      }
      const state = reactive({
         items: [],
         initLoading: true,
         search: '',
         timeout: ''
      })

      const tableContent = computed(() =>
         state.items.filter(item => filterList(item, state.search)).map(mapList)
      )

      const onInput = event => {
         const value = event.target.value
         clearTimeout(timeout)
         state.timeout = setTimeout(() => (state.search = value), 500)
      }
      const mockRequest = () => {
         return new Promise(resolve => {
            setTimeout(() => {
               state.items = dummy
               resolve()
            }, 500)
         })
      }
      onMounted(async () => {
         await mockRequest()
         state.loading = false
      })

      function selectRecord(data) {
         isModalActive.value = true
         modalData.value = data
      }

      function closeModal() {
         isModalActive.value = null
         modalData.value = ''
      }

      function changeRecordData(data){
        state.items = state.items.map(item => {
           if(item.id === data.id){
              return {...item, status: data.isVerified, name: data.name};
           } else return item
        })
      }

      return {
         tableContent,
         tableConfig,
         onInput,
         selectRecord,
         isModalActive,
         modalData,
         closeModal,
         changeRecordData,
      }
   }
}
</script>
