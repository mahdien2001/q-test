<template>
    <div>
       
        <!-- main -->
        <div class="q-pa-lg">
            <!-- filter & sort -->
            <div class="row justify-around q-my-lg">
                <q-input class="col-md-4 col-xs-12 self-start q-ma-md" standout color="secondary" v-model="search"
                    @input="search_countries" label="Search for country ..." />

                <q-select class="col-md-3 col-xs-12 self-end q-ma-md" standout color="secondary" v-model="region"
                    :options="options" @input="search_by_region" label="Filter by region" />

                <q-select class="col-md-3 col-xs-12 self-end q-ma-md" standout color="secondary" v-model="sort_type"
                    :options="sort_options" @input="sort" label="Sort by ..." />
            </div>


            <!-- countrys -->
            <div class="row wrap justify-center">
                <q-card class=" q-my-md col-md-3 q-mx-md col-xs-12" flat bordered v-for="country in countries"
                    :key="country.id" @click="$router.push({ path: '/details', query: { code: country.cca2 } })">
                    <img class="shadow-2" height="180px" :src="country.flags.png">
                    <q-card-section>
                        <div class="text-h6 q-mx-md">{{ country.name.common }}</div>
                    </q-card-section>

                    <q-card-section>
                        <div class="text-subtitle2 q-mx-md">
                            <q-icon color="secondary" name="people" />
                            Population : {{ country.population }}
                        </div>
                        <div class="text-subtitle2 q-mx-md">
                            <q-icon color="secondary" name="south_america" />
                            Region : {{ country.region }}
                        </div>
                        <div class="text-subtitle2 q-mx-md" v-for="c in country.capital" :key="c.name">
                            <q-icon color="secondary" name="location_on" />
                            Capital : {{ c }}
                        </div>
                    </q-card-section>

                </q-card>
            </div>
        </div>

    </div>
</template>

<script>
import axios from "axios"


export default {

    data() {
        return {
            search: "",
            region: "",
            sort_type: "",
            options: [{ label: "Africa", value: "africa" }, { label: "America", value: "america" }, { label: "Asia", value: "asia" }, { label: "Europe", value: "europe" }, { label: "Oceania", value: "oceania" }],
            sort_options: [{ label: "The largest population", value: "more" }, { label: "The smallest population", value: "least" }, { label: "Alphabet", value: "alphabet" }],
            // API
            countries: [],
        };
    },
    mounted() {
        this.get_countries();
    },
    methods: {

        async get_countries() {
            this.$store.commit('loading_status', true)
            await axios({
                method: "GET",
                url: "https://restcountries.com/v3.1/all",
            })
                .then((res) => {
                    this.countries = res.data;
                })
                .catch((err) => {
                    console.log(err);
                })
                .finally(() => {
                    this.$store.commit('loading_status', false)

                });
        },

        async search_by_region() {
            this.$store.commit('loading_status', true)

            await axios({
                method: "GET",
                url: "https://restcountries.com/v3.1/region/" + this.region.value,
            })
                .then((res) => {
                    this.countries = res.data;
                })
                .catch((err) => {
                    console.log(err);
                })
                .finally(() => {
                    this.$store.commit('loading_status', false)
                });
        },

        async search_countries() {
            this.$store.commit('loading_status', true)
            await axios({
                method: "GET",
                url: "https://restcountries.com/v3.1/name/" + this.search,
                headers: {
                    token: localStorage.getItem("token")
                },
                params: {
                    id: this.$route.query.id
                }
            })
                .then((res) => {
                    this.countries = res.data;
                })
                .catch((err) => {
                    console.log(err);
                })
                .finally(() => {
                    this.$store.commit('loading_status', false)
                });
        },

        sort() {
            if (this.sort_type.value === 'more') {
                this.countries.sort((a, b) => b.population - a.population)
            }
            if (this.sort_type.value === 'least') {
                this.countries.sort((a, b) => a.population - b.population)
            }
            if (this.sort_type.value === 'alphabet') {
                this.countries.sort((a, b) => a.name.common.localeCompare(b.name.common))
            }

        }
    },
}
</script>

<style lang="sass" scoped>

.my-card
  width: 22.5%
  max-width: 100%
</style>0