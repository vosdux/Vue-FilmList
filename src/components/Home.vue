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
            p {{ filmTime }}

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
            p {{ serialTime }}

        .tag-list.tag-list--add
          .ui-tag__wrapper(
            @click="tagMenuShow = !tagMenuShow"
          )
            .ui-tag
              span.tag-title Add new
              span.button-close(
                :class="{ active: !tagMenuShow }"
              )
        
        .tag-list.tag-list--menu(
          v-if="tagMenuShow"
        )
          input.tag-add--input(
            type="text"
            placeholder="new tag"
            v-model="tagTitle"
            @keyup.enter="newTag"
          )
          .button.button-default(
            @click="newTag"
          ) Send

        .tag-list
          .ui-tag__wrapper(
            v-for="tag in tags"
            :key="tag.title"
          )
            .ui-tag(
              @click="addTagUsed(tag)"
              :class="{ used: tag.use }"
            )
              span.tag-title {{ tag.title }}
              span.button-close
</template>

<script>
export default {
  data () {
    return {
      taskTitle: '',
      taskDescription: '',
      tagTitle: '',
      tagMenuShow: false,
      type: 'Film',
      taskId: 3,
      filmHours: 1,
      filmMinutes: 30,

      serialSeason: 1,
      serialSeries: 11,
      serialSeriesMinutes: 40,
      tagsUsed: [],
      tags: [
        {
          title: 'Comedy',
          use: false
        },
        {
          title: 'Westerns',
          use: false
        },
        {
          title: 'Adventure',
          use: false
        }

      ]
    }
  },
  methods: {
    newTag() {
      if(this.tagTitle === '') {
        return
      }
      this.tags.push({
        title: this.tagTitle,
        use: false
      })
    },
    newTask () {
      if(this.taskTitle === '') {
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
      this.taskTitle = ''
      this.taskDescription = ''
    },
    addTagUsed (tag) {
      tag.use = !tag.use
      if (tag.use) {
        this.tagsUsed.push(
          tag.title
        )
      } else {
        this.tagsUsed.splice(tag.title, 1)
      }
    },
    getHoursAndMinutes (minutes) {
      let hours = Math.trunc(minutes / 60)
      let min = minutes % 60
      return `${hours} Hours ${min} Minutes`
    }
  },
  computed: {
    filmTime () {
      let min = this.filmHours * 60 + this.filmMinutes
      return this.getHoursAndMinutes(min)
    },
    serialTime () {
      let min = this.serialSeason * this.serialSeries * this.serialSeriesMinutes
      return this.getHoursAndMinutes(min)
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
  display flex
  margin-bottom 6px

.time-input
  max-width 80px
  margin-right 10px

.tag-list
  margin-bottom 20px

.ui-tag__wrapper
  margin-right 18px
  margin-bottom 18px
  &:last-child
    margin-right 0

.ui-tag
  .button-close
    &.active
      transform rotate(45deg)
  &.used
    background-color #444ce0
    color #fff
    .button-close
      &:before,
      &:after
        background-color #fff

.tag-list--menu
  display flex
  justify-content space-between
  align-items center

.tag-add--input
  margin-bottom 0
  margin-right 10px
  height 42px
</style>
