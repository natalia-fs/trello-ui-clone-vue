<template>
  <div id="app">
    <Header />
    <div class="board">
      <div class="lane">
        <h2 class="lane-title">Backlog</h2>
        <Container
          group-name="list"
          @drag-start="handleDragStart('backlog', $event)"
          @drop="handleDrop('backlog', $event)"
          :get-child-payload="getChildPayload"
          :drop-placeholder="{className: 'placeholder'}"
        >
          <Draggable v-for="card in cards.backlog" :key="card.id">
            <Card>{{card.text}}</Card>
          </Draggable>
        </Container>
      </div>
      <div class="lane">
        <h2 class="lane-title">Desenvolvimento</h2>
        <Container
          group-name="list"
          @drag-start="handleDragStart('dev', $event)"
          @drop="handleDrop('dev', $event)"
          :get-child-payload="getChildPayload"
          :drop-placeholder="{className: 'placeholder'}"
        >
          <Draggable v-for="card in cards.dev" :key="card.id">
            <Card>{{card.text}}</Card>
          </Draggable>
        </Container>
      </div>
      <div class="lane">
        <h2 class="lane-title">Testes</h2>
        <Container
          group-name="list"
          @drag-start="handleDragStart('tests', $event)"
          @drop="handleDrop('tests', $event)"
          :get-child-payload="getChildPayload"
          :drop-placeholder="{className: 'placeholder'}"
        >
          <Draggable v-for="card in cards.tests" :key="card.id">
            <Card>{{card.text}}</Card>
          </Draggable>
        </Container>
      </div>
      <div class="lane">
        <h2 class="lane-title">Concluído</h2>
        <Container
          group-name="list"
          @drag-start="handleDragStart('done', $event)"
          @drop="handleDrop('done', $event)"
          :get-child-payload="getChildPayload"
          :drop-placeholder="{className: 'placeholder'}"
        >
          <Draggable v-for="card in cards.done" :key="card.id">
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

export default {
  name: 'App',
  components: {
    Header,
    Card,
    Container,
    Draggable
  },
  data: () => ({
    cards: {
      backlog: initalCards.backlog,
      dev: initalCards.dev,
      tests: initalCards.test,
      done: [],
    },
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
          lane,
          index: payload,
          cardData: {
            ...this.cards[lane][payload]
          }
        }
      }
      console.log(this.draggingCard)
    },
    handleDrop(lane, dropResult){
      console.log(dropResult, lane)
      const {removedIndex, addedIndex } = dropResult;
      if(lane === this.draggingCard.lane && removedIndex === addedIndex){
        return;
      }
      if(removedIndex !== null){
        this.cards[lane].splice(removedIndex, 1);
      }
      if(addedIndex !== null){
        this.cards[lane].splice(addedIndex, 0, this.draggingCard.cardData)
      }
      console.log(lane, dropResult, removedIndex, addedIndex)
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
