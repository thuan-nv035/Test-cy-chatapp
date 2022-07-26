<template>
  <div>
    <div class="bg-grey-lighter min-h-screen flex flex-col">
      <form @submit.prevent="signup">
        <div
          class="container max-w-sm mx-auto flex-1 flex flex-col items-center justify-center px-2"
        >
          <div class="bg-white px-6 py-8 rounded shadow-md text-black w-full">
            <h1 class="mb-8 text-3xl text-center">Sign up</h1>
            <input
              type="text"
              class="block border border-grey-light w-full p-3 rounded mb-4"
              name="email"
              placeholder="Email"
              v-model="email"
            />

            <input
              type="password"
              class="block border border-grey-light w-full p-3 rounded mb-4"
              name="password"
              placeholder="Password"
              v-model="password"
            />
            <input
              type="password"
              class="block border border-grey-light w-full p-3 rounded mb-4"
              name="confirm_password"
              placeholder="Confirm Password"
              v-model="confirmPassword"
            />

            <button
              type="submit"
              class="w-full text-center py-3 rounded bg-green-400 text-black  focus:outline-none my-1"
            >
              Create Account
            </button>

            <div class="text-center text-sm text-grey-dark mt-4">
              By signing up, you agree to the
              <a
                class="no-underline border-b border-grey-dark text-grey-dark"
                href="#"
              >
                Terms of Service
              </a>
              and
              <a
                class="no-underline border-b border-grey-dark text-grey-dark"
                href="#"
              >
                Privacy Policy
              </a>
            </div>
          </div>

          <div class="text-grey-dark mt-6">
            Already have an account?
            <a
              class="no-underline border-b border-blue text-blue"
              href="../login/"
            >
              Log in </a
            >.
          </div>
        </div>
      </form>
    </div>
  </div>
</template>

<script>
import { createUserWithEmailAndPassword } from "firebase/auth";
import { auth } from "@/firebase";
import {
  collection,
  getDocs,
  deleteDoc,
  doc,
  onSnapshot,
  addDoc,
  query,
  where,
} from "firebase/firestore";
import { db } from "@/firebase";

export default {
  name: "Signup",
  data() {
    return {
      email: "",
      password: "",
      confirmPassword: "",
    };
  },
  methods: {
    async signup() {
      if (
        this.email == "" ||
        this.password == "" ||
        this.confirmPassword == ""
      ) {
        alert("không được bo trống trường nào");
        return;
      } else if (this.password !== this.confirmPassword) {
        alert("password và confirmPassword không giống nhau");
        return;
      }
      try {
        const response = await createUserWithEmailAndPassword(
          auth,
          this.email,
          this.password
        );
        const collectionRef = collection(db, "users");
        console.log(response.user.email);
        await addDoc(collectionRef, {
          email: response.user.email,
          userId: response.user.uid,
        });
        alert("Dang ky thanh cong");
        this.$router.push("/");
      } catch (error) {
        console.log(error.message);
      }
    },
  },
};
</script>

<style scoped></style>
