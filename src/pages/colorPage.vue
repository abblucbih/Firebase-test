<template>
  <q-page>
    <div class="row justify-center q-mx-sm">
      <div class="col-xs-12 col-sm-8 col-md-6">
        <h3 class="text-center">Lampa skriver värde</h3>
        <div class="text-center">
          <img
            height="500"
            :style="{ backgroundColor: `rgba(255,255,0,${lightValue / 100}` }"
            src="bulb.png"
            alt="bulb"
          />
        </div>
        <div class="row justify-center align-center">
          <div class="col-auto q-mr-xs self-center">
            <q-icon color="deep-orange" size="lg" name="brightness_medium" />
            0
          </div>
          <div class="col">
            <q-slider
              v-model="lightValue"
              :min="0"
              :max="100"
              :step="4"
              label
              color="deep-orange"
            />
          </div>
          <div class="col-auto self-center q-ml-sm">100</div>
        </div>
      </div>
    </div>
  </q-page>
</template>

<script>
import { db } from "../boot/firebase";
import { ref, set, get } from "firebase/database";

const bulbRef = ref(db, "bulb");

export default {
  created() {
    get(bulbRef)
      .then((snapshot) => {
        if (snapshot.exists()) {
          this.lightValue = snapshot.val();
        } else {
          console.log("No data available");
        }
      })
      .catch((error) => {
        console.error(error);
      });
  },
  data() {
    return {
      lightValue: 0,
    };
  },
  watch: {
    lightValue: function (val) {
      set(bulbRef, val);
    },
  },
};
</script>
