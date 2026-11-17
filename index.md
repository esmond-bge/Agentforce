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

        --launcher-size: 64px;
        --launcher-ring-width: 7px;

        --launcher-gradient:
            conic-gradient(
                from 210deg,
                #0075bd 0deg,
                #006bb4 105deg,
                #4e4aa3 190deg,
                #893e98 270deg,
                #d5007f 340deg,
                #0075bd 360deg
            );
    }

    /*
     * FRONTEND RING
     *
     * This is repositioned around:
     * - the initial custom launcher
     * - the Einstein minimized host
     * - the Agentforce minimized host
     *
     * It is hidden while the full chat is open.
     */
    #chat-launcher-ring {
        position: fixed;
        z-index: 999990;

        display: none;

        border-radius: 50%;

        background:
            var(--launcher-gradient);

        box-shadow:
            0 3px 12px rgba(0, 59, 92, 0.2);

        pointer-events: none;
    }

    /*
     * INITIAL CUSTOM LAUNCHER
     *
     * The launcher remains white. The separate frontend
     * ring is positioned behind this button.
     */
    #custom-chat-launcher {
        position: fixed;
        right: 24px;
        bottom: 24px;
        z-index: 999998;

        display: none;

        width: var(--launcher-size);
        height: var(--launcher-size);

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
        z-index: 999999;

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

        padding: 6px;

        align-items: center;
        justify-content: center;

        border-radius: 50%;

        background:
            var(--launcher-gradient);
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
        #custom-chat-launcher {
            right: 18px;
            bottom: 18px;
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


<div
    id="chat-launcher-ring"
    aria-hidden="true"
></div>


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
                
