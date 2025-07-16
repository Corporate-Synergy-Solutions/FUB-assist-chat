<template>
  <div class="background-img"></div>
  <div class="container">
    <div id="chat-response">
      <div class="bot-icon">
        <svg xmlns="http://www.w3.org/2000/svg" width="32" height="32" viewBox="0 0 24 24" fill="none"
          stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"
          class="lucide lucide-bot-icon lucide-bot">
          <path d="M12 8V4H8" />
          <rect width="16" height="12" x="4" y="8" rx="2" />
          <path d="M2 14h2" />
          <path d="M20 14h2" />
          <path d="M15 13v2" />
          <path d="M9 13v2" />
        </svg>
      </div>
      <h1 class="greeting">Welcome back, how can I assist you today?</h1>
    </div>
    <div class="bottom-search">
      <div class="search-container">
        <div class="search-input-wrapper">
          <textarea rows="1" class="search-input resize-none overflow-hidden" autocomplete="off" autocapitalize="on"
            placeholder="Type your question here..." v-model="userInput" @keydown.enter.prevent="handleSend" />
          <div class="icons">
            <span class="material-symbols-outlined send-icon" @click="handleSend">send</span>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    content: { type: Object, required: true }
  },
  data() {
    return {
      userInput: ''
    };
  },
  mounted() {
    const link = document.createElement('link');
    link.href = "https://cdn.jsdelivr.net/npm/bootstrap@5.3.2/dist/css/bootstrap.min.css";
    link.rel = "stylesheet";
    document.head.appendChild(link);
  },
  methods: {
    async handleSend() {
      const userMessage = this.userInput.trim();
      if (!userMessage) return;

      const chatContainer = document.getElementById("chat-response");

      const userDiv = document.createElement("div");
      userDiv.classList.add("self-chat");
      userDiv.textContent = "👤Me";

      const userBubble = document.createElement("div");
      userBubble.classList.add("chat-bubble", "user");
      userBubble.textContent = userMessage;

      chatContainer.appendChild(userDiv);
      chatContainer.appendChild(userBubble);

      this.userInput = "";

      const typingBubble = document.createElement("div");
      typingBubble.classList.add("chat-bubble", "bot", "typing");
      typingBubble.setAttribute("id", "typing-indicator");
      typingBubble.textContent = "FUB Assist is typing...";
      chatContainer.appendChild(typingBubble);

      try {
        const response = await fetch(`https://api.corporatesynergysolutions.com/api:eUcYQFg2/getIntentMessage?user_query=${encodeURIComponent(userMessage)}`, {
          method: "GET",
          headers: {
            Accept: "application/json",
            Authorization: "Bearer DSJKFJ45R43858778R87565687"
          }
        });

        const data = await response.json();
        document.getElementById("typing-indicator")?.remove();

        const rawContent = typeof data === "string" ? data : data.tool_response || data.assistant || "";
        const cleaned = rawContent.replace(/```html\n?|```/g, "").trim();

        const botBubble = document.createElement("div");
        const botDiv = document.createElement("div");
        const isHTML = /<\/?[a-z][\s\S]*>/i.test(cleaned);

        botDiv.classList.add("bot-chat");
        botDiv.textContent = "🤖 Bot";

        if (isHTML) {
          botBubble.classList.add("chat-bubble", "bot-html");
          botBubble.innerHTML = cleaned;
        } else {
          botBubble.classList.add("chat-bubble", "bot");
          botBubble.textContent = cleaned;
        }

        chatContainer.appendChild(botDiv);
        chatContainer.appendChild(botBubble);

        const scriptTags = botBubble.querySelectorAll("script");
        scriptTags.forEach((script) => {
          const newScript = document.createElement("script");
          if (script.src) {
            newScript.src = script.src;
          } else {
            newScript.textContent = script.textContent;
          }
          document.body.appendChild(newScript);
          document.body.removeChild(newScript);
        });
      } catch (error) {
        console.error("Error:", error);
        document.getElementById("typing-indicator")?.remove();

        const errorBubble = document.createElement("div");
        errorBubble.classList.add("chat-bubble", "bot");
        errorBubble.textContent = "An error occurred while fetching response.";
        chatContainer.appendChild(errorBubble);
      }
    }
  }
};
</script>

