<template>
  <ol class="announcements">
    <li class="announcement" v-for="announcement in activeAnnouncements">
      <header>{{ announcement.title }}</header>
      <div class="content">{{ announcement.content }}</div>
      <router-link v-for="action in announcement.actions" :to="action.href">
        {{ action.title }}
      </router-link>
      <button @click="dismiss(announcement)">dismiss</button>
    </li>
  </ol>
</template>

<script>
export default {
  name: 'Announcements',
  data () {
    return {
      announcements: [],
    }
  },
  mounted () {
    const announcements = this.$page.frontmatter.announcements || []

    // copy all announcements and extend them
    announcements.forEach(announcement => {
      const newAnnouncement = { ...announcement, dismissed: false }
      this.announcements.push(newAnnouncement)
    })
  },
  computed: {
    activeAnnouncements () {
      const announcements = this.announcements
      if (announcements.length === 0) return []
      return announcements.filter(this.isActive)
    },
  },
  methods: {
    isActive (announcement) {
      if (announcement.dismissed) return false
      const now = Date.now()
      const startDate = Date.parse(announcement.activeFrom)
      const endDate = Date.parse(announcement.activeTill)
      return startDate <= now && endDate > now
    },
    dismiss (announcement) {
      announcement.dismissed = true
      // TODO: set a flag in localStorage somehow?
    },
  },
}
</script>

<style scoped>
.announcements {
  position: absolute;
  top: 60px;
  left: 320px;
  display: block;
  width: calc(100vw - 340px);
  margin: 0;
  padding: 0;
  list-style: none;
}

.announcement {
  display: flex;
  flex-flow: row nowrap;
  justify-content: space-between;
  align-items: center;
  min-height: 4em;
  margin-bottom: .5em;
  background: #EEE;
  box-shadow: 2px 2px 2px black;
}

.announcement > header {
  margin: 0 1em;
  font-weight: bold;
}
.announcement > .content {
  flex: 1 1 50%;
}
.announcement > button {
  margin: 0 1em;
  padding: .5em;
  opacity: .7;
}

@media screen and (max-width: 705px) {
  .announcement {
    width: 95vw;
    left: 2vw;
  }
}

.fade-enter-active, .fade-leave-active { transition: opacity .2s; }
.fade-enter, .fade-leave-to { opacity: 0; }
</style>
