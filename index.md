---
layout: default
title: Agentforce
---

<div markdown="0">

<style>
    #custom-chat-launcher {
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

    #custom-chat-launcher:hover {
        background: #005637;
        transform: scale(1.05);
    }

    #custom-chat-launcher:focus-visible,
    #bot-selector-minimise:focus-visible,
    .bot-choice:focus-visible {
        outline: 3px solid #ffb81c;
        outline-offset: 3px;
    }

    #custom-chat-launcher svg {
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

    #bot-selector-header-left {
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
        line-height: 1.3;
    }

    #bot-selector-header-text p {
        margin: 3px 0 0;
        color: rgba(255, 255, 255, 0.88);
        font-size: 13px;
        line-height: 1.3;
    }

    #bot-selector-minimise {
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

    #bot-selector-minimise:hover {
        background: rgba(255, 255, 255, 0.15);
    }

    #bot-selector-minimise svg {
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

    #bot-options {
        display: grid;
        gap: 12px;
    }

    .bot-choice {
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

    .bot-choice:hover:not(:disabled) {
        border-color: #006f44;
        background: #f2faf6;
    }

    .bot-choice:disabled {
        cursor: wait;
        opacity: 0.65;
    }

    .bot-choice-icon {
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

    .bot-choice-icon svg {
        width: 23px;
        height: 23px;
        fill: currentColor;
    }

    .bot-choice-content {
        min-width: 0;
        flex: 1;
    }

    .bot-choice-content strong {
        display: block;
        margin-bottom: 4px;
        color: #1f2937;
        font-size: 16px;
        line-height: 1.25;
    }

    .bot-choice-description {
        display: block;
        color: #646b75;
        font-size: 13px;
        line-height: 1.35;
    }

    .bot-choice-arrow {
        color: #006f44;
        font-size: 24px;
        line-height: 1;
    }

    #selector-status {
        display: none;
        margin-top: 16px;
        padding: 12px 14px;
        border-radius: 8px;
        background: #fff4e5;
        color: #6b3d00;
        font-size: 13px;
        line-height: 1.4;
    }

    #selector-status.is-visible {
        display: block;
    }

    #selector-status.is-error {
        background: #fdecec;
        color: #9d1c24;
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

    @media (max-width: 600px) {
        #custom-chat-launcher {
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
    }
</style>

<button
    id="custom-chat-launcher"
    type="button"
    aria-label="Open chat"
    title="Chat with us"
>
    <svg viewBox="0 0 24 24" aria-hidden="true">
        <path d="M4 4h16a2 2 0 0 1 2 2v10a2 2 0 0 1-2 2H8l-5 4v-4.5A2 2 0 0 1 2 16V6a2 2 0 0 1 2-2Zm2.5 6a1.5 1.5 0 1 0 0 3 1.5 1.5 0 0 0 0-3Zm5.5 0a1.5 1.5 0 1 0 0 3 1.5 1.5 0 0 0 0-3Zm5.5 0a1.5 1.5 0 1 0 0 3 1.5 1.5 0 0 0 0-3Z"></path>
    </svg>
</button>

<div
    id="bot-selector-window"
    role="dialog"
    aria-modal="false"
    aria-labelledby="bot-selector-title"
>
    <div id="bot-selector-header">

        <div id="bot-selector-header-left">

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
            id="bot-selector-minimise"
            type="button"
            aria-label="Minimise chat selection"
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

        <div id="bot-options">

            <button
                id="agentforce-option"
                class="bot-choice"
                type="button"
                data-bot="agentforce"
            >
                <span class="bot-choice-icon">
                    <svg viewBox="0 0 24 24" aria-hidden="true">
                        <path d="M12 2a4 4 0 0 1 4 4v1h1a4 4 0 0 1 4 4v4a4 4 0 0 1-4 4h-1.55L12 22l-3.45-3H7a4 4 0 0 1-4-4v-4a4 4 0 0 1 4-4h1V6a4 4 0 0 1 4-4Zm-2 5h4V6a2 2 0 1 0-4 0v1Zm-3 2a2 2 0 0 0-2 2v4a2 2 0 0 0 2 2h2.3l2.7 2.35L14.7 17H17a2 2 0 0 0 2-2v-4a2 2 0 0 0-2-2H7Z"></path>
                    </svg>
                </span>

                <span class="bot-choice-content">
                    <strong>Agentforce</strong>
                    <span class="bot-choice-description">
                        Start the Agentforce conversation.
                    </span>
                </span>

                <span class="bot-choice-arrow" aria-hidden="true">›</span>
            </button>

            <button
                id="einstein-option"
                class="bot-choice"
                type="button"
                data-bot="einstein"
            >
                <span class="bot-choice-icon">
                    <svg viewBox="0 0 24 24" aria-hidden="true">
                        <path d="M12 2a9 9 0 0 1 9 9c0 3.04-1.51 5.73-3.82 7.36L18 22l-3.75-2A8.92 8.92 0 0 1 12 20a9 9 0 1 1 0-18Zm0 2a7 7 0 1 0 1.82 13.76l.38-.1 1.09.58-.26-1.17.5-.32A7 7 0 0 0 12 4Z"></path>
                    </svg>
                </span>

                <span class="bot-choice-content">
                    <strong>Einstein</strong>
                    <span class="bot-choice-description">
                        Start the Einstein conversation.
                    </span>
                </span>

                <span class="bot-choice-arrow" aria-hidden="true">›</span>
            </button>

        </div>

        <div id="selector-status" role="status" aria-live="polite"></div>

    </div>

    <div id="bot-selector-footer">
        Bord Gáis Energy virtual assistance
    </div>

