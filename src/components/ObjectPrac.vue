<script setup>
import { reactive, ref, watchEffect } from 'vue';
import dataSource from '../../data.json';
import _ from 'lodash';
import CollectReply from './CollectReply.vue';
import HandleEdit from './HandleEdit.vue';
import Score from './Score.vue';
import ToggleProvider from './ToggleProvider.vue';
import ToggleButton from './ToggleButton.vue';
import ToggledElement from './ToggledElement.vue';
import editIcon from '../../images/icon-edit.svg'
import replyIcon from '../../images/icon-reply.svg'
import Modal from './Modal.vue';
import Form from './Form.vue';
import juliusomo from '../../images/avatars/image-juliusomo.png'
import maxblagun from '../../images/avatars/image-maxblagun.png'
import ramsesmiron from '../../images/avatars/image-ramsesmiron.png'
import amyrobson from '../../images/avatars/image-amyrobson.png'


// Function to initialize data with reactivity
function initializeData() {
  let initialData;

  if (localStorage.getItem('userData') !== null) {
    initialData = JSON.parse(localStorage.getItem('userData'));
  } else {
    initialData = _.cloneDeep(dataSource);
  }

  return reactive(initialData);
}

// Initialize reactive data
const newObj = initializeData();

// Extract reactive properties
const comments = newObj.comments;
const currentUser = newObj.currentUser;

// Function to update localStorage
function setLocalStorage() {
  localStorage.setItem('userData', JSON.stringify(newObj));
}

// Ensure localStorage is updated whenever newObj changes
watchEffect(() => {
  setLocalStorage();
});

// Sort replies function in descending order using lodash
function sortReplies() {
  for (let i = 0; i < comments.length; i++) {
    comments[i].replies = _.orderBy(comments[i].replies, o => o.score, 'desc');
  }
}

sortReplies();


// Handle reply submission
const handleReplySubmit = (reply, commentId, username) => {
  comments[commentId - 1].replies.push({
    id: commentId + 1,
    content: reply,
    createdAt: 'Just now',
    score: 0,
    replyingTo: username,
    user: {
        image: {
            png: currentUser.image.png
        },
      username: currentUser.username,
    },
  });
};
// const newComment = ref('')
const handleCommentSubmit = (comment) => {
    let newIndex = newObj.comments.length
    console.log("New Index", newIndex)
    newObj.comments.push({
        id: newIndex + 1,
        content: comment,
        createdAt: "Just now",
        score: 0,
        user: {
            image: {
                png: currentUser.image.webp
            },
            username: currentUser.username,
        },
        replies: []
    })
}

// Handle edit submission
function handleEdit(newReply, commentIndex, replyIndex) {
  comments[commentIndex].replies[replyIndex].content = newReply;
}

function handleDelete(commentIndex, replyIndex) {

    comments[commentIndex].replies.splice(replyIndex, 1)
}

// Hack to make the images show since I can't find any other way
// to make the images show in the build
// Vue is weird this also worked in the browser but was showing one particular image in the build
// irrespective of the array order
function findImageImport(username) {
    let arr = [maxblagun, juliusomo, ramsesmiron, amyrobson]
    for (let i = 0; i < arr.length; i++) {
        if (arr[i].includes(username)) {
            return arr[i]
        }
    }

}

function getImageUrl(username) {
    return new URL(`../../images/avatars/image-${username}.png`, import.meta.url).href
}

function handleScoreIncrease(commentIndex) {
    comments[commentIndex].score++
}
function handleScoreDecrease(commentIndex) {
    comments[commentIndex].score--
}

function handleReplyScoreIncrease(commentIndex, replyIndex) {
    comments[commentIndex].replies[replyIndex].score++
}
function handleReplyScoreDecrease(commentIndex, replyIndex) {
    comments[commentIndex].replies[replyIndex].score--
}
</script>

