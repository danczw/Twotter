<template>
    <div class='user-profile'>
        <div class='user-profile__sidebar'>
            <div class='user-profile__user-panel'>
                <h1 class='user-profile__username'>@{{ state.user.username }}</h1>
                <div class='user-profile__admin-badge' v-if='state.user.isAdmin'>
                    Admin
                </div>
                <div class='user-profile__follower-count'>
                    <strong>Follower: </strong> {{ state.followers }}
                </div>
            </div>
            <CreateTwootPanel @add-twoot='addTwoot'/>
        </div>
        <div class='user-profile__twotts-wrapper'>
            <TwootItem
                v-for='twoot in state.user.twoots'
                :key='twoot.id'
                :username='state.user.username'
                :twoot='twoot'
            />
        </div>
    </div>
</template>

<script>
import { reactive } from 'vue';
import TwootItem from './TwootItem';
import CreateTwootPanel from './CreateTwootPanel'

export default {
    name: 'UserProfile',
    components: { CreateTwootPanel, TwootItem },
    setup() {
        const state = reactive({
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
        })
    
        function addTwoot(twoot) {
            state.user.twoots.unshift({ id: state.user.twoots.lenght + 1, content: twoot});
        }

        return {
            state,
            addTwoot
        }
    }
}
</script>

<style lang='scss' scoped>
.user-profile {
    display: grid;
    grid-template-columns: 1fr 3fr;
    grid-gap: 50px;
    padding: 50px 5%;

    .user-profile__user-panel {
        display: flex;
        flex-direction: column;
        padding: 20px;
        background-color: white;
        border-radius: 5px;
        border: 1px solid #2c3e50;
        margin-bottom:  auto;

        h1 {
            margin: 0;
        }

        .user-profile__admin-badge {
            background: darkcyan;
            color: white;
            border-radius: 5px;
            margin: 5px auto 5px 5px;
            padding: 1px 10px;
            font-weight: bold;
        }
    }

    .user-profile__twoots-wrapper {
        display: grid;
        grid-gap: 10px;
        margin-bottom: auto;
    }
}
</style>
