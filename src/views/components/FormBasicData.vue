<template>
    
                        <div class="pl-md-5" id="formfield">
                            <h3 id="test">Basisdaten</h3>
                            <!--<h3>Überschrift Formular</h3>-->
                            <!--<p class="lead">Don't let your uses guess by attaching tooltips and popoves to any element.
                                Just make sure you enable them first via JavaScript.</p>-->
                            <form id="basic">
                                <div class="row">
                                <div class="col-md-4">
                                    <input v-model="modkurz" class="form-control" type="text" placeholder="Mod.kürzel" disabled> 
                                    </div></div>
                                    <div class="row">
                                    <div class="col-md-9">
                                    <input class="form-control" type="text" v-model="description" placeholder="Modulbezeichnung">
                                    </div>
                                    <div class="col-md-3">
                                    <select v-model="spo" class="form-control">
                                        <option disabled value="">SPO auswählen</option>
                                        <option>2015</option>
                                        <option>2016</option>
                                        <option>2017</option>
                                        <option>2018</option>
                                        <option>2019</option>
                                        <option>2020</option>
                                        </select>
                                    </div>
                                    <div class="col-md-9">
                                    <select v-model="modtype" class="form-control">
                                        <option disabled value="">Modultyp</option>
                                        <option>Pflicht</option>
                                        <option>Wahlpflicht</option>
                                        <option>Transfer</option>
                                        </select>
                                    </div>
                                    <div class="col-md-3">
                                    <input v-model="grade" class="form-control" type="number" placeholder="Notengewichtung" >
                                    </div>
                                    <div class="col-md-6">
                                    <input v-model="semcount" class="form-control" type="number" placeholder="Lehrveranstaltungen" >
                                    </div>
                                    <div class="col-md-6">
                                    <input v-model="usability" class="form-control" type="text" placeholder="Verwendbarkeit">
                                    </div>
                                    <div class="col-md-6">
                                    <input v-model="teachingsws" class="form-control" type="number" placeholder="SWS" >
                                    </div>
                                    <div class="col-md-6">
                                    <input v-model="ects" class="form-control" type="number" placeholder="ECTS" >
                                    </div>
                                    <div class="col-md-4">
                                    <input v-model="duration" class="form-control" type="number" placeholder="Dauer Semester">
                                    </div>
                                    <div class="col-md-8">
                                    <select v-model="frequency" class="form-control">
                                        <option disabled value="">Häufigkeit</option>
                                        <option>WiSe</option>
                                        <option>SoSe</option>
                                        <option>jedes Semester</option>
                                        <option>alle 2 Jahre</option>
                                        </select>
                                    </div></div>
                                    <div class="row">
                                    <div class="col-md-3">
                                    <p>Sprachen:</p>
                                    </div>
                                    <div class="col-md-9">
                                    <div id="checkboxes" class="form-check">
                                        <label class="form-check-label">deutsch</label>
                                        <input class="form-check-input" type="checkbox" value="de" v-model="lang">
                                        
                                        <label class="form-check-label">englisch</label>
                                        <input class="form-check-input" type="checkbox" value="en" v-model="lang">
                                        
                                        <label class="form-check-label">russisch</label>
                                        <input class="form-check-input" type="checkbox" value="ru" v-model="lang">
                                    </div>
                                    </div>
                                    <div class="col-md-12">
                                    <input v-model="requirements" class="form-control" type="text" placeholder="Voraussetzungen">
                                    </div>
                                    <div class="col-md-12">
                                    <input v-model="website" class="form-control" type="text" placeholder="Website" default="www.th-brandenburg.de">
                                    </div>
                                    <div class="col-md-12">
                                    
                                    <textarea class="form-control" v-model="comment" rows="3" placeholder="Kommentar / Besonderes"></textarea>
                                    
                                    </div><div>&nbsp;</div>
                                    
                                    <div class="col-md-12">
                                    <base-button type="primary">Speichern</base-button>
                                    <base-button type="secondary">Download</base-button>   
                                    <p>Message is: {{ description }}</p>
                                    </div>

                                </div>
                            <a id="test3" href="/" class="font-weight-bold text-warning mt-5"></a>
                        </form>
                    </div>
</template>
<script>
export default {
    
    data() {
        return{
            spo:"",
            modtype:"",
            frequency:"",
            lang:[]
        }
    }};

</script>
<style>

    .form-control {
      margin-bottom: 10px;
  }

    .form-check{
        margin-bottom: 10px;
    }

    .form-check-label{
        margin-right: 28px;
        margin-left: 10px;
    }

</style>