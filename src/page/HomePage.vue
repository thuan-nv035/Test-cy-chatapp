<template>
  <div>
    <div class="container mx-auto">
      <div class="w-1/2 h-34 border rounded mx-auto">
        <div class="lg:col-span-2 lg:block">
          <div class="w-full">
            <div
              class="relative flex items-center p-3 border-b border-gray-300 justify-between"
            >
              <div class="flex items-center">
                <img
                  class="object-cover w-10 h-10 rounded-full"
                  src="https://cdn.pixabay.com/photo/2018/01/15/07/51/woman-3083383__340.jpg"
                  alt="username"
                />
                <span class="block ml-2 font-bold text-gray-600">{{
                  user
                }}</span>
              </div>
              <button
                @click="logout()"
                class="bg-red-500 px-4 py-2 text-white rounded-md"
              >
                Đăng xuất
              </button>
            </div>
            <div class="relative w-full p-6 overflow-y-auto h-[35rem]">
              <div class="space-y-2">
                <div class="flex flex-col h-full justify-end">
                <div v-for="(item, index) in listAllmessages" :key="index">
                  <div class="relative pr-1">
                    <!-- Ânr hết trường hợp đặc biệt -->
                    <span>
                      <span
                        class="my-1 p-1 min-width max-width rounded-md break-words  px-4 py-2"
                        :class="{
                          'bg-gray-200 float-right': item.uid == uid,
                          'bg-orange-200 float-left': item.uid != uid,
                        }"
                      > 
                      <!-- <span>{{item.email}}</span> -->
                        <div :inner-html.prop="item.message"></div>
                      </span>
                    </span>
                  </div>
                </div>
              </div>
              </div>
            </div>
            <form @submit.prevent="addMessage">
              <div
                class="flex items-center justify-between w-full p-3 border-t border-gray-300"
              >
                <button>
                  <svg
                    xmlns="http://www.w3.org/2000/svg"
                    class="w-6 h-6 text-gray-500"
                    fill="none"
                    viewBox="0 0 24 24"
                    stroke="currentColor"
                  >
                    <path
                      stroke-linecap="round"
                      stroke-linejoin="round"
                      stroke-width="2"
                      d="M14.828 14.828a4 4 0 01-5.656 0M9 10h.01M15 10h.01M21 12a9 9 0 11-18 0 9 9 0 0118 0z"
                    />
                  </svg>
                </button>
                <button>
                  <svg
                    xmlns="http://www.w3.org/2000/svg"
                    class="w-5 h-5 text-gray-500"
                    fill="none"
                    viewBox="0 0 24 24"
                    stroke="currentColor"
                  >
                    <path
                      stroke-linecap="round"
                      stroke-linejoin="round"
                      stroke-width="2"
                      d="M15.172 7l-6.586 6.586a2 2 0 102.828 2.828l6.414-6.586a4 4 0 00-5.656-5.656l-6.415 6.585a6 6 0 108.486 8.486L20.5 13"
                    />
                  </svg>
                </button>

                <input
                  type="text"
                  placeholder="Message"
                  class="block w-full py-2 pl-4 mx-3 bg-gray-100 rounded-full outline-none focus:text-gray-700"
                  name="message"
                  required
                  v-model="message"
                />
                <button>
                  <svg
                    xmlns="http://www.w3.org/2000/svg"
                    class="w-5 h-5 text-gray-500"
                    fill="none"
                    viewBox="0 0 24 24"
                    stroke="currentColor"
                  >
                    <path
                      stroke-linecap="round"
                      stroke-linejoin="round"
                      stroke-width="2"
                      d="M19 11a7 7 0 01-7 7m0 0a7 7 0 01-7-7m7 7v4m0 0H8m4 0h4m-4-8a3 3 0 01-3-3V5a3 3 0 116 0v6a3 3 0 01-3 3z"
                    />
                  </svg>
                </button>
                <button type="submit">
                  <svg
                    class="w-5 h-5 text-gray-500 origin-center transform rotate-90"
                    xmlns="http://www.w3.org/2000/svg"
                    viewBox="0 0 20 20"
                    fill="currentColor"
                  >
                    <path
                      d="M10.894 2.553a1 1 0 00-1.788 0l-7 14a1 1 0 001.169 1.409l5-1.429A1 1 0 009 15.571V11a1 1 0 112 0v4.571a1 1 0 00.725.962l5 1.428a1 1 0 001.17-1.408l-7-14z"
                    />
                  </svg>
                </button>
              </div>
            </form>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { db } from "../firebase";
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
export default {
  name: "HomePage",
  data() {
    return {
      user: localStorage.getItem("email"),
      uid: localStorage.getItem("uid"),
      message: "",
      listAllmessages: [],
    };
  },

  mounted() {
    this.getMessage();
  },

  methods: {
    async addMessage() {
      if (!this.message) {
        return;
      }
      const collectionRef = collection(db, "listmessages");
      await addDoc(collectionRef, {
        uid: this.uid,
        message: this.message,
        date: new Date(),
        email: this.user,
      });
      this.message = "";
    },

    async getMessage() {
      const collectionRef = collection(db, "listmessages");
      let unsub = onSnapshot(collectionRef, (snapshot) => {
        const localListMessage = [];
        snapshot.forEach((doc) => {
          localListMessage.push({
            id: doc.id,
            message: doc.data().message,
            uid: doc.data().uid,
            email: doc.data().email,
            // date: new Date(doc.data().date.seconds * 1000),
          });
        });
        this.listAllmessages = localListMessage
        // this.listAllmessages = this.localListMessage.sort((a, b) => a.date - b.date)
      });
    },

    logout() {
      localStorage.removeItem("token");
      localStorage.removeItem("email");
      setTimeout(() => {
        this.$router.push("/");
      }, 1000);
      alert("Đăng xuất thành công");
    },
  },
};
</script>
