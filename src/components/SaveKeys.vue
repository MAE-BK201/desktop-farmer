<template lang="pug">
.full-width
  .row.justify-center.q-gutter-lg
    .col
      .row.justify-center
        .col-8
          div {{ lang.yourPrivateKey }}
  .row.justify-center
    .col-8
      .row.justify-center(style="height: 70px")
        .row.justify-center.q-mb-md.full-width
          q-input(input-class="pkdisplay" outlined readonly ref="pkDisplay" style="width: 800px; height: 80px" type="textarea" v-model="generatedPk" v-show="revealKey")
        .row.justify-center.q-mb-lg.full-width.bg-grey-2(v-if="!revealKey")
          q-btn.full-width.full-width(@click="revealKey = true" flat label="reveal" size="lg")
      .row.justify-center.q-mt-md
        q-btn.full-width(@click="copyPk" label="copy to clipboard" outline style="max-width: 200px")
  .row.q-ma-md
    p The private key is necessary to utilize Subspace and can't be recovered. Ensure the key is backed up to a secure location.
  .row.q-mt-md
    q-checkbox(:label="lang.userConfirm" size="lg" v-model="userConfirm")
</template>

<style lang="sass">
.pkdisplay
  font-size: 20px
  padding-top: 0px
  margin-top: 0px
</style>

<script lang="ts">
import { defineComponent } from "vue"
// import { saveKeys as lang } from "src/loc/en"
import * as global from "src/lib/global"
const lang = global.data.loc.text.saveKeys
import { QInput, Dialog, Notify } from "quasar"

export default defineComponent({
  name: "PageIndex",
  data() {
    let userConfirm: boolean = false
    let generatedPk = "98da4ef21b864d2cc526dbdb2a120bd2874c36c9d0a1fb7f8c63d7f7a8b41de8f"
    let revealKey = false
    return { revealKey, userConfirm, lang, generatedPk }
  },
  computed: {
    canContinue(): boolean {
      return this.userConfirm
    },
  },
  methods: {
    copyPk() {
      const displaypk = this.$refs["pkDisplay"] as QInput
      const previousState = this.revealKey
      this.revealKey = true
      this.$nextTick(() => {
        displaypk.focus()
        displaypk.select()
        var successful = document.execCommand("copy")
        var msg = successful ? "successful" : "unsuccessful"
        console.log(msg)
        this.revealKey = previousState
        // Dialog.create({ message: "Private Key copied to clipboard" })
        Notify.create({ message: "Private Key copied to clipboard. Backup key in a secure location.", icon: "content_copy" })
      })
    },
  },
  emits: ["userConfirm"],
  watch: {
    userConfirm(val) {
      this.$emit("userConfirm", val)
    },
  },
})
</script>
