<template>
  <div class="row">
    <div class="col-md-8">
      <card>
        <h5 slot="header" class="title">Generate judgment AI opinion</h5>
        <div class="row">
          <div class="col-md-6 pr-md-1">
            <base-input
              label="First Name"
              v-model="model.firstName"
              placeholder="First Name"
            >
            </base-input>
          </div>
        </div>
        <div class="row">
          <div class="col-md-8">
            <base-input>
              <label>About Me</label>
              <textarea
                rows="4"
                cols="80"
                class="form-control"
                placeholder="Here can be your description"
                v-model="model.about"
              >
          </textarea>
            </base-input>
          </div>
        </div>
        <base-button slot="footer" type="primary" fill>Save</base-button>
      </card>
    </div>
    <div class="col-md-4">
      <user-card :user="user"></user-card>
    </div>
  </div>
</template>
<script>
  import EditProfileForm from './Profile/EditProfileForm';
  import UserCard from './Profile/UserCard'
  export default {
    components: {
      EditProfileForm,
      UserCard
    },
    data() {
      return {
        model: {
          company: 'Creative Code Inc.',
          email: 'mike@email.com',
          username: 'michael23',
          firstName: 'Mike',
          lastName: 'Andrew',
          address: 'Bld Mihail Kogalniceanu, nr. 8 Bl 1, Sc 1, Ap 09',
          city: 'Melbourne',
          country: 'Australia',
          about: 'Lamborghini Mercy, Your chick she so thirsty, I\'m in that two seat Lambo.'
        },
        user: {
          fullName: 'Mike Andrew',
          title: 'Ceo/Co-Founder',
          description: `Do not be scared of the truth because we need to restart the human foundation in truth And I love you like Kanye loves Kanye I love Rick Owens’ bed design but the back is...`,
        }
      }
    }
  }
</script>
<style>
</style>
