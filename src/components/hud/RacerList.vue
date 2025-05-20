```vue
<template>
  <div class="racers-holder">
    <div
      class="box"
      v-for="(racer, index) in shortenedRacers"
      :key="racer.RacerSource"
      :class="{
        me: index === globalStore.activeRace.position-1,
      }"
    >
      <div class="position">{{ index + 1 }}</div>
      <span class="name">{{ racer.RacerName }}</span>
      <span class="time" v-if="index === 0 && racer.Finished">{{ translate('winner') }}</span>
      <span class="time" v-else-if="racer.Finished">{{ translate('finished') }}</span>
      <span class="time" v-else-if="index !== globalStore.activeRace.position-1">{{ getTimeDifference(racers[globalStore.activeRace.position - 1], racer) }}</span>
    </div>
  </div>
</template>

<script setup lang="ts">
import { useGlobalStore } from "@/store/global";
import { ActiveRacer } from "../../store/types";
import { computed } from "vue";
import { translate } from "@/helpers/translate";

const props = defineProps<{
  racers: ActiveRacer[];
}>();

const globalStore = useGlobalStore();
const shortenedRacers = computed(() =>
  props.racers?.slice(
    0,
    globalStore.baseData?.data?.hudSettings?.maxPositions || 10
  )
);

const formatTimeDifference = (timeDiffMs: number): string => {
  if (timeDiffMs === 0) {
    return "0.000";
  }

  const isAhead = timeDiffMs > 0;
  const absoluteDiffSeconds = Math.abs(timeDiffMs) / 1000;
  const formattedTime = absoluteDiffSeconds.toFixed(3);
  return isAhead ? `+${formattedTime}` : `-${formattedTime}`;
};

const getTimeDifference = (racer1: ActiveRacer, racer2: ActiveRacer) => {
  const racer1Checkpoints = racer1.CheckpointTimes.length;
  const racer2Checkpoints = racer2.CheckpointTimes.length;

  if (racer1Checkpoints === 0 || racer2Checkpoints === 0) {
    return '';
  }

  const lastCommonCheckpoint = Math.min(racer1Checkpoints, racer2Checkpoints) - 1;
  const racer1Time = racer1.CheckpointTimes[lastCommonCheckpoint].time;
  const racer2Time = racer2.CheckpointTimes[lastCommonCheckpoint].time;
  return formatTimeDifference(racer2Time - racer1Time);
};
</script>

<style scoped lang="scss">
.racers-holder {
  display: flex;
  flex-direction: column;
  gap: 4px;
  padding: 8px;
  background: rgba(10, 25, 41, 0.4);
  backdrop-filter: blur(8px);
  border-radius: 8px;
  max-width: 400px;
}

.box {
  display: flex;
  align-items: center;
  gap: 12px;
  padding: 8px 16px;
  background: rgba(30, 42, 59, 0.6);
  border: 1px solid rgba(0, 163, 255, 0.1);
  border-radius: 4px;
  font-family: 'Roboto', sans-serif;
  transition: all 0.2s ease;

  &:hover {
    background: rgba(30, 42, 59, 0.8);
  }

  &.me {
    background: rgba(0, 163, 255, 0.2);
    border-color: rgba(0, 163, 255, 0.4);
  }
}

.position {
  min-width: 24px;
  height: 24px;
  display: flex;
  align-items: center;
  justify-content: center;
  background: rgba(0, 163, 255, 0.2);
  border-radius: 4px;
  font-weight: 600;
  color: #00A3FF;
}

.name {
  flex: 1;
  font-size: 14px;
  color: #fff;
  text-overflow: ellipsis;
  overflow: hidden;
  white-space: nowrap;
}

.time {
  font-size: 14px;
  font-family: 'Roboto Mono', monospace;
  color: #00A3FF;
  min-width: 80px;
  text-align: right;
}
</style>
```