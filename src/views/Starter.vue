<template>
    <div>

        <div class="position-relative">
            <!-- shape Hero -->
            <section class="section-shaped my-0">
                <div class="shape shape-style-1 bg-gradient-warning">
                </div>
                <div class="container shape-container d-flex">
                    <div class="col px-0">
                        <div class="row">
                            <div class="col-lg-10">
                                <h1 class="display-3  text-white">Modulkatalog@THB
                                    <span>Fachbereich Wirtschaft</span>
                                </h1>
                            </div>
                        </div>
                    </div>
                    <base-nav class="navbar-main" type="" expand>
                        <ul class="navbar-nav">
                            <div class="nav-link col-lg-6">
                                <base-input class="mb-2">
                                    <select class="form-control form-control-alternative">
                                        <option disabled value="" selected>Studiengänge auswählen</option>
                                        <option value="wi">Wirtschaftsinformatik</option>
                                        <option value="bwl">BWL</option>
                                    </select>
                                </base-input>
                            </div>

                            <div class="nav-link col-lg-6">
                                <base-input class="mb-2">
                                    <select class="form-control form-control-alternative" @change="coursePost()">
                                        <option disabled value="" selected>Module auswählen</option>
                                        <option v-for="(coursejson, index) in siteLoad" v-bind:value="index">
                                                {{ coursejson.label.value }}
                                        </option>
                                    </select>
                                </base-input>
                            </div>
                        </ul>
                    </base-nav>
                </div>
            </section>
            <!-- 1st Hero Variation -->
        </div>

        <section class="section bg-secondary">
            <div class="container">
                <div ref="dim" class="row row-grid align-items-center">
                    <div class="col-md-6">
                        <Graph :data="data"></Graph>
                        <!--<div style="padding-top: 10px; text-align: center" @click="changeData()">
                            <button>Change Data</button>
                        </div>-->
                        <!--<div v-for="(json, index) in siteLoad" v-bind:key="index">
                            <p>{{ json.label.value }}</p>
                        </div>-->
                    </div>
                    <div class="col-md-6">
                        <div class="pl-md-5">

                            <h3 v-if="Object.keys(siteLoad).length != 0">{{ siteLoad[3].label.value }}</h3>
                            <!--<p class="lead">Don't let your uses guess by attaching tooltips and popoves to any element.
                                Just make sure you enable them first via JavaScript.</p>-->
                            <p>The kit comes with three pre-built pages to help you get started faster. You can change
                                the text and images and you're good to go.</p>
                            <p>The kit comes with three pre-built pages to help you get started faster. You can change
                                the text and images and you're good to go.</p>
                            <a href="/" class="font-weight-bold text-warning mt-5">A beautiful UI Kit for impactful
                                websites</a>
                        </div>
                    </div>
                </div>
            </div>
        </section>
    </div>
</template>

<script>
    import Graph from "./components/Graph";
    import BaseDropdown from "@/components/BaseDropdown";
    import BaseNav from "@/components/BaseNav";
    import CloseButton from "@/components/CloseButton";
    import axios from "axios";

    export default {
        name: "home",
        components: {
            Graph,
            BaseDropdown,
            BaseNav,
            CloseButton
        },
        data() {
            return {
                dims: {
                    width: null,
                    height: null
                },
                data: null,
                dataList: ["assets/data.json"],
                siteLoad: [],
                courseData: []
            }
        },
        mounted() {
            const {
                width,
                height
            } = this.$refs.dim.getBoundingClientRect();
            this.dims.width = width / 2;
            this.dims.height = height + 170;
            this.changeData();
        },
        methods: {
            changeData() {
                const dataIndex = Math.floor(Math.random() * this.dataList.length)
                d3.json(this.dataList[dataIndex]).then(data => {
                    this.data = data
                }).catch(error => {
                    console.error(error)
                })
            },
            startPost(query) {
                axios.post('http://fbw-sgmwi.th-brandenburg.de:3030/modcat/query', query, {headers: {'Content-Type': 'application/sparql-query'}})
                    .then(response => {
                        // JSON responses are automatically parsed.
                         this.siteLoad = response.data.results.bindings
                        //return response.data.results.bindings
                    })
                    .catch(e => {
                        this.errors.push(e)
                    })
            },
            coursePost(query) {
                axios.post('http://fbw-sgmwi.th-brandenburg.de:3030/modcat/query', query, {headers: {'Content-Type': 'application/sparql-query'}})
                    .then(response => {
                        // JSON responses are automatically parsed.
                        this.courseData = response.data.results.bindings
                    })
                    .catch(e => {
                        this.errors.push(e)
                    })
            }
        },
        beforeMount(){
            this.startPost('PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#> SELECT DISTINCT ?label ' +
                'FROM <https://bmake.th-brandenburg.de/module/> WHERE { ?course a <https://schema.org/Course> ; ' +
                'rdfs:label ?label .}')
        }
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

    path.link {
        fill: none;
        stroke: #666;
        stroke-width: 1.5px;
    }
    path.link.depends {
        stroke: #005900;
        stroke-dasharray: 5, 2;
    }
    path.link.needs {
        stroke: black;
    }

    circle {
        fill: #ffff99;
        stroke: #191900;
        stroke-width: 1.5px;
    }
    circle.system {
        fill: #cce5ff;
        stroke: #003366;
    }
    circle.mount {
        fill: #f3a4b5;
        stroke: darkred;
    }
    circle.init {
        fill: #b2e8b2;
        stroke: #001900;
    }

    circle.selected {
        stroke: #ff6666FF !important;
        stroke-width: 3px;
        animation: selected 2s infinite alternate ease-in-out;
    }

    @keyframes selected {
        from {
            stroke-width: 5px;
            r: 26;
        }
        to {
            stroke-width: 1px;
            r: 30;
        }
    }

    text {
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
    }
</style>