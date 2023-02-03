<template>
    <div class="user-profile">
        <div class="user-profile__user-panel">
            <h1 class="user-profile__username">@{{ user.username }}</h1>
            <div class="default-badge-style admin-badge" v-if="user.accountType === 0">Admin</div>
            <div class="default-badge-style validator-badge" v-else-if="user.accountType === 1">Validator</div>
            <div class="default-badge-style encoder-badge" v-else-if="user.accountType === 2">Encoder</div>
            <div class="default-badge-style viewer-badge" v-else>Viewer</div>
            <div class="follower-count">
                <strong>Followers: </strong> {{ followers }}
            </div>
            <form class="user-profile__create-twoot">
                <label for="newTwoot"><strong>New Twoot</strong></label>
                <textarea id="newTwoot" rows="4"/>

                <div class="user-profile__create-twoot-type">
                    <label for="newTwootType"><strong>Type: </strong></label>
                    <select id="newTwootType">
                        <option :value="option.value" v-for="(option, index) in twootTypes" :key="index">
                            {{ option.name }}
                        </option>
                    </select>
                </div>
            </form>
        </div>
        <div class="user-profile__twoots-wrapper">
            <TwootItem 
                v-for="twoot in user.twoots" 
                :key="twoot.id" 
                :username="user.username" 
                :twoot="twoot" 
                @favourite="toggleFavourite"
            />
        </div>
    </div>
</template>

<script>
  import TwootItem from "./TwootItem";

  export default {
    name: 'UserProflie',
    components: { TwootItem },
    data() {
      return {
        twootTypes: [
            { value: 'draft', name: 'Draft' },
            { value: 'instant', name: 'Instant Twoot' },
        ],
        followers: 0,
        user: {
          id: 1,
          username: '_TheHunter',
          firstName: 'Hunter Kit',
          lastName: 'Sabio',
          email: 'sampleEmail@gmail.com',
          accountType: 0,
          twoots: [
            { id: 1, content: "Twotter is Amazing!" },
            { id: 2, content: "Don't forget to subscribe to The Earth is Square!" }
          ]
        }
      }
    },
    watch: {
      followers(newFollowerCount, oldFollowerCount) {
        if (oldFollowerCount < newFollowerCount) {
          console.log(`${this.user.username} has gained a follower!`)
        }
      }
    },
    computed: {
      fullName() {
        return `${this.user.firstName} ${this.user.lastName}`;
      }
    },
    methods: {
      followUser() {
        this.followers ++
      },
      toggleFavourite(id){
        console.log(`Favourited Tweet ${id}`)
      }
    },
    mounted() {
      //this.followUser();
    }
  }
</script>

<style>
.user-profile {
    display: grid;
    grid-template-columns: 1fr 3fr;
    grid-gap: 50px;
    padding: 50px 5%;
}

.user-profile__user-panel {
    display: flex;
    flex-direction: column;
    padding: 20px;
    background-color: white;
    border-radius: 5px;
    border: 1px solid #DFE3E8;
}

.default-badge-style {
    color: white;
    border-radius: 5px;
    margin-right: auto;
    padding: 5px 10px;
    font-weight: bold;
}

.admin-badge {
    background: rebeccapurple;
}

.validator-badge {
    background: red;
}

.encoder-badge {
    background: blue;
}

.viewer-badge {
    background: green;
}

h1 {
    margin: 0;
}

.user-profile__twoots-wrapper {
    display: grid;
    grid-gap: 10px;
}

.user-profile__create-twoot {
    padding-top: 20px;
    display: flex;
    flex-direction: column;
}
</style>