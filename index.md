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
        --bge-page: #ffffff;
        --bge-close: #f4f4f4;
        --bge-teal: #00b7a5;
        --bge-purple: #89449b;
    }

    #custom-chat-launcher,
    #chat-choice-panel,
    #custom-chat-launcher *,
    #chat-choice-panel * {
        box-sizing: border-box;
        font-family: Arial, Helvetica, sans-serif;
    }

    /*
     * GENERIC CHAT LAUNCHER
     */
    #custom-chat-launcher {
        position: fixed;
        right: 24px;
        bottom: 24px;
        z-index: 999998;

        display: flex;
        width: 62px;
        height: 62px;

        padding: 0;

        align-items: center;
        justify-content: center;

        border: 0;
        border-radius: 50%;

        background: var(--bge-navy);
        color: #ffffff;

        cursor: pointer;

        box-shadow:
            0 5px 15px rgba(0, 0, 0, 0.18);

        transition:
            transform 0.15s ease,
            box-shadow 0.15s ease;
    }

    #custom-chat-launcher:hover {
        transform: translateY(-2px);

        box-shadow:
            0 7px 18px rgba(0, 0, 0, 0.22);
    }

    #custom-chat-launcher svg {
        width: 29px;
        height: 29px;
        fill: currentColor;
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
     * SMALL HANDLE AT TOP
     */
    #chat-drag-handle {
        width: 40px;
        height: 4px;

        margin:
            0 auto
            12px;

        border-radius: 50px;

        background: #b9b9b5;
    }

    /*
     * HEADER
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
     * OPTIONS
     */
    #chat-options {
        display: grid;
        gap: 12px;
    }

    .chat-option {
        position: relative;

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

    .chat-option:hover {
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
     * ICON WRAPPER WITH COLOURED BORDER
     */
    .chat-option-icon-shell {
        position: relative;

        display: flex;

        width: 46px;
        height: 46px;

        flex: 0 0 46px;

        align-items: center;
        justify-content: center;

        border-radius: 9px;

        background:
            linear-gradient(
                135deg,
                var(--bge-teal) 0%,
                var(--bge-teal) 46%,
                var(--bge-purple) 54%,
                var(--bge-purple) 100%
            );
    }

    .chat-option-icon-inner {
        display: flex;

        width: 40px;
        height: 40px;

        align-items: center;
        justify-content: center;

        border-radius: 7px;

        background: #ffffff;

        color: var(--bge-navy);
    }

    .chat-option-icon-inner svg {
        width: 23px;
        height: 23px;
    }

    /*
     * TEXT
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

    /*
     * RIGHT CHEVRON
     */
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
        font-weight: 400;
        line-height: 1.35;
    }

    /*
     * NAVIGATION STATUS
     */
    #chat-navigation-status {
        display: none;

        margin-top: 12px;
        padding: 10px 12px;

        border-radius: 8px;

        background: #f2f7f8;

        color: var(--bge-navy);

        font-size: 13px;
        line-height: 1.35;
    }

    #chat-navigation-status.visible {
        display: block;
    }

    /*
     * FOCUS
     */
    #custom-chat-launcher:focus-visible,
    #chat-choice-close:focus-visible,
    .chat-option:focus-visible {
        outline:
            3px solid
            #00a99d;

        outline-offset: 3px;
    }

    /*
     * MOBILE
     */
    @media (max-width: 600px) {

        #custom-chat-launcher {
            right: 18px;
            bottom: 18px;
        }

        #chat-choice-panel {
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


<!-- GENERIC CHAT BUTTON -->

<button
    id="custom-chat-launcher"
    type="button"
    aria-label="Chat with us"
    title="Chat with us"
>
    <svg
        viewBox="0 0 24 24"
        aria-hidden="true"
    >
        <path
            d="M4 4h16a2 2 0 0 1 2 2v10a2 2 0 0 1-2 2H8l-5 4v-4.5A2 2 0 0 1 2 16V6a2 2 0 0 1 2-2Zm2.5 6a1.5 1.5 0 1 0 0 3 1.5 1.5 0 0 0 0-3Zm5.5 0a1.5 1.5 0 1 0 0 3 1.5 1.5 0 0 0 0-3Zm5.5 0a1.5 1.5 0 1 0 0 3 1.5 1.5 0 0 0 0-3Z"
        ></path>
    </svg>
