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
        --bge-panel: #ffffff;
        --bge-close: #f4f4f4;
        --bge-teal: #00b7a5;
        --bge-purple: #89449b;
        --bge-pink: #d5007f;
        --bge-error: #9d1c24;
        --bge-error-background: #fdecec;
    }

    #custom-chat-launcher,
    #chat-launcher-ring,
    #chat-choice-panel,
    #chat-loading-panel,
    #custom-chat-launcher *,
    #chat-choice-panel *,
    #chat-loading-panel * {
        box-sizing: border-box;
        font-family: Arial, Helvetica, sans-serif;
    }

    /*
     * SHARED GRADIENT RING
     *
     * This is owned by the website frontend.
     * It sits behind:
     *
     * 1. The initial custom speech icon
     * 2. The minimized Agentforce launcher
     * 3. The minimized Einstein launcher
     */
    #chat-launcher-ring {
        position: fixed;
        right: 20px;
        bottom: 20px;
        z-index: 999990;

        display: block;

        width: 72px;
        height: 72px;

        border-radius: 50%;

        background:
            linear-gradient(
                135deg,
                #0072bc 0%,
                #1268b2 30%,
                #65439e 64%,
                #d5007f 100%
            );

        box-shadow:
            0 4px 14px rgba(0, 59, 92, 0.2);

        pointer-events: none;
    }

    /*
     * INITIAL CUSTOM LAUNCHER
     *
     * 62 × 62 sits centrally inside the 72 × 72 ring.
     */
    #custom-chat-launcher {
        position: fixed;
        right: 25px;
        bottom: 25px;
        z-index: 999998;

        display: flex;
        width: 62px;
        height: 62px;
        padding: 0;

        align-items: center;
        justify-content: center;

        border: 0;
        border-radius: 50%;

        background: #ffffff;

        cursor: pointer;

        box-shadow:
            0 2px 8px rgba(0, 59, 92, 0.15);

        transition:
            transform 0.15s ease,
            box-shadow 0.15s ease;
    }

    #custom-chat-launcher:hover {
        transform: translateY(-1px);

        box-shadow:
            0 4px 12px rgba(0, 59, 92, 0.22);
    }

    /*
     * New speech image.
     *
     * The source image is 48 × 48, but it is deliberately
     * rendered at 38 × 38 to avoid touching the outer ring.
     */
    #custom-chat-launcher img {
        display: block;

        width: 38px;
        height: 38px;

        object-fit: contain;
    }

    /*
     * CHAT CHOICE WINDOW
     */
    #chat-choice-panel {
        position: fixed;
        right: 24px;
        bottom: 24px;
        z-index: 999999;

        display: none;

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

        background: var(--bge-panel);
        color: var(--bge-text);

        box-shadow:
            0 8px 30px rgba(0, 0, 0, 0.16);
    }

    #chat-choice-panel.is-open {
        display: block;
    }

    /*
     * LOADING WINDOW
     */
    #chat-loading-panel {
        position: fixed;
        right: 24px;
        bottom: 24px;
        z-index: 999999;

        display: none;

        width: min(
            390px,
            calc(100vw - 32px)
        );

        padding:
            8px
            16px
            20px;

        border: 1px solid #eeeeee;
        border-radius: 20px 20px 0 0;

        background: #ffffff;
        color: var(--bge-text);

        box-shadow:
            0 8px 30px rgba(0, 0, 0, 0.16);
    }

    #chat-loading-panel.is-open {
        display: block;
    }

    /*
     * TOP HANDLE
     */
    .chat-top-handle {
        width: 40px;
        height: 4px;

        margin:
            0 auto
            12px;

        border-radius: 50px;

        background: #b9b9b5;
    }

    /*
     * SELECTOR HEADER
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
        font-weight: 400;
        line-height: 1.35;
    }

    /*
     * CLOSE BUTTON
     */
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

        background: var(--bge-close);
        color: var(--bge-navy);

        cursor: pointer;

        transition:
            background 0.15s ease,
            transform 0.15s ease;
    }

    #chat-choice-close:hover {
        background: #eaeaea;
        transform: scale(1.03);
    }

    #chat-choice-close svg {
        width: 22px;
        height: 22px;

        stroke: currentColor;
        stroke-width: 2;

        fill: none;

        stroke-linecap: round;
    }

    /*
     * DIVIDER
     */
    #chat-divider {
        height: 1px;

        margin:
            0 -16px
            13px;

        background: #e3e3e3;
    }

    /*
     * CHAT OPTIONS
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
            box-shadow 0.15s ease,
            transform 0.1s ease;
    }

    .chat-option:hover:not(:disabled) {
        border-color: #b9b9b9;

        box-shadow:
            0 3px 8px rgba(0, 0, 0, 0.07);
    }

    .chat-option:active {
        transform: scale(0.995);
    }

    .chat-option:disabled {
        cursor: wait;
        opacity: 0.65;
    }

    /*
     * OPTION ICONS
     */
    .chat-option-icon-shell {
        display: flex;

        width: 46px;
        height: 46px;

        flex: 0 0 46px;

        align-items: center;
        justify-content: center;

        border-radius: 50%;

        background:
            linear-gradient(
                135deg,
                #0072bc 0%,
                #1268b2 30%,
                #65439e 64%,
                #d5007f 100%
            );
    }

    .chat-option-icon-inner {
        display: flex;

        width: 38px;
        height: 38px;

        align-items: center;
        justify-content: center;

        border-radius: 50%;

        background: #ffffff;
        color: var(--bge-navy);
    }

    .chat-option-icon-inner svg {
        width: 22px;
        height: 22px;
    }

    /*
     * OPTION TEXT
     */
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
        font-weight: 400;
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

    /*
     * FOOTNOTE
     */
    #chat-choice-footnote {
        margin:
            13px 0
            0
            48px;

        color: #606060;

        font-size: 12px;
        line-height: 1.35;
    }

    /*
     * LOADING CONTENT
     */
    #loading-header {
        display: flex;

        align-items: center;

        gap: 13px;

        padding-bottom: 14px;

        border-bottom: 1px solid #e3e3e3;
    }

    #loading-icon-shell {
        display: flex;

        width: 46px;
        height: 46px;

        flex: 0 0 46px;

        align-items: center;
        justify-content: center;

        border-radius: 50%;

        background:
            linear-gradient(
                135deg,
                #0072bc 0%,
                #1268b2 30%,
                #65439e 64%,
                #d5007f 100%
            );
    }

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

    #loading-icon-inner svg {
        width: 22px;
        height: 22px;
    }

    #loading-title-copy h2 {
        margin: 0 0 4px;

        color: var(--bge-navy);

        font-size: 18px;
        font-weight: 700;
        line-height: 1.25;
    }

    #loading-title-copy p {
        margin: 0;

        color: var(--bge-muted);

        font-size: 13px;
        line-height: 1.35;
    }

    #loading-body {
        padding-top: 18px;
    }

    #loading-message {
        margin: 0;

        color: var(--bge-text);

        font-size: 15px;
        line-height: 1.5;
    }

    #loading-progress {
        position: relative;

        overflow: hidden;

        height: 4px;

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
                var(--bge-teal),
                var(--bge-purple),
                var(--bge-pink)
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

    /*
     * ERROR
     */
    #chat-error-message {
        display: none;

        margin-top: 16px;
        padding: 12px 14px;

        border-radius: 8px;

        background: var(--bge-error-background);
        color: var(--bge-error);

        font-size: 13px;
        line-height: 1.45;
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

    /*
     * ACCESSIBILITY
     */
    #custom-chat-launcher:focus-visible,
    #chat-choice-close:focus-visible,
    .chat-option:focus-visible,
    #back-to-options:focus-visible {
        outline: 3px solid var(--bge-teal);
        outline-offset: 3px;
    }

    /*
     * MOBILE
     */
    @media (max-width: 600px) {
        #chat-launcher-ring {
            right: 14px;
            bottom: 14px;
        }

        #custom-chat-launcher {
            right: 19px;
            bottom: 19px;
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


