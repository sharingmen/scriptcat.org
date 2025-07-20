
// ==UserScript==
// @name         (秒刷)智慧中小学寒暑假教师研修 - 脚本安装助手
// @namespace    http://tampermonkey.net/
// @version      0.700a
// @author       hydrachs
// @description  禁止非法修改与二次分发-换用新方法进行刷课，谢谢九州.提供的技术支持
// @license      Proprietary
// @match        https://basic.smartedu.cn/*
// @match        https://www.smartedu.cn/*
// @grant        GM_openInTab
// ==/UserScript==

(function() {
    'use strict';

    const REMOTE_SCRIPT_URL = 'http://szmy067gj.hd-bkt.clouddn.com/v0.702.user.js';

    function createFloatingButton() {
        const container = document.createElement('div');
        container.id = 'script-installer';
        container.style.cssText = `
            position: fixed;
            top: 50px;
            right: 50px;
            z-index: 9999;
            background: rgba(255, 255, 255, 0.9);
            border-radius: 12px;
            box-shadow: 0 4px 12px rgba(0, 0, 0, 0.15);
            padding: 8px;
            width: 140px;
            height: 50px;
            box-sizing: border-box;
            cursor: move;
        `;

        const button = document.createElement('button');
        button.style.cssText = `
            background: #4CAF50;
            color: white;
            border: none;
            border-radius: 8px;
            width: 100%;
            height: 100%;
            font-size: 14px;
            font-weight: bold;
            cursor: pointer;
            transition: background 0.2s ease;
        `;
        button.textContent = '加载刷课脚本';

        button.addEventListener('mouseenter', () => {
            button.style.background = '#45a049';
        });

        button.addEventListener('mouseleave', () => {
            button.style.background = '#4CAF50';
        });

        button.addEventListener('click', () => {
            if (confirm('点击确定后将打开速刷脚本的安装页面。\n请手动打开新脚本，请手动打开新脚本，请手动打开新脚本')) {
                GM_openInTab(REMOTE_SCRIPT_URL, { active: true, insert: true });
            }
        });

        container.appendChild(button);
        document.body.appendChild(container);

        let isDragging = false;
        let offsetX, offsetY;
        let initialStyles = {};

        container.addEventListener('mousedown', (e) => {
            initialStyles = {
                width: container.style.width,
                height: container.style.height,
                borderRadius: container.style.borderRadius
            };

            isDragging = true;
            const rect = container.getBoundingClientRect();
            offsetX = e.clientX - rect.left;
            offsetY = e.clientY - rect.top;

            e.preventDefault();
        });

        document.addEventListener('mousemove', (e) => {
            if (isDragging) {
                const x = e.clientX - offsetX;
                const y = e.clientY - offsetY;

                const maxX = Math.max(0, window.innerWidth - container.offsetWidth);
                const maxY = Math.max(0, window.innerHeight - container.offsetHeight);

                container.style.left = Math.min(x, maxX) + 'px';
                container.style.top = Math.min(y, maxY) + 'px';

                container.style.width = initialStyles.width;
                container.style.height = initialStyles.height;
                container.style.borderRadius = initialStyles.borderRadius;
            }
        });

        document.addEventListener('mouseup', () => {
            isDragging = false;
        });
    }

    if (document.readyState === 'loading') {
        document.addEventListener('DOMContentLoaded', createFloatingButton);
    } else {
        createFloatingButton();
    }
})();
