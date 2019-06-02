<template>
  <div style="width: 600px">
    <v-dialog
        v-model="dialog"
        max-width="290"
    >
      <v-card>
        <v-card-title class="headline">Settings</v-card-title>
        <v-text-field
            label="Filename"
            v-model="newFilename"
        >
        </v-text-field>
        <v-spacer></v-spacer>
        <v-radio-group v-model="radioGroup">
          <v-radio label="File" value="file"></v-radio>
          <v-radio label="Directory" value="dir"></v-radio>
        </v-radio-group>
        <v-card-actions>
          <v-spacer></v-spacer>

          <v-btn
              color="green darken-1"
              flat="flat"
              @click="dialog = false"
          >
            Close
          </v-btn>

          <v-btn
              color="green darken-1"
              flat="flat"
              @click="dialog = false; createNewFile()"
          >
            Create
          </v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>

    <v-text-field
        label="Doc/Dir name"
        required
        outline
        v-model="path"
    ></v-text-field>
    <v-btn small outline color="indigo" class="ml-0" v-on:click="getFiles">Search</v-btn>
    <v-divider class="mt-3 mb-3"></v-divider>
    <ul v-if="!isError">
      <tree-item
          class="item"
          :item="treeData"
          @createFile="showDialog"
      ></tree-item>
    </ul>

    <p v-if="isError" class="red--text"> {{ errMessage }}</p>
  </div>
</template>

<script>
    import TreeItem from './TreeItem';

    export default {
        components: {
            TreeItem
        },
        name: "FilesView",
        data() {
            return {
                treeData: {},
                path: '',
                isError: false,
                errMessage: '',
                dialog: false,
                radioGroup: 'file',
                parentItem: {},
                newFilename: ''
            }
        },
        methods: {
            getFiles: function () {
                this.isError = false;
                this.$http.post('http://localhost:8080/allFiles', {
                    path: this.path,
                    postfix: ''
                }).then(response => {
                        this.treeData = response.body;
                    }, response => {
                        this.isError = true;
                        this.errMessage = response.body.message;
                    }
                )
            },
            showDialog(item) {
                this.dialog = true;
                this.parentItem = item;
                console.log(this.parentItem)
            },
            createNewFile() {
                this.parentItem.children.push({
                    children: [],
                    file: this.parentItem.file + '/' + this.newFilename,
                    izFile: this.radioGroup === 'file',
                    name: this.newFilename
                });
                this.newFilename = ''
            }
        },
    }
</script>

<style scoped>

</style>