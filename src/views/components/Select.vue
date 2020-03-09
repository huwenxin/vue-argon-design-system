<template>
    <ul class="navbar-nav">
        <div class="nav-link">
            <base-input class="mb-2">
                <select class="form-control form-control-alternative" @change="queryModuleList($event.target.value)">
                    <option disabled value="" selected>Studiengänge auswählen</option>
                    <option value="WIB">WI Bachelor</option>
                    <option value="WIM">WI Master</option>
                </select>
            </base-input>
        </div>
        <div class="nav-link">
            <select v-model="selected" class="form-control form-control-alternative">
                <option disabled value=''>Module auswählen</option>
                <option v-if="Object.keys(moduleList).length > 0" v-for="(modulejson, index) in moduleList" v-bind:value="modulejson.module.value" v-bind:key="index">
                    {{ modulejson.label.value }}
                </option>
                <!--<option v-else disabled value="">
                    Bitte einen Studiengang auswählen
                </option>-->
            </select>
        </div>
        <!--<span>Selected: {{ selected }}</span>-->
    </ul>
</template>

<script>
    import axios from "axios";
    import { VBTooltip } from "bootstrap-vue/esm/directives/tooltip/tooltip";

    export default {
        name: 'Select',
        directives: {
            BTooltip: VBTooltip,
        },
        data() {
            return {
                moduleList: [],
                selected: '',
            }
        },
        watch: {
            selected(v){
                this.$emit('module', v);
            }
        },
        methods: {
            queryModuleList(sp) {
                let query = 'PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#> ' +
                    'PREFIX module: <https://bmake.th-brandenburg.de/module/> ' +
                    'PREFIX schema: <https://schema.org/> ' +
                    'SELECT DISTINCT ?module ?label ' +
                    'WHERE { ' +
                    '  ?module a module:Module ; ' +
                    '          schema:isPartOf module:'+ sp +' ; ' +
                    '          rdfs:label ?label. ' +
                    '}';

                axios.post('http://fbw-sgmwi.th-brandenburg.de:3030/modcat/query', query, {headers: {'Content-Type': 'application/sparql-query'}})
                    .then(response => {
                        this.moduleList = response.data.results.bindings
                    })
                    .catch(e => {
                        this.errors.push(e)
                    })
            }
        }
    }
</script>

<style scoped>
    .nav-link {
        width: 230px;
    }
</style>