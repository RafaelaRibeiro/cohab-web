<template>
  <div class="mt-3 min-h-[80vh]">
    <div>
      <h1 class="headline font-bold text-gray-700 mb-4">
        <i> <v-icon size="35" class="px-3">mdi-tray-arrow-up</v-icon>Upload </i>
      </h1>
    </div>

    <div class="bg-white flex justify-center h-full">
      <div class="w-3/5">
        <div class="my-4 mb-6 font-medium text-2xl">
          <div class="flex justify-end">
            <v-btn dark color="#394da1" @click="dialog = true">Simulador</v-btn>
          </div>

          <v-dialog v-model="dialog" max-width="600px">
            <div class="w-full bg-white rounded-lg shadow p-6">
              <h1 class="font-medium text-xl mb-6">Simulador</h1>

              <div class="flex flex-row justify-center items-center">
                <v-text-field
                  name="name"
                  label="Nome"
                  v-model="user.name"
                  outlined
                  dense
                ></v-text-field>
              </div>

              <div>
                <v-text-field
                  label="Celular"
                  v-model="user.phone"
                  outlined
                  dense
                ></v-text-field>
              </div>
              <div>
                <v-text-field
                  label="CPF"
                  v-model="user.cpf"
                  outlined
                  dense
                ></v-text-field>
              </div>
              <div class="mt-4 flex justify-end">
                <v-btn class="mr-4" @click="send" dark color="#004011"
                  >Enviar</v-btn
                >
                <v-btn @click="dialog = false" color="error">Cancelar</v-btn>
              </div>
            </div>
          </v-dialog>
        </div>
        <div class="flex flex-col">
          <section
            class="drag-files bg-[#ececfc] border-dashed border-2 border-[#394da1] rounded-lg text-center py-4 relative"
            ref="dropArea"
            @dragover="handleDragOver"
            @dragleave="handleDragLeave"
            @change="handleFileSelected"
          >
            <v-icon x-large color="#394da1">mdi-tray-arrow-up</v-icon>
            <h3 class="mt-2 text-[#394da1]">Anexar arquivos</h3>
            <span class="text-sm text-[#746e82]">
              Arraste ou clique para fazer upload
            </span>
            <input
              type="file"
              ref="fileInput"
              @change="handleFileUpload"
              multiple
            />
          </section>

          <section class="selected-files my-5 grid gap-3">
            <v-data-table
              :items="selectedFiles"
              :headers="headers"
              hide-default-footer
              :items-per-page="99999"
            >
              <!-- eslint-disable-next-line vue/valid-v-slot -->
              <template v-slot:item.file_name="{ item }">
                <span>{{ item.file_name }}</span>
              </template>
              <!-- eslint-disable-next-line vue/valid-v-slot -->
              <template v-slot:item.file_size="{ item }">
                <span> {{ formatFileSize(item.file_size) }}</span>
              </template>
              <!-- eslint-disable-next-line vue/valid-v-slot -->
              <template v-slot:item.upload="{}">
                <span>
                  <v-progress-linear
                    v-if="currentTime > 0"
                    color="deep-purple accent-4"
                    :value="Math.floor(100 * (currentTime / timeout))"
                    rounded
                    height="10"
                  ></v-progress-linear>
                  <span class="text-green-600" v-else>
                    <v-icon color="green">mdi-check-circle-outline</v-icon>
                    Upload Concluído</span
                  >
                </span>
              </template>
            </v-data-table>
          </section>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import DivBase from '../UI/DivBase.vue'

export default {
  components: { DivBase },
  data() {
    return {
      dialog: false,
      selectedFiles: [],
      isUploading: false,
      fileInputRef: null,
      timeout: 5 * 1000,
      currentTime: 0,
      user: {
        name: '',
        cpf: null,
        phone: null,
      },
      headers: [
        {
          text: 'Nome',
          align: 'start',
          sortable: true,
          value: 'file_name',
          width: '20%',
        },
        {
          text: 'Tamanho',
          align: 'start',
          sortable: true,
          value: 'file_size',
          width: '10%',
        },
        {
          text: 'Upload',
          align: 'start',
          sortable: true,
          value: 'upload',
          width: '10%',
        },
      ],
    }
  },
  methods: {
    emitFileInputRef() {
      this.$emit('fileInputRef', this.$refs.fileInput)
    },
    handleDragOver() {
      this.$refs.dropArea.classList.add('dragover')
    },
    handleDragLeave() {
      this.$refs.dropArea.classList.remove('dragover')
    },
    handleFileSelected() {
      this.emitFileInputRef()
    },

    async send() {
      const proxyUrl = 'https://cors-anywhere.herokuapp.com/'
      const url =
        'https://n8n.agenciatotalk.com.br/webhook-test/51706762-c77f-4702-9f7b-0e3f75d996fd'
      try {
        await this.$axios.$post(proxyUrl + url, {
          name: this.user.name,
          phone: this.user.phone,
          cpf: this.user.cpf,
        })

        this.dialog = false
        this.user = {}
      } catch (error) {
        console.error('An error occurred while fetching events:', error)
      }
    },

    handleFileUpload() {
      const files = this.$refs.fileInput.files
      for (let i = 0; i < files.length; i++) {
        const file = files[i]
        const fileSizeInBytes = file.size
        const fileSizeInKb = Math.round(fileSizeInBytes / 1024)
        this.selectedFiles.push({
          file_name: file.name,
          file_size: fileSizeInBytes,
          size: fileSizeInKb + ' KB',
        })
      }

      this.syncPbar()
    },
    formatFileSize(size) {
      if (size > 1024 * 1024) {
        return `${Math.round(size / (1024 * 1024))} MB`
      } else if (size > 1024) {
        return `${Math.round(size / 1024)} KB`
      } else {
        return `${size} bytes`
      }
    },

    removeFile(index) {
      this.selectedFiles.splice(index, 1)
    },
    syncPbar() {
      //Create a timeout every 100 miliseconds
      setTimeout(() => {
        //Increment the time counter by 100
        this.currentTime += 100

        //If our current time is larger than the timeout
        if (this.timeout <= this.currentTime) {
          //Wait 500 miliseconds for the dom to catch up, then reset the snackbar
          setTimeout(() => {
            this.$emit('input', false) //Update the v-model to false
            this.currentTime = 0 // reset the current time
          }, 500)
        } else {
          //Recursivly update the progress bar
          this.syncPbar()
        }
      }, 100)
    },
  },
  watch: {
    value(v) {
      if (v) this.syncPbar()
    },
  },
}
</script>

<style>
.drag-files input {
  all: unset;
  opacity: 0;
  inset: 0;
  position: absolute;
}

.box {
  padding: 8px;

  background: #ffffff;
  box-shadow: 0px 4px 16px #ececfc;
  border-radius: 8px;

  display: flex;
  gap: 12px;

  --icon-bg: #eee;
  --icon-color: #aaa;
  --progress-color: black;
  --progress-text: #999;

  position: relative;
}

.uploading {
  --icon-bg: #ececfc;
  --icon-color: #cbd0fb;
  --progress-color: linear-gradient(90deg, #394da1 0%, #cbd0fb 100%);
  --progress-text: #9892a6;
}

.box .icon {
  background: var(--icon-bg);
  border-radius: 4px;

  padding: 12px 12px;

  display: flex;
  align-items: center;
  justify-content: center;
}

table th + th {
  border-left: 1px solid #dddddd;
}
table td + td {
  border-left: 1px solid #dddddd;
}

table {
  border: 1px solid #dddddd;
}
</style>
