---
layout: default
title: Chat with us
---

<div markdown="0">

<style>
    :root {
        --bge-navy: #003b5c;
        --bge-text: #454545;
        --bge-muted: #666666;
        --bge-border: #dddddd;
        --bge-teal: #00b7a5;
        --bge-error: #9d1c24;
        --bge-error-background: #fdecec;
    }

    /*
     * The Salesforce minimized launcher is positioned at:
     *
     * bottom: 24px
     * right: 24px
     *
     * A typical Salesforce launcher is approximately 60px.
     * This 78px ring is centred behind that launcher.
     *
     * The lower z-index is intentional:
     * - the maximized Salesforce chat appears above the ring
     * - the minimized Salesforce button appears above the ring
     */
    #chat-launcher-ring {
        position: fixed;
        right: 15px;
        bottom: 15px;
        z-index: 9998;

        display: none;

        width: 78px;
        height: 78px;

        border-radius: 50%;

        background:
            linear-gradient(
                135deg,
                #0075bd 0%,
                #006bb4 28%,
                #4e4aa3 52%,
                #893e98 72%,
                #d5007f 100%
            );

        box-shadow:
            0 5px 15px rgba(0, 59, 92, 0.22);

        pointer-events: none;
    }

    /*
     * Initial generic launcher.
     *
     * Centre:
     * right: 22px + 32px = 54px
     *
     * Ring centre:
     * right: 15px + 39px = 54px
     */
    #custom-chat-launcher {
        position: fixed;
        right: 22px;
        bottom: 22px;
        z-index: 9999;

        display: none;

        width: 64px;
        height: 64px;

        padding: 0;

        align-items: center;
        justify-content: center;

        border: 0;
        border-radius: 50%;

        background: #ffffff;

        cursor: pointer;

        box-shadow:
            0 2px 7px rgba(0, 59, 92, 0.18);

        transition:
            transform 0.15s ease,
            box-shadow 0.15s ease;
    }

    #custom-chat-launcher:hover {
        transform: translateY(-1px);

        box-shadow:
            0 4px 12px rgba(0, 59, 92, 0.24);
    }

    #custom-chat-launcher img {
        display: block;

        width: 38px;
        height: 38px;

        object-fit: contain;
    }

    /*
     * CHOICE AND LOADING PANELS
     */
    #chat-choice-panel,
    #chat-loading-panel {
        position: fixed;
        right: 24px;
        bottom: 24px;
        z-index: 10002;

        display: none;

        box-sizing: border-box;

        width: min(
            390px,
            calc(100vw - 32px)
        );

        padding:
            8px
            16px
            18px;

        border: 1px solid #eeeeee;
        border-radius: 20px 20px 0 0;

        background: #ffffff;
        color: var(--bge-text);

        font-family:
            Arial,
            Helvetica,
            sans-serif;

        box-shadow:
            0 8px 30px rgba(0, 0, 0, 0.16);
    }

    #chat-choice-panel *,
    #chat-loading-panel * {
        box-sizing: border-box;

        font-family:
            Arial,
            Helvetica,
            sans-serif;
    }

    #chat-choice-panel.is-open,
    #chat-loading-panel.is-open {
        display: block;
    }

    .chat-top-handle {
        width: 40px;
        height: 4px;

        margin:
            0
            auto
            12px;

        border-radius: 50px;

        background: #b9b9b5;
    }

    /*
     * CHOICE HEADER
     */
    #chat-choice-header {
        display: flex;

        align-items: flex-start;
        justify-content: space-between;

        gap: 16px;

        margin-bottom: 13px;
    }

    #chat-heading-content {
        min-width: 0;
        flex: 1;
    }

    #chat-heading-content h2 {
        margin: 0 0 5px;

        color: var(--bge-navy);

        font-size: 20px;
        font-weight: 700;
        line-height: 1.2;
    }

    #chat-heading-content p {
        margin: 0;

        color: var(--bge-muted);

        font-size: 15px;
        line-height: 1.35;
    }

    #chat-choice-close {
        display: flex;

        width: 44px;
        height: 44px;

        flex: 0 0 44px;

        padding: 0;

        align-items: center;
        justify-content: center;

        border: 0;
        border-radius: 50%;

        background: #f4f4f4;
        color: var(--bge-navy);

        cursor: pointer;
    }

    #chat-choice-close:hover {
        background: #eaeaea;
    }

    #chat-choice-close svg {
        width: 22px;
        height: 22px;

        fill: none;

        stroke: currentColor;
        stroke-width: 2;
        stroke-linecap: round;
    }

    #chat-divider {
        height: 1px;

        margin:
            0
            -16px
            13px;

        background: #e3e3e3;
    }

    /*
     * BOT OPTIONS
     */
    #chat-options {
        display: grid;
        gap: 12px;
    }

    .chat-option {
        display: flex;

        width: 100%;
        min-height: 110px;

        padding:
            17px
            16px
            17px
            12px;

        align-items: center;

        gap: 14px;

        border: 1px solid var(--bge-border);
        border-radius: 12px;

        background: #ffffff;
        color: var(--bge-text);

        text-align: left;

        cursor: pointer;

        transition:
            border-color 0.15s ease,
            box-shadow 0.15s ease;
    }

    .chat-option:hover:not(:disabled) {
        border-color: #b9b9b9;

        box-shadow:
            0 3px 8px rgba(0, 0, 0, 0.07);
    }

    .chat-option:disabled {
        cursor: wait;
        opacity: 0.65;
    }

    .chat-option-icon-shell,
    #loading-icon-shell {
        display: flex;

        width: 50px;
        height: 50px;

        flex: 0 0 50px;

        align-items: center;
        justify-content: center;

        border-radius: 50%;

        background:
            linear-gradient(
                135deg,
                #0075bd 0%,
                #006bb4 28%,
                #4e4aa3 52%,
                #893e98 72%,
                #d5007f 100%
            );
    }

    .chat-option-icon-inner,
    #loading-icon-inner {
        display: flex;

        width: 38px;
        height: 38px;

        align-items: center;
        justify-content: center;

        border-radius: 50%;

        background: #ffffff;
        color: var(--bge-navy);
    }

    .chat-option-icon-inner svg,
    #loading-icon-inner svg {
        width: 22px;
        height: 22px;
    }

    .chat-option-copy {
        min-width: 0;
        flex: 1;
    }

    .chat-option-copy strong {
        display: block;

        margin-bottom: 4px;

        color: var(--bge-navy);

        font-size: 15.5px;
        font-weight: 700;
        line-height: 1.25;
    }

    .chat-option-copy span {
        display: block;

        color: #555555;

        font-size: 15px;
        line-height: 1.4;
    }

    .chat-option-chevron {
        display: flex;

        width: 26px;
        height: 26px;

        flex: 0 0 26px;

        align-items: center;
        justify-content: center;

        color: #666666;
    }

    .chat-option-chevron svg {
        width: 17px;
        height: 17px;

        fill: none;

        stroke: currentColor;
        stroke-width: 2;
        stroke-linecap: round;
        stroke-linejoin: round;
    }

    #chat-choice-footnote {
        margin:
            13px
            0
            0
            48px;

        color: #606060;

        font-size: 12px;
        line-height: 1.35;
    }

    /*
     * LOADING PANEL
     */
    #loading-header {
        display: flex;

        align-items: center;

        gap: 13px;

        padding-bottom: 14px;

        border-bottom: 1px solid #e3e3e3;
    }

    #loading-title-copy h2 {
        margin: 0 0 4px;

        color: var(--bge-navy);

        font-size: 18px;
        line-height: 1.25;
    }

    #loading-title-copy p {
        margin: 0;

        color: var(--bge-muted);

        font-size: 13px;
    }

    #loading-body {
        padding-top: 18px;
    }

    #loading-message {
        margin: 0;

        font-size: 15px;
        line-height: 1.5;
    }

    #loading-progress {
        position: relative;

        overflow: hidden;

        height: 5px;

        margin-top: 18px;

        border-radius: 20px;

        background: #e8e8e8;
    }

    #loading-progress::after {
        position: absolute;
        top: 0;
        bottom: 0;
        left: -45%;

        width: 45%;

        border-radius: 20px;

        background:
            linear-gradient(
                90deg,
                #0075bd,
                #683f9c,
                #d5007f
            );

        content: "";

        animation:
            loading-slide
            1.15s
            ease-in-out
            infinite;
    }

    @keyframes loading-slide {
        from {
            left: -45%;
        }

        to {
            left: 100%;
        }
    }

    #chat-error-message {
        display: none;

        margin-top: 16px;
        padding: 12px 14px;

        border-radius: 8px;

        background: var(--bge-error-background);
        color: var(--bge-error);

        font-size: 13px;
        line-height: 1.4;
    }

    #chat-error-message.visible {
        display: block;
    }

    #back-to-options {
        display: none;

        width: 100%;

        margin-top: 14px;
        padding: 12px 16px;

        border: 0;
        border-radius: 8px;

        background: var(--bge-navy);
        color: #ffffff;

        font-size: 14px;
        font-weight: 700;

        cursor: pointer;
    }

    #back-to-options.visible {
        display: block;
    }

    #custom-chat-launcher:focus-visible,
    #chat-choice-close:focus-visible,
    .chat-option:focus-visible,
    #back-to-options:focus-visible {
        outline: 3px solid var(--bge-teal);
        outline-offset: 3px;
    }

    @media (max-width: 600px) {
        #chat-launcher-ring {
            right: 10px;
            bottom: 10px;
        }

        #custom-chat-launcher {
            right: 17px;
            bottom: 17px;
        }

        #chat-choice-panel,
        #chat-loading-panel {
            right: 0;
            bottom: 0;

            width: 100vw;

            border-right: 0;
            border-bottom: 0;
            border-left: 0;

            border-radius:
                20px
                20px
                0
                0;
        }
    }
