<template>
  <div class="container">
    <div><h4>Existing Items</h4></div>

    <div class="item-list">
      <div class="">
        <p v-if="itemListForAppList.length === 0">
          Nice job! no Items are in there
        </p>

        <TransitionGroup name="list" tag="ul">
          <li
            v-for="item in itemListForAppList"
            :key="item.id"
            class="item-list-element"
          >
            <span>
              <span
                class="item-list-element-name"
                :class="{
                  strikeout: item.itemPurchesed === true,
                  priority: item.priority == 'true',
                }"
                @click="sendTogglePurchaseSignal(item.id)"
              >
                {{ item.id }} | {{ item.itemName }}
              </span>

              <span
                v-if="item.itemType === 'true' && !item.itemPurchesed"
                class="label-ebook"
              >
                {{ itemTypeDict[0] }}
              </span>
              <span
                v-if="item.itemType === 'false' && !item.itemPurchesed"
                class="label-paperback"
              >
                {{ itemTypeDict[1] }}
              </span>
            </span>
            <span>
              <AppButton variant="delete" @click="sendDeleteSignal(item.id)">
                Delete</AppButton
              >
            </span>
          </li>
        </TransitionGroup>
      </div>
    </div>
  </div>
</template>
<script>
import AppButton from "./AppButton.vue";

export default {
  components: {
    AppButton,
  },
  props: {
    itemListForAppList: { type: Array, required: true },
  },
  emits: ["deleteSignal", "togglePurchase"],
  data() {
    return {
      itemTypeDict: ["Paperback", "ebook"],
    };
  },

  methods: {
    sendDeleteSignal(itemId) {
      this.$emit("deleteSignal", itemId);
    },
    sendTogglePurchaseSignal(itemId) {
      this.$emit("togglePurchase", itemId);
    },
  },
};
</script>

<style>
.list-enter-active,
.list-leave-active {
  transition: all 0.5s ease;
}
.list-enter-from,
.list-leave-to {
  opacity: 0;
  transform: translateX(30px);
}

.list-leave-active {
  position: absolute;
}

.container {
  margin-top: 1.5rem;
  background: rgb(255 255 255 / 90%);
  padding: 1rem;
  border-style: solid;
  border-width: 1pt;
  border-color: rgb(255, 84, 17);
  height: 21rem;
}
ul {
  margin: 0;
  padding: 0;
}
.container h4 {
  margin-top: -1px;
}
.item-list {
  min-width: 20rem;
  height: 18rem;

  overflow-y: scroll;
  -ms-overflow-style: none; /* IE and Edge */
  scrollbar-width: none; /* Firefox */
}

.item-list::-webkit-scrollbar {
  display: none; /* Hide scrollbar for Chrome, Safari and Opera */
}

.item-list-element {
  margin: 0.5rem 0.5rem 0.8rem 0.5rem;
  display: flex;
  justify-content: space-between;
  padding: 0.5rem;
  background-color: white;
  border-radius: 0.5rem;
  background: rgba(255, 255, 255, 0.5);

  box-shadow: 0 1px 3px rgba(0, 0, 0, 0.12), 0 1px 2px rgba(0, 0, 0, 0.24);
  transition: all 0.3s cubic-bezier(0.25, 0.8, 0.25, 1);
}
.item-list-element:hover {
  box-shadow: 0 14px 28px rgba(0, 0, 0, 0.25), 0 10px 10px rgba(0, 0, 0, 0.22);
}
.item-list-element > span:first-child {
  width: 12rem;
}

.item-list-element-name {
  cursor: pointer;
  max-width: 95px;
  display: inline-block;
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;
  text-transform: capitalize;
}
.item-list-element-name:hover {
  font-size: 1.005em;
  text-overflow: clip;
}

li span:nth-child(2) {
  margin-top: 0.1rem;
  margin-left: 1rem;
  letter-spacing: 2px;
  display: inline-block;
  font-weight: 900;
  font-size: 0.6rem;
}

.strikeout {
  text-decoration: line-through;
  color: #b8c2cc;
}

.strikeout:hover {
  color: #8795a1;
}

.priority {
  color: #de751f;
}

.label-ebook {
  color: #006ec4;
  background-color: #0af9c130;
}
.label-paperback {
  color: rgb(13, 136, 13);
  background-color: #4cf0003a;
}
</style>
