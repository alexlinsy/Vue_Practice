<template>
  <div class="content">
    <div class="preview">
        <CollapsibleSection>
      <div class="preview-content">
        <div class="top-row">
          <img :src="selectedBot.head.src" alt="head"/>
        </div>
        <div class="middle-row">
          <img :src="selectedBot.leftArm.src" class="rotate-left" alt="left arm"/>
          <img :src="selectedBot.torso.src" alt="torso"/>
          <img :src="selectedBot.rightArm.src" class="rotate-right" alt="right arm"/>
        </div>
        <div class="bottom-row">
          <img :src="selectedBot.base.src" alt="base"/>
        </div>
      </div>
      </CollapsibleSection>
      <button class="add-to-cart" @click="addToCart()">Add to Cart</button>
    </div>
    <div class="top-row">
      <!-- <div class="robot-name">
                {{ selectedBot.head.title }}
                <span v-if="selectedBot.head.onSale" class="sale">
                    Sale!
                </span>
            </div> -->
      <div class="top part">
        <PartSelector
          :parts="availableParts.heads"
          position="top"
          @partSelected="(part) => (selectedBot.head = part)"
        />
      </div>
    </div>
    <div class="middle-row">
      <div class="left part">
        <PartSelector
          :parts="availableParts.arms"
          position="left"
          @partSelected="(part) => (selectedBot.leftArm = part)"
        />
      </div>
      <div class="center part">
        <PartSelector
          :parts="availableParts.torsos"
          position="center"
          @partSelected="(part) => (selectedBot.torso = part)"
        />
      </div>
      <div class="right part">
        <PartSelector
          :parts="availableParts.arms"
          position="right"
          @partSelected="(part) => (selectedBot.rightArm = part)"
        />
      </div>
    </div>
    <div class="bottom-row">
      <div class="bottom part">
        <PartSelector
          :parts="availableParts.bases"
          position="bottom"
          @partSelected="(part) => (selectedBot.base = part)"
        />
      </div>
    </div>
    <div>
      <h1>Cart</h1>
      <table>
        <thead>
          <tr>
            <th>Robot</th>
            <th class="cost">Cost</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="(robot, index) in cart" :key="index">
            <td>{{ robot.head.title }}</td>
            <td class="cost">{{ robot.cost }}</td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>

<script>
import availableParts from '../data/parts';
import PartSelector from './PartSelector.vue';
import CollapsibleSection from '../shared/CollapsibleSection.vue';

export default {
  name: 'RobotBuilder',
  data() {
    return {
      availableParts,
      cart: [],
      selectedBot: {
        head: {},
        leftArm: {},
        rightArm: {},
        torso: {},
        base: {},
      },
    };
  },
  components: {
    PartSelector,
    CollapsibleSection,
  },
  methods: {
    addToCart() {
      const robot = this.selectedBot;
      const cost = robot.head.cost
        + robot.leftArm.cost
        + robot.rightArm.cost
        + robot.torso.cost
        + robot.base.cost;
      this.cart.push({
        ...robot,
        cost,
      });
      // Remove duplicate object from the cart
      this.cart = this.cart.filter(
        (item, index, self) => self.findIndex((t) => t.cost === item.cost) === index,
      );
    },
  },
};
</script>

<style>
.part {
  position: relative;
  width: 165px;
  height: 165px;
  border: 3px solid #aaa;
}

.part img {
  width: 165px;
}

.top-row {
  display: flex;
  justify-content: space-around;
}

.middle-row {
  display: flex;
  justify-content: center;
}

.bottom-row {
  display: flex;
  justify-content: space-around;
  border-top: none;
}

.head {
  border-bottom: none;
}

.left {
  border-right: none;
}

.right {
  border-left: none;
}

.left img {
  transform: rotate(-90deg);
}

.right img {
  transform: rotate(90deg);
}

.bottom {
  border-top: none;
}

.prev-selector {
  position: absolute;
  z-index: 1;
  top: -3px;
  left: -28px;
  width: 25px;
  height: 171px;
}

.next-selector {
  position: absolute;
  z-index: 1;
  top: -3px;
  right: -28px;
  width: 25px;
  height: 171px;
}

.center .prev-selector,
.center .next-selector {
  opacity: 0.8;
}

.left .prev-selector {
  top: -28px;
  left: -3px;
  width: 144px;
  height: 25px;
}

.left .next-selector {
  top: auto;
  bottom: -28px;
  left: -3px;
  width: 144px;
  height: 25px;
}

.right .prev-selector {
  top: -28px;
  left: 24px;
  width: 144px;
  height: 25px;
}

.right .next-selector {
  top: auto;
  bottom: -28px;
  left: 24px;
  width: 144px;
  height: 25px;
}

.right .next-selector {
  right: -3px;
}

.content {
  position: relative;
}

.add-to-cart {
  position: absolute;
  width: 210px;
  padding: 3px;
  font-size: 16px;
}

td,
th {
  text-align: left;
  padding: 5px;
  padding-right: 20px;
}

.cost {
  text-align: right;
}

.preview {
  position: absolute;
  top: -20px;
  right: 0;
  width: 210px;
  height: 210px;
  padding: 5px;
}
.preview-content {
  border: 1px solid #999;
}
.preview img {
  width: 50px;
  height: 50px;
}
.rotate-right {
  transform: rotate(90deg);
}
.rotate-left {
  transform: rotate(-90deg);
}
</style>
