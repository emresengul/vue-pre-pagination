<template>
    <div class="paginations no-select">
        <div @click="paginateManual(-1)" v-if="hasSlot('prev')">
            <slot name="prev"></slot>
        </div>
        <div v-else class="pagination previous func" @click="paginateManual(-1)">
            <svg
                width="24"
                height="24"
                xmlns="http://www.w3.org/2000/svg"
                fill-rule="evenodd"
                clip-rule="evenodd"
                fill="#fff"
            >
                <path
                    d="M2.117 12l7.527 6.235-.644.765-9-7.521 9-7.479.645.764-7.529 6.236h21.884v1h-21.883z"
                />
            </svg>
        </div>
        <div
            :class="getClass(p)"
            v-for="(p, i) in pagination(activePage, {
        min: 1,
        total: records,
        length: count,
      })"
            :key="i"
            @click="activePage = p"
        >{{ p }}</div>
        <div @click="paginateManual(1)" v-if="hasSlot('next')">
            <slot name="next"></slot>
        </div>
        <div v-else class="pagination next func" @click="paginateManual(1)">
            <svg
                width="24"
                height="24"
                xmlns="http://www.w3.org/2000/svg"
                fill-rule="evenodd"
                clip-rule="evenodd"
                fill="#fff"
            >
                <path
                    d="M21.883 12l-7.527 6.235.644.765 9-7.521-9-7.479-.645.764 7.529 6.236h-21.884v1h21.883z"
                />
            </svg>
        </div>
    </div>
</template>

<script>
    export default {
      props: {
        records: {
          type: Number,
          required: true,
        },
        count: {
          type: Number,
          default: 10,
        },
        start: {
          type: Number,
          default: 1,
          validator: function (value) {
            console.log("Status =", value > 0);
            return value > 0;
          },
        },
        paginationClass: {
          type: String,
          default: "pagination",
        },
        activeClass: {
          type: String,
          default: "active",
        },
      },
      data() {
        return {
          activePage: 1,
        };
      },
      created() {
        this.activePage = this.start;
        if (this.activePage > this.records) {
          this.activePage = this.records;
        } else if (this.activePage < this.records) {
          this.activePage = 1;
        }
      },
      methods: {
        pagination(current, { min = 1, total = 20, length = 5 } = {}) {
          if (length > total) length = total;
          let start = current - Math.floor(length / 2);
          start = Math.max(start, min);
          start = Math.min(start, min + total - length);
          return Array.from({ length: length }, (el, i) => start + i);
        },
        paginateManual(value) {
          if (
            this.activePage + value > 0 &&
            this.activePage + value <= this.records
          ) {
            this.activePage += value;
          }
        },
        hasSlot(name = "default") {
          return !!this.$slots[name];
        },
        getClass(index) {
          let classList = "static-pagination " + this.paginationClass;
          if (index === this.activePage) {
            classList += ` ${this.activeClass}`;
          }
          return classList;
        },
      },
    };
</script>

<style scoped>
.paginations {
    width: 100%;
    height: auto;
    display: flex;
}
.paginations .pagination {
    width: 25px;
    height: 25px;
    background-color: gray;
    display: flex;
    justify-content: center;
    align-items: center;
    margin-right: 5px;
    border-radius: 5px;
}
.paginations .pagination:last-child {
    margin-right: 0;
}
.paginations .pagination:hover {
    cursor: pointer;
}
.func {
    background-color: blue !important;
    color: #fff;
    cursor: pointer;
}
.active {
    background-color: #0a0a !important;
}
.no-select {
    -webkit-touch-callout: none; /* iOS Safari */
    -webkit-user-select: none; /* Safari */
    -khtml-user-select: none; /* Konqueror HTML */
    -moz-user-select: none; /* Firefox */
    -ms-user-select: none; /* Internet Explorer/Edge */
    user-select: none; /* Non-prefixed version, currently
                                  supported by Chrome and Opera */
}
.static-pagination:hover {
    cursor: pointer;
}
</style>
