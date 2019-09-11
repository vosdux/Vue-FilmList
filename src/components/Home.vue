<template lang="pug">
  .content-wrapper
    section
      .container
        h1.ui-title-1 Home
        input(
          type="text"
          placeholder="What we wiil watch"
          v-model="taskTitle"
          @keyup.enter="newTask"
        )
        textarea(
          type="text"
          v-model="taskDescription"
          @keyup.enter="newTask"
        )
        .option-list
          input.what-watch--radio(
            type="radio"
            id="radioFilm"
            value="Film"
            v-model="type"
          )
          label(
            for="radioFilm"
          ) Film
          input.what-watch--radio(
            type="radio"
            id="radioSerial"
            value="Serial"
            v-model="type"
          )
          label(
            for="radioSerial"
          ) Serial
        .total-time
          .total-time__film(
            v-if="type === 'Film'"
          )
            span.time-title Hours
            input.time-input(
              type="number"
              v-model="filmHours"
            )
            span.time-title Minutes
            input.time-input(
              type="number"
              v-model="filmMinutes"
            )
          .total-time__serial(
            v-if="type === 'Serial'"
          )
            span.time-title How many seasons?
            input.time-input(
              type="number"
              v-model="serialSeason"
            )
            span.time-title How many series?
            input.time-input(
              type="number"
              v-model="serialSeason"
            )
            span.time-title How long is one series?
            input.time-input(
              type="number"
              v-model="serialSeriesMinutes"
            )
        .tag-list
          .ui-tag__wrapper
            .ui-tag
              span.tag-title tag
              span.button-close
</template>


<script>
export default {
  data() {
    return {
      taskTitle: "",
      taskDescription: "",
      type: 'Film',
      taskId: 3,
      filmHours: 1,
      filmMinutes: 30,

      serialSeason: 1,
      serialSeries: 11,
      serialSeriesMinutes: 40
    }
  },
  methods: {
    newTask() {
      if(taskTitle === '') {
        return
      }
      const task = {
        id: this.taskId,
        title: this.taskTitle,
        description: this.taskDescription,
        type: this.type,
        completed: false,
        editing: false
      }

      this.taskId += 1
      this.taskTitle = ""
      this.taskDescription = ""
    }
  }
}
</script>

<style lang="stylus" scoped>
.option-list
  display flex
  .what-watch--radio
    margin-right 12px
  label
    margin-right 20px
    &:last-child
      margin-right 0

.total-time
  margin-bottom 20px
.time-title
  display 
  margin-bottom 6px

.time-input
  max-width 80px
  margin-right 10px

</style>