<style lang="scss">
@import url("https://fonts.googleapis.com/css2?family=Google+Sans:wght@400;500&display=swap");
@import url("https://fonts.googleapis.com/css2?family=Inter:ital,opsz,wght@0,14..32,100..900;1,14..32,100..900&display=swap");
@import url("https://fonts.googleapis.com/css2?family=Material+Symbols+Outlined:opsz,wght,FILL,GRAD@20..48,100..700,0..1,-50..200");
@import url("https://cdn.jsdelivr.net/npm/bootstrap@5.3.2/dist/css/bootstrap.min.css");
/**** Paste all original CSS from <style> here ****/
 :root {
            --primary-color: #70b4f6;
            --secondary-color: #f9a82c;
            --tertiary-color: #f0f4f8;
        }

        body {
            min-height: 100vh;
            width: 100%;
            background: #f0f4f8;
            position: relative;
            font-family: "Inter", sans-serif;
        }

        .background-img {
            position: fixed;
            top: 0;
            bottom: 0;
            left: 0;
            right: 0;
            background-image: url("https://api.corporatesynergysolutions.com/vault/bhLguVHK/ZWSxJF0CKUkQxluxuuGDtVYShX4/dwxTeg../7152d1e4-55c8-4de0-a711-92bdf3ba52f3.jpg");
            background-repeat: no-repeat;
            background-size: cover;
            z-index: -1;
        }

        .container {
            width: 100%;
            box-sizing: border-box;
            /* margin-left: auto !important;
        margin-right: auto !important; */
            display: flex;
            justify-content: center;
            align-items: center;
        }

        .search-container {
            width: 100%;
            max-width: 800px;
            background-color: #fff;
            border-radius: 28px;
            box-shadow: 0 4px 10px rgb(0 0 0 / 54%);
            display: flex;
            flex-direction: column;
            align-items: center;
            padding: 15px 64px 15px 20px;
            box-sizing: border-box;
            /* position: fixed;
      bottom: 20px; */
            z-index: 10;
            position: relative;
        }

        .search-input-wrapper {
            display: flex;
            align-items: center;
            width: 100%;
            /* position: relative; */
        }

        .search-input {
            /* min-height: 40px; */
            max-height: 150px;
            line-height: 1.4;
            width: 100%;
            resize: none;
            overflow-y: auto;
            border: none !important;
            outline: none;
        }

        .search-input::placeholder {
            color: #80868b;
        }

        /* message start */
        /* .chat-message */
        .chat-bubble {
            max-width: 70%;
            padding: 8px 14px;
            border-radius: 12px;
            font-size: 14px;
            line-height: 1.8;
            word-wrap: break-word;
            margin-bottom: 10px;
        }

        /* .chat-message.left, */
        .chat-bubble.bot {
            align-self: flex-start;
            background-color: #f0f4f8;
            border: 1px solid #70b4f6;
            color: #333;
            border-bottom-left-radius: 0;
        }

        /* .chat-message.right, */
        .chat-bubble.user {
            align-self: flex-end;
            background-color: #70b4f6;
            border: 1px solid rgb(58 155 254);
            color: #fff;
            border-bottom-right-radius: 0;
        }

        .self-chat {
            font-size: 14px;
            color: #6b6b6b;
            padding-right: 6px;
            padding-bottom: 2px;
            text-align: right;
        }

        .bot-chat {
            font-size: 14px;
            color: #6b6b6b;
            text-align: start;
            padding-left: 6px;
            padding-bottom: 2px;
        }

        /* message end */


        .icons {
            display: flex;
            align-items: center;
            position: absolute;
            right: 1.3rem;
            top: 50%;
            transform: translateY(-50%);
        }

        .material-symbols-outlined {
            font-variation-settings: "FILL" 0, "wght" 400, "GRAD" 0, "opsz" 24;
            color: #5f6368;
            cursor: pointer;
            font-size: 28px;
        }

        #chat-response {
            max-width: 950px;
            width: 100%;
            padding: 25px 10px;
            margin-bottom: 100px;
            box-sizing: border-box;
            /* text-align: center; */
            display: flex;
            flex-direction: column;
            /* align-items: center; */
        }

        #chat-response .bot-icon {
            display: flex;
            justify-content: center
        }

        #chat-response .greeting {
            color: #160211;
            text-align: center;
            font-size: 26px;
            font-style: normal;
            font-weight: 600;
            line-height: normal;
            margin-top: 12px;
            margin-bottom: 2rem;
        }

        .chat-bubble.bot-html {
            padding: 0;
            align-self: start;
        }

        .profile-container {
            display: grid !important;
            grid-template-columns: 1fr 1fr 1fr;
            column-gap: 0.8rem !important;
        }

        button:disabled {
            opacity: 0.6;
            cursor: not-allowed;
            display: flex;
        }

        .chat-bubble.typing {
            font-style: italic;
            opacity: 0.6;
        }

        .bottom-search {
            left: 0;
            position: fixed;
            bottom: 0;
            right: 0;
            background: #fff0;
            display: flex;
            justify-content: center;
            padding: 0 1rem 1rem;
        }

        @media (min-width: 576px) and (max-width: 1024px) {
            .profile-container {
                grid-template-columns: 1fr;
            }
        }

        @media screen and (max-width: 576px) {
            .greeting {
                font-size: 2rem;
                line-height: normal;
                margin-bottom: 1rem;
            }

            .chat-bubble {
                max-width: 100%;
            }

            .search-container {
                width: 100%;
            }

            .chat-bubble.bot-html {
                width: 100%;
            }

            .profile-container {
                grid-template-columns: 1fr;
            }
        }
</style>
