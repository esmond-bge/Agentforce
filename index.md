---
layout: default
title: Chat
---

<style>
    #bot-launcher {
        position: fixed;
        right: 24px;
        bottom: 24px;
        width: 64px;
        height: 64px;
        border: none;
        border-radius: 50%;
        background: #006f44;
        color: #ffffff;
        cursor: pointer;
        z-index: 999998;
        display: flex;
        align-items: center;
        justify-content: center;
        box-shadow: 0 5px 18px rgba(0, 0, 0, 0.28);
        transition: transform 0.2s ease, background 0.2s ease;
    }

    #bot-launcher:hover {
        background: #005637;
        transform: scale(1.05);
    }

    #bot-launcher:focus-visible,
    .bot-option:focus-visible,
    #bot-selector-close:focus-visible,
    #custom-chat-minimise:focus-visible {
        outline: 3px solid #ffb81c;
        outline-offset: 3px;
    }

    #bot-launcher svg {
        width: 30px;
        height: 30px;
        fill: currentColor;
    }

    #bot-selector-backdrop {
        position: fixed;
        inset: 0;
        z-index: 999999;
        display: none;
        align-items: center;
        justify-content: center;
        padding: 20px;
        background: rgba(0, 0, 0, 0.5);
    }

    #bot-selector-backdrop.is-open {
        display: flex;
    }

    #bot-selector {
        position: relative;
        width: min(420px, 100%);
        padding: 28px;
        border-radius: 16px;
        background: #ffffff;
        color: #1f2937;
        box-shadow: 0 20px 45px rgba(0, 0, 0, 0.3);
    }

    #bot-selector h2 {
        margin: 0 40px 8px 0;
        font-size: 24px;
        line-height: 1.25;
    }

    #bot-selector p {
        margin: 0 0 22px;
        color: #5f6670;
    }

    #bot-selector-close {
        position: absolute;
        top: 14px;
        right: 14px;
        width: 38px;
        height: 38px;
        border: none;
        border-radius: 50%;
        background: transparent;
        color: #30343b;
        font-size: 28px;
        line-height: 1;
        cursor: pointer;
    }

    #bot-selector-close:hover {
        background: #f1f3f5;
    }

    .bot-options {
        display: grid;
        gap: 12px;
    }

    .bot-option {
        width: 100%;
        padding: 16px 18px;
        border: 2px solid #d8dde2;
        border-radius: 12px;
        background: #ffffff;
        color: #1f2937;
        text-align: left;
        cursor: pointer;
    }

    .bot-option:hover {
        border-color: #006f44;
        background: #f3faf6;
    }

    .bot-option strong {
        display: block;
        margin-bottom: 4px;
        font-size: 17px;
    }

    .bot-option span {
        color: #646b75;
        font-size: 14px;
    }

    #chat-frame-container {
        position: fixed;
        right: 16px;
        bottom: 16px;
        width: min(420px, calc(100vw - 32px));
        height: min(720px, calc(100vh - 32px));
        z-index: 1000000;
        display: none;
        overflow: hidden;
        border-radius: 14px;
        background: transparent;
    }

    #chat-frame-container.is-open {
        display: block;
    }

    #chat-frame {
        width: 100%;
        height: 100%;
        border: 0;
        background: transparent;
    }

    #custom-chat-minimise {
        position: absolute;
        top: 7px;
        right: 54px;
        z-index: 1000002;
        width: 42px;
        height: 38px;
        border: none;
        border-radius: 8px;
        background: rgba(255, 255, 255, 0.96);
        color: #202124;
        font-size: 25px;
        line-height: 1;
        cursor: pointer;
        box-shadow: 0 2px 7px rgba(0, 0, 0, 0.16);
    }

    #custom-chat-minimise:hover {
        background: #f1f3f5;
    }

    #chat-loading {
        position: absolute;
        inset: 0;
        z-index: 1000001;
        display: flex;
        align-items: center;
        justify-content: center;
        padding: 24px;
        background: #ffffff;
        color: #30343b;
        text-align: center;
        font-family: Arial, sans-serif;
    }

    #chat-loading.is-hidden {
        display: none;
    }

    #chat-status {
        position: fixed;
        left: 16px;
        bottom: 16px;
        z-index: 1000001;
        max-width: 340px;
        padding: 12px 14px;
        border-radius: 8px;
        background: #a9252a;
        color: #ffffff;
        font-size: 14px;
        display: none;
    }

    @media (max-width: 600px) {
        #chat-frame-container {
            right: 0;
            bottom: 0;
            width: 100vw;
            height: 100dvh;
            border-radius: 0;
        }

        #bot-launcher {
            right: 18px;
            bottom: 18px;
        }
    }
