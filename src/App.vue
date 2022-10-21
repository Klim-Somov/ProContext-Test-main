<template>
  <div class="wrapper">
    <div class="left-window">
      <ul>
        <div v-for="list in lists" :key="list.id" class="list-div">
          <li>
            <i
              class="arrow"
              :style="{ transform: list.rotation }"
              @click="hideItems(list)"
            ></i>
            <input
              type="checkbox"
              v-indeterminate="Indeterm(list)"
              v-model="list.selectAll"
              @click="selectList(list)"
            />
            {{ list.listName }}
          </li>
          <div
            class="item-box"
            v-for="item in list.items"
            :key="item.id"
            :style="{ display: list.visibility }"
          >
            <div class="input-wrapper">
              <input type="checkbox" v-model="list.selected" :value="item.id" />
              {{ selectItem(list) }}
              {{ item.name }}
            </div>
            <div>
              <input
                type="number"
                class="item-input-number"
                min="0"
                oninput="validity.valid||(value='');"
                v-model="item.number"
              />
            </div>
            <input type="color" v-model="item.color" />
          </div>
        </div>
      </ul>
    </div>
    <div class="right-window">
      <div class="inner" v-for="list in lists" :key="list.id">
        {{ list.listName }}
        <div v-for="item in list.items" :key="item.id">
          <div v-if="list.selected.indexOf(item.id) > -1">
            <span v-for="n in item.number" :key="n.id">
              <span
                class="square"
                :style="{ color: item.color }"
                @click="deleteItem(item)"
              >
                ■
              </span>
            </span>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "App",
  data: function () {
    return {
      lists: [
        {
          listName: "List 1",
          selectAll: false,
          selected: [],
          rotation: "rotate(45deg)",
          visibility: "flex",
          items: [
            { id: 1, name: "Item 1", number: 2, color: "#9E26EE" },
            { id: 2, name: "Item 2", number: 6, color: "#FCB019" },
            { id: 3, name: "Item 3", number: 13, color: "#3A7994" },
            { id: 4, name: "Item 4", number: 5, color: "#ff0000" },
          ],
        },
        {
          listName: "List 2",
          selectAll: false,
          selected: [],
          rotation: "rotate(45deg)",
          visibility: "flex",
          items: [
            { id: 5, name: "Item 1", number: 4, color: "#9E26EE" },
            { id: 6, name: "Item 2", number: 15, color: "#FCB019" },
            { id: 7, name: "Item 3", number: 32, color: "#3A7994" },
            { id: 8, name: "Item 4", number: 5, color: "#ff0000" },
            { id: 9, name: "Item 5", number: 25, color: "#00FF00" },
          ],
        },
        {
          listName: "List 3",
          selectAll: false,
          selected: [],
          rotation: "rotate(45deg)",
          visibility: "flex",
          items: [
            { id: 10, name: "Item 1", number: 3, color: "#9E26EE" },
            { id: 11, name: "Item 2", number: 7, color: "#008080" },
            { id: 12, name: "Item 3", number: 1, color: "#3A7994" },
            { id: 13, name: "Item 4", number: 5, color: "#300000" },
            { id: 14, name: "Item 5", number: 8, color: "#FF9090" },
            { id: 15, name: "Item 6", number: 40, color: "#ff0000" },
          ],
        },
        {
          listName: "List 4",
          selectAll: false,
          selected: [],
          rotation: "rotate(45deg)",
          visibility: "flex",
          items: [
            { id: 16, name: "Item 1", number: 8, color: "#9E26EE" },
            { id: 17, name: "Item 2", number: 33, color: "#008080" },
            { id: 18, name: "Item 3", number: 7, color: "#3A7994" },
            { id: 19, name: "Item 4", number: 2, color: "#300000" },
            { id: 20, name: "Item 5", number: 15, color: "#FF9090" },
            { id: 21, name: "Item 6", number: 12, color: "#ff0000" },
            { id: 22, name: "Item 7", number: 13, color: "#ff0700" },
          ],
        },
      ],
    };
  },

  methods: {
    Indeterm: function (list) {
      if (
        list.selected.length < list.items.length &&
        list.selected.length > 0
      ) {
        return true;
      } else {
        return false;
      }
    },

    selectItem: function (list) {
      if (list.selected.length == list.items.length) {
        list.selectAll = true;
      } else {
        list.selectAll = false;
      }
    },
    selectList: function (list) {
      if (list.selectAll == true) {
        list.selected = [];
      } else {
        let selected = [];
        list.items.forEach(function (item) {
          selected.push(item.id);
        });
        list.selected = selected;
      }
    },
    deleteItem: function (item) {
      item.number = --item.number;
    },
    hideItems: function (list) {
      if (list.rotation == "rotate(45deg)") {
        list.rotation = "rotate(-45deg)";
        list.visibility = "none";
      } else {
        list.rotation = "rotate(45deg)";
        list.visibility = "flex";
      }
    },
  },

  directives: {
    indeterminate: function (el, binding) {
      el.indeterminate = !!binding.value;
    },
  },
};
</script>
<style>
ul {
  list-style: none;
}
.wrapper {
  border: 1px solid black;
  display: flex;
  justify-content: space-between;
  padding: 50px;
}

.left-window {
  border: 1px solid black;
  width: 40%;
  padding: 10px;
}
.item-box {
  gap: 10px;
  display: flex;
  align-items: center;
}

.right-window {
  border: 1px solid black;
  width: 40%;
  padding: 10px;
}

.inner {
  border: 1px solid black;
  padding: 10px;
  margin: 10px;
}

.square:hover {
  cursor: pointer;
}

.square {
  font-size: 20px;
}

.list-div {
  margin: 10px;
}

.arrow {
  border: solid black;
  border-width: 0 2px 2px 0;
  display: inline-block;
  padding: 4px;
  margin-right: 4px;
  cursor: pointer;
}
.input-wrapper {
  flex: 1 0;
}
.item-input-number {
  width: 50px;
}
@media (max-width: 700px) {
  .wrapper {
    flex-direction: column;
    align-items: center;
    gap: 40px;
  }
  .right-window {
    width: 80%;
  }
  .left-window {
    width: 80%;
  }
}
</style>
