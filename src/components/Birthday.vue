<template>
  <div>
    <p>birthday field</p>

    <cleave
      v-model="birthday"
      :options="options"
      class="form-control"
      name="card"
      autofocus="autofocus"
      v-on:keydown.native="format_date"
      placeholder="mm/dd/yyyy"
    />
    <p>Raw value: {{ birthday }}</p>

    <h3>Customization</h3>
    <li>
      When user enter 1 in month, then '/' (slash) will automatically prefix 1
      with 0.
    </li>
    <li>
      When user enter 1,2,or 3 in day, then '/' (slash) will automatically
      prefix 1,2,or 3 with 0
    </li>

    <h3>The following is taken care by library itself</h3>
    <ul>
      <li>
        When user enter 00 in month or day, it will automatically translate
        month or day to 01.
      </li>
    </ul>
  </div>
</template>

<script>
import Cleave from "vue-cleave-component";

export default {
  data() {
    return {
      birthday: null,
      options: {
        date: true,
        datePattern: ["m", "d", "Y"],
        numericOnly: false,
        delimiter: "/",
      },
      raw: false,
      currentTime: new Date().getTime(),
    };
  },
  methods: {
    // @todo: current implementation only works with date pattern m/d/Y
    format_date: function (event) {
      if (event.key != this.options.delimiter || this.birthday == null) {
        return;
      }

      // Format month
      if (this.birthday.length == 1 && this.birthday == 1) {
        this.birthday = "01";
      }

      // Format day
      if (this.birthday.length == 3 && this.birthday.slice(-1) < 4) {
        this.birthday =
          this.birthday.slice(0, -1) + "0" + this.birthday.slice(-1);
      }
    },
  },
  components: {
    Cleave,
  },
};
</script>