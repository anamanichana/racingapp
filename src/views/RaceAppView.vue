<template>
  <div class="ui-container">
    <div class="screen-container" v-if="globalStore.baseData.data">
      <TopBar></TopBar>
      <div class="app-container">
        <v-snackbar
          v-model="snackbar"
          attach=".app-container"
          transition="slide-y-transition"
          location="top"
          contained
          multi-line
          vertical
          :timeout="snackTimeout"
          :color="globalStore.notification?.type"
        >
          <h3>{{ globalStore.notification?.title }}</h3>
          <p class="text-subtitle-1 pt-2" v-if="globalStore.notification?.text">{{ globalStore.notification.text }}</p>
          <template v-slot:actions>
            <v-btn variant="text" @click="snackbar = false">
              {{ translate('close') }}
            </v-btn>
          </template>
        </v-snackbar>
        <v-layout>
          <SideBar></SideBar>
          <div class="tabs-container" v-if="globalStore.currentPage === 'settings' || !hasProblem">
            <RacingPage v-if="globalStore.currentPage === 'racing'"></RacingPage>
            <ResultsPage v-if="globalStore.currentPage === 'results'"></ResultsPage>
            <MyTracksPage v-if="globalStore.currentPage === 'mytracks'"></MyTracksPage>
            <RacersPage v-if="globalStore.currentPage === 'racers'"></RacersPage>
            <CrewPage v-if="globalStore.currentPage === 'crew'"></CrewPage>
            <SettingsPage v-if="globalStore.currentPage === 'settings'"></SettingsPage>
          </div>
          <div id="revoked-message-container" v-else>
            <div class="revoked-message-holder">
              <v-alert
                :title="hasProblem.title"
                :text="hasProblem.text"
                :color="hasProblem.color ? hasProblem.color : 'error'"
                :icon="hasProblem.icon ? hasProblem.icon : '$error'"
              >
              </v-alert>
              <UserCreation v-if="globalStore.baseData.data.anyoneCanCreate && globalStore.baseData.data.racerNames.length === 0"></UserCreation>
            </div>
          </div>
        </v-layout>
        <CryptoModal></CryptoModal>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
// ... existing script code ...
</script>

<style scoped lang="scss">
.ui-container {
  height: 100vh;
  display: flex;
  justify-content: center;
  width: 100%;
  align-items: center;
  z-index: 2000;
  position: absolute;
  background: linear-gradient(180deg, rgba(5,11,19,0.95) 0%, rgba(10,25,41,0.95) 100%);
}

.screen-container {
  width: 80vw;
  height: 80vh;
  font-family: "Gill Sans", "Gill Sans MT", Calibri, "Trebuchet MS", sans-serif;
  border: 1px solid rgba(255, 255, 255, 0.1);
  background: $background-color;
  padding: 1em;
  border-radius: 1em;
  box-shadow: 0 8px 32px rgba(0, 0, 0, 0.3);
  backdrop-filter: blur(10px);
}

.app-container {
  background: $background-color;
  overflow-y: hidden;
  overflow-x: hidden;
  position: relative;
  display: flex;
  height: calc(100% - 2.6em);
  border-radius: 0.5em;
}

.tabs-container {
  width: 100%;
  margin-left: 56px;
  background: $background-color-lighter;
  border-radius: 0.5em;
}

::-webkit-scrollbar {
  width: 8px;
  background: $background-color-lighter;
}

::-webkit-scrollbar-thumb {
  background: $secondary-color-light;
  border-radius: 4px;
}

::-webkit-scrollbar-thumb:hover {
  background: $primary-color;
}

// ... rest of existing styles ...
</style>