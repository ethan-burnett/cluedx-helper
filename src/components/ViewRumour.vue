<template>
    <div>
        <b-row>
            <b-col>
                <p v-if="falsifier === null">Rumour by <strong
                        :style="'background-color: ' + caster.character.colour">{{ caster.name }}
                </strong>, no one answered.</p>
                <p v-else>Rumour by <strong
                        :style="'background-color: ' + caster.character.colour">{{ caster.name }}
                </strong>, answered by <strong
                        :style="'background-color: ' + falsifier.character.colour">{{ falsifier.name }}
                </strong>.</p>
            </b-col>
            <b-col cols="1">
                <b-button variant="danger" pill size="sm" @click="removeRumour(rumour.id)"><b-icon-x/></b-button>
            </b-col>
        </b-row>
        <b-row>
            <b-col>
                <cluedx-card :players="players" :card="findCard(rumour.cards[0], table.who)"></cluedx-card>
            </b-col>
            <b-col>
                <cluedx-card :players="players" :card="findCard(rumour.cards[1], table.what)"></cluedx-card>
            </b-col>
            <b-col>
                <cluedx-card :players="players" :card="findCard(rumour.cards[2], table.where)"></cluedx-card>
            </b-col>
        </b-row>
    </div>
</template>

<script>
    import {characters} from "@/constants";
    import CluedxCard from "@/components/CluedxCard";

    export default {
        name: "ViewRumour",
        components: {CluedxCard},
        props: {
            table: Object,
            players: Array,
            rumour: Object
        },
        data() {
            return {
                who: {
                    name: '',
                    id: null,
                    isTheOne: false,
                    isFound: false,
                    players: []
                },
                what: {
                    name: '',
                    id: null,
                    isTheOne: false,
                    isFound: false,
                    players: []
                },
                where: {
                    name: '',
                    id: null,
                    isTheOne: false,
                    isFound: false,
                    players: []
                },
                caster: {
                    name: '',
                    position: 1,
                    isMe: false,
                    character: characters[0].value,
                    cards: []
                },
                falsifier: null,
            }
        },
        mounted() {
            this.who = this.findCard(this.rumour.cards[0], this.table.who);
            this.what = this.findCard(this.rumour.cards[1], this.table.what);
            this.where = this.findCard(this.rumour.cards[2], this.table.where);
            this.caster = this.players[this.rumour.playerPosition - 1];
            if (this.rumour.playerWhoAnswered > 0) {
                this.falsifier = this.players[this.rumour.playerWhoAnswered - 1];
            }
        },
        methods: {
            /**
             * Finds and returns the card with the given ID in the list of cards.
             * @param cardId the card's ID
             * @param cards a list of cards with IDs
             * @returns the card with the given ID
             */
            findCard(cardId, cards) {
                for (let i = 0; i < cards.length; i++) {
                    if (cardId === cards[i].id) {
                        return cards[i];
                    }
                }
            },
            removeRumour(id) {
                this.$emit("remove-rumour", id);
            }
        }
    }
</script>

<style scoped>

</style>