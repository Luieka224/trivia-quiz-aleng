<template>

  <div class="bg-white rounded-lg p-8 shadow-xl">
    <p class="text-gray-400">Question {{ question_index + 1 }}/10</p>
    <p class="text-gray-600 text-lg font-semibold text-center my-8">{{ quiz_details[question_index].question }}</p>
    <ButtonAnswer :choice="this.choices[0]" :isDisabled="this.isDisabled" :correct_ans="this.quiz_details[this.question_index].correct_answer" @add-score-and-index="addScoreAndIndex" @disable-btns="disableBtns" @set-isNext-false="setIsNextFalse" :isNext="this.isNext" />
    <ButtonAnswer :choice="this.choices[1]" :isDisabled="this.isDisabled" :correct_ans="this.quiz_details[this.question_index].correct_answer" @add-score-and-index="addScoreAndIndex" @disable-btns="disableBtns" @set-isNext-false="setIsNextFalse" :isNext="this.isNext"/>
    <ButtonAnswer :choice="this.choices[2]" :isDisabled="this.isDisabled" :correct_ans="this.quiz_details[this.question_index].correct_answer" @add-score-and-index="addScoreAndIndex" @disable-btns="disableBtns" @set-isNext-false="setIsNextFalse" :isNext="this.isNext"/>
    <ButtonAnswer :choice="this.choices[3]" :isDisabled="this.isDisabled" :correct_ans="this.quiz_details[this.question_index].correct_answer" @add-score-and-index="addScoreAndIndex" @disable-btns="disableBtns" @set-isNext-false="setIsNextFalse" :isNext="this.isNext"/>
    
    <!-- <p v-if="this.isDisabled" class="text-sm text-gray-600 text-center mb-2">Correct Answer: {{ quiz_details[question_index].correct_answer }}</p> -->
    
    <div class="flex" v-if="this.isDisabled">
      <div class="flex-grow"></div>
      <button class="bg-yellow-500 text-white rounded-md py-2 px-8" v-on:click="emitIncreaseIndex">NEXT</button>
    </div>
   
</div>
</template>
<script>
 
import ButtonAnswer from './ButtonAnswer.vue';
  export default {
    name: "Quizcard",
    props: {
        quiz_details: {
            type: Object,
            required: true
        },
        question_index: {
            type: Number,
            required: true
        },
        score: {
            type: Number,
            required: true
        }
    },
    data() {
      return {
        isDisabled: false,
        isNext: false,
      }
    },
    computed: {
        choices() {
            const wrong = this.quiz_details[this.question_index].incorrect_answers;
            const correct = this.quiz_details[this.question_index].correct_answer;
            wrong[3] = correct;
            //convert wrong Object to array
            const temp_choices = Object.values(wrong);
            //shuffle
            let curId = temp_choices.length;
            while (curId !== 0) {
                let randId = Math.floor(Math.random() * curId);
                curId -= 1;
                let temp = temp_choices[curId];
                temp_choices[curId] = temp_choices[randId];
                temp_choices[randId] = temp;
            }
            return temp_choices;
        }
    },
    methods: {
        addScoreAndIndex() {
          //set the buttons unclickable
          this.isDisabled = true;
          //increase index

          this.$emit('increase-score');
        },
        disableBtns() {
          this.isDisabled = true;
        },
        emitIncreaseIndex() {
          //disable to false
          this.isDisabled = false;
          this.isNext = true;
          this.$emit('increase-index')
        },
        setIsNextFalse(){
          this.isNext = false;
        }
    },
    components: { ButtonAnswer }
}
</script>