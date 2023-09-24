<script setup>
import { onMounted, ref } from 'vue';

defineProps({
  msg: String
});
let message = ref('');
onMounted(async () => {
  const model = tf.sequential();
  model.add(tf.layers.dense({ units: 1, inputShape: [1] }));

  // Prepare the model for training: Specify the loss and the optimizer.
  model.compile({ loss: 'meanSquaredError', optimizer: 'sgd' });

  // Generate some synthetic data for training. (y = 2x - 1)
  const xs = tf.tensor2d([-1, 0, 1, 2, 3, 4], [6, 1]);
  const ys = tf.tensor2d([-3, -1, 1, 3, 5, 7], [6, 1]);

  // Train the model using the data.
  await model.fit(xs, ys, { epochs: 250 });
  message = model.predict(tf.tensor2d([20], [1, 1])).dataSync();
});

const count = ref(0);
</script>

<template>
  <div class="div">
    {{ message }}
  </div>
</template>

<style scoped>
.read-the-docs {
  color: #888;
}
</style>
