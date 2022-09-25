<script>
import { defineComponent } from "vue";

import Draggable from "vuedraggable";
import DraggableListItem from "./DraggableListItem.vue";

export default defineComponent({
  components: {
    Draggable,
    DraggableListItem,
  },
  data() {
    return {
      list: [
        {
          Name: "Category1",
          Id: 130,
          ParentId: 128,
          RecordType: "Category",
          tasks: [
            {
              Name: "Section1.1",
              Id: 230,
              ParentId: 128,
              RecordType: "Section",
              tasks: [
                {
                  Name: "Article1",
                  Id: 430,
                  ParentId: 128,
                  RecordType: "Article",
                },
              ],
            },
            {
              Name: "Section1.2",
              Id: 235,
              ParentId: 128,
              RecordType: "Section",
              tasks: [
                {
                  Name: "Article2",
                  Id: 430,
                  ParentId: 128,
                  RecordType: "Article",
                },
                {
                  Name: "Article2.1",
                  Id: 430,
                  ParentId: 128,
                  RecordType: "Article",
                },
              ],
            },
          ],
        },
        {
          Id: 9930,
          RecordType: "Article",
          Name: "Article Level One",
        },
        {
          Name: "Category2",
          Id: 131,
          ParentId: 128,
          RecordType: "Category",
          tasks: [
            {
              Name: "Section2.1",
              Id: 230,
              ParentId: 128,
              RecordType: "Section",
              tasks: [],
            },
            {
              Name: "Section2.2",
              Id: 235,
              ParentId: 128,
              RecordType: "Section",
              tasks: [],
            },
          ],
        },
        {
          Name: "Category3",
          Id: 132,
          ParentId: 128,
          RecordType: "Category",
          tasks: [
            {
              Name: "Section3.1",
              Id: 230,
              ParentId: 128,
              RecordType: "Section",
              tasks: [],
            },
            {
              Name: "Article3",
              Id: 450,
              ParentId: 128,
              RecordType: "Article",
              tasks: [],
            },
          ],
        },
      ],
    };
  },
  methods: {
    onMoveCallback(event) {
      const to = +event.to.getAttribute("data-level");
      const from = +event.from.getAttribute("data-level");

      // Allow for same level
      if (from === to) return true;

      // Allow By RecordType
      if (event?.draggedContext?.element?.RecordType === "Article") return true;

      return false;
    },
  },
});
</script>

<template>
  <div class="bg-[#E4E4E4]">
    <div class="mx-auto py-16 max-w-4xl">
      <draggable
        :list="list"
        :move="onMoveCallback"
        class="space-y-4"
        data-level="1"
        item-key="Name"
        group="draggable"
      >
        <template #item="{ element: category }">
          <div :data-record-type="category.RecordType">
            <draggable-list-item>
              {{ category.Name }}
            </draggable-list-item>

            <!-- LEVEL 2 -->
            <draggable
              v-if="category.tasks && category.RecordType !== 'Article'"
              :list="category.tasks"
              :move="onMoveCallback"
              item-key="Name"
              data-level="2"
              group="draggable"
              class="ml-8 space-y-4 mt-4"
            >
              <template #item="{ element: section }">
                <div :data-record-type="section.RecordType">
                  <draggable-list-item>
                    {{ section.Name }}
                  </draggable-list-item>

                  <!-- LEVEL 3 -->
                  <draggable
                    v-if="section.tasks && section.RecordType !== 'Article'"
                    :list="section.tasks"
                    :move="onMoveCallback"
                    item-key="Name"
                    data-level="3"
                    group="draggable"
                    class="ml-8 space-y-4 mt-4"
                  >
                    <template #item="{ element: article }">
                      <draggable-list-item
                        :data-record-type="article.RecordType"
                      >
                        {{ article.Name }}
                      </draggable-list-item>
                    </template>
                  </draggable>
                </div>
              </template>
            </draggable>
          </div>
        </template>
      </draggable>
    </div>
  </div>
</template>
