<script>
import { defineComponent } from "vue";
import { VueDraggableNext } from "vue-draggable-next";

import DraggableListItem from "./DraggableListItem.vue";

export default defineComponent({
  components: {
    draggable: VueDraggableNext,
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
      if (event.draggedContext.element.RecordType === "Article") return true;

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
        data-level="1"
        class="space-y-4"
        group="draggable"
      >
        <div
          v-for="(category, categoryIndex) in list"
          :key="categoryIndex"
          :data-record-type="category.RecordType"
        >
          <!-- :class="
              category.RecordType === 'Category'
                ? 'bg-purple-100'
                : 'bg-red-100'
            " -->
          <draggable-list-item>
            {{ category.Name }}
          </draggable-list-item>

          <!-- LEVEL 2 -->
          <draggable
            v-if="category.tasks && category.RecordType !== 'Article'"
            :list="category.tasks"
            :move="onMoveCallback"
            data-level="2"
            group="draggable"
            class="ml-8 space-y-4 mt-4"
          >
            <div
              v-for="(section, sectionIndex) in category.tasks"
              :key="sectionIndex"
              :data-record-type="section.RecordType"
            >
              <!-- :class="
                  section.RecordType === 'Section'
                    ? 'bg-blue-100'
                    : 'bg-red-100'
                " -->
              <draggable-list-item>
                {{ section.Name }}
              </draggable-list-item>

              <!-- LEVEL 3 -->
              <draggable
                v-if="section.tasks && section.RecordType !== 'Article'"
                :list="section.tasks"
                :move="onMoveCallback"
                data-level="3"
                group="draggable"
                class="ml-8 space-y-4 mt-4"
              >
                <div
                  v-for="(article, articleIndex) in section.tasks"
                  :key="articleIndex"
                  :data-record-type="article.RecordType"
                >
                  <!-- class="bg-red-100" -->
                  <draggable-list-item>
                    {{ article.Name }}
                  </draggable-list-item>
                </div>
              </draggable>
            </div>
          </draggable>
        </div>
      </draggable>
    </div>
  </div>
</template>
