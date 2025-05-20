```vue
<template>
  <div class="race-hud">
    <div class="stats-container">
      <div class="stat-group">
        <div class="stat-box checkpoints">
          <div class="stat-label">Checkpoints</div>
          <div class="stat-value">{{ globalStore.activeRace.currentCheckpoint }}/{{ globalStore.activeRace.totalCheckpoints }}</div>
        </div>
        
        <div class="stat-box time">
          <div class="stat-label">Total Time</div>
          <div class="stat-value">{{ msToHMS(globalStore.activeRace.totalTime) }}</div>
        </div>

        <div class="stat-box lap">
          <div class="stat-label">Lap</div>
          <div class="stat-value">{{ lapText }}</div>
        </div>

        <div class="stat-box best">
          <div class="stat-label">Best Lap</div>
          <div class="stat-value">{{ msToHMS(globalStore.activeRace.bestLap) }}</div>
        </div>

        <div class="stat-box current">
          <div class="stat-label">Current Lap</div>
          <div class="stat-value">{{ msToHMS(globalStore.activeRace.time) }}</div>
        </div>
      </div>

      <div class="position-box" v-if="globalStore.activeRace.totalRacers > 1">
        <div class="position-label">Position</div>
        <div class="position-value">{{ globalStore.activeRace.position }}/{{ globalStore.activeRace.totalRacers }}</div>
      </div>
    </div>

    <div class="ghost-indicator" v-if="globalStore.activeRace.ghosted">
      <v-icon color="primary" icon="mdi-ghost"></v-icon>
      <span>GHOST MODE</span>
    </div>
  </div>
</template>

<script setup lang="ts">
import { useGlobalStore } from "@/store/global";
import { msToHMS } from "@/helpers/msToHMS";
import { computed } from "vue";
const globalStore = useGlobalStore();

const lapText = computed(() => {
  if (globalStore.activeRace.totalLaps === 0) return "Sprint";
  if (globalStore.activeRace.totalLaps === -1) return `${globalStore.activeRace.currentLap}/-`;
  return `${globalStore.activeRace.currentLap}/${globalStore.activeRace.totalLaps}`;
});
</script>

<style scoped lang="scss">
.race-hud {
  position: fixed;
  top: 24px;
  right: 24px;
  font-family: 'Roboto', sans-serif;
  color: #fff;
  z-index: 1000;
}

.stats-container {
  display: flex;
  gap: 16px;
  align-items: flex-start;
}

.stat-group {
  display: flex;
  flex-direction: column;
  gap: 8px;
  background: rgba(10, 25, 41, 0.85);
  backdrop-filter: blur(8px);
  padding: 16px;
  border-radius: 8px;
  border: 1px solid rgba(0, 163, 255, 0.1);
}

.stat-box {
  display: flex;
  justify-content: space-between;
  align-items: center;
  gap: 24px;
  padding: 8px;
  background: rgba(30, 42, 59, 0.4);
  border-radius: 4px;
  min-width: 240px;

  .stat-label {
    font-size: 14px;
    color: rgba(255, 255, 255, 0.7);
    text-transform: uppercase;
  }

  .stat-value {
    font-family: 'Roboto Mono', monospace;
    font-size: 16px;
    color: #00A3FF;
    font-weight: 500;
  }
}

.position-box {
  background: rgba(10, 25, 41, 0.85);
  backdrop-filter: blur(8px);
  padding: 16px;
  border-radius: 8px;
  border: 1px solid rgba(0, 163, 255, 0.1);
  text-align: center;

  .position-label {
    font-size: 14px;
    color: rgba(255, 255, 255, 0.7);
    text-transform: uppercase;
    margin-bottom: 8px;
  }

  .position-value {
    font-family: 'Roboto Mono', monospace;
    font-size: 32px;
    color: #00A3FF;
    font-weight: 600;
  }
}

.ghost-indicator {
  position: absolute;
  top: -40px;
  right: 0;
  display: flex;
  align-items: center;
  gap: 8px;
  padding: 8px 16px;
  background: rgba(0, 163, 255, 0.2);
  border-radius: 4px;
  font-size: 14px;
  color: #00A3FF;
  font-weight: 500;
}
</style>
```