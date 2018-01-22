<template>
    <div class="modal" :class="openmodal">
        <div class="modal-background"></div>
        <div class="modal-card">
            <header class="modal-card-head">
                <p class="modal-card-title">Show {{list.name}} details</p>
                <button class="delete" aria-label="close" @click="close"></button>
            </header>
            <section class="modal-card-body">

                <li class="panel-block">
                    <label class="clumn is-2"><strong>Name: </strong></label>&nbsp;{{list.name}}
                </li>
                <li class="panel-block">
                    <label class="clumn is-2"><strong>Phone: </strong></label>&nbsp;{{list.phone}}
                </li>
                <li class="panel-block">
                    <label class="clumn is-2"><strong>Email: </strong></label>&nbsp;{{list.email}}
                </li>

            </section>
            <footer class="modal-card-foot">
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
                list: ''
            }
        },
        methods: {
            close() {
                this.$emit('closeRequest')
            }
        }
    }
</script>