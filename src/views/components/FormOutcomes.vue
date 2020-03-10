<template>
    
                        <div class="pl-md-5" id="formfield">
                            <h3 id="out">Lernergebnisse des Moduls</h3>
                            <!--<h3>Überschrift Formular</h3>-->
                            <!--<p class="lead">Don't let your uses guess by attaching tooltips and popoves to any element.
                                Just make sure you enable them first via JavaScript.</p>-->
                            <form id="outcome">
                                <div class="row">
                                <div class="col-md-4">
                                    <input v-model="modkurz" class="form-control" type="text" placeholder="Mod.kürzel" disabled> 
                                    </div></div>  
                                    <div class="row">
                                    <div class="col-md-6">
                                    <input class="form-control" type="number" v-model="anzahllernergebnisse" placeholder="Anzahl Lernergebnisse">
                                    </div>
                                    <div class="col-md-6">
                                    <input v-model="lernergebnissnr" class="form-control" type="text" placeholder="Lernergebnis Nr" >
                                    </div>
                                    <div class="col-md-6">
                                    <input v-model="inhaltselemente" class="form-control" type="number" placeholder="Anzahl Inhaltselemente" >
                                    </div>
                                    <div class="col-md-6">
                                    <input v-model="element" class="form-control" type="text" placeholder="Art Inhaltselement" >
                                    </div>
                                    <div class="col-md-6">
                                    <input v-model="pruefungsvorleistungen" class="form-control" type="number" placeholder="Anzahl Prüfungsvorleistungen" >
                                    </div>
                                    <div class="col-md-6">
                                    <input v-model="vorleistung" class="form-control" type="text" placeholder="Art Prüfungsvorleistung" >
                                    </div>
                                    <div class="col-md-6">
                                    <input v-model="prüfungsleistung" class="form-control" type="number" placeholder="Anzahl Prüfungsleistungen">
                                    </div>
                                    <div class="col-md-6">
                                    <input v-model="leistung" class="form-control" type="text" placeholder="Art Prüfungsleistung">
                                    </div>
                                     <div class="col-md-6">
                                    
                                    <select v-model="selected" class="form-control">
                                        <option disabled value="">Bloomsche Taxonomie</option>
                                        <option>erschaffen</option>
                                        <option>evaluieren</option>
                                        <option>analysieren</option>
                                        <option>anwenden</option>
                                        <option>verstehen</option>
                                        <option>erinnern</option>
                                        </select>
                                    </div>
                                    <div>&nbsp;</div>
                                    
                                    <div class="col-md-12">
                                    <base-button type="primary">Speichern</base-button>
                                    <base-button type="secondary">Download</base-button>   
                                    <p>Message is: {{ selected }}</p>
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
            selected:""
        }
    }

}


</script>
<style>

    .form-control {
      margin-bottom: 10px;
  }

</style>