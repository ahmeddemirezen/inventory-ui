<template>
  <div class="main-inv-frame q-pl-xs q-pr-xl q-pt-xs">
    <q-card class="main-inv">
      <div class="q-pb-md q-gutter-sm">
        <q-bar dark class="bg-primary text-white">
          <q-btn dense flat round icon="lens" size="10px" color="red" />
          <q-btn dense flat round icon="lens" size="10px" color="yellow" />
          <q-btn dense flat round icon="lens" size="10px" color="green" />
          <div class="col text-center text-weight-bold">My-Inventory</div>
        </q-bar>
      </div>
      <div
        class="
          item-frame
          q-ml-md q-mr-md q-mt-xs
          rounded-borders
          row
          q-col-gutter-x-xs q-col-gutter-y-lg
        "
      >
        <div
          class="item col-2 q-pa-none"
          v-for="index in items.length"
          :key="index"
        >
          <div
            class="item-left-pos float-left rounded-borders"
            @drop="onDrop($event, index - 1)"
            @dragover.prevent
            @dragenter.prevent
          ></div>
          <div
            class="item-right-pos float-right rounded-borders"
            @drop="onDrop($event, index)"
            @dragover.prevent
            @dragenter.prevent
          ></div>
          <div class="item-inv q-pa-md">
            <Item
              :Id="'ID ' + items[index - 1].Id"
              :Name="items[index - 1].Name"
              :Image="items[index - 1].Image"
              :Weight="items[index - 1].Weight"
              draggable="true"
              @dragstart="startDrag($event, items[index - 1])"
            ></Item>
          </div>
        </div>
      </div>
      <div class="row q-pt-md absolute-bottom">
        <div class="col q-pa-xs">
          <q-btn
            color="secondary"
            icon-right="add"
            label="Add Item"
            @click="addItem()"
          />
        </div>
        <div class="col q-pa-xs">
          <q-btn
            color="secondary"
            icon-right="refresh"
            label="Sort Items"
            @click="sortItems()"
          />
        </div>
        <div class="col q-pa-xs">
          <q-btn
            color="secondary"
            icon-right="delete"
            label="Clear Items"
            @click="clearItems()"
          />
        </div>
      </div>
    </q-card>
  </div>
</template>
<style scoped>
.main-inv {
  background-color: rgb(216, 78, 78);
  width: 800px;
  height: 900px;
}
.item-inv:hover {
  animation: shake 0.7s;
  animation-iteration-count: 1;
}
.item-frame {
  background-color: rgba(0, 0, 0, 0.3);
  min-height: 130px;
}
.item-left-pos {
  background-color: rgba(0, 252, 0, 0);
  width: 10px;
  height: 100%;
}
.item-left-pos:hover {
  background-color: rgba(0, 252, 0, 0.5);
}
.item-right-pos {
  background-color: rgba(0, 252, 0, 0);
  width: 10px;
  height: 100%;
}
.item-right-pos:hover {
  background-color: rgba(0, 252, 0, 0.5);
}
@keyframes shake {
  0% {
    transform: translate(1px, 1px) rotate(0deg);
  }
  10% {
    transform: translate(-1px, -2px) rotate(-1deg);
  }
  20% {
    transform: translate(-3px, 0px) rotate(1deg);
  }
  30% {
    transform: translate(3px, 2px) rotate(0deg);
  }
  40% {
    transform: translate(1px, -1px) rotate(1deg);
  }
  50% {
    transform: translate(-1px, 2px) rotate(-1deg);
  }
  60% {
    transform: translate(-3px, 1px) rotate(0deg);
  }
  70% {
    transform: translate(3px, 1px) rotate(-1deg);
  }
  80% {
    transform: translate(-1px, -1px) rotate(1deg);
  }
  90% {
    transform: translate(1px, 2px) rotate(0deg);
  }
  100% {
    transform: translate(1px, -2px) rotate(-1deg);
  }
}
@keyframes slidein {
  from {
    margin-left: 100%;
    width: 110%;
  }

  to {
    margin-left: 0%;
    width: 100%;
  }
}
@keyframes zoomin {
  from {
    transform: scale(1);
  }

  to {
    transform: scale(1.1);
  }
}
</style>
<script>
import Item from "../components/Item.vue";
import InvJson from "../assets/data/inv-data.json";
export default {
  invJson: InvJson,
  components: {
    Item,
  },
  data: () => ({
    items: [],
  }),
  methods: {
    startDrag(evt, item) {
      evt.dataTransfer.dropEffect = "move";
      evt.dataTransfer.effectAllowed = "move";
      evt.dataTransfer.setData("itemPos", item.ItemPos);
      evt.dataTransfer.setData("itemId", item.Id);
    },
    onDrop(evt, newPos) {
      const itemId = evt.dataTransfer.getData("itemId");
      const itemPos = evt.dataTransfer.getData("itemPos");
      const item = this.items.find((item) => item.Id == itemId);
      item.ItemPos = newPos;
      this.items.splice(itemPos, 1);
      this.items.splice(newPos, 0, item);
      this.updatePos();
      this.saveData();
      console.log(newPos + " " + itemPos);
    },
    updatePos() {
      this.items.forEach((item, index) => {
        item.ItemPos = index;
      });
    },
    addItem() {
      let newItem = {
        Id: this.items.length,
        Amount: Math.floor(Math.random() * 50),
        Name: "Name" + Math.floor(Math.random() * 1000),
        Weight: Math.floor(Math.random() * 20),
        Image: "https://static.thenounproject.com/png/3548770-200.png",
        ItemPos: 0,
      };
      this.items.push(newItem);
      this.updatePos();
      this.saveData();
    },
    sortItems() {
      this.items.sort((a, b) => {
        return a.Id - b.Id;
      });
      this.items.forEach((item, index) => {
        item.ItemPos = index;
      });
      this.saveData();
    },
    clearItems() {
      this.items = [];
      this.saveData();
    },
    saveData() {
      const data = JSON.stringify(this.items);
      window.localStorage.setItem("items", data);
    },
    readData() {
      let data = JSON.parse(window.localStorage.getItem("items"));
      if (data == null) {
        this.saveData();
        return;
      }
      this.items = JSON.parse(window.localStorage.getItem("items"));
    },
  },
  created() {
    // console.log("worked")
    // for (let i = 0; i < this.items.length; i++) {
    //   this.items[i].ItemPos = i;
    //   this.items[i].Id = i;
    //   console.log(this.items[i]);
    // }
    // this.saveData()
    this.readData();
  },
};
</script>