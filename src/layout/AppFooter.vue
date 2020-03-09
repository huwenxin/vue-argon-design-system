<template>
    <footer class="footer bg-gradient-warning ">
        <div class="container">
            <div class="row align-items-center justify-content-md-between">
                <div class="col-md-6 font-weight-bold">
                    <div class="copyright">
                        &copy; {{year}}
                        <a href="https://bmake.th-brandenburg.de" target="_blank" rel="noopener">BMaKE Group</a>@<a href="https://www.th-brandenburg.de/startseite/" target="_blank" rel="noopener">THB</a>
                    </div>
                </div>
            </div>
        </div>
    </footer>
</template>
<script>
export default {
  name: 'app-footer',
  data() {
    return {
      year: new Date().getFullYear()
    }
  }
};
</script>
<style>
    .footer {
        position: absolute;
        bottom: 0;
        width: 100%;
        height: px;
    }
</style>
