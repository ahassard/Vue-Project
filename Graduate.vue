<template>
  <div class="column">
    <div class="columns is-multiline is-mobile">
      <div class="column is-full">
        <div class="column">
          <h1 class="has-text-weight-semibold has-text-centered"
          style="font-size: 24px;">Graduate Next Steps
          </h1>
        </div>
      </div>
      <div class="column is-full">
        <div class="column is-full">
          <b-table
            :data="getCandidates"
            :paginated="isPaginated"
            :per-page="perPage"
            :default-sort="defaultSort"
            :current-page.sync="currentPage"
            :pagination-position="paginationPosition"
            :default-sort-direction="defaultSortDirection"
            :sort-icon="sortIcon"
            :sort-icon-size="sortIconSize"
            :striped="isStriped"
            :bordered="isBordered"
            checkable
            :checked-rows.sync="currentSelectedCandidates"
            :row-class="(row, index) => 'tall-row'"
            aria-next-label="Next page"
            aria-previous-label="Previous page"
            aria-page-label="Page"
            aria-current-label="Current page"
          >
            <template slot-scope="props">
              <b-table-column field="lastName" label="Last Name" sortable>
                <span class="subtitle">{{ props.row.lastName }}</span>
              </b-table-column>
              <b-table-column field="firstName" label="First Name" sortable>
                <span class="subtitle">{{ props.row.firstName }}</span>
              </b-table-column>
              <b-table-column field="middleInitial" label="MN" width="20">
                <span class="subtitle">{{ props.row.middleInitial }}</span>
              </b-table-column>
              <b-table-column field="mos" label="MOS">
                <span class="subtitle">
                  {{ props.row.militaryProfile.edges[0].node.mos.mosCode }}</span>
              </b-table-column>
               <b-table-column label="Graduation Date" field="gradDate" centered>
              <!--will need populated with "props.row.graduationInfo.edges[0].node.gradDate" -->
              <input
               v-model="graduationInfo.gradDate"
                class="input event-input"
                v-on:keypress="isNumber($event)"
                min="0"
                max="500"
                type="date"
              />
            </b-table-column>
            <b-table-column label="Airborne">
              <toggle-button
                  v-model="props.row.militaryProfile.edges[0].node.airborne"
                  :labels="{checked: 'Yes', unchecked: 'No'}"
                  :color="toggleBtn.color"
                  :sync="false"
                  :width="toggleBtn.width"
                  :height="toggleBtn.height"
                  :font-size="toggleBtn.fontSize"/>
            </b-table-column>
            <!--will need populated with "props.row.militaryProfile.edges[0].node.medic" -->
            <b-table-column label="Medic">
              <toggle-button
                  :v-model="graduationInfo.medic"
                  :labels="{checked: 'Yes', unchecked: 'No'}"
                  :color="toggleBtn.color"
                  :sync="false"
                  :width="toggleBtn.width"
                  :height="toggleBtn.height"
                  :font-size="toggleBtn.fontSize"/>
            </b-table-column>
            </template>
            <template slot="empty">
              <section class="section">
                <div class="content is-centered has-text-grey has-text-centered">
                  <center><div
                  class="image is-128x128 loading-spin-icon"
                  alt="loading-spin-icon"
                  >
                  </div>
                  </center>
                </div>
              </section>
            </template>
          </b-table>
        </div>
      </div>
    </div>
    <div class="padder"></div>
    <div class="fixed-palette">
      <button class="button is-medium is-apple-green"
        @click="checkInfo()">
        <span class="is-size-5 is-size-6-touch">Save</span>
      </button>
   </div>
  </div>
</template>
<script>
import EventBus from '@/assets/models/eventBus';
import getGraduateCandidatesQL from '@/assets/graphql/getGraduateCandidates.gql';
import createGraduationGQL from '@/assets/graphql/createGraduation.gql';
import updateGraduationGQL from '@/assets/graphql/updateGraduation.gql';
import GraduationData from '@/assets/models/graduationModel';


export default {
  name: 'Graduation',
  components: {
  },
  props: {
  },
  watch: {
  },
  computed: {
  },
  mounted() {
    EventBus.$on('graduation-save', (graduationData) => {
      this.processGraduationSave(graduationData);
    });
  },
  beforeDestroy() {
  },
  data() {
    return {
      toggleBtn: {
        color: '#81C926',
        width: 199,
        height: 36,
        fontSize: 20,
      },
      currentSelectedCandidates: [],
      availableCandidates: [],
      activeCandidates: this.$store.state.selectedCandidates,
      openSelectCandidates: false,
      selectedChoice: '',
      choiceName: '',
      tempUser: '',
      isPaginated: true,
      isStriped: true,
      isBordered: true,
      defaultSort: ['lastName', 'asc'],
      paginationPosition: 'bottom',
      defaultSortDirection: 'asc',
      sortIcon: 'arrow-up',
      sortIconSize: 'is-small',
      currentPage: 1,
      perPage: 15,
      getCandidates: [],
      graduationInfo: [], /* will remove once back-end popluated */
    };
  },
  apollo: {
    getCandidates: {
      query: getGraduateCandidatesQL,
    },
  },
  methods: {
    newGraduate() {
      this.selectedGraduate = new GraduationData();
      this.isUpdate = false;
    },
    createGraduation(val) {
      this.$apollo.mutate({
        mutation: createGraduationGQL,
        variables: {
          gradId: val.gradId,
          brandiId: val.brandiId,
          gradDate: val.gradDate,
        },
      }).then(EventBus.$emit('candidate-update'));
    },
    updateGraduation(val) {
      this.$apollo.mutate({
        mutation: updateGraduationGQL,
        variables: {
          gradId: val.gradId,
          brandiId: val.brandiId,
          gradDate: val.gradDate,
        },
      }).then(EventBus.$emit('candidate-update'));
    },
    processGraduationSave(val) {
      if (this.isUpdate) {
        this.updateGraduation(val);
      } else {
        this.createGraduation(val);
      }
      this.newGraduate();
    },
    update(val) {
      const retVal = Object.assign({}, val);
      this.isUpdate = true;
      EventBus.$emit('graduation-save', retVal);
    },
    checkInfo() {
      if (!this.graduationInfo.gradDate) { /* will need to be changed
      once back-end data available */
        this.$buefy.toast.open({
          message: 'All fields are required',
          type: 'is-danger',
        });
      } else {
        this.saveGraduation();
      }
    },
    saveGraduation() {
      const retVal = Object.assign({}, this.currentInput);
      EventBus.$emit('graduation-save', retVal);
      this.$buefy.toast.open({
        message: 'Saved Graduate Data Successfuly',
        type: 'is-success',
      });
    },
  },
};
</script>
<style lang="scss" scoped>
#test{
  border: 2px;
  margin: 10%;
}
</style>
