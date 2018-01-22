<template>
    <div class="modal" :class="openmodal">
        <div class="modal-background"></div>
        <div class="modal-card">
            <header class="modal-card-head">
                <p class="modal-card-title">Add user</p>
                <button class="delete" aria-label="close" @click="close"></button>
            </header>
            <section class="modal-card-body">

                <div class="field">
                    <label class="label">Name</label>
                    <div class="control">
                        <input class="input" type="text" placeholder="Name" v-model="list.name"
                               :class="{'is-danger' : errors.name}">
                    </div>
                    <small class="has-text-danger" v-if="errors.name">{{errors.name[0]}}</small>
                </div>

                <div class="field">
                    <label class="label">Phone</label>
                    <div class="control">
                        <input class="input" type="number" placeholder="Phone" v-model="list.phone"
                               :class="{'is-danger' : errors.phone}">
                    </div>
                    <small class="has-text-danger" v-if="errors.phone">{{errors.phone[0]}}</small>
                </div>

                <div class="field">
                    <label class="label">Email</label>
                    <div class="control">
                        <input class="input" type="email" placeholder="Email" v-model="list.email"
                               :class="{'is-danger' : errors.email}">
                    </div>
                    <small class="has-text-danger" v-if="errors.email">{{errors.email[0]}}</small>
                </div>

            </section>
            <footer class="modal-card-foot">
                <button class="button is-success" @click="save">Save</button>
                <button class="button" @click="close">Cancel</button>
            </footer>
        </div>
    </div>
</template>

<script>
    export default {
        props: ['openmodal'],
        data() {
            return {
                list: {
                    name: '',
                    phone: '',
                    email: '',
                },
                errors: {}
            }
        },
        methods: {
            close() {
                this.$emit('closeRequest')
            },
            save() {
                axios.post('/phonebook', this.$data.list)
                    .then((response) => {
                        this.close();
                        this.$parent.lists.push(response.data);
                        this.$parent.lists.sort(function (a, b) {
                            if (String(a.name).toLowerCase() > String(b.name).toLocaleLowerCase()) {
                                return -1;
                            } else {
                                return 1;
                            }
                        });
                        this.list = {};
                    })
                    .catch((error) => this.errors = error.response.data.errors);
            }
        }
    }
</script>