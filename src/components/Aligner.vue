<template>
  <div>
    <svg
      version="1.1"
      id="Layer_1"
      xmlns="http://www.w3.org/2000/svg"
      xmlns:xlink="http://www.w3.org/1999/xlink"
      x="0px"
      y="0px"
      viewBox="-200 -200 1133.9 396.9"
      style="enable-background:new 0 0 1133.9 396.9;"
      xml:space="preserve"
    >
      <rect
        :x="box1.x"
        :y="box1.y"
        class="st0"
        :width="box1.width"
        :height="box1.height"
      />
      <rect
        :x="box2.x"
        :y="box2.y"
        class="st0"
        :width="box2.width"
        :height="box2.height"
      />

      <rect
        :x="box3.x"
        :y="box3.y"
        class="st0"
        :width="box3.width"
        :height="box3.height"
      />
      <text :transform="box3.textTransform1" class="st1 st2">
        {{ box3.kicadText1 }}
      </text>
      <text :transform="box3.textTransform2" class="st1 st2">
        {{ box3.kicadText2 }}
      </text>
      <text :transform="box3.textTransform3" class="st1 st2">
        {{ box3.kicadText3 }}
      </text>
      <text :transform="box3.textTransform4" class="st1 st2">
        {{ box3.kicadText4 }}
      </text>

      <text :transform="box3.textTransform5" class="st1 st2">
        {{ box3.kicadText5 }}
      </text>
      <rect
        :x="box4.x"
        :y="box4.y"
        class="st0"
        :width="box4.width"
        :height="box4.height"
      />
    </svg>
    <div>
      <label>
        <b>target</b>: x
        <input type="text" v-model="x1" />
      </label>

      <label>
        y
        <input type="text" v-model="y1" />
      </label>
    </div>
    <div>
      <label>
        <b>anchor</b>: x
        <input type="text" v-model="x2" />
      </label>

      <label>
        y
        <input type="text" v-model="y2" />
      </label>
    </div>
    <div>
      <label>
        <b>line width</b>:
        <input type="text" v-model="width" />
      </label>
    </div>

    <div>
      <label>
        <b>layer</b>:
        <select v-model="layer">
          <option>F.SilkS</option>
          <option>Dwgs.User</option>
          <option>Cmts.User</option>
          <option>Dwgs.User</option>
          <option>F.SilkS</option>
          <option>Eco2.User</option>
        </select>
      </label>
    </div>

    <div>
      <label>
        <b>direction</b>:
        <input type="radio" id="one" value="Left" v-model="direction" />
        <label for="one">Left</label>
        <input type="radio" id="two" value="Right" v-model="direction" />
        <label for="two">Right</label>
      </label>
    </div>
  </div>
</template>

<script lang="ts">
import { Component, Prop, Vue } from "vue-property-decorator";

@Component
export default class Aligner extends Vue {
  direction = "Left";
  x1 = 16.54302;
  y1 = 9.398;
  layer = "Dwgs.User";
  x2 = 11.78052;
  y2 = 9.398;
  width = 0.1524;

  get box1() {
    const mult = 1;
    return {
      x: -this.x1,
      y: -this.y1,
      height: this.y1 * 2 * mult,
      width: this.x1 * 2 * mult
    };
  }

  get box2() {
    const mult = 1;
    return {
      x: -this.x2,
      y: -this.y2,
      height: this.y2 * 2 * mult,
      width: this.x2 * 2 * mult
    };
  }

  get box3() {
    const y = -this.y1;
    const height = this.y1 * 2;

    const width = this.x1 * 2;
    const itsWidth = this.x2 * 2;

    const myEnd = -this.x1 + width;
    const itsEnd = -this.x2 + itsWidth;
    const endDiff = myEnd - itsEnd;

    const thisx =
      this.direction === "Left" ? this.x1 - endDiff : this.x1 + endDiff;
    const thisxAdjust = -thisx + 100;
    const newX = (width - thisx).toFixed(4);

    const point1 = { x: -thisx, y: -this.y1 };
    const point2 = { x: newX, y: -this.y1 };
    const point3 = { x: newX, y: this.y1 };
    const point4 = { x: -thisx, y: this.y1 };
    return {
      endDiff,
      newX,
      x: thisxAdjust,
      y,
      height,
      width,
      textTransform1: `matrix(1 0 0 1 ${thisxAdjust} ${y + 35} )`,
      textTransform2: `matrix(1 0 0 1 ${thisxAdjust} ${y + 35 + 12} )`,
      textTransform3: `matrix(1 0 0 1 ${thisxAdjust} ${y + 35 + 24} )`,
      textTransform4: `matrix(1 0 0 1 ${thisxAdjust} ${y + 35 + 36} )`,
      textTransform5: `matrix(1 0 0 1 ${thisxAdjust} ${y + 35 + 52} )`,

      kicadText1: `(fp_line (start ${point1.x} ${point1.y} ) (end ${point2.x} ${point2.y}) (layer ${this.layer}) (width 0.1524))`,
      kicadText2: `(fp_line (start ${point2.x} ${point2.y} ) (end ${point3.x} ${point3.y}) (layer ${this.layer}) (width 0.1524))`,
      kicadText3: `(fp_line (start ${point3.x} ${point3.y}) (end ${point4.x} ${point4.y}) (layer ${this.layer}) (width 0.1524))`,
      kicadText4: `(fp_line (start ${point4.x} ${point4.y}) (end ${point1.x} ${point1.y}) (layer ${this.layer}) (width 0.1524))`,

      kicadText5: `${newX}`
    };
  }
  get box4() {
    const mult = 1;
    return {
      x: -this.x2 + 100,
      y: -this.y2,
      height: this.y2 * 2 * mult,
      width: this.x2 * 2 * mult
    };
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
.st0 {
  fill: #ffffff;
  stroke: #000000;
  stroke-miterlimit: 10;
}

.st2 {
  font-size: 12px;
}
</style>
