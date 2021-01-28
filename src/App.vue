<template>
  <div id="app">
    <Header />
    <div class="board">
      <div class="lane" v-for="(list, index) in cards" :key="index">
        <h2 class="lane-title">{{list.title}}</h2>
        <Container
          group-name="list"
          @drag-start="handleDragStart(list.title, $event)"
          @drop="handleDrop(list.title, $event)"
          :get-child-payload="getChildPayload"
          :drop-placeholder="{className: 'placeholder'}"
        >
          <Draggable v-for="card in list.cards" :key="card.id">
            <Card>{{card.text}}</Card>
          </Draggable>
        </Container>
      </div>
    </div>
  </div>
</template>

<script>

import Header from './components/Header';
import Card from './components/Card';
import initalCards from "./initalCards";
import {Container, Draggable} from "vue-smooth-dnd";
import collect from 'collect.js';

export default {
  name: 'App',
  components: {
    Header,
    Card,
    Container,
    Draggable
  },
  data: () => ({
    cards: collect([
      {title: 'Backlog', cards: initalCards.backlog},
      {title: 'Desenvolvimento', cards: initalCards.dev},
      {title: 'Testes', cards: initalCards.test},
      {title: 'Concluído', cards: []},
    ]),
    draggingCard: {
      lane: '',
      index: -1,
      cardData: {},
    }
  }),
  methods: {
    handleDragStart(lane, dragResult){
      const { payload, isSource } = dragResult;
      if(isSource){
        this.draggingCard = {
          index: payload,
          cardData: {
            ...this.cards.where('title', lane).first().cards[payload]
          }
        }
      }
    },
    handleDrop(lane, dropResult){
      const {removedIndex, addedIndex } = dropResult;
      let list = this.cards.where('title', lane).first().cards;
      if(lane === this.draggingCard.lane && removedIndex === addedIndex){
        return;
      }
      if(list){
        if(removedIndex !== null){
          list.splice(removedIndex, 1);
        }
        if(addedIndex !== null){
          list.splice(addedIndex, 0, this.draggingCard.cardData)
        }
      }
    },
    getChildPayload(index){
      return index;
    }
  }
}
</script>

<style>
.board{
  display: flex;
  margin: 1.2rem .8rem;
  align-items: flex-start;
}

.lane{
  background: var(--color-grey);
  width: 23rem;
  border-radius: .8rem;
  box-shadow: 0 .1rem .2rem 0 rgba(33, 33, 33, 0.1);
  padding: 0 .7rem;
  margin: 0 .8rem
}

.lane-title{
  padding: .8rem .5rem;
  margin-bottom: .6rem;
}

.placeholder {
  background: rgba(33, 33, 33, .08);
  border-radius: .4rem;
  transform: scaleY(0.85);
  transform-origin: 0% 0%;
}

</style>
