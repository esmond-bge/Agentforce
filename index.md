---
layout: default
title: Agentforce
---

<div markdown="0">

<style>
    #bot-launcher {
        position: fixed;
        right: 24px;
        bottom: 24px;
        z-index: 999998;
        display: flex;
        width: 64px;
        height: 64px;
        padding: 0;
        align-items: center;
        justify-content: center;
        border: 0;
        border-radius: 50%;
        background: #006f44;
        color: #ffffff;
        cursor: pointer;
        box-shadow: 0 5px 18px rgba(0, 0, 0, 0.28);
        transition: background 0.2s ease, transform 0.2s ease;
    }

    #bot-launcher:hover {
        background: #005637;
        transform: scale(1.05);
    }

    #bot-launcher:focus-visible,
    #bot-selector-close:focus-visible,
    .bot-option:focus-visible {
        outline: 3px solid #ffb81c;
        outline-offset: 3px;
    }

    #bot-launcher svg {
        width: 30px;
        height: 30px;
        fill: currentColor;
    }

    #bot-selector-window {
        position: fixed;
        right: 24px;
        bottom: 24px;
        z-index: 999999;
        display: none;
        overflow: hidden;
        width: min(400px, calc(100vw - 32px));
        height: min(570px, calc(100vh - 32px));
        border-radius: 16px;
        background: #ffffff;
        color: #1f2937;
        font-family: Arial, sans-serif;
        box-shadow: 0 10px 38px rgba(0, 0, 0, 0.3);
    }

    #bot-selector-window.is-open {
        display: flex;
        flex-direction: column;
    }

    #bot-selector-header {
        display: flex;
        min-height: 72px;
        box-sizing: border-box;
        align-items: center;
        justify-content: space-between;
        padding: 16px 18px;
        background: #006f44;
        color: #ffffff;
    }

    #bot-selector-header-content {
        display: flex;
        align-items: center;
        gap: 12px;
    }

    #bot-selector-header-icon {
        display: flex;
        width: 38px;
        height: 38px;
        flex: 0 0 38px;
        align-items: center;
        justify-content: center;
        border-radius: 50%;
        background: rgba(255, 255, 255, 0.16);
    }

    #bot-selector-header-icon svg {
        width: 22px;
        height: 22px;
        fill: currentColor;
    }

    #bot-selector-header-text h2 {
        margin: 0;
        color: #ffffff;
        font-size: 18px;
        font-weight: 600;
    }

    #bot-selector-header-text p {
        margin: 3px 0 0;
        color: rgba(255, 255, 255, 0.88);
        font-size: 13px;
    }

    #bot-selector-close {
        display: flex;
        width: 40px;
        height: 40px;
        padding: 0;
        align-items: center;
        justify-content: center;
        border: 0;
        border-radius: 50%;
        background: transparent;
        color: #ffffff;
        cursor: pointer;
    }

    #bot-selector-close:hover {
        background: rgba(255, 255, 255, 0.15);
    }

    #bot-selector-close svg {
        width: 22px;
        height: 22px;
        fill: currentColor;
    }

    #bot-selector-body {
        display: flex;
        flex: 1;
        flex-direction: column;
        box-sizing: border-box;
        padding: 24px 20px;
        background: #f7f8f9;
    }

    #bot-welcome-message {
        max-width: 88%;
        margin-bottom: 22px;
        padding: 14px 16px;
        border-radius: 4px 14px 14px 14px;
        background: #ffffff;
        color: #30343b;
        font-size: 15px;
        line-height: 1.45;
        box-shadow: 0 2px 8px rgba(0, 0, 0, 0.08);
    }

    #bot-welcome-message p {
        margin: 0;
    }

    #bot-welcome-message p + p {
        margin-top: 8px;
    }

    .bot-options {
        display: grid;
        gap: 12px;
    }

    .bot-option {
        display: flex;
        box-sizing: border-box;
        width: 100%;
        min-height: 76px;
        padding: 14px 16px;
        align-items: center;
        gap: 14px;
        border: 1px solid #d5d9dd;
        border-radius: 12px;
        background: #ffffff;
        color: #1f2937;
        font-family: Arial, sans-serif;
        text-align: left;
        cursor: pointer;
        box-shadow: 0 2px 6px rgba(0, 0, 0, 0.05);
        transition: border-color 0.2s ease, background 0.2s ease;
    }

    .bot-option:hover {
        border-color: #006f44;
        background: #f2faf6;
    }

    .bot-option-icon {
        display: flex;
        width: 42px;
        height: 42px;
        flex: 0 0 42px;
        align-items: center;
        justify-content: center;
        border-radius: 50%;
        background: #e9f5ef;
        color: #006f44;
    }

    .bot-option-icon svg {
        width: 23px;
        height: 23px;
        fill: currentColor;
    }

    .bot-option-content {
        min-width: 0;
        flex: 1;
    }

    .bot-option strong {
        display: block;
        margin-bottom: 4px;
        color: #1f2937;
        font-size: 16px;
    }

    .bot-option span {
        color: #646b75;
        font-size: 13px;
    }

    .bot-option-arrow {
        color: #006f44;
        font-size: 24px !important;
    }

    #bot-selector-footer {
        box-sizing: border-box;
        padding: 14px 18px;
        border-top: 1px solid #e1e4e8;
        background: #ffffff;
        color: #6b7280;
        font-size: 12px;
        text-align: center;
    }

    #salesforce-chat-container {
        position: fixed;
        right: 16px;
        bottom: 16px;
        z-index: 1000000;
        display: none;
        overflow: hidden;
        width: min(440px, calc(100vw - 32px));
        height: min(720px, calc(100vh - 32px));
        border-radius: 16px;
        background: transparent;
    }

    #salesforce-chat-container.is-open {
        display: block;
    }

    .salesforce-chat-frame {
        display: none;
        width: 100%;
        height: 100%;
        border: 0;
        background: transparent;
    }

    .salesforce-chat-frame.is-active {
        display: block;
    }

    #chat-loading {
        position: absolute;
        inset: 0;
        z-index: 10;
        display: flex;
        box-sizing: border-box;
        align-items: center;
        justify-content: center;
        padding: 24px;
        background: #ffffff;
        color: #30343b;
        font-family: Arial, sans-serif;
        text-align: center;
    }

    #chat-loading.is-hidden {
        display: none;
    }

    #chat-loading-title {
        display: block;
        margin-bottom: 8px;
        font-size: 17px;
    }

    #chat-loading-message {
        color: #646b75;
        font-size: 14px;
    }

    #chat-status {
        position: fixed;
        left: 16px;
        bottom: 16px;
        z-index: 1000002;
        display: none;
        max-width: 360px;
        padding: 12px 14px;
        border-radius: 8px;
        background: #a9252a;
        color: #ffffff;
        font-family: Arial, sans-serif;
        font-size: 14px;
        box-shadow: 0 4px 14px rgba(0, 0, 0, 0.25);
    }

    @media (max-width: 600px) {
        #bot-launcher {
            right: 18px;
            bottom: 18px;
        }

        #bot-selector-window {
            right: 0;
            bottom: 0;
            width: 100vw;
            height: min(620px, 100dvh);
            border-radius: 16px 16px 0 0;
        }

        #salesforce-chat-container {
            right: 0;
            bottom: 0;
            width: 100vw;
            height: 100dvh;
            border-radius: 0;
        }
    }
