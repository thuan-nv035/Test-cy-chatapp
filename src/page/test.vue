<template>
  <div>
    <div class="container mx-auto shadow-lg rounded-lg">
      <!-- headaer -->
      <div
        class="px-5 py-5 flex justify-between items-center bg-white border-b-2"
      >
        <div class="font-semibold text-2xl">GoingChat</div>
        <div class="w-1/2">
          <input
            type="text"
            name=""
            id=""
            placeholder="search IRL"
            class="rounded-2xl bg-gray-100 py-3 px-5 w-full"
          />
        </div>
        <div @click="logout()" class="bg-yellow-500 p-3 rounded-md">
          Sign out
        </div>
        <div
          class="h-12 w-12 p-2 bg-yellow-500 rounded-full text-white font-semibold flex items-center justify-center"
        >
          RA
        </div>
      </div>
      <!-- end header -->
      <!-- Chatting -->
      <div class="flex flex-row justify-between bg-white">
        <!-- chat list -->
        <div class="flex flex-col w-2/5 border-r-2 overflow-y-auto">
          <!-- search compt -->
          <div class="border-b-2 py-4 px-2">
            <input
              type="text"
              placeholder="search chatting"
              class="py-2 px-2 border-2 border-gray-200 rounded-2xl w-full"
            />
          </div>
          <!-- end search compt -->
          <!-- user list -->
          <div
            class="flex flex-row py-4 px-2 justify-center items-center border-b-2"
          >
            <div class="w-1/4">
              <img
                src="https://source.unsplash.com/_7LbC5J-jw4/600x600"
                class="object-cover h-12 w-12 rounded-full"
                alt=""
              />
            </div>
            <div class="w-full">
              <div class="text-lg font-semibold">Luis1994</div>
              <span class="text-gray-500">Pick me at 9:00 Am</span>
            </div>
          </div>
          <div class="flex flex-row py-4 px-2 items-center border-b-2">
            <div class="w-1/4">
              <img
                src="https://source.unsplash.com/otT2199XwI8/600x600"
                class="object-cover h-12 w-12 rounded-full"
                alt=""
              />
            </div>
            <div class="w-full">
              <div class="text-lg font-semibold">Everest Trip 2021</div>
              <span class="text-gray-500">Hi Sam, Welcome</span>
            </div>
          </div>
          <div
            class="flex flex-row py-4 px-2 items-center border-b-2 border-l-4 border-blue-400"
          >
            <div class="w-1/4">
              <img
                src="https://source.unsplash.com/L2cxSuKWbpo/600x600"
                class="object-cover h-12 w-12 rounded-full"
                alt=""
              />
            </div>
            <div class="w-full">
              <div class="text-lg font-semibold">MERN Stack</div>
              <span class="text-gray-500">Lusi : Thanks Everyone</span>
            </div>
          </div>
<div class="flex flex-row py-4 px-2 items-center border-b-2">
            <div class="w-1/4">
              <img
                src="https://source.unsplash.com/vpOeXr5wmR4/600x600"
                class="object-cover h-12 w-12 rounded-full"
                alt=""
              />
            </div>
            <div class="w-full">
              <div class="text-lg font-semibold">Javascript Indonesia</div>
              <span class="text-gray-500">Evan : some one can fix this</span>
            </div>
          </div>
          <div class="flex flex-row py-4 px-2 items-center border-b-2">
            <div class="w-1/4">
              <img
                src="https://source.unsplash.com/vpOeXr5wmR4/600x600"
                class="object-cover h-12 w-12 rounded-full"
                alt=""
              />
            </div>
            <div class="w-full">
              <div class="text-lg font-semibold">Javascript Indonesia</div>
              <span class="text-gray-500">Evan : some one can fix this</span>
            </div>
          </div>

          <div class="flex flex-row py-4 px-2 items-center border-b-2">
            <div class="w-1/4">
              <img
                src="https://source.unsplash.com/vpOeXr5wmR4/600x600"
                class="object-cover h-12 w-12 rounded-full"
                alt=""
              />
            </div>
            <div class="w-full">
              <div class="text-lg font-semibold">Javascript Indonesia</div>
              <span class="text-gray-500">Evan : some one can fix this</span>
            </div>
          </div>
          <!-- end user list -->
        </div>
        <!-- end chat list -->
        <!-- message -->
        <div class="w-full px-5 flex flex-col justify-between">
          <div class="flex flex-col mt-5">
            <!-- Render message o cho nay -->
            <div id="conversation " class="h-screen overflow-y-scroll">
              <div class="flex flex-col h-full justify-end">
                <div v-for="(item, index) in user_messages" :key="index">
                  <div class="relative pr-1">
                    <!-- Ânr hết trường hợp đặc biệt -->
                    <span>
                      <div
                        v-if="
                          index > 0 &&
                          item.userId != userId &&
                          user_messages[index].userId !=
                            user_messages[index - 1].userId
                        "
                      >
                        {{ item.userEmail }}
                      </div>
                      <span
                        class="my-1 p-1 min-width max-width rounded-md break-words truong-hop-gui-anh-thuong"
                        :class="{
                          'bg-gray-200 float-right': item.userId == userId,
                          'bg-orange-200 float-left ml-3':
                            item.userId != userId,
                        }"