</style>


<!-- FRONTEND GRADIENT RING -->

<div
    id="chat-launcher-ring"
    aria-hidden="true"
></div>


<!-- INITIAL CUSTOM LAUNCHER -->

<button
    id="custom-chat-launcher"
    type="button"
    aria-label="Chat with us"
    title="Chat with us"
>
    <img
        src="https://assets-us-01.kc-usercontent.com/7dd6b71d-d672-0004-6a4d-7043e1d0db33/f0a833dc-280f-4efb-a218-1433b989f8e8/speech%201.png"
        alt=""
        aria-hidden="true"
    >
</button>


<!-- CHOICE PANEL -->

<section
    id="chat-choice-panel"
    role="dialog"
    aria-modal="false"
    aria-labelledby="chat-choice-title"
>
    <div
        class="chat-top-handle"
        aria-hidden="true"
    ></div>

    <div id="chat-choice-header">

        <div id="chat-heading-content">
            <h2 id="chat-choice-title">
                Chat with us
            </h2>

            <p>
                Choose what you need help with
            </p>
        </div>

        <button
            id="chat-choice-close"
            type="button"
            aria-label="Close chat options"
            title="Close"
        >
            <svg
                viewBox="0 0 24 24"
                aria-hidden="true"
            >
                <path d="M7 7l10 10"></path>
                <path d="M17 7L7 17"></path>
            </svg>
        </button>

    </div>

    <div
        id="chat-divider"
        aria-hidden="true"
    ></div>

    <div id="chat-options">

        <button
            class="chat-option"
            type="button"
            data-bot="agentforce"
        >
            <span
                class="chat-option-icon-shell"
                aria-hidden="true"
            >
                <span class="chat-option-icon-inner">
                    <svg viewBox="0 0 24 24">
                        <path
                            d="M13.1 2L6.6 12.2H11L9.9 22L17.4 10.4H13L13.1 2Z"
                            fill="currentColor"
                        ></path>
                    </svg>
                </span>
            </span>

            <span class="chat-option-copy">
                <strong>
                    Explore plans &amp; switch
                </strong>

                <span>
                    New or existing customer?<br>
                    Compare plans, renew or check<br>
                    your contract
                </span>
            </span>

            <span
                class="chat-option-chevron"
                aria-hidden="true"
            >
                <svg viewBox="0 0 24 24">
                    <path d="M9 6l6 6-6 6"></path>
                </svg>
            </span>
        </button>

        <button
            class="chat-option"
            type="button"
            data-bot="einstein"
        >
            <span
                class="chat-option-icon-shell"
                aria-hidden="true"
            >
                <span class="chat-option-icon-inner">
                    <svg viewBox="0 0 24 24">
                        <g
                            fill="none"
                            stroke="currentColor"
                            stroke-width="1.8"
                            stroke-linecap="round"
                            stroke-linejoin="round"
                        >
                            <rect
                                x="5"
                                y="7"
                                width="14"
                                height="11"
                                rx="2"
                            ></rect>

                            <path d="M12 4v3"></path>
                            <path d="M10 4h4"></path>
                            <path d="M8 18v2"></path>
                            <path d="M16 18v2"></path>

                            <circle
                                cx="9.5"
                                cy="11.5"
                                r="1"
                                fill="currentColor"
                                stroke="none"
                            ></circle>

                            <circle
                                cx="14.5"
                                cy="11.5"
                                r="1"
                                fill="currentColor"
                                stroke="none"
                            ></circle>

                            <path d="M9 15h6"></path>
                        </g>
                    </svg>
                </span>
            </span>

            <span class="chat-option-copy">
                <strong>
                    Help with my account
                </strong>

                <span>
                    Bills, payments, meter reads and<br>
                    more
                </span>
            </span>

            <span
                class="chat-option-chevron"
                aria-hidden="true"
            >
                <svg viewBox="0 0 24 24">
                    <path d="M9 6l6 6-6 6"></path>
                </svg>
            </span>
        </button>

    </div>

    <p id="chat-choice-footnote">
        To switch later, close the chat and choose again.
    </p>