</style>


<!-- GENERIC CHAT LAUNCHER -->

<button
    id="bot-launcher"
    type="button"
    aria-label="Open chat"
    title="Chat with us"
>
    <svg viewBox="0 0 24 24" aria-hidden="true">
        <path d="M4 4h16a2 2 0 0 1 2 2v10a2 2 0 0 1-2 2H8l-5 4v-4.5A2 2 0 0 1 2 16V6a2 2 0 0 1 2-2Zm2.5 6a1.5 1.5 0 1 0 0 3 1.5 1.5 0 0 0 0-3Zm5.5 0a1.5 1.5 0 1 0 0 3 1.5 1.5 0 0 0 0-3Zm5.5 0a1.5 1.5 0 1 0 0 3 1.5 1.5 0 0 0 0-3Z"></path>
    </svg>
</button>


<!-- BOT SELECTOR -->

<div
    id="bot-selector-window"
    role="dialog"
    aria-modal="false"
    aria-labelledby="bot-selector-title"
>
    <div id="bot-selector-header">

        <div id="bot-selector-header-content">

            <div id="bot-selector-header-icon">
                <svg viewBox="0 0 24 24" aria-hidden="true">
                    <path d="M4 4h16a2 2 0 0 1 2 2v10a2 2 0 0 1-2 2H8l-5 4v-4.5A2 2 0 0 1 2 16V6a2 2 0 0 1 2-2Zm2.5 6a1.5 1.5 0 1 0 0 3 1.5 1.5 0 0 0 0-3Zm5.5 0a1.5 1.5 0 1 0 0 3 1.5 1.5 0 0 0 0-3Zm5.5 0a1.5 1.5 0 1 0 0 3 1.5 1.5 0 0 0 0-3Z"></path>
                </svg>
            </div>

            <div id="bot-selector-header-text">
                <h2 id="bot-selector-title">Chat with us</h2>
                <p>Select an assistant to continue</p>
            </div>

        </div>

        <button
            id="bot-selector-close"
            type="button"
            aria-label="Minimise"
            title="Minimise"
        >
            <svg viewBox="0 0 24 24" aria-hidden="true">
                <path d="M6 11h12v2H6z"></path>
            </svg>
        </button>

    </div>

    <div id="bot-selector-body">

        <div id="bot-welcome-message">
            <p><strong>How can we help?</strong></p>
            <p>Select the assistant you would like to chat with.</p>
        </div>

        <div class="bot-options">

            <button
                class="bot-option"
                type="button"
                data-bot="agentforce"
            >
                <span class="bot-option-icon">
                    <svg viewBox="0 0 24 24" aria-hidden="true">
                        <path d="M12 2a4 4 0 0 1 4 4v1h1a4 4 0 0 1 4 4v4a4 4 0 0 1-4 4h-1.55L12 22l-3.45-3H7a4 4 0 0 1-4-4v-4a4 4 0 0 1 4-4h1V6a4 4 0 0 1 4-4Zm-2 5h4V6a2 2 0 1 0-4 0v1Zm-3 2a2 2 0 0 0-2 2v4a2 2 0 0 0 2 2h2.3l2.7 2.35L14.7 17H17a2 2 0 0 0 2-2v-4a2 2 0 0 0-2-2H7Z"></path>
                    </svg>
                </span>

                <span class="bot-option-content">
                    <strong>Agentforce</strong>
                    <span>Start the Agentforce conversation.</span>
                </span>

                <span class="bot-option-arrow">›</span>
            </button>


            <button
                class="bot-option"
                type="button"
                data-bot="einstein"
            >
                <span class="bot-option-icon">
                    <svg viewBox="0 0 24 24" aria-hidden="true">
                        <path d="M12 2a9 9 0 0 1 9 9c0 3.04-1.51 5.73-3.82 7.36L18 22l-3.75-2A8.92 8.92 0 0 1 12 20a9 9 0 1 1 0-18Zm0 2a7 7 0 1 0 1.82 13.76l.38-.1 1.09.58-.26-1.17.5-.32A7 7 0 0 0 12 4Z"></path>
                    </svg>
                </span>

                <span class="bot-option-content">
                    <strong>Einstein</strong>
                    <span>Start the Einstein conversation.</span>
                </span>

                <span class="bot-option-arrow">›</span>
            </button>

        </div>

    </div>

    <div id="bot-selector-footer">
        Bord Gáis Energy virtual assistance
    </div>

