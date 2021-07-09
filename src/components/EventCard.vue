<template>
  <v-card class="mx-auto" max-width="400">
    <div
      v-bind:class="[eventData.color]"
      class="white--text align-end"
      height="200px"
    >
      <v-card-title>{{ eventData.name }}</v-card-title>
    </div>
    <v-container class="">
      <v-row class="p-2" no-gutters>
        <v-col cols="1"><v-icon>mdi-clock</v-icon></v-col>
        <v-col cols="11" class="pl-2">
          <div>{{ getEventDate(eventData.start) }}</div>
          <div class="subtitle">
            {{ getEventTime(eventData.start) }} -
            {{ getEventTime(eventData.end) }}
          </div>
        </v-col>
      </v-row>
      <v-row class="pt-2" no-gutters>
        <v-col cols="1"><v-icon>mdi-menu</v-icon></v-col>
        <v-col cols="11" class="pl-2">{{ eventData.details }}</v-col>
      </v-row>
      <v-row class="pt-2" no-gutters>
        <v-col cols="1"><v-icon>mdi-account-group</v-icon></v-col>
        <v-col cols="11" class="pl-2"
          >{{ eventData.participants.length }} Participants<v-list>
            <v-list-item
              v-for="participant in eventData.participants"
              :key="participant.name"
            >
              <v-list-item-content>
                <v-list-item-subtitle
                  v-html="participant.name"
                ></v-list-item-subtitle>
              </v-list-item-content>
            </v-list-item> </v-list
        ></v-col>
      </v-row>
    </v-container>

    <v-card-actions>
      <v-btn color="error">
        Cancel
      </v-btn>
      <v-btn color="primary">
        Attendance
      </v-btn>
    </v-card-actions>
  </v-card>
</template>

<script>
import moment from "moment";

export default {
  name: "card",

  data() {
    return {
      eventData: {
        name: "Dejardins",
        details: "French Class",
        start: "2021-01-01 09:00:00",
        end: "2021-01-01 10:00:00",
        color: "green",
        participants: [
          {
            name: "John Doe",
          },
          {
            name: "Jane Smith",
          },
          {
            name: "Taylor Ireland",
          },
        ],
      },
    };
  },

  mounted() {},

  methods: {
    getEventDate: function(date) {
      return moment(date, "YYYY-MM-DD hh:mm:ss").format("dddd, MMMM D");
    },
    getEventTime: function(date) {
      return moment(date, "YYYY-MM-DD hh:mm:ss").format("hh:mma");
    },
  },
};
</script>

<style>
.v-list-item {
  min-height: 24px !important;
}
.v-list-item__content {
  padding: 0 !important;
}
.v-card__actions {
  justify-content: flex-end;
}
.subtitle {
  font-size: 0.875rem;
  color: rgba(0, 0, 0, 0.6);
}
</style>