</section>


<!-- LOADING PANEL -->

<section
    id="chat-loading-panel"
    aria-labelledby="loading-title"
>
    <div
        class="chat-top-handle"
        aria-hidden="true"
    ></div>

    <div id="loading-header">

        <div
            id="loading-icon-shell"
            aria-hidden="true"
        >
            <div id="loading-icon-inner">

                <svg
                    id="loading-agentforce-icon"
                    viewBox="0 0 24 24"
                >
                    <path
                        d="M13.1 2L6.6 12.2H11L9.9 22L17.4 10.4H13L13.1 2Z"
                        fill="currentColor"
                    ></path>
                </svg>

                <svg
                    id="loading-einstein-icon"
                    viewBox="0 0 24 24"
                    style="display:none"
                >
                    <g
                        fill="none"
                        stroke="currentColor"
                        stroke-width="1.8"
                        stroke-linecap="round"
                        stroke-linejoin="round"
                    >
                        <rect
                            x="5"
                            y="7"
                            width="14"
                            height="11"
                            rx="2"
                        ></rect>

                        <path d="M12 4v3"></path>
                        <path d="M10 4h4"></path>
                        <path d="M8 18v2"></path>
                        <path d="M16 18v2"></path>

                        <circle
                            cx="9.5"
                            cy="11.5"
                            r="1"
                            fill="currentColor"
                            stroke="none"
                        ></circle>

                        <circle
                            cx="14.5"
                            cy="11.5"
                            r="1"
                            fill="currentColor"
                            stroke="none"
                        ></circle>

                        <path d="M9 15h6"></path>
                    </g>
                </svg>

            </div>
        </div>

        <div id="loading-title-copy">
            <h2 id="loading-title">
                Opening chat
            </h2>

            <p id="loading-subtitle">
                Connecting to your assistant
            </p>
        </div>

    </div>

    <div id="loading-body">

        <p id="loading-message">
            Please wait while the conversation opens.
        </p>

        <div
            id="loading-progress"
            aria-hidden="true"
        ></div>

        <div
            id="chat-error-message"
            role="alert"
            aria-live="assertive"
        ></div>

        <button
            id="back-to-options"
            type="button"
        >
            Back to chat options
        </button>

    </div>
