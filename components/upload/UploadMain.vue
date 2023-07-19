<template>
  <div-base class="p-4">
    <div class="my-4 mb-6 font-medium text-2xl">
      <h1>Upload</h1>
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
        <v-data-table :items="selectedFiles" :headers="headers">
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
                color="deep-purple accent-4"
                indeterminate
                striped
                rounded
                height="10"
              ></v-progress-linear>
            </span>
          </template>
        </v-data-table>
      </section>
    </div>
  </div-base>
</template>

<script>
import DivBase from '../UI/DivBase.vue'

export default {
  components: { DivBase },
  data() {
    return {
      selectedFiles: [],
      isUploading: false,
      fileInputRef: null,
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
</style>
