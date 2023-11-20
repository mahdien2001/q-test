<template>
    <div>
        <q-toolbar class=" shadow-10 q-px-xl q-py-lg">
            <q-toolbar-title>
                Where in the world?
            </q-toolbar-title>
            <q-btn flat dense @click="change_mode()">
                <q-icon color="secondary" name="nightlight_round" />
                Dark mode
            </q-btn>
        </q-toolbar>
    </div>
</template>

<script>
import { Dark } from 'quasar'

export default {

    data() {
        return {
            mode: false,
        }
    },
 
    methods: {
        change_mode() {
            this.mode = !Dark.mode
            Dark.set(this.mode)
        },
    },

}
</script>