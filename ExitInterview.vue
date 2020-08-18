<template>
<div class="columns is-multiline is-mobile" id="interviewBody">
        <div class="column is-half">
        <p class="title">Exit Interview</p>
        </div>
      <div class="column is-full">
        <div class="columns">
          <div class="column is-half">
            <div class="field">
              <label class="label">First Name:</label>
              <div class="field-body">
                <div class="field">
                  <p class="control">
                    <input class="input"
                    v-model="selectedData.firstName"
                    v-on:keypress="isLetter($event)"
                    type="text" />
                  </p>
                </div>
              </div>
            </div>
          </div>
          <div class="column is-half">
            <div class="field">
              <label class="label">Last Name</label>
              <div class="field-body">
                <div class="field">
                  <p class="control">
                    <input class="input"
                    v-model="selectedData.lastName"
                    v-on:keypress="isLetter($event)"
                    type="text" />
                  </p>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
      <div class="column is-full">
        <p class="info-paragraph" style="margin-bottom: 30px; line-height: 24px;">
          The following questions are based soley on your
          opinion and perception. There are no incorrect answers and your honest feedback can help
          us improve overall training for Soldiers.</p>
      </div>
      <div class="column is-full">
        <div class="columns">
            <div class="column is-half">
              <label class="label">Drop Reason</label>
              <div class="field-body">
                <div class="field">
                  <div class="control">
                    <multiselect
                      v-model="drop.exitReason"
                      :options="sampleArray.exitReason"
                      track-by="code"
                      label="description"
                      :multiple="false"
                      :close-on-select="true"
                      :clear-on-select="false"
                      placeholder="Select Option"
                      :taggable="false"
                      :showLabels="true"
                    ></multiselect>
                  </div>
                </div>
              </div>
            </div>
            <div class="column is-half">
                <label class="label">Submit Date:</label>
                <div class="field-body">
                <div class="field">
                  <div class="control">
                    <flat-pickr
                      v-model="transitionInfo.ordersSubmitted"
                      :config="pickerConfig"
                    >
                    </flat-pickr>
                  </div>
                </div>
              </div>
            </div>
        </div>
      </div>
      <div class="column is-full">
        <div class="columns">
            <div class="column is-half">
              <label class="label">
                When did you receive your option 40 contract?</label>
              <div class="field-body">
                <div class="field">
                  <div class="control">
                    <multiselect
                      v-model="contract.interview_option40"
                      :options="sampleArray.interview_option40"
                      track-by="code"
                      label="description"
                      :multiple="false"
                      :close-on-select="true"
                      :clear-on-select="false"
                      placeholder="Select Option"
                      :taggable="false"
                      :showLabels="true"
                    ></multiselect>
                  </div>
                </div>
              </div>
            </div>
            <div class="column is-half">
            <div class="field">
              <label class="label">Did you take creatine or protein during RASP 1?</label>
              <div class="field-body">
                <toggle-button
                  v-model="supplements"
                  :labels="{checked: 'Yes', unchecked: 'No'}"
                  :value="false"
                  :color="toggleBtn.color"
                  :sync="true"
                  :width="toggleBtn.width"
                  :height="toggleBtn.height"
                  :font-size="toggleBtn.fontSize"
                />
              </div>
            </div>
          </div>
        </div>
      </div>
      <div style="margin-top: 30px;" v-if="drop.exitReason.code == 'VW'">
      <div class="column is-full">
        <div class="columns">
          <div class="column is-half">
            <label class="label">What are the factors that contributed to you
              deciding to withdraw from the course?</label>
          </div>
          <div class="column is-half">
            <div class="control">
                <textarea class="textarea"
                v-model="remarks1" placeholder="Candidate Remarks"
                type="text" rows="6"></textarea>
            </div>
          </div>
        </div>
      </div>
      <div class="column is-full">
        <div class="columns">
          <div class="column is-half">
            <label class="label">Did you have any outside influence to making
              this decision?</label>
              <div class="field-body">
                <toggle-button
                      v-model="influence"
                      :labels="{checked: 'Yes', unchecked: 'No'}"
                      :value="false"
                      :color="toggleBtn.color"
                      :sync="true"
                      :width="toggleBtn.width"
                      :height="toggleBtn.height"
                      :font-size="toggleBtn.fontSize"
                       />
              </div>
          </div>
          <div class="column is-half">
            <div class="control">
                <textarea class="textarea"
                v-model="outsideInfluence" placeholder="Candidate Remarks"
                type="text" rows="6"></textarea>
            </div>
          </div>
        </div>
      </div>
      <div class="column is-full">
        <div class="columns">
          <div class="column is-half">
            <label class="label">Did you try to quit at basic training or AIT?</label>
            <div class="field-body">
            <toggle-button
                  v-model="otherTraining"
                  :labels="{checked: 'Yes', unchecked: 'No'}"
                  :value="false"
                  :color="toggleBtn.color"
                  :sync="true"
                  :width="toggleBtn.width"
                  :height="toggleBtn.height"
                  :font-size="toggleBtn.fontSize"
                />
                </div>
          </div>
          <div class="column is-half">
            <div class="control">
                <textarea class="textarea"
                v-model="basicBail" placeholder="Candidate Remarks"
                type="text" rows="6"></textarea>
            </div>
          </div>
        </div>
      </div>
      <div class="column is-full">
        <div class="columns">
          <div class="column is-half">
            <label class="label">Did you do poorly or fail any events that led to
              your decision to voluntarily withdrawing?</label>
              <div class="field-body">
                  <toggle-button
                  v-model="performance"
                  :labels="{checked: 'Yes', unchecked: 'No'}"
                  :value="false"
                  :color="toggleBtn.color"
                  :sync="true"
                  :width="toggleBtn.width"
                  :height="toggleBtn.height"
                  :font-size="toggleBtn.fontSize"
                  />
              </div>
          </div>
          <div class="column is-half">
            <div class="control">
                <textarea class="textarea"
                v-model="failures" placeholder="Candidate Remarks"
                type="text" rows="6"></textarea>
            </div>
          </div>
        </div>
      </div>
      </div>
      <div style="margin-top: 30px;" v-if="drop.exitReason.code == 'M'">
      <div class="column is-full">
        <div class="columns">
          <div class="column is-half">
            <label class="label">What injury were you diagnosed with?</label>
          </div>
          <div class="column is-half">
            <div class="control">
                <textarea class="textarea"
                v-model="diagnosis" placeholder="Candidate Remarks"
                type="text" rows="6"></textarea>
            </div>
          </div>
        </div>
      </div>
      <div class="column is-full">
        <div class="columns">
          <div class="column is-half">
            <label class="label">Did your injury occur before joining the military,
              during basic training or AIT, during your time in Pre-RASP, or
              during RASP 1?</label>
          </div>
          <div class="column is-half">
            <div class="control">
                <textarea class="textarea"
                v-model="injuryTime" placeholder="Candidate Remarks"
                type="text" rows="6"></textarea>
            </div>
          </div>
        </div>
      </div>
      </div>
      <div style="margin-top: 30px;" v-if="drop.exitReason.code == 'O'">
      <div class="column is-full">
        <div class="columns">
          <div class="column is-half">
            <label class="label">If you failed a training requirement,
              what factors contributed to that? What additional
              training do you think would have better prepared you?</label>
          </div>
          <div class="column is-half">
            <div class="control">
                <textarea class="textarea"
                v-model="failureFactors" placeholder="Candidate Remarks"
                type="text" rows="6"></textarea>
            </div>
        </div>
      </div>
      </div>
      </div>
