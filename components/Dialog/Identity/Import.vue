<template>
  <dialog class="dialog">
    <Box title="Import Private Key">
      <div class="space-y-12">
        <ButtonsMenu
          v-model="section"
          :data="{ 'Recovery Phrase': 'seed', 'PEM': 'pem', 'Protobuf': 'protobuf', 'Key File': 'file' }" />

        <!-- Seed -->
        <form v-show="section === 'seed'" @submit.prevent="importSeed">
          <Field title="Recovery Phrase" description="Recover your identity with the words generated during its creation.">
            <textarea v-model="seed"
                      required
                      placeholder="cake unusual profit tired mass law brick fruit one chunk clinic..."
                      class="h-40 input" />
          </Field>

          <Button class="button--danger" :loading="loading">
            Import
          </Button>
        </form>

        <!-- PEM -->
        <form v-show="section === 'pem'" @submit.prevent="importPem">
          <Field title="PEM" description="Import using the contents of the key.">
            <textarea v-model="pem"
                      required
                      placeholder="-----BEGIN RSA PRIVATE KEY-----"
                      class="h-40 input" />
          </Field>

          <Button class="button--danger" :loading="loading">
            Import
          </Button>
        </form>

        <!-- Protobuf -->
        <form v-show="section === 'protobuf'" @submit.prevent="importProtobuf">
          <Field title="Protobuf" hint="`Identity.PrivKey` in the IPFS node configuration.">
            <textarea v-model="protobuf"
                      required
                      placeholder="CAASrBIwggkoAgEAAoICAQCzcx5z/vT..."
                      class="h-40 input" />
          </Field>

          <Button class="button--danger" :loading="loading">
            Import
          </Button>
        </form>

        <!-- Key File -->
        <Field v-show="section === 'file'" title="Key File">
          <input v-show="false"
                 ref="file"
                 type="file"
                 @change="upload">

          <div class="buttons">
            <Button class="button--danger" :loading="loading" @click="$refs.file.click()">
              Upload
            </Button>
          </div>
        </Field>
      </div>

      <template #footer>
        <div class="flex justify-end gap-3">
          <Button class="button--danger button--sm" :loading="loading" @click="close">
            Close
          </Button>
        </div>
      </template>
    </Box>
  </dialog>
</template>

<script lang="ts">
import { PrivateKey } from '@opendreamnet/ipfs'
import Dialog from '~/mixins/Dialog'

export default Dialog.extend({
  data: () => ({
    section: 'seed',
    seed: '',
    pem: '',
    protobuf: '',
    loading: false
  }),

  methods: {
    async upload(event: Event) {
      try {
        const input = event.target as HTMLInputElement
        const { files } = input

        if (!files || files.length === 0) {
          return
        }

        this.loading = true

        const file = files[0]

        console.log(file)

        const pem = await file.text()

        console.log(pem)

        const privateKey = await PrivateKey.fromPem(pem)
        this.$accessor.settings.setIpfsPrivateKey(privateKey.toProtobuf())
        this.$accessor.settings.save()

        location.reload()
      } catch (err) {
        alert('Error: ' + err.message)
      } finally {
        this.loading = false
      }
    },

    async importSeed() {
      try {
        this.loading = true

        const privateKey = await PrivateKey.fromMnemonic(this.seed)
        this.$accessor.settings.setIpfsPrivateKey(privateKey.toProtobuf())
        this.$accessor.settings.save()

        location.reload()
      } catch (err) {
        console.error(err.message, err)
        alert('Error: ' + err.message)
      } finally {
        this.loading = false
      }
    },

    async importPem() {
      try {
        this.loading = true

        const privateKey = await PrivateKey.fromPem(this.pem)
        this.$accessor.settings.setIpfsPrivateKey(privateKey.toProtobuf())
        this.$accessor.settings.save()

        location.reload()
      } catch (err) {
        alert('Error: ' + err.message)
      } finally {
        this.loading = false
      }
    },

    async importProtobuf() {
      try {
        this.loading = true

        const privateKey = await PrivateKey.fromProtobuf(this.protobuf)
        this.$accessor.settings.setIpfsPrivateKey(privateKey.toProtobuf())
        this.$accessor.settings.save()

        location.reload()
      } catch (err) {
        alert('Error: ' + err.message)
      } finally {
        this.loading = false
      }
    }
  }
})
</script>

<style lang="scss" scoped>
form {
  @apply space-y-9;
}
</style>
