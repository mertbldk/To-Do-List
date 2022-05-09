<template>
  <div id="container">
    <div id="title">
      <p>To Do List</p>
    </div>
    <div id="main">
      <div id="mainSaveClear">
        <div id="mainSave" class="mainSC">
          <button @click="save">
            <fa icon="bookmark"></fa>
            <span>Save</span>
          </button>
        </div>
        <div id="mainList" class="mainSC">
          <button @click="list">
            <fa icon="address-book"></fa>
            <span>List</span>
          </button>
        </div>
        <div id="mainClear" class="mainSC">
          <button @click="clear">
            <fa icon="trash-can"></fa>
            <span>Clear All</span>
          </button>
        </div>
      </div>
      <div :style="{ display: displayN }" id="maiCardSaveContainer">
        <div v-if="localSave">
          <CardSave v-for="i in localSave" :key="i" :item="i" />
        </div>
        <div v-else id="mainNone">
          <button @click="listSave">Save from list.</button>
        </div>
      </div>
      <div :style="{ display: displayB }" id="maiCardListContainer">
        <div v-if="localData">
          <CardList v-for="i in localData" :key="i.id" :item="i" />
        </div>
        <div v-else id="mainNone">
          <button @click="add_to_do">Add to do.</button>
        </div>
      </div>
    </div>
    <div id="footer">
      <input
        @keyup.enter="textValueButton"
        v-model="textValue"
        type="text"
        placeholder="Value"
        id="footerText"
      />
      <button @click="textValueButton"><fa icon="plus"></fa></button>
    </div>
  </div>
</template>

<script>
import CardList from "./components/CardList.vue";
import CardSave from "./components/CardSave.vue";

export default {
  components: {
    CardList,
    CardSave,
  },
  data() {
    return {
      textValue: "",
      lettersStore: "",
      dateStore: [],
      saveStore: [],
      displayB: "block",
      displayN: "none",
      localSave:
        JSON.parse(localStorage.getItem("save")) == "" ||
        JSON.parse(localStorage.getItem("save")) == null
          ? false
          : JSON.parse(localStorage.getItem("save")),
      localData:
        JSON.parse(localStorage.getItem("data")) == "" ||
        JSON.parse(localStorage.getItem("data")) == null
          ? false
          : JSON.parse(localStorage.getItem("data")),
    };
  },
  methods: {
    textValueButton() {
      if (this.textValue != "") {
        let lettersB = "ABCÇDEFGĞHİIJKLMNOÖPRSŞTUÜVYZ";
        let lettersK = "abcçdefgğhiıjklmnoöprsştuüvyz";

        for (let index = 0; index < 10; index++) {
          this.lettersStore +=
            lettersB.split("")[Math.ceil(Math.random() * 28)] +
            "" +
            lettersK.split("")[Math.ceil(Math.random() * 28)] +
            "" +
            Math.ceil(Math.random() * 9);
        }

        let data = {
          id: this.lettersStore,
          date:
            (new Date().getDate() <= 10
              ? "0" + new Date().getDate()
              : new Date().getDate()) +
            "." +
            (new Date().getMonth() <= 10
              ? "0" + new Date().getMonth()
              : new Date().getMonth()) +
            "." +
            new Date().getFullYear(),
          text: this.textValue,
        };

        if (this.localData == null || this.localData == "") {
          this.dateStore.push(data);
          localStorage.setItem("data", JSON.stringify(this.dateStore));
        } else {
          this.dateStore = this.localData.map((element) => element);
          this.dateStore.push(data);
          localStorage.setItem("data", JSON.stringify(this.dateStore));
        }

        window.location.reload();

      } else alert("Please type your task into list.");
    },
    add_to_do() {
      document.getElementById("footerText").focus();
    },
    clear() {
      if (this.localData || this.localSave) {
        localStorage.removeItem("data");
        localStorage.removeItem("save");
        window.location.reload();
      }else alert('No notes have found.');
    },
    list() {
      this.displayB = "block";
      this.displayN = "none";
    },
    save() {
      this.displayB = "none";
      this.displayN = "block";
    },
    listSave() {
      this.displayB = "block";
      this.displayN = "none";
    },
  },
};
</script>

<style lang="scss">
@import "./scss/main.scss";
</style>