<div class="fixed-palette">
      <button
        class="button is-medium is-apple-green"
        @click="confirmSave()">
        <span class="is-size-5 is-size-6-touch">Submit</span>
      </button>
    </div>
</div>
</template>

<script>
import EventBus from '@/assets/models/eventBus';
import mockDD from '@/assets/models/mockDD.json';
import createExitInterviewGQL from '@/assets/graphql/createInterview.gql';
import updateExitInterviewGQL from '@/assets/graphql/updateInterview.gql';
import ExitInterviewData from '@/assets/models/exitInterviewModel';

export default {
  name: 'ExitInterview',
  props: {
    selectedData: {
      type: Object,
    },
    militaryProfile: {
      type: Object,
    },
    personalProfile: {
      type: Object,
    },
    transitionInfo: {
      type: Object,
    },
  },
  mounted() {
    EventBus.$on('exit-interview-save', (exitInterviewData) => {
      this.processInterviewSave(exitInterviewData);
    });
  },
  data() {
    return {
      dropdowns: this.$store.state.fks,
      sampleArray: mockDD,
      pickerConfig: {
        altFormat: 'Y M d',
        altInput: true,
      },
      toggleBtn: {
        color: '#81C926',
        width: 199,
        height: 36,
        fontSize: 20,
      },
      contract: {
        interview_option40: [],
      },
      drop: {
        exitReason: [
        ],
      },
      influence: false,
      otherTraining: false,
      performance: false,
      exitId: '',
      optionFortyCode: '',
      transId: '',
      supplements: false,
      factors: '',
      outsideInfluence: '',
      basicBail: '',
      failures: '',
      diagnosis: '',
      injuryTime: '',
      failureFactors: '',
    };
  },
  methods: {
    newInterview() {
      this.selectedInterview = new ExitInterviewData();
      this.isUpdate = false;
    },
    createExitInterview(val) {
      this.$apollo.mutate({
        mutation: createExitInterviewGQL,
        variables: {
          exitId: val.exitId,
          optionFortyCode: val.optionFortyCode,
          transId: val.transId,
          supplements: val.supplements,
          factors: val.factors,
          outsideInfluence: val.outsideInfluence,
          basicBail: val.basicBail,
          failures: val.failures,
          diagnosis: val.diagnosis,
          injuryTime: val.injuryTime,
          failureFactors: val.failureFactors,
        },
      }).then(EventBus.$emit('candidate-update'));
    },
    updateExitInterview(val) {
      this.$apollo.mutate({
        mutation: updateExitInterviewGQL,
        variables: {
          exitId: val.exitId,
          optionFortyCode: val.optionFortyCode,
          transId: val.transId,
          supplements: val.supplements,
          factors: val.factors,
          outsideInfluence: val.outsideInfluence,
          basicBail: val.basicBail,
          failures: val.failures,
          diagnosis: val.diagnosis,
          injuryTime: val.injuryTime,
          failureFactors: val.failureFactors,
        },
      }).then(EventBus.$emit('candidate-update'));
    },
    processInterviewSave(val) {
      if (this.isUpdate) {
        this.updateExitInterview(val);
      } else {
        this.createExitInterview(val);
      }
      this.newInterview();
    },
    update(val) {
      const retVal = Object.assign({}, val);
      this.isUpdate = true;
      EventBus.$emit('exit-interview-save', retVal);
    },
    saveExitInterview() {
      const retVal = Object.assign({}, this.selectedData + this.drop + this.transitionInfo
      + this.contract);
      this.$buefy.toast.open({
        message: 'Data Saved To Local Storage',
        type: 'is-info',
      });
      console.log(this.diagnosis); //* verify return of input from diagnosis question*/
      EventBus.$emit('exit-interview-save', retVal);
    },
    confirmSave() {
      if (!this.selectedData.lastName
      || !this.selectedData.firstName
      || !this.drop.exitReason
      || !this.transitionInfo.ordersSubmitted
      || !this.contract.interview_option40) {
        this.$buefy.toast.open({
          message: 'All fields are required',
          type: 'is-danger',
        });
      } else {
        this.saveExitInterview();
      }
    },
  },
};
</script>

<style scoped>
#interviewBody {
  margin: 15px;
}
.info-paragraph {
  color: red;
  font-style: oblique;
  text-align: center;
  font-size: 1.25em;
  margin: 30px 20px 30px 20px;
}
</style>
