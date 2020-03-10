<template>
    
                        <div class="pl-md-5" id="formfield">
                            <h3 id="test">Lehr und Lernmethoden</h3>
                            <!--<h3>Überschrift Formular</h3>-->
                            <!--<p class="lead">Don't let your uses guess by attaching tooltips and popoves to any element.
                                Just make sure you enable them first via JavaScript.</p>-->
                            <form id="basic">
                                <div class="row">
                                <div class="col-md-3">
                                    <input v-model="modkurz" class="form-control" type="text" placeholder="Mod.kürzel" disabled> 
                                    </div></div>
                                    <div class="row">
                                    <div class="col-md-12">
                                        <!-- erstmal so gelassen, Checkboxen vorerst wahrscheinlich nicht sinnvoll-->
                                    <input class="form-control" type="text" v-model="description" placeholder="Lehr und Lernmethoden">
                                    </div>
                                    <div class="col-md-12">
                                    <textarea class="form-control" v-model="more" rows="3" placeholder="Weitere Lehr- und Lernmethoden"></textarea>
                                    </div>
                                    <div class="col-md-6">
                                    <input v-model="modtype" class="form-control" type="number" placeholder="Anzahl Workload-Komponenten" >
                                    </div>
                                    <div class="col-md-6">
                                    <input v-model="grade" class="form-control" type="text" placeholder="Workload Komponente" >
                                    </div>
                                    <div>&nbsp;</div>
                                    
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
export default {};
</script>
<style>

    .form-control {
      margin-bottom: 10px;
  }

</style>