<template>
  <div id="app">
    <Header />
    <div class="board">
      <div class="lane">
        <h2 class="lane-title">Backlog</h2>
        <Container
          group-name="list"
          @drag-start="handleDragStart('backlog', $event)"
          @drop="handleDrop"
          :get-child-payload="getChildPayload"
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
          @drop="handleDrop"
          :get-child-payload="getChildPayload"
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
          @drop="handleDrop"
          :get-child-payload="getChildPayload"
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
          @drop="handleDrop"
          :get-child-payload="getChildPayload"
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
    handleDrop(){

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
}

.lane{
  background: var(--color-grey);
  width: 23rem;
  height: 30rem;
  border-radius: .8rem;
  box-shadow: 0 .1rem .2rem 0 rgba(33, 33, 33, 0.1);
  padding: 0 .7rem;
  margin: 0 .8rem
}

.lane-title{
  padding: .8rem .5rem;
  margin-bottom: .6rem;

}
</style>