<template>
    <main className="font-rubik sm:px-40 lg:px-60 xl:px-0 bg-very-light-gray ">
        <div class="xl:m-auto xl:p-0 xl:max-w-[75ch] xl:py-16">
            <div className="p-5 pt-0 first:pt-[4rem] xl:first:pt-0 max-w-[375px] sm:max-w-none md:w-[576px] lg:min-w-full mx-auto" v-for="(comment, commentIndex) in comments" :key="comment.id">
            <!-- Comment -->

                <div>
                    <ToggleProvider>
                        <template v-slot="{ isVisible, toggleVisibility }">


                            <div class="gap-4 p-3 font-normal bg-white rounded-lg lg:p-6 lg:flex lg:flex-row-reverse">
                                <div class="w-full" >
                                    <div class="flex justify-between font-medium">
                                        <div class="flex items-center gap-3 mb-3">
                                            <img class="w-8 h-8" :src='getImageUrl(comment.user.username)' alt="no pic avai">
                                            <h1 class="font-bold text-dark-blue">{{ comment.user.username }}</h1>
                                            <p class="font-normal text-grayish-blue">{{ comment.createdAt }}</p>
                                        </div>

                                        <!-- Reply Button for bigger screens -->
                                        
                                        <ToggleButton class="hidden text-moderate-blue lg:block" :isVisible="isVisible" :toggleVisibility="toggleVisibility">
                                            <img :src="replyIcon" alt="reply">    
                                        </ToggleButton>
                                    </div>
                                    <p class="text-grayish-blue">{{ comment.content }}</p>
                                </div>
                                <div class="hidden lg:block">
                                    <Score @increase-number="handleScoreIncrease(commentIndex) " @decrease-number="handleScoreDecrease(commentIndex)" :score="comment.score" />
                                </div>
                                <div class="flex items-center justify-between font-medium lg:hidden">
                                    <Score @increase-number="handleScoreIncrease(commentIndex) " @decrease-number="handleScoreDecrease(commentIndex)" :score="comment.score" />

                                    <!-- Reply Button for small screens and mobile devices -->

                                    <ToggleButton class="text-moderate-blue lg:hidden" :isVisible="isVisible" :toggleVisibility="toggleVisibility">
                                        <img :src="replyIcon" alt="reply">    
                                    </ToggleButton>
                                </div>
                            </div>

                            <!-- The input component -->

                            <ToggledElement :isVisible="isVisible" :toggleVisibility="toggleVisibility">
                                <div class="p-5 mt-3 bg-white" >
                                    <CollectReply class="text-moderate-blue" @toggle="toggleVisibility" @submit-reply="(reply) => handleReplySubmit(reply, comment.id, comment.user.username)">
                                        <img class="w-8 h-8" :src="getImageUrl(currentUser.username)" alt="no pic avai">
                                    </CollectReply>
                                </div> 
                            </ToggledElement>
                        </template>                      
                    </ToggleProvider>                   
                </div>

                <!-- Replies -->
                
                <div class="border-l-2 md:ml-11">
                    
                    <div class="mt-5 ml-5 md:ml-10" v-for="(reply, replyIndex) in comment.replies" :key="reply.id">
                        <ToggleProvider>
                            <template v-slot="{ isVisible, toggleVisibility }">

                                <div class="flex flex-row-reverse gap-4 p-3 bg-white md:p-6 rounded-xl">
                                    <div class="w-full">

                                        <div class="flex justify-between gap-3 mb-3 font-medium">
                                            <div class="flex items-center gap-3">
                                                <img class="w-8 h-8" :src='getImageUrl(reply.user?.username)' alt="no pic avai">
                                                <h2 class="font-bold text-dark-blue">{{ reply.user?.username }}</h2>
                                                <p class="px-2 text-sm text-white bg-moderate-blue" v-if="reply.user?.username === currentUser.username">you</p>
                                                <p class="font-normal text-grayish-blue">{{ reply.createdAt }}</p>
                                            </div>
                                            <div>
                                                <div class="items-center justify-center hidden gap-5 lg:flex" v-if="reply.user?.username === currentUser.username">
                                                
                                    
                                                        <!-- The Modal -->
                                                                                                        
                                                            <Modal 
                                                                @handle-delete="handleDelete(commentIndex, replyIndex)"
                                                            />
                                                    
                                                    <div class="flex flex-row items-center gap-3">
                                                        <ToggleButton tipo="Edit"  :isVisible="isVisible" :toggleVisibility="toggleVisibility">
                                                            <img :src="editIcon" alt="edit icon">
                                                        </ToggleButton>
                                                    </div>
                                                
                                                </div>
                                            
                                                <div class="flex-row items-baseline hidden gap-3 cursor-pointer lg:flex" v-else>
                                                    <img :src="replyIcon" alt="reply-icon">
                                                    <button class="text-moderate-blue" >Reply</button>
                                                </div>
                                            </div>
                                        </div>

                                        <!-- Reply Content and modal -->

                                        <p class="text-grayish-blue"><span class="font-bold text-moderate-blue">@{{ reply.replyingTo }}</span> {{ reply.content }}</p>
                                        <div class="flex justify-between font-medium lg:justify-end lg:hidden">
                                            <div class="lg:hidden">
                                                <Score @increase-number="handleReplyScoreIncrease(commentIndex, replyIndex)" @decrease-number="handleReplyScoreDecrease(commentIndex, replyIndex)" :score="reply.score" />
                                            </div>    
                                            <div class="flex items-center justify-center gap-3" v-if="reply.user?.username === currentUser.username">
                                                
                                                <!-- The modal for smaller screens -->
                                                <Modal 
                                                    @handle-delete="handleDelete(commentIndex, replyIndex)"
                                                />
                                                
                                                <div class="flex flex-row items-center gap-3">
                                                    <img :src="editIcon" alt="edit icon"> 
                                                    <ToggleButton tipo="Edit"  :isVisible="isVisible" :toggleVisibility="toggleVisibility"/>
                                                </div>
                                                
                                            </div>

                                            <div v-else class="flex items-center justify-center gap-3">
                                                <img :src="replyIcon" alt="reply-icon">
                                                <button class="text-moderate-blue">Reply</button>
                                            </div>
                                        </div>
                                    </div>
                                    <div class="hidden lg:block">
                                        <Score @increase-number="handleReplyScoreIncrease(commentIndex, replyIndex)" @decrease-number="handleReplyScoreDecrease(commentIndex, replyIndex)" :score="reply.score" />
                                    </div>
                                </div>
                                <ToggledElement :isVisible="isVisible" :toggleVisibility="toggleVisibility">
                                    <div class="p-5 mt-3 bg-white">
                                        <HandleEdit :value="reply.content" @toggle="toggleVisibility" @submit-edit="(edit) => handleEdit(edit, commentIndex, replyIndex)" />
                                    </div>
                                </ToggledElement>
                            </template>
                        </ToggleProvider>
                    </div>

                </div>

                
            </div>

                <!-- Mobile form -->

                <Form @handle-comment-submit="(newComment) => handleCommentSubmit(newComment)">
                    <img class="w-8 h-8" :src="getImageUrl(currentUser.username)" alt="user-image">
                </Form>
        </div>
        
    </main>
</template>