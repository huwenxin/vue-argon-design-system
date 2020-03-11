<template>
    <div>
        <div class="position-relative">
            <!-- shape Hero -->
            <section class="section-shaped my-0">
                <div class="shape shape-style-1 bg-gradient-warning">
                </div>
                <div class="container shape-container d-flex">
                    <div class="col px-0 w-50">
                        <div class="row">
                            <div class="col-lg-10">
                                <h1 class="display-3  text-white">Modulkatalog@THB
                                    <span>Fachbereich Wirtschaft</span>
                                </h1>
                            </div>
                        </div>
                    </div>
                    <base-nav class="navbar-main w-50" type="" expand>
                        <Select @module="getModule"></Select>
                    </base-nav>
                </div> 

            </section>
            <!-- 1st Hero Variation -->
        </div>

        <section class="section">
            <div class="container">
                <div ref="dim" class="row row-grid align-items-center">
                    <div class="col-md-6">
                        <SvgGraph :module-uri="selectedModule" @modBasicData="getModBasicData"
                                  @modOutcomes="getModOutcomes" @modMethods="getModMethod" @modLiterature="getModLiter"
                                  @modTeachers="getModTeacher" @formType="getFormType">
                        </SvgGraph>
                    </div>
                <div class="col-md-6">
                <base-button v-on:click="form = 'BasicData'">Basicdata</base-button>
                <base-button v-on:click="form = 'Outcomes'">Outcomes </base-button>
                <base-button v-on:click="form = 'Methods'">Methods</base-button>
                <br><br>
                <keep-alive>

                <component v-bind:is="form = this.form" :modBasis="modBasis"></component>
                <!--<component v-bind:is="form = 'Dynamic'"></component>-->
                
                </keep-alive>              
               </div>
            </div></div>
        </section>
    </div>
</template>

<script>
    import Graph from "./components/Graph";
    import BaseDropdown from "@/components/BaseDropdown";
    import BaseNav from "@/components/BaseNav";
    import CloseButton from "@/components/CloseButton";
    import axios from "axios";
    import SvgGraph from "./components/SvgGraph";
    import Select from "./components/Select";
    import FormBasicData from "./components/FormBasicData";
    import FormMethods from "./components/FormMethods";
    import FormOutcomes from "./components/FormOutcomes";
    import FormLiterature from "./components/FormLiterature";
    import FormTeachers from "./components/FormTeachers";
    import FormDynamic from "./components/FormDynamic";

    export default {
        name: "home",
        components: {
            Graph,
            BaseDropdown,
            BaseNav,
            CloseButton,
            SvgGraph,
            Select,
            'BasicData':FormBasicData,
            'Methods':FormMethods,
            'Outcomes':FormOutcomes,
            'Literature':FormLiterature,
            'Teachers':FormTeachers,
            'Dynamic':FormDynamic,
        },
        data() {
            return {
                dims: {
                    width: null,
                    height: null
                },
                selectedModule: '',
                modBasis: null,
                modOutcome: null,
                modMethod: null,
                modLiter: null,
                modTeacher: null,
                form: 'BasicData'
            }
        },
        mounted() {
            const {
                width,
                height
            } = this.$refs.dim.getBoundingClientRect();
            this.dims.width = width / 2;
            this.dims.height = height + 170;
            /*this.changeData();*/
        },
        methods: {
            /*changeData() {
                const dataIndex = Math.floor(Math.random() * this.dataList.length)
                d3.json(this.dataList[dataIndex]).then(data => {
                    this.data = data
                }).catch(error => {
                    console.error(error)
                })
            },*/
            /*coursePost(query) {
                axios.post('http://fbw-sgmwi.th-brandenburg.de:3030/modcat/query', query, {headers: {'Content-Type': 'application/sparql-query'}})
                    .then(response => {
                        // JSON responses are automatically parsed.
                        this.courseData = response.data.results.bindings
                    })
                    .catch(e => {
                        this.errors.push(e)
                    })
            },*/
            getModule(value) {
                this.selectedModule = value;
            },
            /*getModuleInfo(value) {
                this.moduleInfo = value;
            },*/
            getModBasicData(value) {
                this.modBasis = value;
                console.log("ModBasisValueStarter", this.modBasis)
            },
            getModOutcomes(value) {
                this.modOutcome = value;
            },
            getModMethod(value) {
                this.modMethod = value;
            },
            getModLiter(value) {
                this.modLiter = value;
            },
            getModTeacher(value) {
                this.modTeacher = value;
            },
            getFormType(value) {
                this.form = value;
            }
        },
        /*beforeMount(){
            this.startPost('PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#> ' +
                'PREFIX module: <https://bmake.th-brandenburg.de/module/> ' +
                'PREFIX schema: <https://schema.org/> ' +
                'SELECT DISTINCT ?module ?label ' +
                'WHERE { ' +
                '  ?module a module:Module ; ' +
                '          schema:isPartOf module:WIB ; ' +
                '          rdfs:label ?label. ' +
                '}')
        }*/
    };

</script>
<style>
    .faded {
        opacity: 0.1;
        transition: 0.3s opacity;
    }
    .highlight {
        opacity: 1;
    }

    g.selected rect {
        stroke: #cd201f !important;
        stroke-width: 2px !important;
    }

/*    text {
        font: 10px sans-serif;
        pointer-events: none;
        text-shadow: 0 1px 0 #fff, 1px 0 0 #fff, 0 -1px 0 #fff, -1px 0 0 #fff;
    }

    text.edgelabel {
        font: 10px sans-serif;
        fill: #343a40;
        pointer-events: none;
        text-shadow: 0 1px 0 #fff, 1px 0 0 #fff, 0 -1px 0 #fff, -1px 0 0 #fff;
    }

    rect.caption {
        fill: #CCCCCCAC;
        stroke: #666;
        stroke-width: 1px;
    }
    text.caption {
        font-size: 14px;
        font-weight: bold;
    }*/
</style>