<template>
  <div class="modal fade" tabindex="-1">
    <div class="modal-dialog">
      <div class="modal-content">
        <div class="modal-header text-center">
          <div class="container">
            <div class="row">
              <div class="col-md-12">
                <!-- <h3>Hint</h3> -->
                <h3>힌트</h3>
              </div>
            </div>
          </div>
          <button
            type="button"
            class="close"
            data-dismiss="modal"
            aria-label="Close"
          >
            <span aria-hidden="true">&times;</span>
          </button>
        </div>
        <form method="POST" @submit.prevent="submitHint">
          <div class="modal-body">
            <div class="container">
              <div class="row">
                <div class="col-md-12">
                  <div class="form-group">
                    <label class="text-muted">
                      <!-- Hint<br /> -->
                      힌트<br />
                      <!-- <small>Markdown &amp; HTML are supported</small> -->
                      <small>마크다운 &amp; HTML 지원됩니다.</small>
                    </label>
                    <textarea
                      type="text"
                      class="form-control markdown"
                      name="content"
                      rows="7"
                      ref="content"
                    ></textarea>
                  </div>

                  <div class="form-group">
                    <label>
                      <!-- Cost<br /> -->
                      비용<br />
                      <!-- <small>How many points it costs to see your hint.</small> -->
                      <small>힌트를 열람하기 위한 포인트 비용을 설정합니다.</small>
                    </label>
                    <input
                      type="number"
                      class="form-control"
                      name="cost"
                      v-model.lazy="cost"
                    />
                  </div>
                  <input type="hidden" id="hint-id-for-hint" name="id" />
                </div>
              </div>
            </div>
          </div>
          <div class="modal-footer">
            <div class="container">
              <div class="row">
                <div class="col-md-12">
                  <!-- <button class="btn btn-primary float-right">Submit</button> -->
                  <button class="btn btn-primary float-right">적용</button>
                </div>
              </div>
            </div>
          </div>
        </form>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "HintCreationForm",
  props: {
    challenge_id: Number
  },
  data: function() {
    return {
      cost: 0
    };
  },
  methods: {
    getCost: function() {
      return this.cost || 0;
    },
    getContent: function() {
      return this.$refs.content.value;
    },
    submitHint: function() {
      console.log(this.co);
      let params = {
        challenge_id: this.$props.challenge_id,
        content: this.getContent(),
        cost: this.getCost()
      };
      CTFd.fetch("/api/v1/hints", {
        method: "POST",
        credentials: "same-origin",
        headers: {
          Accept: "application/json",
          "Content-Type": "application/json"
        },
        body: JSON.stringify(params)
      })
        .then(response => {
          return response.json();
        })
        .then(response => {
          if (response.success) {
            this.$emit("refreshHints", this.$options.name);
          }
        });
    }
  }
};
</script>

<style scoped></style>
