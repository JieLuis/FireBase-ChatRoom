<template>
  <header>
    <a href="/index.html"
      ><img class="icon" src="../../images/icon@4x.png" alt=""
    /></a>

    <h1 class="headbar-title">Thinkaboutmoonlight</h1>

    <nav>
      <img
        v-if="screenWidth < 760"
        class="icon icon--right"
        @click="handleClick"
        src="../../images/icon-right.svg"
        alt=""
      />
      <ul :class="getClass()">
        <li><a href="/index.html">Home</a></li>
        <li><a href="#">About</a></li>
        <li><a href="#">Contact</a></li>
        <div class="language-menu">
          <div class="selected-language">English</div>
          <ul class="language-list">
            <li><a href="#" class="en">English</a></li>
            <li><a href="/index-cn.html" class="cn">中文</a></li>
          </ul>
        </div>
        <button @click="logout" class="btn--classic">
          Me :
          <div v-if="user">{{ user.displayName }}</div>
        </button>
      </ul>
    </nav>
  </header>
</template>
<script>
import { ref } from "vue";
import { useWindowSize } from "@vueuse/core";
import useLogout from "../../composable/useLogout";
import getUser from "../../composable/getUser";
export default {
  setup() {
    const showCollapisbleContent = ref(false);
    const handleClick = () => {
      showCollapisbleContent.value = !showCollapisbleContent.value;
    };
    const { width } = useWindowSize();
    const screenWidth = ref(width);
    const { user } = getUser();

    const getClass = () => {
      if (screenWidth.value < 760 && showCollapisbleContent.value)
        return "contentBlock";
      else if (screenWidth.value > 760) return "contentFlex";
      return "contentNone";
    };

    const logout = async () => {
      const { error: logoutError } = await useLogout();
      if (logoutError && logoutError.value) {
        console.error("Logout error:", logoutError.value);
      } else {
        console.log("User logged out successfully");
      }
    };

    return {
      handleClick,
      showCollapisbleContent,
      screenWidth,
      getClass,
      logout,
      user,
    };
  },
};
</script>

<style>
.contentBlock {
  display: block;
  position: absolute;
  border: 2px solid #000000;
  top: 60px;
  background-color: #333;
  padding: 5px;
  border-radius: 5px;
  right: 1.3px;
}

.contentFlex {
  display: flex;
  align-items: center;
  margin-right: 10px;
}

.contentFlex .btn--classic {
  margin-left: 10px;
}

.contentNone {
  display: none;
}
</style>
