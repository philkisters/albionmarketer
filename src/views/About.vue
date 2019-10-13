<template>
  <div class="about">
    <h1>AlbionMarketer</h1>
    <div class="about-grid">
      <div>
        <h3>About</h3>
        <p>
          AlbionMarketer is build to give you an overview about the profit you could 
          make by transporting goods from one city of albion into the other. 
        </p>
        <p>
          The used data is collected from the <a href="https://www.albion-online-data.com">Albion Online Data Project</a> 
          so download their client and use the collected data directly within this tool.
        </p>
        <p>
          It is planned for the future that the the tool will give you advice what to transport depending on the locations selected.
        </p>
      </div>
      <div class="changelog-container">
        <h3>Changelog</h3>
        <timeline></timeline>
      </div>
    </div>
  </div>
</template>
<script>
import Timeline from '@/components/Timeline.vue'

export default {
  components: {
    Timeline
  }
}
</script>
<style lang="less">
.about {
  max-width: 900px;
  margin: 0 auto;
}

.about-grid {
  display: flex;
  flex-flow: row wrap;

  margin-left: -20px;

  >div {
    flex-grow: 1;
    width: 45%;
    padding: 20px;
  }
}
</style>