</div>


<!-- SALESFORCE CHAT HOST -->

<div id="salesforce-chat-container">

    <div id="chat-loading">

        <div>
            <strong id="chat-loading-title">
                Connecting...
            </strong>

            <div id="chat-loading-message">
                Please wait while the conversation opens.
            </div>
        </div>

    </div>

    <iframe
        id="agentforce-chat-frame"
        class="salesforce-chat-frame"
        title="Agentforce chat"
        allow="microphone; camera"
    ></iframe>

    <iframe
        id="einstein-chat-frame"
        class="salesforce-chat-frame"
        title="Einstein chat"
        allow="microphone; camera"
    ></iframe>

</div>


<div
    id="chat-status"
    role="alert"
    aria-live="assertive"
></div>


<script type="text/javascript">

(function () {

    "use strict";


    /*************************************************
     * SALESFORCE CONFIGURATION
     *************************************************/

    var ORG_ID = "00DRR00000V4G17";

    var SCRT2_URL =
        "https://bordgaisenergyeandu--afdev2.sandbox.my.salesforce-scrt.com";


    var BOT_CONFIG = {

        agentforce: {

            key: "agentforce",

            name: "Agentforce",

            deploymentName: "MIAWPOC_Github",

            siteURL:
                "https://bordgaisenergyeandu--afdev2.sandbox.my.site.com/ESWMIAWPOCGithub1793617406174",

            bootstrapURL:
                "https://bordgaisenergyeandu--afdev2.sandbox.my.site.com/ESWMIAWPOCGithub1793617406174/assets/js/bootstrap.min.js",

            frameId:
                "agentforce-chat-frame"
        },


        einstein: {

            key: "einstein",

            name: "Einstein",

            deploymentName: "BGE_Github_Deployment",

            siteURL:
                "https://bordgaisenergyeandu--afdev2.sandbox.my.site.com/ESWBGEGithubDep1791797820412",

            bootstrapURL:
                "https://bordgaisenergyeandu--afdev2.sandbox.my.site.com/ESWBGEGithubDep1791797820412/assets/js/bootstrap.min.js",

            frameId:
                "einstein-chat-frame"
        }

    };


    /*************************************************
     * DOM ELEMENTS
     *************************************************/

    var launcher =
        document.getElementById("bot-launcher");

    var selectorWindow =
        document.getElementById("bot-selector-window");

    var selectorClose =
        document.getElementById("bot-selector-close");

    var chatContainer =
        document.getElementById("salesforce-chat-container");

    var chatLoading =
        document.getElementById("chat-loading");

    var chatLoadingTitle =
        document.getElementById("chat-loading-title");

    var chatLoadingMessage =
        document.getElementById("chat-loading-message");

    var chatStatus =
        document.getElementById("chat-status");

    var botOptions =
        document.querySelectorAll(".bot-option");


    /*************************************************
     * BOT STATE
     *************************************************/

    var botState = {

        agentforce: {
            ready: false,
            initializing: false,
            failed: false,
            launchRequested: false
        },

        einstein: {
            ready: false,
            initializing: false,
            failed: false,
            launchRequested: false
        }

    };


    var activeBot = null;

    var connectionTimer = null;


    /*************************************************
     * GENERAL UI
     *************************************************/

    function getFrame(botKey) {

        var config = BOT_CONFIG[botKey];

        if (!config) {
            return null;
        }

        return document.getElementById(
            config.frameId
        );
    }


    function showLauncher() {
        launcher.style.display = "flex";
    }


    function hideLauncher() {
        launcher.style.display = "none";
    }


    function openSelector() {

        chatContainer.classList.remove(
            "is-open"
        );

        selectorWindow.classList.add(
            "is-open"
        );

        hideLauncher();
    }


    function closeSelector() {

        selectorWindow.classList.remove(
            "is-open"
        );

        showLauncher();

        launcher.focus();
    }


    function showChatContainer() {

        selectorWindow.classList.remove(
            "is-open"
        );

        chatContainer.classList.add(
            "is-open"
        );

        hideLauncher();
    }


    function showLoading(botName) {

        chatLoadingTitle.textContent =
            "Opening " + botName;

        chatLoadingMessage.textContent =
            "Please wait while the conversation opens.";

        chatLoading.classList.remove(
            "is-hidden"
        );
    }


    function hideLoading() {

        chatLoading.classList.add(
            "is-hidden"
        );

        if (connectionTimer) {

            window.clearTimeout(
                connectionTimer
            );

            connectionTimer = null;
        }
    }


    function showStatus(message) {

        chatStatus.textContent = message;

        chatStatus.style.display =
            "block";

        window.setTimeout(
            function () {
                chatStatus.style.display =
                    "none";
            },
            10000
        );
    }


    function activateFrame(botKey) {

        Object.keys(BOT_CONFIG)
            .forEach(function (key) {

                var frame =
                    getFrame(key);

                if (!frame) {
                    return;
                }

                if (key === botKey) {

                    frame.classList.add(
                        "is-active"
                    );

                } else {

                    frame.classList.remove(
                        "is-active"
                    );
                }

            });
    }


    function resetVisibleChat() {

        activeBot = null;

        hideLoading();

        chatContainer.classList.remove(
            "is-open"
        );

        Object.keys(BOT_CONFIG)
            .forEach(function (key) {

                var frame =
                    getFrame(key);

                if (frame) {

                    frame.classList.remove(
                        "is-active"
                    );
                }

            });

        showLauncher();
    }


    /*************************************************
     * FRAME COMMUNICATION
     *************************************************/

    function sendFrameCommand(
        botKey,
        command
    ) {

        var frame =
            getFrame(botKey);

        if (
            !frame ||
            !frame.contentWindow
        ) {
            return;
        }


        frame.contentWindow.postMessage(
            {
                source:
                    "bge-chat-parent",

                type:
                    command,

                botKey:
                    botKey
            },
            "*"
        );
    }


    /*************************************************
     * CREATE ISOLATED SALESFORCE ENVIRONMENT
     *************************************************/

    function createFrameDocument(config) {

        var safeConfig =
            JSON.stringify({

                botKey:
                    config.key,

                orgId:
                    ORG_ID,

                deploymentName:
                    config.deploymentName,

                siteURL:
                    config.siteURL,

                bootstrapURL:
                    config.bootstrapURL,

                scrt2URL:
                    SCRT2_URL

            }).replace(
                /</g,
                "\\u003c"
            );


        return [

            "<!DOCTYPE html>",

            "<html lang=\"en\">",

            "<head>",

            "<meta charset=\"UTF-8\">",

            "<meta name=\"viewport\" content=\"width=device-width, initial-scale=1.0\">",

            "<style>",

            "html, body {",

            "width:100%;",
            "height:100%;",
            "margin:0;",
            "overflow:hidden;",
            "background:transparent;",

            "}",

            "</style>",

            "</head>",

            "<body>",


            "<script type=\"text/javascript\">",


            "(function(){",

            "\"use strict\";",


            "var config=" +
                safeConfig +
                ";",


            "var messagingReady=false;",

            "var launchPending=false;",


            "function notifyParent(type,detail){",

            "window.parent.postMessage({",

            "source:\"bge-embedded-chat\",",

            "type:type,",

            "detail:detail||null,",

            "botKey:config.botKey,",

            "deploymentName:config.deploymentName",

            "},\"*\");",

            "}",


            "function hideNativeLauncher(){",

            "try{",

            "if(window.embeddedservice_bootstrap &&",

            "embeddedservice_bootstrap.utilAPI &&",

            "typeof embeddedservice_bootstrap.utilAPI.hideChatButton === \"function\"){",

            "embeddedservice_bootstrap.utilAPI.hideChatButton();",

            "}",

            "}catch(error){",

            "console.warn(\"Unable to hide Salesforce launcher\",error);",

            "}",

            "}",


            "function launchChat(){",

            "if(!messagingReady){",

            "launchPending=true;",

            "return;",

            "}",


            "if(!window.embeddedservice_bootstrap ||",

            "!embeddedservice_bootstrap.utilAPI ||",

            "typeof embeddedservice_bootstrap.utilAPI.launchChat !== \"function\"){",

            "notifyParent(\"chat-error\",\"Launch Chat API unavailable.\");",

            "return;",

            "}",


            "embeddedservice_bootstrap.utilAPI.launchChat()",

            ".then(function(){",

            "notifyParent(\"chat-launched\");",

            "})",

            ".catch(function(error){",

            "notifyParent(",

            "\"chat-error\",",

            "error && error.message ?",

            "error.message :",

            "\"The selected chat could not be launched.\"" ,

            ");",

            "});",

            "}",


            "window.addEventListener(",

            "\"onEmbeddedMessagingReady\",",

            "function(){",

            "messagingReady=true;",

            "hideNativeLauncher();",

            "notifyParent(\"chat-ready\");",


            "if(launchPending){",

            "launchPending=false;",

            "launchChat();",

            "}",

            "}",

            ");",


            "window.addEventListener(",

            "\"onEmbeddedMessagingButtonCreated\",",

            "function(){",

            "hideNativeLauncher();",

            "}",

            ");",


            "window.addEventListener(",

            "\"onEmbeddedMessagingConversationOpened\",",

            "function(){",

            "notifyParent(\"conversation-opened\");",

            "}",

            ");",


            "window.addEventListener(",

            "\"onEmbeddedMessagingConversationClosed\",",

            "function(){",

            "hideNativeLauncher();",

            "notifyParent(\"conversation-closed\");",

            "}",

            ");",


            "window.addEventListener(",

            "\"message\",",

            "function(event){",

            "if(!event.data ||",

            "event.data.source !== \"bge-chat-parent\" ||",

            "event.data.botKey !== config.botKey){",

            "return;",

            "}",


            "if(event.data.type === \"launch-chat\"){",

            "launchChat();",

            "}",

            "}",

            ");",


            "window.initEmbeddedMessaging=function(){",

            "try{",

            "embeddedservice_bootstrap.settings.language=\"en_US\";",

            "embeddedservice_bootstrap.settings.hideChatButtonOnLoad=true;",


            "embeddedservice_bootstrap.init(",

            "config.orgId,",

            "config.deploymentName,",

            "config.siteURL,",

            "{scrt2URL:config.scrt2URL}",

            ");",

            "}",

            "catch(error){",

            "notifyParent(",

            "\"initialisation-error\",",

            "error && error.message ?",

            "error.message :",

            "\"Embedded Messaging failed to initialise.\"" ,

            ");",

            "}",

            "};",


            "var bootstrapScript=document.createElement(\"script\");",

            "bootstrapScript.type=\"text/javascript\";",

            "bootstrapScript.src=config.bootstrapURL;",

            "bootstrapScript.onload=window.initEmbeddedMessaging;",


            "bootstrapScript.onerror=function(){",

            "notifyParent(",

            "\"initialisation-error\",",

            "\"The Salesforce bootstrap file could not be loaded.\"" ,

            ");",

            "};",


            "document.body.appendChild(bootstrapScript);",


            "})();",


            "<\/script>",

            "</body>",

            "</html>"

        ].join("\n");

    }


    /*************************************************
     * PRELOAD DEPLOYMENTS
     *************************************************/

    function preloadBot(botKey) {

        var config =
            BOT_CONFIG[botKey];

        var state =
            botState[botKey];

        var frame =
            getFrame(botKey);


        if (
            !config ||
            !state ||
            !frame ||
            state.initializing ||
            state.ready
        ) {
            return;
        }


        state.initializing = true;

        state.failed = false;


        frame.srcdoc =
            createFrameDocument(
                config
            );
    }


    function preloadAllBots() {

        /*
         * Both deployments start bootstrapping
         * immediately in separate JS environments.
         */

        preloadBot("agentforce");

        preloadBot("einstein");
    }


    /*************************************************
     * LAUNCH SELECTED BOT
     *************************************************/

    function launchBot(botKey) {

        var config =
            BOT_CONFIG[botKey];

        var state =
            botState[botKey];


        if (!config || !state) {

            showStatus(
                "The selected chat deployment is not configured."
            );

            return;
        }


        activeBot =
            botKey;


        state.launchRequested =
            true;


        activateFrame(
            botKey
        );


        showChatContainer();


        /*
         * If preload has completed this call
         * happens immediately.
         */

        if (state.ready) {

            chatLoading.classList.add(
                "is-hidden"
            );

            state.launchRequested =
                false;


            sendFrameCommand(
                botKey,
                "launch-chat"
            );

        } else {

            showLoading(
                config.name
            );


            if (state.failed) {

                state.ready =
                    false;

                state.initializing =
                    false;

                state.failed =
                    false;

                preloadBot(
                    botKey
                );

            } else if (
                !state.initializing
            ) {

                preloadBot(
                    botKey
                );
            }

        }


        if (connectionTimer) {

            window.clearTimeout(
                connectionTimer
            );
        }


        connectionTimer =
            window.setTimeout(

                function () {

                    if (
                        activeBot === botKey &&
                        !botState[botKey].ready
                    ) {

                        showStatus(
                            config.name +
                            " did not become ready. Check the Salesforce deployment, Allowed Domains and browser console."
                        );


                        resetVisibleChat();
                    }

                },

                20000

            );
    }


    /*************************************************
     * GENERIC LAUNCHER
     *************************************************/

    launcher.addEventListener(
        "click",
        function () {

            /*
             * Active conversation already exists.
             * Salesforce launchChat will maximise
             * the minimized conversation.
             */

            if (
                activeBot &&
                botState[activeBot] &&
                botState[activeBot].ready
            ) {

                activateFrame(
                    activeBot
                );


                showChatContainer();


                sendFrameCommand(
                    activeBot,
                    "launch-chat"
                );


                return;
            }


            openSelector();
        }
    );


    /*************************************************
     * SELECTOR EVENTS
     *************************************************/

    selectorClose.addEventListener(
        "click",
        function () {
            closeSelector();
        }
    );


    Array.prototype.forEach.call(
        botOptions,
        function (button) {

            button.addEventListener(
                "click",
                function () {

                    launchBot(
                        button.getAttribute(
                            "data-bot"
                        )
                    );

                }
            );

        }
    );


    window.addEventListener(
        "keydown",
        function (event) {

            if (
                event.key === "Escape" &&
                selectorWindow.classList.contains(
                    "is-open"
                )
            ) {

                closeSelector();
            }

        }
    );


    /*************************************************
     * SALESFORCE EVENTS
     *************************************************/

    window.addEventListener(
        "message",
        function (event) {

            var message =
                event.data;


            if (
                !message ||
                message.source !==
                    "bge-embedded-chat" ||
                !message.botKey ||
                !botState[
                    message.botKey
                ]
            ) {

                return;
            }


            var state =
                botState[
                    message.botKey
                ];


            var config =
                BOT_CONFIG[
                    message.botKey
                ];


            switch (
                message.type
            ) {


                case "chat-ready":

                    state.ready =
                        true;

                    state.initializing =
                        false;

                    state.failed =
                        false;


                    console.log(
                        config.name +
                        " preloaded and ready."
                    );


                    if (
                        state.launchRequested
                    ) {

                        state.launchRequested =
                            false;


                        sendFrameCommand(
                            message.botKey,
                            "launch-chat"
                        );
                    }

                    break;


                case "chat-launched":

                    if (
                        activeBot ===
                        message.botKey
                    ) {

                        hideLoading();
                    }

                    break;


                case "conversation-opened":

                    if (
                        activeBot ===
                        message.botKey
                    ) {

                        hideLoading();
                    }

                    break;


                case "conversation-closed":

                    state.launchRequested =
                        false;


                    if (
                        activeBot ===
                        message.botKey
                    ) {

                        resetVisibleChat();
                    }

                    break;


                case "chat-error":

                case "initialisation-error":

                    state.ready =
                        false;

                    state.initializing =
                        false;

                    state.failed =
                        true;

                    state.launchRequested =
                        false;


                    console.error(
                        config.name +
                        " error:",
                        message.detail
                    );


                    if (
                        activeBot ===
                        message.botKey
                    ) {

                        showStatus(
                            config.name +
                            ": " +
                            (
                                message.detail ||
                                "The deployment failed to load."
                            )
                        );


                        resetVisibleChat();
                    }

                    break;

            }

        }
    );


    /*************************************************
     * START PRELOADING
     *************************************************/

    if (
        document.readyState ===
        "loading"
    ) {

        document.addEventListener(
            "DOMContentLoaded",
            function () {

                preloadAllBots();

            }
        );

    } else {

        preloadAllBots();

    }


})();

</script>

</div>
