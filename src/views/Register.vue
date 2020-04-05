<template>
 <form className="mt-3" @submit.prevent="register">
        <div className="container">
          <div className="row justify-content-center">
            <div className="col-lg-8">
              <div className="card bg-light">
                <div className="card-body">
                  <h3 className="font-weight-light mb-3">Register</h3>
                  <div className="form-row">
                  <div class="col-12 alert alert-danger px-3" v-if="error">
                   {{error}}
                  </div>
                    <section className="col-sm-12 form-group">
                      <label
                        className="form-control-label sr-only"
                        htmlFor="displayName"
                      >
                            Display Name
                          </label>
                      <input
                        className="form-control"
                        type="text"
                        id="displayName"
                        placeholder="Display Name"
                        name="displayName"
                        v-model="displayName"
                        required
                      />
                    </section>
                  </div>
                  <section className="form-group">
                    <label
                      className="form-control-label sr-only"
                      htmlFor="email"
                    >
                      Email
                    </label>
                    <input
                      className="form-control"
                      type="email"
                      id="email"
                      placeholder="Email Address"
                      required
                      name="email"
                      v-model="email"
                    />
                  </section>
                  <div className="form-row">
                    <section className="col-sm-6 form-group">
                      <input
                        className="form-control"
                        type="password"
                        name="passOne"
                        placeholder="Password"
                        v-model="passOne"
                      />
                    </section>
                    <section className="col-sm-6 form-group">
                      <input
                        className="form-control"
                        type="password"
                        required
                        name="passTwo"
                        placeholder="Repeat Password"
                        v-model="passTwo"
                      />
                    </section>
                  </div>
                  <div className="form-group text-right mb-0">
                    <button className="btn btn-primary" type="submit">
                      Register
                    </button>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>
      </form>
</template>

<script>
import Firebase from 'firebase';
export default {
    name: 'Register',
    data: function() {
        return {
            error:null,
            displayName: null,
            email: null,
            passOne: null,
            passTwo: null
        };
    },
    methods: {
        register: function() {
            const info = {
                displayName: this.displayName,
                email: this.email,
                password: this.passOne
            };
            if(!this.error) {
                Firebase.auth()
                .createUserWithEmailAndPassword(info.email, info.password)
                .then (
                    userCredentials => {
                        return userCredentials.user.updateProfile({
                            displayName: info.displayName
                        }).then(() =>{
                            this.$router.replace('meetings');
                        });
                    }, error => {
                        this.error = error.message;
                    }
                );
            }
        }
    },
    watch: {
        passTwo: function() {
            if (this.passOne !== '' &&
                this.passTwo !== '' &&
                this.passOne !== this.passTwo
            ) {
                this.error = "passwords must match";
            } else {
                this.error = null;
            }
        }
    }
};
</script>