<template>
    <!-- Modal -->
    <div class="modal fade" :id="'role-'+role.id" tabindex="-1" role="dialog" aria-labelledby="exampleModalScrollableTitle" aria-hidden="true">
      <div class="modal-dialog modal-dialog-scrollable" role="document">
        <div class="modal-content">
          <div class="modal-header">
            <h5 class="modal-title" id="exampleModalScrollableTitle">
                <b>{{role.name}}</b>
            </h5>
            <button type="button" class="close" data-dismiss="modal" aria-label="Close">
              <span aria-hidden="true">&times;</span>
            </button>
          </div>
          <div class="modal-body">
            <form>
                <div class="form-group">
                    <label for="">Role Name</label>
                    <input type="text" class="form-control" v-model="name">
                </div>
                <hr>
                <label for="">Permission</label>
                <div class="form-group">
                    <label for="">Admin</label>
                    <input type="checkbox" v-model="access" value="1">

                </div>
                <div class="form-group">
                    <label for="">User</label>
                    <input type="checkbox" v-model="access" value="2">
                </div>
            </form>
          </div>
          <div class="modal-footer">
            <button type="button" class="btn btn-secondary" data-dismiss="modal">Close</button>
            <button type="button" class="btn btn-primary" @click="save()">Save changes</button>
          </div>
        </div>
      </div>
    </div>
</template>

<script>
    export default {
        props: ['role'],
        data(){
          return{
            id: this.role.id,
            name: this.role.name,
            access: [],
          }
        },
        mounted() {
            axios.post('roles/getRoleAccess',this.role).then(response => {
                this.access = response.data;
            });
        },
        methods: {
            save(){
                let data = {
                    id: this.id,
                    name: this.name,
                    access: this.access,
                };

                axios.post('/roles/save',data).then(response => {

                    if(response.status == 200){
                        this.$emit('role',response);
                    }

                });
            }
       }
    }
</script>
