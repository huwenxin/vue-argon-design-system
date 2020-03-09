<template>
    <div id="svgfield" :style="{width: width + 'px', height: height + 'px', border: '1px solid grey'}" class="bg-secondary">
        <!--<svg width="100%" height="100%">
            <defs>
                <pattern id="innerGrid" :width="innerGridSize" :height="innerGridSize" patternUnits="userSpaceOnUse">
                    <rect width="100%" height="100%" fill="none" stroke="#CCCCCC7A" stroke-width="0.5"/>
                </pattern>
                <pattern id="grid" :width="gridSize" :height="gridSize" patternUnits="userSpaceOnUse">
                    <rect width="100%" height="100%" fill="url(#innerGrid)" stroke="#CCCCCC7A" stroke-width="1.5"/>
                </pattern>
            </defs>
        </svg>-->
        <!--<span v-if="Object.keys(moduleInfo).length > 0">{{moduleInfo[0].code.value}}</span>-->
    </div>
</template>

<script>
    import axios from "axios";

    export default {
        name: "SvgGraph",
        props: ['moduleUri'],
        data() {
            return {
                svgfile: require("../../assets/modcat.svg"),
                width: 1024,
                height: 768,
                gridSize: 100,
                moduleInfo:[]
            }
        },
        /*computed: {
            innerGridSize() { return this.gridSize / 10 },
            nodes() { return this.data.nodes },
            links() { return this.data.links },
            // These are needed for captions
            linkTypes() {
                const linkTypes = []
                this.links.forEach(link => {
                    if (linkTypes.indexOf(link.type) === -1)
                        linkTypes.push(link.type)
                })
                return linkTypes.sort()
            },
            classes() {
                const classes = []
                this.nodes.forEach(node => {
                    if (classes.indexOf(node.class) === -1)
                        classes.push(node.class)
                })
                return classes.sort()
            },
        },*/
        created() {
            this.width = window.outerWidth/3.2
            this.height = window.innerHeight/3
        },
        mounted() {

            d3.xml(this.svgfile).then((xml) => {
                var importedNode = document.importNode(xml.documentElement, true);
                d3.select("div#svgfield")
                    .classed("svg-container", true)
                    .each(function() {
                        this.appendChild(importedNode);
                    })

                var svg = d3.select('svg'),
                    g = svg.append('g');

                g.node().appendChild(svg.select('g').node());

                //this is where my problem happens
                d3.select('svg').call(d3.zoom().on("zoom", function() {
                    g.attr("transform", d3.event.transform)
                }));

                let relaCenSem = d3.select('#rectSoWiSeXY').node().getBBox().width / 2 - 3
                console.log("relaCenSem", relaCenSem)
                d3.select("#textSoWiSeXY").attr("text-anchor", "middle").attr("dx", relaCenSem)

                this.styleImportedSVG();

            });

        },
        methods: {
            styleImportedSVG(d) {
                let i = 0;
                let _this = this
                d3.select('#nodes').selectAll("g")
                    .on('mouseover', function() {
                        /*console.log('mouseover');
                        console.log('this', this);*/
                        /*d3.select(this)
                            .style({
                                'opacity': 0.1,
                                'stroke-opacity': 0.3
                            })*/
                        this.style.opacity = 0.5;
                        this.style.transition = '0.3s opacity';
                    })
                    .on('mouseout', function() {
                        this.style.opacity = 1;
                    })
                    .on("click", function() {
                        const g = d3.select('#nodes').selectAll("g")
                        g.classed('selected', false)
                        d3.select(this).classed('selected', true)

                        const id = this.id;

                        if (id == "nodePerson") {
                            parent.document.getElementById("test").innerHTML = "<h3 id=\"test\">" + _this.moduleInfo[0].label.value +"-Person</h3>";
                            parent.document.getElementById("test1").innerHTML = "<p id=\"test1\"> Dozent: "+ _this.moduleInfo[0].person.value +"</p>";
                        } else if (id == "nodeSoWiSeXY") {
                            parent.document.getElementById("test").innerHTML = "<h3 id=\"test\">" + _this.moduleInfo[0].label.value +"-Semester</h3>";
                            parent.document.getElementById("test1").innerHTML = "<p id=\"test1\"> Modulkürzel: "+ _this.moduleInfo[0].code.value +"</p>";
                            parent.document.getElementById("test2").innerHTML = "<p id=\"test2\">So/WiSe XY was clicked!!!!</p>";
                        } else if (id == "nodeModulKuerzel") {
                            parent.document.getElementById("test").innerHTML = "<h3 id=\"test\">" + _this.moduleInfo[0].label.value +"</h3>";
                            parent.document.getElementById("test1").innerHTML = "<p id=\"test1\"> Modulkürzel: "+ _this.moduleInfo[0].code.value +"</p>";
                        } else {
                            i++;
                            parent.document.getElementById("test3").innerHTML = "<p id=\"test3\">clicked " + i + " times</p>";
                        }

                    })



            },
            queryModuleInfo() {

                d3.select('#nodes').selectAll("g").classed('selected', false)
                d3.select("#nodeModulKuerzel").classed('selected', true)

                let query = "PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#> " +
                            "PREFIX schema: <https://schema.org/> " +
                            "SELECT DISTINCT * " +
                            "WHERE { " +
                            "<" + this.moduleUri + "> rdfs:label ?label; " +
                            "          schema:courseCode ?code; " +
                            "          schema:hasCourseInstance ?instance; " +
                            "          schema:accountablePerson ?person. " +
                            "}";
                console.log(query);
                axios.post('http://fbw-sgmwi.th-brandenburg.de:3030/modcat/query', query, {headers: {'Content-Type': 'application/sparql-query'}})
                    .then(response => {
                        // JSON responses are automatically parsed.
                        this.moduleInfo = response.data.results.bindings
                    })
                    .catch(e => {
                        this.errors.push(e)
                    })
            },
            updateGraphText() {


                let module = d3.select('#textModulKuerzel').select("tspan");
                let semester = d3.select('#textSoWiSeXY').select("tspan");
                let tModule = 'Modul ' + this.moduleInfo[0].code.value;
                let tSemester = this.moduleInfo[0].instance.value.substring(39);

                module.text(tModule);
                semester.text(tSemester);

                let relaCenMod = d3.select('#rectModulKuerzel').node().getBBox().width / 2 - 3
                d3.select("#textModulKuerzel").attr("text-anchor", "middle").attr("dx", relaCenMod)

                console.log("test", this.moduleInfo[0].label.value);
                parent.document.getElementById("test").innerHTML = "<h3 id=\"test\">" + this.moduleInfo[0].label.value +"</h3>";
                parent.document.getElementById("test1").innerHTML = "<p id=\"test1\"> Modulkürzel: "+ this.moduleInfo[0].code.value +"</p>";
                parent.document.getElementById("test2").innerHTML = "<p id=\"test2\"> Semester: "+ this.moduleInfo[0].instance.value +"</p>";
            }
        },
        watch: {
            /*data: {
                handler(newData) {
                    this.updateData()
                },
                deep: true
            },*/
            moduleUri: {
                handler(newData) {
                    this.queryModuleInfo()
                }
            },
            moduleInfo: {
                handler(newData) {
                    this.updateGraphText()
                }
            }
        }
    }
</script>

<style scoped>
    .svg-container {
        display: inline-block;
        position: relative;
        width: 100%;
        padding-bottom: 100%; /* aspect ratio */
        vertical-align: top;
        overflow: hidden;
    }
</style>