>
                        <div :inner-html.prop="item.message"></div>
                      </span>
                    </span>
                  </div>
                </div>
              </div>
            </div>
            <!-- Render message o cho nay -->
          </div>
          <div class="py-5">
            <div class="flex flex-row justify-center">
              <input
                class="w-full bg-gray-300 py-5 px-3 rounded-xl"
                type="text"
                v-model="send_message"
                placeholder="type your message here..."
              />
              <div class="flex flex-row align-middle pl-2">
                <button @click="send()">
                  <svg
                    xmlns="http://www.w3.org/2000/svg"
                    class="h-6 w-6 rotate-90 text-gray-600"
                    fill="none"
                    viewBox="0 0 24 24"
                    stroke="currentColor"
                    stroke-width="2"
                  >
                    <path
                      stroke-linecap="round"
                      stroke-linejoin="round"
                      d="M12 19l9 2-9-18-9 18 9-2zm0 0v-8"
                    />
                  </svg>
                </button>
              </div>
            </div>
          </div>
        </div>
        <!-- end message -->
        <div class="w-2/5 border-l-2 px-5">
          <div class="flex flex-col">
            <div class="font-semibold text-xl py-4">Mern Stack Group</div>
            <img
              src="https://source.unsplash.com/L2cxSuKWbpo/600x600"
              class="object-cover rounded-xl h-64"
              alt=""
            />
            <div class="font-semibold py-4">Created 22 Sep 2021</div>
            <div class="font-light">
              Lorem ipsum dolor sit amet consectetur adipisicing elit. Deserunt,
              perspiciatis!
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
//import { defineComponent } from '@vue/composition-api'
import { getStorage, uploadBytes, ref, getDownloadURL } from "firebase/storage";
import {
  collection,
  getDocs,
  deleteDoc,
  doc,
  onSnapshot,
  getDoc,
  query,
  where,
  addDoc,
  orderBy,
  limit,
} from "firebase/firestore";
import { db } from "../firebase";

export default {
  name: "ChatApp",
  setup() {},
  mounted() {
    (async () => {
      this.userId = localStorage.getItem("uid");
      ////////
      const querySnapshot = await getDocs(collection(db, "newchat"));
      this.user_messages = [];
      querySnapshot.forEach((doc) => {
        this.user_messages.push({
          id: doc.id,
          message: doc.data().message,
          userId: doc.data().userId,
          date: new Date(doc.data().date.seconds * 1000),
          userEmail: doc.data().userEmail,
        });
      });

      this.user_messages = this.user_messages.sort((a, b) => a.date - b.date);
console.log("user_messages", this.user_messages);
      this.realTime();
    })();
  },
  updated() {},
  methods: {
    logout() {
      localStorage.removeItem("token");
      localStorage.removeItem("email");
      setTimeout(() => {
        this.$router.push("/");
      }, 1000);
      alert("Đăng xuất thành công");
    },
    async realTime() {
      (async () => {
        const collectionRef = collection(db, "newchat");
        this.user_messages = [];
        const unSub = onSnapshot(collectionRef, (snapShot) => {
          this.user_messages = [];
          snapShot.forEach((doc) => {
            /* console.log(
              "data tra ve",
              new Date(doc.data().date.seconds * 1000)
            ); */
            this.user_messages.push({
              id: doc.id,
              message: doc.data().message,
              userId: doc.data().userId,
              date: new Date(doc.data().date.seconds * 1000),
              userEmail: doc.data().userEmail,
            });
          });
          this.user_messages = this.user_messages.sort(
            (a, b) => a.date - b.date
          );
        });
      })();
    },
    async send() {
      //   /alert(new Date());
      const collectionRef = collection(db, "newchat");
      await addDoc(collectionRef, {
        message: this.send_message,
        userId: this.userId,
        date: new Date(),
        userEmail: localStorage.getItem("email"),
      });
      this.send_message = "";
    },
  },
  data() {
    return {
      showImg: "",
      confirmedUrlMessage: "",
      confirmedNameProduct: "",
      order: [],
      productList: [],
      ProductInfo: "",
      showOrder: false,
      ////////////////////////////////////////////////// trên là đơn hàng
      cheatingShowing: false,
      renderUpload: [], //render ảnh hay video
      connectCompleted: false, // tìm kiếm phòng thành công
      showContainer: false, // show khung ảnh chuẩn bị gửi
      file_name: [],
      selectedFiles: [],
      previewFileType: false,
      NotAllowTosend: false,
      enlarge: false,
      enlargeIndex: "",
      count: 0,
      //phiá trên là code upload file,
      lastIndex: "",
      ClearSendding: false,
      threeUserDots: false,
      showChatApp: false,
      totalUserInform: 0,
      myProp: false,
      user_messages: [],
      send_message: null,
      connected: false,
      showChatBox: false,
      hideChatbox: false,
      preventSubcribe: false,
      roomBox: "",
      userId: "",
      idUserReceive: "",
      emojiShow: true,
      existEmoji: false,
    };
  },
};
</script>
