<template>
  <div id="mainCard">
    <div id="cardDateTextSaveClear">
      <div id="cardDate">{{ item.date }}</div>
      <div id="cardTextSaveClear">
        <button @click="cardText" id="cardT"><fa icon="pencil"></fa></button>
        <button
          @click="cardSave"
          :style="{
            backgroundColor: saveStyle.ColorBack,
            color: saveStyle.Color,
            boxShadow: saveStyle.BoxShadow,
          }"
          id="cardS"
        >
          <fa icon="bookmark"></fa>
        </button>
        <button @click="cardDelet" id="cardD"><fa icon="xmark"></fa></button>
      </div>
    </div>
    <p id="cardText" :style="{ display: cardB }">{{ item.text }}</p>
    <textarea
      type="text"
      @blur="cardTextBlurUp"
      @keyup.enter="cardTextBlurUp"
      v-model="cardTextValue"
      :style="{ display: cardN }"
    ></textarea>
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
      dataStore: [],
      saveStore: [],
      cardB: "block",
      cardN: "none",
      cardTextValue: this.item.text,
      saveStyle: {
        ColorBack: "",
        Color: "",
        BoxShadow: "",
      },
    };
  },
  methods: {
    cardDelet() {
      this.localData.forEach((element) => {
        if (element.id != this.item.id) {
          this.dataStore.push(element);
        }
      });

      if (this.localSave) {
        this.localSave.forEach((element) => {
          if (element != this.item.id) {
            this.saveStore.push(element);
          }
        });
        localStorage.setItem("save", JSON.stringify(this.saveStore));
      }

      localStorage.setItem("data", JSON.stringify(this.dataStore));
      window.location.reload();
    },
    cardText(e) {
      this.cardB = "none";
      this.cardN = "block";

      setTimeout(() => {
        e.path.forEach((element) => {
          if (element.id == "cardDateTextSaveClear")
            element.nextElementSibling.nextElementSibling.focus();
        });
      }, 100);
    },
    cardTextBlurUp() {
      this.cardB = "block";
      this.cardN = "none";

      this.dataStore = this.localData.map((element) => {
        if (element.id == this.item.id) {
          element.text = this.cardTextValue;
          return element;
        } else return element;
      });

      localStorage.setItem("data", JSON.stringify(this.dataStore));
      window.location.reload();
    },
    cardSave() {
      if (this.localSave == null || this.localSave == "") {
        this.saveStore.push(this.item.id);
        localStorage.setItem("save", JSON.stringify(this.saveStore));
      } else {
        if (this.localSave.includes(this.item.id)) {
          this.localSave.forEach((element) => {
            if (element != this.item.id) {
              this.saveStore.push(element);
            }
          });
        } else {
          this.localSave.forEach((element) => this.saveStore.push(element));
          this.saveStore.push(this.item.id);
        }
      }

      localStorage.setItem("save", JSON.stringify(this.saveStore));
      window.location.reload();
    },
  },
  created() {
    if (this.localSave) {
      if (this.localSave.includes(this.item.id)) {
        this.saveStyle.ColorBack = "#32CD32";
        this.saveStyle.Color = "#FBFCF8";
        this.saveStyle.BoxShadow = "0 0 15px #32CD32";
      } else {
        this.saveStyle.ColorBack = "#FBFCF8";
        this.saveStyle.Color = "#32CD32";
        this.saveStyle.BoxShadow = "0 0 15px #FBFCF8";
      }
    } else {
      this.saveStyle.ColorBack = "#FBFCF8";
      this.saveStyle.Color = "#32CD32";
      this.saveStyle.BoxShadow = "0 0 15px #FBFCF8";
    }
  },
};
</script>
