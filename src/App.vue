<template>
    <div class="container">
        <div class="row">
            <div class="col-xs-12 col-sm-8 col-sm-offset-2 col-md-6 col-md-offset-3">
                <h1>Http</h1>
                <div class="form-group">
                    <label for="">User Name</label>
                    <input type="text" class="form-control" v-model="user.username">
                </div>
                <div class="form-group">
                    <label for="">Mail</label>
                    <input type="text" class="form-control" v-model="user.email">
                </div>
                <button class="btn btn-primary" @click="submit">Submit</button>
                <hr>
                <button class="btn btn-primary" @click="getUsers">Get data</button>
                <hr>
                <ul class="list-group">
                    <li class="list-group-item" v-for="(user, index) in users" :key="index">
                        {{ user.username }} - {{ user.email }}
                    </li>
                </ul>
            </div>
        </div>
    </div>
</template>

<script>
    export default {
        data() {
            return {
                user: {
                    username: '',
                    email: ''
                },
                users: [],
                resource: {}
            }
        },
        methods: {
            getUsers() {
                this.$http.get('data.json')
                    .then(result => {
                        return result.json();
                    }).then(response => {
                        const resultArray = [];
                        for (let key in response) {
                            resultArray.push(response[key]);
                        }
                        this.users = resultArray;
                    });
            },
            submit() {
                //this.resource.save({}, this.user);
                this.resource.saveAlt(this.user);
                // this.$http.post('data.json', this.user).then(response => {
                //     console.log(response);
                // }).catch((err) => {
                //     console.log(err);
                // });
            }
        },
        created() {
            const customActions = {
                saveAlt: { method: 'POST', url: 'alternative.json' }
            }
            this.resource = this.$resource('data.json', {}, customActions);
        }
    }
</script>

<style>
</style>
