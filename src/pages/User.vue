<template>
  <div>

    <div class="row">
      <div class="col-12">
          <div class="col-12">
            <card>
              <div>
                <form @submit.prevent="login">
                  <input type="text" v-model="username" placeholder="Username" />
                  <input type="password" v-model="password" placeholder="Password" />
                  <button type="submit">Login</button>
                </form>
                <p v-if="loginError">Incorrect username or password!</p>
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
