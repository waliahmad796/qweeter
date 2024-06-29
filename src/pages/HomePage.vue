<template>
  <q-page class="relative-position">
    <q-scroll-area
      :thumb-style="thumbStyle"
      :bar-style="barStyle"
      class="absolute full-height full-width"
    >
      <div class="q-py-lg q-px-lg row items-end q-col-gutter-md">
        <div class="col">
          <q-input
            class="input-home"
            bottom-slots
            v-model="text"
            label="What is happening?"
            counter
            maxlength="280"
            autogrow
          >
            <template v-slot:before>
              <q-avatar size="lg">
                <img src="/src/ولی احمد.jpg" />
              </q-avatar>
            </template>
          </q-input>
        </div>
        <div class="col col-shrink">
          <q-btn
            v-on:click="addNewQweet"
            class="q-mb-lg"
            color="primary"
            label="Qweet"
            rounded
            no-caps
            :disable="!text"
          />
        </div>
      </div>
      <q-separator class="divider" size="10px" color="grey-2" />

      <q-list separator>
        <transition-group
          appear
          enter-active-class="animated fadeIn"
          leave-active-class="animated fadeOut"
        >
          <q-item
            class="qweet q-py-md"
            v-for="qweet in qweets"
            :key="qweet.date"
          >
            <q-item-section avatar top>
              <q-avatar>
                <img src="/src/ولی احمد.jpg" />
              </q-avatar>
            </q-item-section>

            <q-item-section>
              <q-item-label class="text-subtitle1">
                <strong>Wali Ahmad Mohammadi</strong>
                <span class="text-grey-7">
                  @wams1 <br class="lt-md" />
                  &bull; {{ relativeDate(qweet.date) }}
                </span>
              </q-item-label>
              <q-item-label class="text-body1">
                {{ qweet.content }}
              </q-item-label>
              <div class="qweet-icons row justify-between q-mt-sm">
                <q-btn
                  flat
                  round
                  color="grey"
                  icon="fa-regular fa-comment"
                  size="sm"
                />
                <q-btn
                  flat
                  round
                  color="grey"
                  icon="fa-solid fa-retweet"
                  size="sm"
                />
                <q-btn
                  @click="toggleLiked(qweet)"
                  flat
                  round
                  :color="qweet.liked ? 'pink' : 'grey'"
                  :icon="
                    qweet.liked ? 'fa-solid fa-heart' : 'fa-regular fa-heart'
                  "
                  size="sm"
                />
                <q-btn
                  @click="deleteQweet(qweet)"
                  flat
                  round
                  color="grey"
                  icon="fa-solid fa-trash"
                  size="sm"
                />
              </div>
            </q-item-section>

            <!-- <q-item-section side top>{{ qweet.date }}</q-item-section> -->
          </q-item>
        </transition-group>
      </q-list>
    </q-scroll-area>
  </q-page>
</template>

<script setup>
// import db from "src/boot/firebase";
// import { collection, query, where, onSnapshot } from "firebase/firestore";

import { formatDistance } from "date-fns";

defineOptions({
  name: "HomePage",
  data() {
    return {
      text: "",
      qweets: [
        {
          content:
            "Lorem ipsum dolor sit amet consectetur adipisicing elit. Sequi cupiditate deleniti adipisci sunt fugit doloribus, eius nulla omnis iste illo aliquam odio harum pariatur quisquam suscipit minima sint ad obcaecati.",
          date: 1719478166098,
          liked: false,
        },
        {
          content:
            "Lorem ipsum dolor sit amet consectetur adipisicing elit. Sequi cupiditate deleniti adipisci sunt fugit doloribus, eius nulla omnis iste illo aliquam odio harum pariatur quisquam suscipit minima sint ad obcaecati.",
          date: 1719478220678,
          liked: false,
        },
      ],
    };
  },
  methods: {
    relativeDate(value) {
      //return formatDistance(value, new Date());
      return formatDistance(value, new Date(), { addSuffix: true });
    },
    addNewQweet() {
      let newQweet = {
        content: this.text,
        date: Date.now(),
        liked: false,
      };
      this.qweets.unshift(newQweet);
      this.text = "";
    },
    deleteQweet(qweet) {
      let dateToDelete = qweet.date;
      let index = this.qweets.findIndex((qweet) => qweet.date === dateToDelete);
      this.qweets.splice(index, 1);
    },
    toggleLiked(qweet) {
      if (!qweet.liked) {
        qweet.liked = true;
      } else {
        qweet.liked = false;
      }
    },
  },
  filters: {},
  // mounted() {
  //   db.collection("qweets").onSnapshot(function (snapshot) {
  //     snapshot.docChanges().forEach((change) => {
  //       if (change.type === "added") {
  //         console.log("New qweet: ", change.doc.data());
  //       }
  //       if (change.type === "modified") {
  //         console.log("Modified qweet: ", change.doc.data());
  //       }
  //       if (change.type === "removed") {
  //         console.log("Removed qweet: ", change.doc.data());
  //       }
  //     });
  //   });
  // },
});
</script>

<style lang="sass">
.input-home
  textarea
    font-size: 19px
    line-height: 1.4 !important
.divider
  border-top: 1px solid
  border-bottom: 1px solid
  border-color: $grey-4

.qweet-icons
  margin-left: -5px
.qweet:not(:last-child)
  border-bottom: 1px solid rgba(0,0 ,0 ,0.12)
</style>
