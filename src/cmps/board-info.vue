<template>
  <div v-if="board" class="board-info flex justify-center align-center">
    <div class="btn-members-container">
      <members
        :hiddenBadge="membersLegnth"
        :size="35"
        toolTipTxt="Board Members"
        classIcon="icon-nav-hader"
        :members="board.members"
      >
        <add-members
          slot="add-members"
          class="top-right"
          @removeMember="emitRemoveMember"
          @addMember="emitAddMember"
          firstTitle="Board Members"
          secondTitle="Site Users"
          :members="board.members"
          :allMembers="filteredUsers"
        />
      </members>
    </div>
    <div class="btn-booard-activity-container">
      <button
        v-tooltip.top="'Board activity'"
        class="activity-btn btn-close btn-second "
        @click="boardActivities"
      >
        Activities /
        <span v-if="activiitesLength" class="blue">{{ activiitesLength }}</span>
      </button>
      <i class="fas fa-chart-line activity-mobile" @click="boardActivities"><span v-if="activiitesLength" class="activity-txt-mobile "> / <span class="blue">{{ activiitesLength }}</span></span></i>
    </div>
  </div>
</template>

<script>
import members from './members'
import addMembers from './add-members'
export default {
  props: {
    board: Object
  },
  data() {
    return {
      isMembersShown: false,
      darkMode: false,
      isBoardActivityShowen: false
    }
  },
  computed: {
    activiitesLength() {
      const boardActiviites = this.$store.getters.boardActivities
      if (!boardActiviites.length || boardActiviites.length <= 0) return '0'
      return boardActiviites.length
    },
    membersLegnth() {
      return this.board.members.length > 3 ? false : true
    },
    filteredUsers() {
      const users = this.$store.getters.miniUsers
      const boardMembers = this.board.members
      const filteredUsers = users.filter(user => {
        return boardMembers.every(boardMember => {
          return boardMember._id !== user._id
        })
      })
      return filteredUsers
    }
  },
  methods: {
    boardActivities() {
      this.isBoardActivityShowen = true
      this.$router.push(`/board/${this.$route.params.boardId}/activities`)
    },
    emitRemoveMember(member) {
      this.$emit('removeMember', member)
    },
    emitAddMember(member) {
      this.$emit('addMember', member)
    },
    toggleMembersList() {
      this.isMembersShown = !this.isMembersShown
    }
  },

  components: {
    addMembers,
    members,
  }
}
</script>

<style></style>
