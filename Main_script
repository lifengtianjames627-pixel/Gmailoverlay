// ==UserScript==
// @name         Gmail Privacy Overlay (No innerHTML, No Button)
// @namespace    https://example.com
// @version      1.0
// @description  Always blur and block Gmail content; no UI button
// @match        https://mail.google.com/*
// @match        https://*.google.com/mail/*
// @grant        GM_addStyle
// @run-at       document-idle
// ==/UserScript==

(function () {
    'use strict';

    const OVERLAY_ID  = 'gmail-privacy-overlay-2026';
    const BLUR_CLASS  = 'gmail-privacy-blur-2026';
    const FIXED_TITLE = 'Mail';

    // 1) Global CSS: blur body and show an overlay on top
    GM_addStyle(`
        body.${BLUR_CLASS} {
            filter: blur(12px) !important;
            pointer-events: none !important;   /* block clicks on Gmail content */
        }
        #${OVERLAY_ID} {
            position: fixed !important;
            top: 0 !important;
            left: 0 !important;
            right: 0 !important;
            bottom: 0 !important;
            background: rgba(0,0,0,0.85) !important;
            z-index: 2147483647 !important;
            display: flex !important;
            align-items: center !important;
            justify-content: center !important;
            color: white !important;
            font-family: sans-serif !important;
            font-size: 24px !important;
            pointer-events: auto !important;   /* overlay itself can take clicks */
        }
        #${OVERLAY_ID} > div {
            text-align: center !important;
        }
        #${OVERLAY_ID} > div > p {
            margin: 0 !important;
            line-height: 1.5 !important;
        }
    `);

    // 2) Create overlay using only createElement / textContent (no innerHTML)
    function createOverlay() {
        const overlay = document.createElement('div');
        overlay.id = OVERLAY_ID;

        const container = document.createElement('div');

        const line1 = document.createElement('p');
        line1.textContent = '🔒 Privacy protection is enabled';

        const line2 = document.createElement('p');
        line2.textContent = 'Gmail content is blurred and cannot be viewed.';

        container.appendChild(line1);
        container.appendChild(line2);
        overlay.appendChild(container);

        document.body.appendChild(overlay);
        return overlay;
    }

    // 3) Make sure overlay exists and is visible
    function ensureOverlay() {
        let overlay = document.getElementById(OVERLAY_ID);
        if (!overlay) {
            overlay = createOverlay();
        }
        overlay.style.display = 'flex';
    }

    // 4) Apply blur class to body
    function applyBlur() {
        document.body.classList.add(BLUR_CLASS);
    }

    // 5) Keep overlay alive if Gmail re-renders / removes it
    new MutationObserver((mutations) => {
        for (const mut of mutations) {
            for (const node of mut.removedNodes) {
                if (node.id === OVERLAY_ID) {
                    ensureOverlay();
                }
            }
        }
    }).observe(document.body, { childList: true, subtree: true });

    // Light periodic check as extra safety
    setInterval(ensureOverlay, 2000);

    // 6) Optional: sanitize tab title so your email address is not shown
    function fixTitle() {
        if (document.title !== FIXED_TITLE) {
            document.title = FIXED_TITLE;
        }
    }
    setInterval(fixTitle, 1000);
    const titleEl = document.querySelector('title');
    if (titleEl) {
        new MutationObserver(fixTitle).observe(titleEl, { childList: true });
    }

    // 7) Initialize: always on
    applyBlur();
    ensureOverlay();
})();
