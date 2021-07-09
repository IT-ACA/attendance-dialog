<template>
  <v-dialog :value="active" width="500" persistent>
    <v-card>
      <v-card-title primary-title>
        {{ title }}
      </v-card-title>
      <v-card-text class="font-weight-bold">
        {{ text }}
      </v-card-text>
      <v-divider></v-divider>
      <v-card-actions>
        <v-spacer></v-spacer>
        <v-btn v-if="type === 'info'" @click="$emit('close')" color="success">
          Close
        </v-btn>
        <v-btn v-if="type === 'alert'" @click="$emit('close')" color="error">
          Close
        </v-btn>
        <v-btn v-if="type === 'confirm'" @click="$emit('close')" color="error">
          Cancel
        </v-btn>
        <v-btn
          v-if="type === 'confirm'"
          @click="$emit('confirm')"
          color="primary"
        >
          Confirm
        </v-btn>
      </v-card-actions>
    </v-card>
  </v-dialog>
</template>

<script>
export default {
  name: "modal",

  props: {
    active: {
      type: Boolean,
      default: false,
    },
    title: {
      type: String,
      default: "",
    },
    text: {
      type: String,
      default: "",
    },
    type: {
      type: String,
      default: "",
    },
  },

  data() {
    return {};
  },
};
</script>
