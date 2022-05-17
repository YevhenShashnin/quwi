<template>
  <div class="c-active-input">
    <div class="b-input">
      <div class="e-placeholder" v-if="value.length && !error">
        {{ placeholder }}
      </div>
      <div class="e-placeholder-error" v-if="error">
        {{ errorText }}
      </div>
      <input
        :value="value"
        @input="$emit('change', $event.target.value)"
        :class="{
          'has-error': error,
        }"
        :placeholder="placeholder"
        :type="type"
        autocomplete
      />
    </div>
  </div>
</template>

<script>
export default {
  name: "CustomInput",
  props: {
    value: {
      type: String,
      default: "",
    },
    type: {
      type: String,
      default: "",
    },
    placeholder: {
      type: String,
      default: "",
    },
    error: {
      type: Boolean,
      default: "",
    },
    errorText: {
      type: String,
      default: "",
    },
    handler: {
      type: Function,
      default: () => {},
    },
  },
};
</script>

<style lang="css">
*:focus {
  outline: none;
}
.c-active-input {
  margin-bottom: 12px;
}
.b-input {
  position: relative;
  width: 100%;
}
.e-placeholder {
  color: #999;
  font-size: 14px;
  word-spacing: 1px;
  margin: 0 10px;
  max-width: calc(100% - 20px);
  position: absolute;
  top: -0.55em;
  background: #fff;
  display: block;
  float: left;
  padding: 0 3px;
  white-space: nowrap;
  text-overflow: ellipsis;
  overflow: hidden;
}

.e-placeholder-error {
  color: #d40000;
  font-size: 14px;
  word-spacing: 1px;
  margin: 0 10px;
  max-width: calc(100% - 20px);
  position: absolute;
  top: -0.55em;
  background: #fff;
  display: block;
  float: left;
  padding: 0 3px;
  white-space: nowrap;
  text-overflow: ellipsis;
  overflow: hidden;
}

.b-input input {
  border: 1px solid #dadada;
  border-radius: 5px;
  width: 100%;
  padding: 15px;
  box-sizing: border-box;
}

.has-error {
  border-color: #d40000 !important;
}
</style>
