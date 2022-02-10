<template>
  <!-- <div>
    <q-card class="my-card">
      <q-card-section class="bg-primary text-white">
        <div class="text-h6">{{ Name }}</div>
        <div class="text-subtitle2">{{ Id }}</div>
      </q-card-section>

      <q-separator />

      <q-card-actions align="center">
        <q-icon size="50px" class="item-image q-ma-lg" :name="'img:' + Image" />
      </q-card-actions>
    </q-card>
  </div> -->
  <div>
    <q-card class="my-card shadow-6">
      <q-img class="item-image rounded-borders" :src="Image" />
      
      <div class="info-frame absolute-bottom text-subtitle2 text-center q-pa-none">
        {{ Id }}
        <q-separator color="orange" inset/>
        {{ Name }}
        <q-btn
        dense
        round
        icon="fitness_center"
        class="q-ma-xs absolute-bottom-right bg-primary"
      >
        <q-badge class="bg-negative" floating>{{ Weight }}</q-badge>
      </q-btn>
      </div>      
    </q-card>
  </div>
</template>

<style scoped>
.item-image {
  width: 100%;
  height: 100%;
}
.info-frame{
  background-color: rgba(0, 0, 0, 0.836);
  color : white;
  font-size: 7pt;
}
</style>

<script>
export default {
  props: {
    Id: Number,
    Amount: Number,
    Name: String,
    Weight: Number,
    Image: String,
    ItemPos: Number,
  },
};
</script>
