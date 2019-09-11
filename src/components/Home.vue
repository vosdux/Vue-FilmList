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
            span Total Film Time
          .total-time__film(
            v-if="type === 'Serial'"
          )
            span Total Serial Time
        .tag-list
          .ui-tag__wrapper
            .ui-tag
              span.tag-title tag
              span.button-close


    section
      .container
        .task-list
          .task-item(
            v-for="task in tasks"
            :key="task.id"
            :class="{ completed: task.completed }"
          )
            .ui-card.ui-card--shadow
              .task-item__info
                .task-item__main-info
                  span.ui-label.ui-label--light {{ task.type }}
                  span Total Time:
                span.button-close
              .task-item__content
                .task-item__header
                  .ui-checkbox-wrapper
                    input.ui-checkbox(
                      type='checkbox'
                      v-model="task.completed"
                    )
                  span.ui-title-3 {{ task.title }}
                .task-item__body
                  p.ui-text-regular {{ task.description }}
</template>


<script>
export default {
  data() {
    return {
      taskTitle: "",
      taskDescription: "",
      type: 'Film',
      taskId: 3,
      tasks: [
        {
          'id': 1,
          'title': 'First film',
          'description': 'new desc',
          'type': 'Film',
          'completed': false,
          'editing': false
        },
        {
          'id': 2,
          'title': 'Second film',
          'description': 'new desc second item',
          'type': 'Serial',
          'completed': false,
          'editing': false
        }
      ]
    }
  },
  methods: {
    newTask() {
      if(taskTitle === '') {
        return
      }
      this.tasks.push({
        id: this.taskId,
        title: this.taskTitle,
        description: this.taskDescription,
        type: this.type,
        completed: false,
        editing: false
      })

      this.taskId += 1
      this.taskTitle = ""
      this.taskDescription = ""
    }
  }
}
</script>

<style lang="stylus" scoped>
.task-item
  margin-bottom 20px
  &:last-child
    margin-bottom 0
  &__info
    display flex
    align-items center
    justify-content space-between
    margin-bottom 20px
    .button-close
      width 20px
      height @width
.ui-label
  margin-right 8px

.task-item__header
  display flex
  align-items center
  margin-bottom 20px
  .ui-checkbox-wrapper
    margin-right 8px
  .ui-title-3
    margin-bottom 0

.option-list
  display flex
  .what-watch--radio
    margin-right 12px
  label
    margin-right 20px
    &:last-child
      margin-right 0
</style>