</section>


<script type="text/javascript">
(function () {
    "use strict";

    var ORG_ID =
        "00DRR00000V4G17";

    var SCRT2_URL =
        "https://bordgaisenergyeandu--afdev2.sandbox.my.salesforce-scrt.com";

    var BOT_CONFIG = {
        agentforce: {
            name:
                "Agentforce",

            title:
                "Explore plans & switch",

            subtitle:
                "Connecting to Agentforce",

            deploymentName:
                "MIAWPOC_Github",

            siteURL:
                "https://bordgaisenergyeandu--afdev2.sandbox.my.site.com/ESWMIAWPOCGithub1793617406174",

            bootstrapURL:
                "https://bordgaisenergyeandu--afdev2.sandbox.my.site.com/ESWMIAWPOCGithub1793617406174/assets/js/bootstrap.min.js"
        },

        einstein: {
            name:
                "Einstein",

            title:
                "Help with my account",

            subtitle:
                "Connecting to Virtual Assistant",

            deploymentName:
                "MIAWPOC_GithubEinstein",

            siteURL:
                "https://bordgaisenergyeandu--afdev2.sandbox.my.site.com/ESWMIAWPOCGithub1794841236383",

            bootstrapURL:
                "https://bordgaisenergyeandu--afdev2.sandbox.my.site.com/ESWMIAWPOCGithub1794841236383/assets/js/bootstrap.min.js"
        }
    };

    var launcherRing =
        document.getElementById(
            "chat-launcher-ring"
        );

    var launcher =
        document.getElementById(
            "custom-chat-launcher"
        );

    var choicePanel =
        document.getElementById(
            "chat-choice-panel"
        );

    var choiceClose =
        document.getElementById(
            "chat-choice-close"
        );

    var chatOptions =
        document.querySelectorAll(
            ".chat-option"
        );

    var loadingPanel =
        document.getElementById(
            "chat-loading-panel"
        );

    var loadingTitle =
        document.getElementById(
            "loading-title"
        );

    var loadingSubtitle =
        document.getElementById(
            "loading-subtitle"
        );

    var loadingMessage =
        document.getElementById(
            "loading-message"
        );

    var loadingProgress =
        document.getElementById(
            "loading-progress"
        );

    var agentforceLoadingIcon =
        document.getElementById(
            "loading-agentforce-icon"
        );

    var einsteinLoadingIcon =
        document.getElementById(
            "loading-einstein-icon"
        );

    var errorMessage =
        document.getElementById(
            "chat-error-message"
        );

    var backButton =
        document.getElementById(
            "back-to-options"
        );

    var selectedBot =
        null;

    var buttonCreated =
        false;

    var launchRequested =
        false;

    var launchStarted =
        false;

    var initializationTimer =
        null;

    var conversationOpen =
        false;

    function getSelectedBotFromURL() {
        var parameters =
            new URLSearchParams(
                window.location.search
            );

        var botKey =
            parameters.get("bot");

        return BOT_CONFIG[botKey]
            ? botKey
            : null;
    }

    function getCleanURL() {
        return (
            window.location.origin +
            window.location.pathname +
            window.location.hash
        );
    }

    function getBotURL(
        botKey
    ) {
        var url =
            new URL(
                window.location.href
            );

        url.search =
            "";

        url.hash =
            "";

        url.searchParams.set(
            "bot",
            botKey
        );

        return url.toString();
    }

    function showRing() {
        launcherRing.style.display =
            "block";
    }

    function hideRing() {
        launcherRing.style.display =
            "none";
    }

    /*
     * Generic state:
     * ring + custom launcher.
     */
    function showGenericLauncher() {
        launcher.style.display =
            "flex";

        showRing();
    }

    function hideGenericLauncher() {
        launcher.style.display =
            "none";
    }

    function openChoicePanel() {
        choicePanel.classList.add(
            "is-open"
        );

        loadingPanel.classList.remove(
            "is-open"
        );

        hideGenericLauncher();
        hideRing();
    }

    function closeChoicePanel() {
        choicePanel.classList.remove(
            "is-open"
        );

        showGenericLauncher();

        launcher.focus();
    }

    function disableOptions(
        disabled
    ) {
        Array.prototype.forEach.call(
            chatOptions,
            function (option) {
                option.disabled =
                    disabled;
            }
        );
    }

    function showLoadingPanel(
        botKey
    ) {
        var config =
            BOT_CONFIG[botKey];

        choicePanel.classList.remove(
            "is-open"
        );

        loadingPanel.classList.add(
            "is-open"
        );

        hideGenericLauncher();
        hideRing();

        loadingTitle.textContent =
            config.title;

        loadingSubtitle.textContent =
            config.subtitle;

        loadingMessage.textContent =
            "Please wait while the conversation opens.";

        loadingProgress.style.display =
            "block";

        errorMessage.classList.remove(
            "visible"
        );

        errorMessage.textContent =
            "";

        backButton.classList.remove(
            "visible"
        );

        if (
            botKey ===
            "agentforce"
        ) {
            agentforceLoadingIcon.style.display =
                "block";

            einsteinLoadingIcon.style.display =
                "none";
        } else {
            agentforceLoadingIcon.style.display =
                "none";

            einsteinLoadingIcon.style.display =
                "block";
        }
    }

    function hideLoadingPanel() {
        loadingPanel.classList.remove(
            "is-open"
        );
    }

    function clearTimer() {
        if (!initializationTimer) {
            return;
        }

        window.clearTimeout(
            initializationTimer
        );

        initializationTimer =
            null;
    }

    function showError(
        message
    ) {
        clearTimer();

        showLoadingPanel(
            selectedBot
        );

        loadingMessage.textContent =
            "We could not open the chat.";

        loadingProgress.style.display =
            "none";

        errorMessage.textContent =
            message;

        errorMessage.classList.add(
            "visible"
        );

        backButton.classList.add(
            "visible"
        );
    }

    function selectBot(
        botKey
    ) {
        if (!BOT_CONFIG[botKey]) {
            return;
        }

        window.location.assign(
            getBotURL(
                botKey
            )
        );
    }

    function launchChat() {
        if (
            !selectedBot ||
            !buttonCreated
        ) {
            launchRequested =
                true;

            return;
        }

        if (launchStarted) {
            return;
        }

        if (
            !window.embeddedservice_bootstrap ||
            !embeddedservice_bootstrap.utilAPI ||
            typeof embeddedservice_bootstrap
                .utilAPI
                .launchChat !==
                "function"
        ) {
            showError(
                "Salesforce loaded, but the Launch Chat API is unavailable."
            );

            return;
        }

        launchRequested =
            false;

        launchStarted =
            true;

        hideRing();

        embeddedservice_bootstrap
            .utilAPI
            .launchChat()
            .then(function () {
                clearTimer();

                hideLoadingPanel();

                hideGenericLauncher();

                /*
                 * The chat starts maximized.
                 * The maximized Salesforce window sits above
                 * the low-z-index ring.
                 *
                 * Keep the ring rendered underneath so it is
                 * immediately visible when the native button
                 * returns after minimization.
                 */
                showRing();
            })
            .catch(function (
                error
            ) {
                launchStarted =
                    false;

                hideRing();

                showError(
                    error &&
                    error.message
                        ? error.message
                        : "Chat could not be opened."
                );
            });
    }

    function addSalesforceEvents() {
        window.addEventListener(
            "onEmbeddedMessagingButtonCreated",
            function () {
                buttonCreated =
                    true;

                clearTimer();

                /*
                 * Do not call hideChatButton().
                 *
                 * Salesforce needs to own and restore this
                 * button after the user minimizes the chat.
                 */
                if (
                    launchRequested
                ) {
                    launchChat();
                }
            },
            {
                once: true
            }
        );

        window.addEventListener(
            "onEmbeddedMessagingConversationOpened",
            function () {
                conversationOpen =
                    true;

                clearTimer();

                hideLoadingPanel();

                hideGenericLauncher();

                /*
                 * Ring remains behind the maximized Salesforce
                 * window because it has a low z-index.
                 */
                showRing();
            }
        );

        window.addEventListener(
            "onEmbeddedMessagingConversationClosed",
            function () {
                conversationOpen =
                    false;

                clearTimer();

                window.location.replace(
                    getCleanURL()
                );
            }
        );
    }

    function initializeDeployment() {
        var config =
            BOT_CONFIG[
                selectedBot
            ];

        try {
            embeddedservice_bootstrap
                .settings
                .language =
                    "en_US";

            /*
             * Salesforce expects:
             * "bottom,right"
             *
             * The native minimized launcher will be centred
             * over the fixed gradient ring.
             */
            embeddedservice_bootstrap
                .settings
                .chatButtonPosition =
                    "24px,24px";

            /*
             * IMPORTANT:
             *
             * Keep the native button available.
             * Salesforce automatically hides the button when
             * maximized and restores it after minimization.
             */
            embeddedservice_bootstrap
                .settings
                .hideChatButtonOnLoad =
                    false;

            embeddedservice_bootstrap.init(
                ORG_ID,
                config.deploymentName,
                config.siteURL,
                {
                    scrt2URL:
                        SCRT2_URL
                }
            );
        } catch (
            error
        ) {
            launchRequested =
                false;

            hideRing();

            showError(
                error &&
                error.message
                    ? error.message
                    : "Chat failed to initialize."
            );

            console.error(
                "Chat initialization failed:",
                error
            );
        }
    }

    function loadSelectedDeployment() {
        var config =
            BOT_CONFIG[
                selectedBot
            ];

        launchRequested =
            true;

        showLoadingPanel(
            selectedBot
        );

        addSalesforceEvents();

        var script =
            document.createElement(
                "script"
            );

        script.type =
            "text/javascript";

        script.src =
            config.bootstrapURL;

        script.async =
            true;

        script.onload =
            initializeDeployment;

        script.onerror =
            function () {
                launchRequested =
                    false;

                hideRing();

                showError(
                    config.name +
                    " could not be loaded."
                );
            };

        document.body.appendChild(
            script
        );

        initializationTimer =
            window.setTimeout(
                function () {
                    if (
                        !buttonCreated
                    ) {
                        launchRequested =
                            false;

                        hideRing();

                        showError(
                            config.name +
                            " did not become ready."
                        );
                    }
                },
                25000
            );
    }

    launcher.addEventListener(
        "click",
        openChoicePanel
    );

    choiceClose.addEventListener(
        "click",
        closeChoicePanel
    );

    Array.prototype.forEach.call(
        chatOptions,
        function (
            option
        ) {
            option.addEventListener(
                "click",
                function () {
                    disableOptions(
                        true
                    );

                    selectBot(
                        option.getAttribute(
                            "data-bot"
                        )
                    );
                }
            );
        }
    );

    backButton.addEventListener(
        "click",
        function () {
            window.location.replace(
                getCleanURL()
            );
        }
    );

    window.addEventListener(
        "keydown",
        function (
            event
        ) {
            if (
                event.key ===
                    "Escape" &&
                choicePanel
                    .classList
                    .contains(
                        "is-open"
                    )
            ) {
                closeChoicePanel();
            }
        }
    );

    function addPreconnect(
        url
    ) {
        var link =
            document.createElement(
                "link"
            );

        link.rel =
            "preconnect";

        link.href =
            url;

        link.crossOrigin =
            "anonymous";

        document.head.appendChild(
            link
        );
    }

    addPreconnect(
        "https://bordgaisenergyeandu--afdev2.sandbox.my.site.com"
    );

    addPreconnect(
        "https://bordgaisenergyeandu--afdev2.sandbox.my.salesforce-scrt.com"
    );

    selectedBot =
        getSelectedBotFromURL();

    if (
        selectedBot
    ) {
        hideGenericLauncher();
        hideRing();

        loadSelectedDeployment();
    } else {
        showGenericLauncher();
    }
})();
</script>

</div>
