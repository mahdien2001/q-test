<template>
    <div>
        <!-- back -->
        <q-btn to="/" class="q-ma-xl" outline color="secondary" label="Back" icon="keyboard_backspace" />

        <!-- info -->
        <div class="row justify-around items-start q-my-xl" v-if="country.flags">
            <img class="col-md-4 col-xs-10 shadow-2" :src="country.flags.png" alt="">
            <div class="row col-md-6 col-xs-10">
                <div class="col-12 q-pt-lg">
                    <strong class="text-h3">{{ country.name.common }}</strong>
                </div>
                <div class="row">
                    <div class="col-md-6 col-xs-12 q-mt-md">
                        <strong>Native name : </strong>
                        <span v-for="name in native_names" :key="name.id"> {{ name }} , </span>
                    </div>
                    <div class="col-md-6 col-xs-12 q-mt-md">
                        <strong>Population : </strong>
                        <span>{{ country.population }}</span>
                    </div>
                    <div class="col-md-6 col-xs-12 q-mt-md">
                        <strong>Region : </strong>
                        <span>{{ country.region }}</span>
                    </div>
                    <div class="col-md-6 col-xs-12 q-mt-md">
                        <strong>Sub Region : </strong>
                        <span>{{ country.subregion }}</span>
                    </div>
                    <div class="col-md-6 col-xs-12 q-mt-md">
                        <strong>Capital : </strong>
                        <span v-for="c in country.capital" :key="c.id">{{ c }}</span>
                    </div>
                    <div class="col-md-6 col-xs-12 q-mt-md">
                        <strong>Top Level Domain : </strong>
                        <!-- <span>{{ country }}</span> -->
                    </div>
                    <div class="col-md-6 col-xs-12 q-mt-md">
                        <strong>Currencies : </strong>
                        <span v-for="currencie in currencies" :key="currencie.id">{{ currencie }} , </span>
                    </div>
                    <div class="col-md-6 col-xs-12 q-mt-md">
                        <strong>languages : </strong>
                        <span v-for="language in languages" :key="language.id">{{ language }} , </span>

                    </div>
                </div>

            </div>
        </div>


    </div>
</template>

<script>
import axios from "axios"

export default {

    data() {
        return {
            native_names: [],
            currencies: [],
            languages: [],

            // API
            country: {},
        }
    },
    mounted() {
        this.get_country()
    },
    methods: {

        async get_country() {
            this.$store.commit('loading_status', true)
            await axios({
                method: 'GET',
                url: 'https://restcountries.com/v3.1/alpha/' + this.$route.query.code,
            })
                .then((res) => {
                    this.country = res.data[0]

                    //create native name array
                    for (let key in this.country.name.nativeName) {
                        if (!this.country.name.nativeName.hasOwnProperty(key)) continue;

                        let obj = this.country.name.nativeName[key];
                        for (let prop in obj) {
                            if (!obj.hasOwnProperty(prop)) continue;

                            // finally array
                            if (prop === 'common') {
                                this.native_names.push(obj[prop])
                            }
                        }
                    }

                    //create currencies array
                    for (let key in this.country.currencies) {
                        if (!this.country.currencies.hasOwnProperty(key)) continue;

                        let obj = this.country.currencies[key];
                        for (let prop in obj) {
                            if (!obj.hasOwnProperty(prop)) continue;

                            // finally array
                            if (prop === 'name') {
                                this.currencies.push(obj[prop])
                            }
                        }
                    }

                    //create languages array
                    for (let key in this.country.languages) {
                        if (!this.country.languages.hasOwnProperty(key)) continue;
                        let obj = this.country.languages[key];

                        // finally array
                        this.languages.push(obj)
                    }
                })
                .catch((err) => {
                    console.log(err);
                })
                .finally(() => {
                    this.$store.commit('loading_status', false)
                })
        },

    },


}
</script>