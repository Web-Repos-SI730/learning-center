<script>
import PvButton from "../../main.js";

export default {
  name: "data-manager",
  components: {PvButton},
  inheritAttrs:"false",
  props:{
    items:{type:Array, required: true},
    title:{type:{singular: '', plural:''},required:true},
    dynamic:{type: Boolean, default: false},
    columns:{type: Array, default: ()=>[]}
  },
  data(){
    return{
      selectedItems:[],
      filters:null
    }
  },
  methods:{
    initFilters(){
      this.filters={global: {value:null, matchMode:FilterMatchMode.CONTAINS}};
    },
    newItem(){
      this.$emit('new-item-requested');
    },
    confirmDeleteSelected(){
      this.$confirm.require({
        message:`Are you sure you want to delete the selected ${this.title.plural}?`,
        header:'confirmation',
        icon:'pi pi-exclamation-triangle',
        rejectClass:'p-button-secondary p-button=outlined',
        rejectLabel:'Cancel',
        acceptLabel:'Delete',
        acceptClass: 'p-button-danger',
        accept: () => this.$emit('delete-selected-items-requested',this.selectedItems),
        reject:()=> {}
      })
    },
    exportToCsv(){
      this.$refs.dt.exportToCsv()
    },
    editItem(item){
      this.$emit('edit-item-requested', item);
    },
    confirmDeleteItem(item){
      this.$confirm.require({
        message:`Are you sure you want to delete the selected ${this.title.singular}?`,
        header:'confirmation',
        icon:'pi pi-exclamation-triangle',
        rejectClass:'p-button-secondary p-button=outlined',
        rejectLabel:'Cancel',
        acceptLabel:'Delete',
        acceptClass: 'p-button-danger',
        accept: () => this.$emit('delete-item-requested',this.selectedItems),
        reject:()=> {}
      })

    },
    created(){
      this.initFilters();
    }
  }
}
</script>

<template>
  <pv-toast/>
  <pv-confirm-dialog/>
  <h3>Manage {{title.plural}}</h3>

  <!--Toolbar section -->
<pv-toolbar class="mb-4">
  <template #start>
    <pv-button class="mr-2" icon="pi pi-plus" label="New" severity="success" @click="newItem"/>
<pv-button :disabled="!selectedItems || !selectedItems.length" icon="pi pi-trash" label="Delete" severity="" @click="confirmDeleteSelected"/>


  </template>
</pv-toolbar>

</template>

<style scoped>

</style>