</div>

<script type="text/javascript">
(function () {
    "use strict";

    var ORG_ID = "00DRR00000V4G17";

    var SCRT2_URL =
        "https://bordgaisenergyeandu--afdev2.sandbox.my.salesforce-scrt.com";

    var BOT_CONFIG = {
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

    var launcher =
        document.getElementById("custom-chat-launcher");

    var selectorWindow =
        document.getElementById("bot-selector-window");

    var selectorMinimise =
        document.getElementById("bot-selector-minimise");

    var selectorStatus =
        document.getElementById("selector-status");

    var botOptions =
        document.querySelectorAll(".bot-choice");

    var selectedBot = null;
    var bootstrapLoading = false;
    var messagingReady = false;
    var launchRequested = false;
    var initializationTimer = null;

    function openSelector() {
        if (selectedBot && messagingReady) {
            launchSelectedChat();
            return;
        }

        selectorWindow.classList.add("is-open");
        launcher.style.display = "none";
    }

    function minimiseSelector() {
        selectorWindow.classList.remove("is-open");
        launcher.style.display = "flex";
        launcher.focus();
    }

    function disableBotOptions(disabled) {
        Array.prototype.forEach.call(
            botOptions,
            function (button) {
                button.disabled = disabled;
            }
        );
    }

    function setLoadingStatus(botName) {
        selectorStatus.classList.remove("is-error");
        selectorStatus.classList.add("is-visible");
        selectorStatus.textContent =
            "Connecting to " + botName + "...";
    }

    function setErrorStatus(message) {
        selectorStatus.classList.add("is-error");
        selectorStatus.classList.add("is-visible");
        selectorStatus.textContent = message;
    }

    function clearStatus() {
        selectorStatus.classList.remove("is-visible");
        selectorStatus.classList.remove("is-error");
        selectorStatus.textContent = "";
    }

    function clearInitializationTimer() {
        if (initializationTimer) {
            window.clearTimeout(initializationTimer);
            initializationTimer = null;
        }
    }

    function launchSelectedChat() {
        if (!selectedBot) {
            return;
        }

        if (!messagingReady) {
            launchRequested = true;
            return;
        }

        if (
            !window.embeddedservice_bootstrap ||
            !embeddedservice_bootstrap.utilAPI ||
            typeof embeddedservice_bootstrap.utilAPI.launchChat !==
                "function"
        ) {
            disableBotOptions(false);

            setErrorStatus(
                "Salesforce loaded, but the Launch Chat API is unavailable."
            );

            return;
        }

        embeddedservice_bootstrap.utilAPI
            .launchChat()
            .then(function () {
                clearInitializationTimer();
                clearStatus();

                selectorWindow.classList.remove("is-open");

                /*
                 * Keep the custom launcher available.
                 * If Salesforce is minimized, selecting this launcher
                 * calls launchChat again and restores the conversation.
                 */
                launcher.style.display = "flex";

                disableBotOptions(false);
            })
            .catch(function (error) {
                disableBotOptions(false);

                setErrorStatus(
                    error && error.message
                        ? error.message
                        : "The selected chat could not be opened."
                );
            });
    }

    function initialiseSelectedBot(botKey) {
        var config = BOT_CONFIG[botKey];

        if (!config) {
            setErrorStatus(
                "The selected chat deployment is not configured."
            );

            return;
        }

        /*
         * Enhanced Web Chat exposes one global
         * embeddedservice_bootstrap runtime.
         *
         * Only the selected deployment is initialized.
         * This avoids the global conflict and avoids placing
         * Salesforce inside an unsupported nested iframe.
         */
        if (selectedBot && selectedBot !== botKey) {
            setErrorStatus(
                "A different assistant is already active. Refresh the page to switch assistants."
            );

            return;
        }

        selectedBot = botKey;
        launchRequested = true;

        disableBotOptions(true);
        setLoadingStatus(config.name);

        if (messagingReady) {
            launchSelectedChat();
            return;
        }

        if (bootstrapLoading) {
            return;
        }

        bootstrapLoading = true;

        window.addEventListener(
            "onEmbeddedMessagingReady",
            function handleMessagingReady() {
                messagingReady = true;
                bootstrapLoading = false;

                clearInitializationTimer();

                try {
                    if (
                        embeddedservice_bootstrap.utilAPI &&
                        typeof embeddedservice_bootstrap.utilAPI
                            .hideChatButton === "function"
                    ) {
                        embeddedservice_bootstrap.utilAPI
                            .hideChatButton();
                    }
                } catch (error) {
                    console.warn(
                        "Unable to hide the Salesforce launcher.",
                        error
                    );
                }

                if (launchRequested) {
                    launchRequested = false;
                    launchSelectedChat();
                }
            },
            { once: true }
        );

        window.addEventListener(
            "onEmbeddedMessagingButtonCreated",
            function handleButtonCreated() {
                try {
                    if (
                        embeddedservice_bootstrap.utilAPI &&
                        typeof embeddedservice_bootstrap.utilAPI
                            .hideChatButton === "function"
                    ) {
                        embeddedservice_bootstrap.utilAPI
                            .hideChatButton();
                    }
                } catch (error) {
                    console.warn(
                        "Unable to hide the Salesforce launcher.",
                        error
                    );
                }
            }
        );

        window.addEventListener(
            "onEmbeddedMessagingConversationOpened",
            function () {
                clearStatus();
                selectorWindow.classList.remove("is-open");
                launcher.style.display = "flex";
            }
        );

        window.addEventListener(
            "onEmbeddedMessagingConversationClosed",
            function () {
                clearStatus();
                selectorWindow.classList.remove("is-open");
                launcher.style.display = "flex";
            }
        );

        window.initSelectedEmbeddedMessaging = function () {
            try {
                embeddedservice_bootstrap.settings.language =
                    "en_US";

                embeddedservice_bootstrap.settings
                    .hideChatButtonOnLoad = true;

                embeddedservice_bootstrap.init(
                    ORG_ID,
                    config.deploymentName,
                    config.siteURL,
                    {
                        scrt2URL: SCRT2_URL
                    }
                );
            } catch (error) {
                bootstrapLoading = false;
                launchRequested = false;

                clearInitializationTimer();
                disableBotOptions(false);

                setErrorStatus(
                    error && error.message
                        ? error.message
                        : config.name +
                          " failed to initialize."
                );

                console.error(
                    "Error initializing " + config.name + ":",
                    error
                );
            }
        };

        var bootstrapScript =
            document.createElement("script");

        bootstrapScript.type = "text/javascript";
        bootstrapScript.src = config.bootstrapURL;
        bootstrapScript.async = true;

        bootstrapScript.onload = function () {
            window.initSelectedEmbeddedMessaging();
        };

        bootstrapScript.onerror = function () {
            bootstrapLoading = false;
            launchRequested = false;

            clearInitializationTimer();
            disableBotOptions(false);

            setErrorStatus(
                config.name +
                " bootstrap file could not be loaded."
            );
        };

        document.body.appendChild(bootstrapScript);

        initializationTimer = window.setTimeout(
            function () {
                if (!messagingReady) {
                    bootstrapLoading = false;
                    launchRequested = false;

                    disableBotOptions(false);

                    setErrorStatus(
                        config.name +
                        " did not become ready. Check the Salesforce deployment, Allowed Domains and browser console."
                    );
                }
            },
            20000
        );
    }

    /*
     * Preconnect to Salesforce domains.
     * This does not initialize either deployment, but it can reduce
     * DNS, TLS and connection setup time after selection.
     */
    function addPreconnect(url) {
        var link = document.createElement("link");

        link.rel = "preconnect";
        link.href = url;
        link.crossOrigin = "anonymous";

        document.head.appendChild(link);
    }

    addPreconnect(
        "https://bordgaisenergyeandu--afdev2.sandbox.my.site.com"
    );

    addPreconnect(
        "https://bordgaisenergyeandu--afdev2.sandbox.my.salesforce-scrt.com"
    );

    launcher.addEventListener(
        "click",
        function () {
            openSelector();
        }
    );

    selectorMinimise.addEventListener(
        "click",
        function () {
            minimiseSelector();
        }
    );

    Array.prototype.forEach.call(
        botOptions,
        function (button) {
            button.addEventListener(
                "click",
                function () {
                    initialiseSelectedBot(
                        button.getAttribute("data-bot")
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
                selectorWindow.classList.contains("is-open")
            ) {
                minimiseSelector();
            }
        }
    );
})();
</script>

</div>
