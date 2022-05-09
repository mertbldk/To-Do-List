<template>
  <div id="cardSaveContainer">
    <div id="cardSaveDate">
      <div id="cardDate">{{ dataResult.date }}</div>
      <div id="cardSave">
        <button @click="save" id="cardS"><fa icon="bookmark"></fa></button>
      </div>
    </div>
    <div id="cardSaveText">{{ dataResult.text }}</div>
  </div>
</template>

<script>
export default {
  props: ["item"],
  data() {
    return {
      localData:
        JSON.parse(localStorage.getItem("data")) == "" ||
        JSON.parse(localStorage.getItem("data")) == null
          ? false
          : JSON.parse(localStorage.getItem("data")),
      localSave:
        JSON.parse(localStorage.getItem("save")) == "" ||
        JSON.parse(localStorage.getItem("save")) == null
          ? false
          : JSON.parse(localStorage.getItem("save")),
      saveStore: [],
      dataResult: {
        date: "",
        text: "",
      },
    };
  },
  methods: {
    save() {
      this.localSave.forEach((element) => {
        if (element != this.item) this.saveStore.push(element);
      });

      localStorage.setItem("save", JSON.stringify(this.saveStore));
      window.location.reload();
    },
  },
  created() {
    this.localData.forEach((element) => {
      if (element.id == this.item) {
        this.dataResult.date = element.date;
        this.dataResult.text = element.text;
      }
    });
  },
};
</script>
