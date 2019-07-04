<template>
  <div class="home">
    <Aligner />
  </div>
</template>

<script lang="ts">
import { Component, Vue } from "vue-property-decorator";
import Aligner from "@/components/Aligner.vue"; // @ is an alias to /src

@Component({
  components: {
    Aligner
  }
})
export default class Home extends Vue {}
</script>
