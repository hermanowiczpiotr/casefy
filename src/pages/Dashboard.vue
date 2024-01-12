<template>
  <div>

    <div class="row">
      <div class="col-12">
          <div class="col-12">
            <card>
              <h5 slot="header" class="title">Generate judgment AI opinion</h5>
              <div class="row">
                <div class="col-12">
                  <base-input
                    label="Judge Name"
                    placeholder="Judge Name"
                    v-model="judgeName"
                    @keyup.enter="fetchJudgments"
                  >
                  </base-input>
                  <base-button  @click="fetchJudgments" slot="footer" type="primary" fill>Generate</base-button>
                  <br><br><br>
                </div>

              </div>

              <div class="row">
                <div class="col-12">
                  <div v-if="isLoading" class="loading-bar"></div>
                  <div class="typography-line" v-if="judgments">
                    <h2>
                    Analiza pracy sędziego {{judgeName}}</h2>

                    <br><br><br>
                    <div v-for="(caseItem, index) in judgments" :key="index">
                      <h3>Case Number: {{ caseItem.case_nr }}</h3>
                      <p v-html="highlightText(caseItem.opinion)"></p><br><br><br><br>
                    </div>
                  </div>
                </div>
              </div>
            </card>
          </div>
      </div>
    </div>
  </div>
</template>
<script>

  import axios from 'axios';

  export default {
    data() {
      return {
        judgments: [],
        judgeName: '',
        isLoading: false,
      }
    },
    computed: {
      enableRTL() {
        return this.$route.query.enableRTL;
      },
      isRTL() {
        return this.$rtl.isRTL;
      },
    },
    methods: {
      async fetchJudgments() {
        this.isLoading = true;
        this.judgments = null
        try {
          const encodedJudgeName = encodeURIComponent(this.judgeName);
          const apiBaseUrl = process.env.API_BASE_URL || 'http://localhost:8001';
          const response = await axios.get(`${apiBaseUrl}/api/judgments?judgeName=${encodedJudgeName}`);          if (response.data === "") {
            this.judgments = "nie znaleziono orzeczeń sądu dla " + this.judgeName
          } else {
            this.judgments = response.data;
          }
        } catch (error) {
          console.error(error);
        } finally {
          this.isLoading = false;
        }
      },
      highlightText(text) {
        return text.replace(/\*\*(.*?)\*\*/g, '<h4><b>$1</b></h4>');
      }
    },
    mounted() {
      this.i18n = this.$i18n;
      if (this.enableRTL) {
        this.i18n.locale = 'ar';
        this.$rtl.enableRTL();
      }
      this.initBigChart(0);
    },
    beforeDestroy() {
      if (this.$rtl.isRTL) {
        this.i18n.locale = 'en';
        this.$rtl.disableRTL();
      }
    }
  };
</script>
<style>
/* Loading Bar Styles */
.loading-bar {
  width: 80%;
  max-width: 600px;
  height: 20px;
  background-color: #ddd;
  margin-top: 20px;
  border-radius: 10px;
  position: relative;
  overflow: hidden;
}

.loading-bar::before {
  content: '';
  position: absolute;
  top: 0;
  left: 0;
  width: 50%;
  height: 100%;
  background: linear-gradient(to right, rgba(255, 255, 255, 0),#2dce89, rgba(255, 255, 255, 0));
  animation: loading 2s infinite;
}

@keyframes loading {
  0% {
    left: -50%;
  }
  100% {
    left: 100%;
  }
}

pre {
  white-space: pre-wrap; /* Ensure preformatted text wraps */
}

.typography-line {
  padding-left: 0;
}

</style>
