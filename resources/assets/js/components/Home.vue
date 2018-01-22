<template>
    <div>
        <nav class="panel column is-offset-2 is-8">
            <p class="panel-heading">
                <span>Phonebook</span>
                <button class="button is-small is-link is-outlined is-pulled-right" @click="openAdd">Add new</button>
            </p>
            <div class="panel-block">
                <p class="control has-icons-left">
                    <input class="input is-small" type="text" placeholder="search" v-model="searchQuery">
                    <span class="icon is-small is-left">
                    <i class="fa fa-search" aria-hidden="true"></i>
                </span>
                </p>
            </div>
            <a class="panel-block" v-for="item, key in temp">
            <span class="panel-icon">
              <i class="fa fa-id-card-o" aria-hidden="true"></i>
            </span>
                <div class="column is-9">
                    <a @click="openShow(key)">{{item.name}}</a>
                </div>
                <div class="buttons column is-3 ">
                    <a class="button is-success is-small is-outlined " @click="openShow(key)">
                        <i class="fa fa-eye" aria-hidden="true"></i></a>
                    <a class="button is-info is-small is-outlined " @click="openUpdate(key)">
                        <i class="fa fa-pencil" aria-hidden="true"></i></a>
                    <a class="button is-danger is-small is-outlined " @click="del(key,item.id)">
                        <i class="fa fa-trash" aria-hidden="true"></i></a>

                </div>
            </a>
        </nav>


        <Add :openmodal="addActive" @closeRequest="close"></Add>
        <Show :openmodal="showActive" @closeRequest="close"></Show>
        <Update :openmodal="updateActive" @closeRequest="close"></Update>
    </div>
</template>

<script>
    let Add = require('./Add.vue');
    let Show = require('./Show.vue');
    let Update = require('./Update.vue');

    export default {
        components: {Add, Show, Update},
        data() {
            return {
                addActive: '',
                showActive: '',
                updateActive: '',
                lists: {},
                errors: {},
                searchQuery: '',
                temp: {},
            }
        },
        watch: {
            searchQuery() {
                if (this.searchQuery.length > 0) {
                    this.temp = this.lists.filter((item) => {
                        return Object.keys(item).some((key) => {
                            let string = String(item[key]);
                            return string.toLowerCase().indexOf(this.searchQuery.toLowerCase()) > -1
                        });
                    });
                } else {
                    this.temp = this.lists;
                }
            }
        },
        mounted() {
            axios.post('/getData')
                .then((response) => {
                    this.lists = this.temp = response.data
                })
                .catch((error) => this.errors = error.response.data.errors);
        },
        methods: {
            openAdd() {
                this.addActive = 'is-active';
            },
            close() {
                this.addActive = this.showActive = this.updateActive = '';
            },
            openShow(key) {
                this.$children[1].list = this.temp[key];
                this.showActive = 'is-active';
            },
            openUpdate(key) {
                this.$children[2].list = this.temp[key];
                this.updateActive = 'is-active';
            },
            del(key, id) {
                if (confirm('Are you sure?')) {
                    axios.delete(`/phonebook/${id}`)
                        .then((response) => {
                            this.lists.forEach((value, index) => {
                                if (id === value.id) {
                                    this.lists.splice(index, 1);
                                }
                            });
                            this.temp.splice(key, 1);
                        })
                        .catch((error) => this.errors = error.response.data.errors);
                }
            }
        }
    }
</script>