</button>


<!-- BOT SELECTOR -->

<section
    id="chat-choice-panel"
    role="dialog"
    aria-modal="false"
    aria-labelledby="chat-choice-title"
>

    <div
        id="chat-drag-handle"
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
            id="agentforce-option"
            class="chat-option"
            type="button"
            data-page="agentforce.html"
            data-name="Explore plans & switch"
        >

            <span
                class="chat-option-icon-shell"
                aria-hidden="true"
            >

                <span
                    class="chat-option-icon-inner"
                >

                    <svg
                        viewBox="0 0 24 24"
                        aria-hidden="true"
                    >
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
            id="einstein-option"
            class="chat-option"
            type="button"
            data-page="einstein.html"
            data-name="Help with my account"
        >

            <span
                class="chat-option-icon-shell"
                aria-hidden="true"
            >

                <span
                    class="chat-option-icon-inner"
                >

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

                            <path
                                d="M12 4v3"
                            ></path>

                            <path
                                d="M10 4h4"
                            ></path>

                            <path
                                d="M8 18v2"
                            ></path>

                            <path
                                d="M16 18v2"
                            ></path>

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

                            <path
                                d="M9 15h6"
                            ></path>
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


    <div
        id="chat-navigation-status"
        role="status"
        aria-live="polite"
    ></div>


    <p id="chat-choice-footnote">
        To switch later, close the chat and choose again.
    </p>

</section>


<script type="text/javascript">
(function () {

    "use strict";


    var launcher =
        document.getElementById(
            "custom-chat-launcher"
        );


    var panel =
        document.getElementById(
            "chat-choice-panel"
        );


    var closeButton =
        document.getElementById(
            "chat-choice-close"
        );


    var status =
        document.getElementById(
            "chat-navigation-status"
        );


    var options =
        document.querySelectorAll(
            ".chat-option"
        );


    var navigating =
        false;


    function openPanel() {

        if (navigating) {
            return;
        }


        panel.classList.add(
            "is-open"
        );


        launcher.style.display =
            "none";


        window.setTimeout(
            function () {

                closeButton.focus();

            },
            10
        );
    }


    function closePanel() {

        if (navigating) {
            return;
        }


        panel.classList.remove(
            "is-open"
        );


        launcher.style.display =
            "flex";


        launcher.focus();
    }


    function showNavigationStatus(name) {

        status.textContent =
            "Opening " +
            name +
            "...";


        status.classList.add(
            "visible"
        );
    }


    function disableOptions() {

        Array.prototype.forEach.call(
            options,
            function (option) {

                option.disabled =
                    true;

            }
        );
    }


    function navigateToBot(button) {

        if (navigating) {
            return;
        }


        var page =
            button.getAttribute(
                "data-page"
            );


        var name =
            button.getAttribute(
                "data-name"
            );


        if (!page) {
            return;
        }


        navigating =
            true;


        showNavigationStatus(
            name ||
            "chat"
        );


        disableOptions();


        window.location.href =
            page;
    }


    launcher.addEventListener(
        "click",
        function () {

            openPanel();

        }
    );


    closeButton.addEventListener(
        "click",
        function () {

            closePanel();

        }
    );


    Array.prototype.forEach.call(
        options,
        function (button) {

            button.addEventListener(
                "click",
                function () {

                    navigateToBot(
                        button
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
                panel.classList.contains(
                    "is-open"
                )
            ) {

                closePanel();

            }

        }
    );


    /*
     * Prefetch both destination pages.
     *
     * Salesforce itself is NOT initialized here.
     */
    function prefetchPage(page) {

        var link =
            document.createElement(
                "link"
            );


        link.rel =
            "prefetch";


        link.href =
            page;


        document.head.appendChild(
            link
        );
    }


    prefetchPage(
        "agentforce.html"
    );


    prefetchPage(
        "einstein.html"
    );


    /*
     * Warm up Salesforce network connections.
     */
    function preconnect(url) {

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


    preconnect(
        "https://bordgaisenergyeandu--afdev2.sandbox.my.site.com"
    );


    preconnect(
        "https://bordgaisenergyeandu--afdev2.sandbox.my.salesforce-scrt.com"
    );


})();
</script>

</div>
