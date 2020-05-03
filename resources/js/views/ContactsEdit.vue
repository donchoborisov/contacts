<template>
<div>

    <div class="flex justify-between">
        <a href="#" class="text-blue-400" @click="$router.back()">
            < Back
        </a>
    </div>
    <form @submit.prevent="submitForm">
        <InputField :data="form.name" name="name" label="Contact Name" @update:Field="form.name = $event" :errors="errors" placeholder="Contact Name" />
        <InputField  :data="form.email" name="email" label="Contact Email" @update:Field="form.email = $event" :errors="errors" placeholder="Contact Email"/>
        <InputField :data="form.company" name="company" label="Company" @update:Field=" form.company = $event " :errors="errors"  placeholder="Company"/>
        <InputField :data="form.birthday" name="birthday" label="Birthday" @update:Field=" form.birthday = $event" :errors="errors" placeholder="MM/DD/YYYY"/>
     <div class="flex  justify-end">
         <button class="rounded text-red-700 border hover:border-red-700 mr-5 py-2 px-4">Cancel</button>
         <button class="bg-blue-500 text-white rounded py-2 px-4 hover:bg-blue-400">Save</button>
     </div>
    </form>
</div>
</template>

<script>
    import InputField from "../components/InputField";
    export default {
        name: "ContactsCreate",
        components:{
            InputField
        },
        mounted() {
            axios.get('/api/contacts/'+ this.$route.params.id)
                .then(response =>{
                    this.form = response.data.data;
                    this.loading = false;
                })
                .catch( error => {
                    this.loading = false;
                    if(error.response.status === 404){
                        this.$router.push('/contacts');
                    }

                });

        },
        data:function () {
            return{
                form:{
                    'name':'',
                    'email':'',
                    'company':'',
                    'birthday':'',
                },
                errors:null,
            }
        },
     methods:{
            submitForm:function () {
                axios.patch('/api/contacts/' + this.$route.params.id ,this.form)
                .then(response =>{
                      this.$router.push(response.data.links.self);
                })
                .catch(errors =>{
                  this.errors = errors.response.data.errors;
                });

            }
     }

    }
</script>

<style scoped>

</style>