</style>

<button
    id="bot-launcher"
    type="button"
    aria-label="Open chat options"
    title="Chat with us"
>
    <svg viewBox="0 0 24 24" aria-hidden="true">
        <path d="M4 4h16a2 2 0 0 1 2 2v10a2 2 0 0 1-2 2H8l-5 4v-4.5A2 2 0 0 1 2 16V6a2 2 0 0 1 2-2Zm2.5 6a1.5 1.5 0 1 0 0 3 1.5 1.5 0 0 0 0-3Zm5.5 0a1.5 1.5 0 1 0 0 3 1.5 1.5 0 0 0 0-3Zm5.5 0a1.5 1.5 0 1 0 0 3 1.5 1.5 0 0 0 0-3Z"/>
    </svg>
</button>

<div
    id="bot-selector-backdrop"
    role="dialog"
    aria-modal="true"
    aria-labelledby="bot-selector-title"
>
    <div id="bot-selector">
        <button
            id="bot-selector-close"
            type="button"
            aria-label="Close chat options"
        >
            &times;
        </button>
        <h2 id="bot-selector-title">How can we help?</h2>
        <p>Select the chat experience that you want to launch.</p>
        <div class="bot-options">
            <button
                class="bot-option"
                type="button"
                data-bot="agentforce"
            >
                <strong>Agentforce</strong>
                <span>Start the Agentforce sales conversation.</span>
            </button>
            <button
                class="bot-option"
                type="button"
                data-bot="einstein"
            >
                <strong>Einstein</strong>
                <span>Start the Einstein service conversation.</span>
            </button>
        </div>
    </div>
</div>

<div id="chat-frame-container">
    <div id="chat-loading">Loading chat...</div>
    <button
        id="custom-chat-minimise"
        type="button"
        aria-label="Minimise chat"
        title="Minimise chat"
    >
        &minus;
    </button>
    <iframe
        id="chat-frame"
        title="Chat"
        allow="microphone; camera"
    ></iframe>
</div>

<div id="chat-status" role="alert"></div>

