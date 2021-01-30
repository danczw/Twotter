<template>
  <div class='user-profile'>
    <div class='user-profile__user-panel'>
        <h1 class='user-profile__username'>@{{ user.username }}</h1>
        <div class='user-profile__admin-badge' v-if='user.isAdmin'>
            admin
        </div>
        <div class='user-profile__follower-count'>
            <strong>Followers: </strong> {{ followers }}
        </div>
        <form class='user-profile__create-twoot' @submit.prevent='createNewTwoot' :class='{ "--exceeded": newTwootCharacterCount > 180 }'>
            <label for='newTwoot'>
                <strong>New Twoot</strong>
                ({{ newTwootCharacterCount }}/180)
            </label>
            <textarea id='newTwoot' rows='4' v-model='newTwootContent'></textarea>
            
            <div class='user-profile__create-twoot-type'>
                <label for='newTwootType'><strong>
                    Type: 
                </strong></label>
                <select id='newTwootType' v-model='selectedTwootType'>
                    <option :value='option.value' v-for='(option, index) in twootTypes' :key='index'>
                        {{ option.name }}
                    </option>
                </select>
            </div>

            <div>
                <button>
                    Twoot!
                </button>
            </div>
        </form>
    </div>
    <div class='user-profile__twoots-wrapper'>
        <TwootItem
            v-for='twoot in user.twoots'
            :key='twoot.id'
            :username='user.username'
            :twoot='twoot'
            @favourite='toggleFavourite'
        />
    </div>
  </div>
</template>

<script>
import TwootItem from './TwootItem';

export default {
    name: 'App',
    components: { TwootItem },
    data() {
        return {
            newTwootContent: '',
            selectedTwootType: 'instant',
            twootTypes: [
                { value: 'draft', name: 'Draft'},
                { value: 'instant', name: 'Instant Twoot'}
            ],
            followers: 0,
            user: {
                id: 1,
                username: 'dancz',
                firstName: 'Daniel',
                lastName: 'Czw',
                email: 'me@you.com',
                isAdmin: true,
                twoots: [
                    { id: 1, content: 'Twotter is fantastic!'},
                    { id: 2, content: 'Do not try this at home.'},
                    { id: 3, content: 'How many twoots can you write?'},
                    { id: 4, content: 'This is the last one, promise.'},
                    { id: 5, content: 'Bye! :)'}
                ]
            }
        }
    },
    watch: {
        followers(newFollowerCount, oldFollowerCount) {
            if (oldFollowerCount < newFollowerCount) {
                console.log(`${this.user.username} has gained a follower`)
            }
        }
    },
    computed: {
        newTwootCharacterCount() {
            return this.newTwootContent.length;
        }
    },
    methods: {
        followUser() {
            this.followers++
        },
        toggleFavourite(id) {
            console.log(`Favourited Twoot #${id}`)
        },
        createNewTwoot() {
            if (this.newTwootContent && this.selectedTwootType !== 'draft') {
                this.user.twoots.unshift( {
                    id: this.user.twoots.length + 1,
                    content: this.newTwootContent
                })
                this.newTwootContent = ''
            }
        }
    },
    mounted() {
        this.followUser();
    }
}
</script>

<style lang='scss' scoped>
.user-profile {
    display: grid;
    grid-template-columns: 1fr 3fr;
    width: 100%;
    padding: 50px 5%;

    .user-profile__user-panel {
        display: flex;
        flex-direction: column;
        margin-right: 50px;
        padding: 20px;
        background-color: white;
        border-radius: 5px;
        border: 1px solid #dfe3e8;
    
        h1 {
            margin: 0;
        }

        .user-profile__admin-badge {
            background: cadetblue;
            color: white;
            border-radius: 5px;
            margin-right: auto;
            padding: 0 10px;
            font-weight: bold;
        }

        .user-profile__follower-count {
            padding-bottom: 10px;
        }

        .user-profile__create-twoot {
            border-top: 1px solid #dfe3e8;
            padding-top: 10px;
            display: flex;
            flex-direction: column;

            &.--exceeded {
                color: rgb(189, 0, 0);
                border-color: rgb(189, 0, 0);

                button {
                    background-color: rgb(54, 54, 54);
                    color: lightgray;
                    
                }
            }

            .user-profile__create-twoot-type {
                padding: 5px;
            }

            button {
                padding: 5px;
            }
        }
    }

    .user-profile__twoots-wrapper {
        display: grid;
        grid-gap: 10px;
    }
}

</style>
