<script setup lang="ts">
let numberRegexp = /^[0-9]+$/;
class ZipCodeValidator{
  isAcceptable(S: string){
    return S.length === 5 && numberRegexp.test(S);
  }
}
</script>