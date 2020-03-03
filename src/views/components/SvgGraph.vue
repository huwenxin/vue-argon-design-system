<template>
    <div id="svgfield" :style="{ width: width + 'px', height: height + 'px', border: '1px solid grey' }">
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
    </div>
</template>

<script>
    export default {
        name: "SvgGraph",
        data() {
            return {
                svgfile: require("../../assets/modcat.svg"),
                width: 1024,
                height: 768,
                gridSize: 100
            }
        },
        computed: {
            /*innerGridSize() { return this.gridSize / 10 },
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
            },*/
        },
        created() {
            this.width = window.outerWidth/3.2
            this.height = window.innerHeight/1.5
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

                this.styleImportedSVG()

            });

        },
        methods: {
            styleImportedSVG(d) {
                let i = 0;
                d3.select('svg').select('#layer1').selectAll("g")
                    .on('mouseover', function() {
                        /*console.log('mouseover');
                        console.log('this', this);*/
                        /*d3.select(this)
                            .style({
                                'opacity': 0.1,
                                'stroke-opacity': 0.3
                            })*/
                        this.style.opacity = 0.5;
                    })
                    .on('mouseout', function() {
                        this.style.opacity = 1;
                    })
                    .on("click", function() {
                        const g = d3.select("svg").select('#layer1').selectAll("g")
                        g.classed('selected', false)
                        d3.select(this).classed('selected', true)

                        var id = this.id;
                        if (id == "nodeOrganisation") {
                            parent.document.getElementById("test1").innerHTML = "<p id=\"test1\">I'm just a test text</p>";
                        } else if (id == "nodeSoWiSeXY") {
                            parent.document.getElementById("test2").innerHTML = "<p id=\"test2\">So/WiSe XY was clicked!!!!</p>";
                        } else {
                            i++;
                            parent.document.getElementById("test3").innerHTML = "<p id=\"test3\">clicked " + i + " times</p>";
                        }

                    })

            },
            getForm(id) {


            }
        },
        watch: {
            data: {
                handler(newData) {
                    this.updateData()
                },
                deep: true
            },
            forceProperties: {
                handler(newForce) {
                    this.updateForces()
                },
                deep: true
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