<script type="text/javascript">
(function () {
    "use strict";

    const ORG_ID = "00DRR00000V4G17";

    const SCRT2_URL =
        "https://bordgaisenergyeandu--afdev2.sandbox.my.salesforce-scrt.com";

    const BOT_CONFIG = {
        agentforce: {
            name: "Agentforce",
            deploymentName: "MIAWPOC_Github",
            siteURL:
                "https://bordgaisenergyeandu--afdev2.sandbox.my.site.com/ESWMIAWPOCGithub1793617406174",
            bootstrapURL:
                "https://bordgaisenergyeandu--afdev2.sandbox.my.site.com/ESWMIAWPOCGithub1793617406174/assets/js/bootstrap.min.js"
        },
        einstein: {
            name: "Einstein",
            deploymentName: "BGE_Github_Deployment",
            siteURL:
                "https://bordgaisenergyeandu--afdev2.sandbox.my.site.com/ESWBGEGithubDep1791797820412",
            bootstrapURL:
                "https://bordgaisenergyeandu--afdev2.sandbox.my.site.com/ESWBGEGithubDep1791797820412/assets/js/bootstrap.min.js"
        }
    };

    const launcher = document.getElementById("bot-launcher");
    const selectorBackdrop = document.getElementById(
        "bot-selector-backdrop"
    );
    const selectorClose = document.getElementById("bot-selector-close");
    const chatContainer = document.getElementById(
        "chat-frame-container"
    );
    const chatFrame = document.getElementById("chat-frame");
    const chatLoading = document.getElementById("chat-loading");
    const chatMinimise = document.getElementById(
        "custom-chat-minimise"
    );
    const chatStatus = document.getElementById("chat-status");
    const botOptions = document.querySelectorAll(".bot-option");

    let activeBot = null;
    let activeFrameLoaded = false;
    let lastFocusedElement = null;

    function openSelector() {
        lastFocusedElement = document.activeElement;
        selectorBackdrop.classList.add("is-open");
        selectorClose.focus();
    }

    function closeSelector() {
        selectorBackdrop.classList.remove("is-open");

        if (
            lastFocusedElement &&
            typeof lastFocusedElement.focus === "function"
        ) {
            lastFocusedElement.focus();
        }
    }

    function showStatus(message) {
        chatStatus.textContent = message;
        chatStatus.style.display = "block";

        window.setTimeout(function () {
            chatStatus.style.display = "none";
        }, 7000);
    }

    function minimiseChat() {
        chatContainer.classList.remove("is-open");
        launcher.style.display = "flex";
        launcher.focus();
    }

    function showChatContainer() {
        closeSelector();
        launcher.style.display = "none";
        chatContainer.classList.add("is-open");
    }

    function createFrameDocument(config) {
        const safeConfig = JSON.stringify({
            orgId: ORG_ID,
            deploymentName: config.deploymentName,
            siteURL: config.siteURL,
            bootstrapURL: config.bootstrapURL,
            scrt2URL: SCRT2_URL
        }).replace(/</g, "\\u003c");

        return `
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta
        name="viewport"
        content="width=device-width, initial-scale=1.0"
    >

    <style>
        html,
        body {
            width: 100%;
            height: 100%;
            margin: 0;
            overflow: hidden;
            background: transparent;
        }

        /*
         * These selectors provide an additional safeguard against
         * Salesforce displaying its standard launcher button.
         */
        #embeddedMessagingConversationButton,
        .embeddedMessagingConversationButton,
        .embeddedMessagingConversationButtonWrapper,
        button.embeddedMessagingConversationButton {
            display: none !important;
            visibility: hidden !important;
            opacity: 0 !important;
            pointer-events: none !important;
        }
    </style>
</head>

<body>
    <script>
        (function () {
            "use strict";
            const config = ${safeConfig};
            let launchRequested = false;
            function sendToParent(type, detail) {
                window.parent.postMessage(
                    {
                        source: "bge-embedded-chat",
                        type: type,
                        detail: detail || null,
                        deploymentName: config.deploymentName
                    },
                    "*"
                );
            }
            function launchChat() {
                if (
                    window.embeddedservice_bootstrap &&
                    embeddedservice_bootstrap.utilAPI &&
                    typeof embeddedservice_bootstrap.utilAPI.launchChat ===
                        "function"
                ) {
                    embeddedservice_bootstrap.utilAPI
                        .launchChat()
                        .then(function () {
                            sendToParent("chat-launched");
                        })
                        .catch(function (error) {
                            sendToParent(
                                "chat-error",
                                error && error.message
                                    ? error.message
                                    : "The chat could not be launched."
                            );
                        });
                    return;
                }
                launchRequested = true;
            }
            window.addEventListener(
                "onEmbeddedMessagingButtonCreated",
                function () {
                    try {
                        embeddedservice_bootstrap.utilAPI.hideChatButton();
                    } catch (error) {
                        console.warn(
                            "Could not hide the Salesforce launcher.",
                            error
                        );
                    }
                    sendToParent("chat-ready");
                    if (launchRequested) {
                        launchRequested = false;
                        launchChat();
                    }
                }
            );
            window.addEventListener(
                "onEmbeddedMessagingConversationOpened",
                function () {
                    sendToParent("conversation-opened");
                }
            );
            window.addEventListener(
                "onEmbeddedMessagingConversationClosed",
                function () {
                    sendToParent("conversation-closed");
                }
            );
            window.addEventListener("message", function (event) {
                if (!event.data || event.data.source !== "bge-chat-parent") {
                    return;
                }
                if (event.data.type === "launch-chat") {
                    launchChat();
                }
            });
            window.initEmbeddedMessaging = function () {
                try {
                    embeddedservice_bootstrap.settings.language = "en_US";
                    embeddedservice_bootstrap.settings
                        .hideChatButtonOnLoad = true;
                    embeddedservice_bootstrap.init(
                        config.orgId,
                        config.deploymentName,
                        config.siteURL,
                        {
                            scrt2URL: config.scrt2URL
                        }
                    );
                } catch (error) {
                    sendToParent(
                        "initialisation-error",
                        error && error.message
                            ? error.message
                            : "Embedded Messaging failed to initialise."
                    );
                }
            };
            const bootstrapScript = document.createElement("script");
            bootstrapScript.type = "text/javascript";
            bootstrapScript.src = config.bootstrapURL;
            bootstrapScript.onload = window.initEmbeddedMessaging;
            bootstrapScript.onerror = function () {
                sendToParent(
                    "initialisation-error",
                    "The Salesforce bootstrap file could not be loaded."
                );
            };
            document.body.appendChild(bootstrapScript);
        })();
    <\/script>
</body>
</html>`;
    }
    function sendLaunchCommand() {
        if (!chatFrame.contentWindow) {
            return;
        }
        chatFrame.contentWindow.postMessage(
            {
                source: "bge-chat-parent",
                type: "launch-chat"
            },
            "*"
        );
    }
    function launchBot(botKey) {
        const config = BOT_CONFIG[botKey];
        if (!config) {
            showStatus("The selected chat deployment is not configured.");
            return;
        }
        showChatContainer();
        chatLoading.classList.remove("is-hidden");
        chatLoading.textContent = "Loading " + config.name + "...";
        /*
         * If the same bot was previously loaded, reopen its existing
         * conversation instead of reloading the deployment.
         */
        if (
            activeBot === botKey &&
            activeFrameLoaded &&
            chatFrame.contentWindow
        ) {
            sendLaunchCommand();
            return;
        }
        /*
         * Loading a different bot creates a fresh isolated iframe.
         * This prevents the two deployments from overwriting the same
         * embeddedservice_bootstrap global variable.
         */
        activeBot = botKey;
        activeFrameLoaded = false;
        chatFrame.srcdoc = createFrameDocument(config);
    }
    launcher.addEventListener("click", function () {
        /*
         * If a bot is already loaded, clicking the generic icon
         * restores that bot directly.
         */
        if (activeBot && activeFrameLoaded) {
            showChatContainer();
            sendLaunchCommand();
            return;
        }
        openSelector();
    });
    selectorClose.addEventListener("click", closeSelector);
    selectorBackdrop.addEventListener("click", function (event) {
        if (event.target === selectorBackdrop) {
            closeSelector();
        }
    });
    botOptions.forEach(function (button) {
        button.addEventListener("click", function () {
            launchBot(button.dataset.bot);
        });
    });
    chatMinimise.addEventListener("click", minimiseChat);
    window.addEventListener("keydown", function (event) {
        if (
            event.key === "Escape" &&
            selectorBackdrop.classList.contains("is-open")
        ) {
            closeSelector();
        }
    });
    window.addEventListener("message", function (event) {
        const message = event.data;
        if (
            !message ||
            message.source !== "bge-embedded-chat"
        ) {
            return;
        }
        if (
            activeBot &&
            message.deploymentName !==
                BOT_CONFIG[activeBot].deploymentName
        ) {
            return;
        }
        switch (message.type) {
            case "chat-ready":
                activeFrameLoaded = true;
                sendLaunchCommand();
                break;
            case "chat-launched":
            case "conversation-opened":
                chatLoading.classList.add("is-hidden");
                break;
            case "conversation-closed":
                activeBot = null;
                activeFrameLoaded = false;
                chatFrame.srcdoc = "";
                minimiseChat();
                break;
            case "chat-error":
            case "initialisation-error":
                chatLoading.classList.add("is-hidden");
                showStatus(
                    message.detail ||
                    "An error occurred while loading the chat."
                );
                minimiseChat();
                break;
        }
    });
})();
</script>
