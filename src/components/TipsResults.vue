<template>
    <div>
      <v-dialog v-model="dialogVisible" persistent>
        <v-card>
          <v-card-title class="text-h5">Tips Distribution</v-card-title>
          <v-card-text>
            <div v-if="waiterTips.length">
              <v-list>
                <v-list-item-group
                  v-for="(tip, index) in waiterTips"
                  :key="index"
                >
                  <v-list-item>
                    <v-list-item-content>
                      <v-list-item-title class="font-weight-bold">
                        {{ tip.waiterName }}
                      </v-list-item-title>
                      <v-list-item-subtitle>
                        <span>
                          <strong>Tip:</strong> ₪{{ tip.calculatedTip }}
                        </span>
                      </v-list-item-subtitle>
                    </v-list-item-content>
                  </v-list-item>
                </v-list-item-group>
              </v-list>
            </div>
          </v-card-text>
          <v-card-actions>
            <v-btn color="primary" text @click="dialogVisible = false">
              Close
            </v-btn>
          </v-card-actions>
        </v-card>
      </v-dialog>
    </div>
  </template>
    
  <script>
  export default {
    props: {
      waiterTips: {
        type: Array,
        required: true
      }
    },
    data() {
      return {
        dialogVisible: false
      };
    },
    mounted() {
      // Watch for changes in waiterTips
      this.$watch('waiterTips', (newValue) => {
        if (newValue && newValue.length > 0) {
          this.dialogVisible = true;
        }
      }, { immediate: true });
    }
  };
  </script>