<!--
    PERSISTENT FRONTEND RING

    The ring is independent of the logo itself.

    When Salesforce is minimized, the native circular ESD
    launcher is expected to sit over this ring.
-->
<div
    id="chat-launcher-ring"
    aria-hidden="true"
></div>


<!-- INITIAL CUSTOM CHAT BUTTON -->

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


<!-- CHAT CHOICE PANEL -->

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

        <!-- AGENTFORCE -->

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


        <!-- EINSTEIN -->

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
                    <svg
                        viewBox="0 0 24 24"
                        aria-hidden="true"
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
                    style="display: none;"
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

    /*
     * SALESFORCE CONFIGURATION
     */
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

    /*
     * DOM
     */
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

    /*
     * STATE
     */
    var selectedBot =
        null;

    var buttonCreated =
        false;

    var bootstrapLoading =
        false;

    var launchRequested =
        false;

    var launchStarted =
        false;

    var initializationTimer =
        null;

    /*
     * URL HELPERS
     */
    function getSelectedBotFromURL() {
        var parameters =
            new URLSearchParams(
                window.location.search
            );

        var botKey =
            parameters.get("bot");

        if (BOT_CONFIG[botKey]) {
            return botKey;
        }

        return null;
    }

    function getCleanURL() {
        return (
            window.location.origin +
            window.location.pathname +
            window.location.hash
        );
    }

    function getBotURL(botKey) {
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

    /*
     * RING VISIBILITY
     */
    function showLauncherRing() {
        launcherRing.style.display =
            "block";
    }

    function hideLauncherRing() {
        launcherRing.style.display =
            "none";
    }

    /*
     * SELECTOR UI
     */
    function openChoicePanel() {
        choicePanel.classList.add(
            "is-open"
        );

        loadingPanel.classList.remove(
            "is-open"
        );

        launcher.style.display =
            "none";

        hideLauncherRing();

        window.setTimeout(
            function () {
                choiceClose.focus();
            },
            10
        );
    }

    function closeChoicePanel() {
        choicePanel.classList.remove(
            "is-open"
        );

        launcher.style.display =
            "flex";

        showLauncherRing();

        launcher.focus();
    }

    function disableOptions(disabled) {
        Array.prototype.forEach.call(
            chatOptions,
            function (option) {
                option.disabled =
                    disabled;
            }
        );
    }

    /*
     * LOADING UI
     */
    function showLoadingPanel(botKey) {
        var config =
            BOT_CONFIG[botKey];

        choicePanel.classList.remove(
            "is-open"
        );

        loadingPanel.classList.add(
            "is-open"
        );

        launcher.style.display =
            "none";

        hideLauncherRing();

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

        if (botKey === "agentforce") {
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

    function showError(message) {
        clearInitializationTimer();

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

    /*
     * TIMER
     */
    function clearInitializationTimer() {
        if (!initializationTimer) {
            return;
        }

        window.clearTimeout(
            initializationTimer
        );

        initializationTimer =
            null;
    }

    /*
     * SELECT BOT
     */
    function selectBot(botKey) {
        if (!BOT_CONFIG[botKey]) {
            return;
        }

        window.location.assign(
            getBotURL(botKey)
        );
    }

    /*
     * HIDE NATIVE SALESFORCE LAUNCHER
     */
    function hideNativeSalesforceLauncher() {
        if (!buttonCreated) {
            return;
        }

        try {
            if (
                window.embeddedservice_bootstrap &&
                embeddedservice_bootstrap.utilAPI &&
                typeof embeddedservice_bootstrap.utilAPI
                    .hideChatButton === "function"
            ) {
                embeddedservice_bootstrap.utilAPI
                    .hideChatButton();
            }
        } catch (error) {
            console.warn(
                "Unable to hide Salesforce launcher.",
                error
            );
        }
    }

    /*
     * LAUNCH SELECTED CHAT
     */
    function launchSelectedChat() {
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
            typeof embeddedservice_bootstrap.utilAPI
                .launchChat !== "function"
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

        hideNativeSalesforceLauncher();

        embeddedservice_bootstrap.utilAPI
            .launchChat()
            .then(function () {
                clearInitializationTimer();

                hideLoadingPanel();

                /*
                 * Keep the frontend gradient ring available
                 * behind Salesforce's minimized launcher.
                 */
                showLauncherRing();

                /*
                 * Keep the custom generic icon hidden because
                 * the selected Salesforce ESD is active.
                 */
                launcher.style.display =
                    "none";
            })
            .catch(function (error) {
                launchStarted =
                    false;

                hideLauncherRing();

                showError(
                    error && error.message
                        ? error.message
                        : BOT_CONFIG[selectedBot].name +
                          " could not be opened."
                );

                console.error(
                    "Launch Chat failed:",
                    error
                );
            });
    }

    /*
     * SALESFORCE EVENTS
     */
    function addSalesforceEventListeners() {
        window.addEventListener(
            "onEmbeddedMessagingReady",
            function () {
                console.log(
                    BOT_CONFIG[selectedBot].name +
                    " messaging runtime is ready."
                );
            },
            {
                once: true
            }
        );

        window.addEventListener(
            "onEmbeddedMessagingButtonCreated",
            function () {
                buttonCreated =
                    true;

                bootstrapLoading =
                    false;

                clearInitializationTimer();

                hideNativeSalesforceLauncher();

                console.log(
                    BOT_CONFIG[selectedBot].name +
                    " chat button was created."
                );

                if (launchRequested) {
                    launchSelectedChat();
                }
            },
            {
                once: true
            }
        );

        window.addEventListener(
            "onEmbeddedMessagingConversationOpened",
            function () {
                clearInitializationTimer();

                hideLoadingPanel();

                /*
                 * The ring remains behind the native Salesforce
                 * minimized launcher after the user minimizes.
                 */
                showLauncherRing();

                launcher.style.display =
                    "none";
            }
        );

        window.addEventListener(
            "onEmbeddedMessagingConversationClosed",
            function () {
                clearInitializationTimer();

                window.location.replace(
                    getCleanURL()
                );
            }
        );
    }

    /*
     * INITIALIZE SELECTED DEPLOYMENT
     */
    function initializeSelectedDeployment() {
        var config =
            BOT_CONFIG[selectedBot];

        try {
            embeddedservice_bootstrap.settings.language =
                "en_US";

            /*
             * Align Salesforce's minimized launcher with
             * the centre of the frontend gradient ring.
             *
             * Format: bottom offset, right offset.
             */
            embeddedservice_bootstrap.settings
                .chatButtonPosition =
                    "25px,25px";

            embeddedservice_bootstrap.settings
                .hideChatButtonOnLoad =
                    true;

            embeddedservice_bootstrap.init(
                ORG_ID,
                config.deploymentName,
                config.siteURL,
                {
                    scrt2URL:
                        SCRT2_URL
                }
            );
        } catch (error) {
            bootstrapLoading =
                false;

            launchRequested =
                false;

            hideLauncherRing();

            showError(
                error && error.message
                    ? error.message
                    : config.name +
                      " failed to initialize."
            );

            console.error(
                "Error initializing " +
                config.name +
                ":",
                error
            );
        }
    }

    /*
     * LOAD SELECTED DEPLOYMENT
     */
    function loadSelectedDeployment() {
        var config =
            BOT_CONFIG[selectedBot];

        bootstrapLoading =
            true;

        launchRequested =
            true;

        showLoadingPanel(
            selectedBot
        );

        addSalesforceEventListeners();

        var bootstrapScript =
            document.createElement(
                "script"
            );

        bootstrapScript.type =
            "text/javascript";

        bootstrapScript.src =
            config.bootstrapURL;

        bootstrapScript.async =
            true;

        bootstrapScript.onload =
            function () {
                initializeSelectedDeployment();
            };

        bootstrapScript.onerror =
            function () {
                bootstrapLoading =
                    false;

                launchRequested =
                    false;

                hideLauncherRing();

                showError(
                    config.name +
                    " bootstrap file could not be loaded."
                );
            };

        document.body.appendChild(
            bootstrapScript
        );

        initializationTimer =
            window.setTimeout(
                function () {
                    if (!buttonCreated) {
                        bootstrapLoading =
                            false;

                        launchRequested =
                            false;

                        hideLauncherRing();

                        showError(
                            config.name +
                            " did not become ready. Check the ESD Allowed Domains, deployment status and browser console."
                        );

                        console.error(
                            config.name +
                            " initialization timed out.",
                            {
                                deploymentName:
                                    config.deploymentName,

                                siteURL:
                                    config.siteURL
                            }
                        );
                    }
                },
                25000
            );
    }

    /*
     * USER EVENTS
     */
    launcher.addEventListener(
        "click",
        function () {
            openChoicePanel();
        }
    );

    choiceClose.addEventListener(
        "click",
        function () {
            closeChoicePanel();
        }
    );

    Array.prototype.forEach.call(
        chatOptions,
        function (option) {
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
        function (event) {
            if (
                event.key === "Escape" &&
                choicePanel.classList.contains(
                    "is-open"
                )
            ) {
                closeChoicePanel();
            }
        }
    );

    /*
     * NETWORK WARM-UP
     */
    function addPreconnect(url) {
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

    /*
     * PAGE START
     */
    selectedBot =
        getSelectedBotFromURL();

    if (selectedBot) {
        /*
         * Selected ESD mode.
         */
        launcher.style.display =
            "none";

        hideLauncherRing();

        loadSelectedDeployment();
    } else {
        /*
         * Initial generic launcher mode.
         */
        launcher.style.display =
            "flex";

        showLauncherRing();
    }
})();
</script>

</div>
