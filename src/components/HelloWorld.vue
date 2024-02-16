<script setup lang="ts">
import { ref } from 'vue';
import DraggableElement from './DraggableElement.vue';
import pI from './p-i.vue';

export interface Val {
  id: string,
  text?: string | null,
}

interface Dimensions {
  height: string,
  width: string
} 

const values = ref<Val | null>(null);
const texts = ref<Val[] | null>(null);
const dimensions = ref<Dimensions>({
  height: '300',
  width: '300'
});

const addText = () => {
  const newText = { id: Date.now().toString(), text: '', isInput: false };
  if (texts.value) {
    const newArray = [...texts.value, newText];
    texts.value = newArray;
  } else {
    texts.value = [newText];
  }
}

const addNewPaper = () => {
  values.value = { id: Date.now().toString() };
  texts.value = [];
}
</script>

<template>
  <div class="wrapper">
    <pI />
    <v-btn 
      prepend-icon="mdi-plus" 
      variant="tonal" 
      color="purple"
      size="x-large"
      @click='addNewPaper'>
      Create new
    </v-btn>
    <div class="value-wrapper" v-if="values">
      <div class="buttons">
        <v-btn 
          variant="tonal"
          color="purple"
          size="large"
          @click="addText()"> Add text </v-btn>
        <v-btn 
          variant="tonal"
          color="purple"
          size="large">Add figure</v-btn>
          <div class="slider">
            <v-slider
              v-model="dimensions.height"
              :max="750"
              :min="0"
              :step="1"
              label="Height"
              color="purple"
              hide-details
              class="ma-4"
            >
              <template v-slot:append>
                <v-text-field
                  v-model="dimensions.height"
                  type="number"
                  :max="750"
                  :min="0"
                  :value="dimensions.height"
                  style="width: 80px"
                  density="compact"
                  hide-details
                  variant="outlined"
                ></v-text-field>
              </template>
            </v-slider>
            <v-slider
              v-model="dimensions.width"
              :max="750"
              :min="0"
              :step="1"
              label="Width"
              color="purple"
              hide-details
              class="ma-4"
            >
              <template v-slot:append>
                <v-text-field
                  v-model="dimensions.width"
                  type="number"
                  :max="750"
                  :min="0"
                  :value="dimensions.width"
                  style="width: 80px"
                  density="compact"
                  hide-details
                  variant="outlined"
                ></v-text-field>
              </template>
            </v-slider>
          </div>
      </div>
      <div class="new-design-block">
        <v-sheet 
          :height="dimensions.height" 
          :width="dimensions.width"
          :elevation="5">
          <div v-if="texts">
            <div v-for="text in texts" 
              :key="text.id">
              <DraggableElement class="text-div" :text="text" />
            </div>
          </div>
        </v-sheet>
      </div>
    </div>
  </div>
</template>

<style>
  .wrapper {
    padding: 10px;
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    width: 100%;
    gap: 20px;
  }

  .value-wrapper {
    display: flex;
    width: 100%;
    justify-content: space-between;
    gap: 8px
  }
  .buttons {
    min-width: 250px;
    max-width: 250px;
    gap: 8px;
    display: flex;
    flex-direction: column;
    flex-grow: 2;
  }

  .new-design-block {
    display: flex;
    justify-content: center;
    position: relative;
    flex-grow: 5;
  }
  .new-paper {
    padding: 5px;
    margin: auto;
    margin-top: 40px;
    background-color: white;
    -webkit-box-shadow: 8px 8px 24px 0px rgba(66, 68, 90, 1);
    -moz-box-shadow: 8px 8px 24px 0px rgba(66, 68, 90, 1);
    box-shadow: 8px 8px 24px 0px rgba(66, 68, 90, 1);
  }
</style>
