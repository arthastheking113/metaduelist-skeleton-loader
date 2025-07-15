Hướng dẫm cập nhật tooltip cho website 

# 1. Cập nhật card component

Thêm 
```html
<div class="tooltip-caret"></div>
``` 
vào tooltip 

```html
<div class="col-2" onclick="openCardQuickView(29095457)">
    <div class="tooltip-special">
        <a class="d-none d-lg-block shining-img" 
            id="card-view-946c0a6a-5064-4843-8b59-8b32e5af9493" 
            onmouseenter="onCardViewEvent(this)" 
            onmouseout="hideWhenMouseOut(this)" 
            href="/card/Primite Drillbeam">
        </a>

                                                                        
        <div class="tooltiptext-special-card tooltip--bottom" id="card-view-tool-tip-946c0a6a-5064-4843-8b59-8b32e5af9493">
            <div class="tooltip-caret"></div>. <!-- THÊM DÒNG NÀY -->
            <div class="row">
                
            </div>
        </div>
    </div>
</div>
```

# 2. Cập nhật hàm tooltip 

```js
        function onCardViewEvent(e) {
            if (!e || !e.id) return;
          
            const currentId = e.id;
            const cardId = currentId.replace("card-view-", "");
            const cardEl = document.getElementById(currentId);
            const tooltip = document.getElementById(`card-view-tool-tip-${cardId}`);
            const topNav = document.getElementById("top-nav");
            const thumbEl = cardEl?.querySelector('.thumbnail');
            let position = 'top';
          
            if (!cardEl || !tooltip) return;
          
            tooltip.style.visibility = "hidden";
            tooltip.style.opacity = "0";
            tooltip.style.top = "unset";
            tooltip.style.bottom = "unset";
            tooltip.style.left = "unset";
            tooltip.style.right = "unset";
          
            const spacing = 8;
            const cardRect = cardEl.getBoundingClientRect();
            const tooltipWidth = tooltip.offsetWidth;
            const tooltipHeight = tooltip.offsetHeight;
            const topNavBottom = topNav ? topNav.getBoundingClientRect().bottom : 0;
            const spaceTop = thumbEl ? parseInt(window.getComputedStyle(thumbEl).marginTop) : 0;
            let header = document.querySelector('header .navbar-sticky'); 
            let headerHeight = header ? header.offsetHeight : 0;
          
            const centerX = window.innerWidth / 2;
            const centerY = window.innerHeight / 2;
            const cardCenterX = cardRect.left + cardRect.width / 2;
            const cardCenterY = cardRect.top + cardRect.height / 2;
          
            const isLeft = cardCenterX < centerX;
            const isRight = cardCenterX >= centerX;
            const isTop = cardCenterY < centerY;
            const isBottom = cardCenterY >= centerY;
          
            const candidates = [];
          
            const tryBottom = () => {
              const top = cardRect.bottom + spacing;
              if (top + tooltipHeight <= window.innerHeight) {
                const left = Math.max(0, Math.min(
                  cardRect.left + (cardRect.width - tooltipWidth) / 2,
                  window.innerWidth - tooltipWidth
                ));
                candidates.push({ pos: 'bottom', top, left });
              }
            };
          
            const tryTop = () => {
              const top = cardRect.top - spacing - tooltipHeight;
              if (top >= topNavBottom) {
                const left = Math.max(0, Math.min(
                  cardRect.left + (cardRect.width - tooltipWidth) / 2,
                  window.innerWidth - tooltipWidth
                ));
                candidates.push({ pos: 'top', top, left });
              }
            };
          
            const tryRight = () => {
              const left = cardRect.right + spacing;
              if (left + tooltipWidth <= window.innerWidth) {
                const top = Math.max(headerHeight, Math.min(
                  cardRect.top + (cardRect.height - tooltipHeight) / 2,
                  window.innerHeight - tooltipHeight
                ));
                candidates.push({ pos: 'right', top, left });
              }
            };
          
            const tryLeft = () => {
              const left = cardRect.left - spacing - tooltipWidth;
              if (left >= 0) {
                const top = Math.max(headerHeight, Math.min(
                  cardRect.top + (cardRect.height - tooltipHeight) / 2,
                  window.innerHeight - tooltipHeight
                ));
                candidates.push({ pos: 'left', top, left });
              }
            };
          
            if (isLeft) {
              tryRight(); tryBottom(); tryTop(); tryLeft();
            } else if (isRight) {
              tryLeft(); tryBottom(); tryTop(); tryRight();
            } else if (isTop) {
              tryBottom(); tryRight(); tryLeft(); tryTop();
            } else {
              tryTop(); tryRight(); tryLeft(); tryBottom();
            }
          
            let positioned = false;
            if (candidates.length > 0) {
            const centerX = window.innerWidth / 2;
            const centerY = window.innerHeight / 2;

            // Tìm candidate có khoảng cách đến tâm màn hình nhỏ nhất
            const best = candidates.reduce((a, b) => {
                const aCenterX = a.left + tooltipWidth / 2;
                const aCenterY = a.top + tooltipHeight / 2;
                const bCenterX = b.left + tooltipWidth / 2;
                const bCenterY = b.top + tooltipHeight / 2;

                const aDist = Math.hypot(centerX - aCenterX, centerY - aCenterY);
                const bDist = Math.hypot(centerX - bCenterX, centerY - bCenterY);

                return aDist < bDist ? a : b;
            });

            tooltip.style.top = `${best.top}px`;
            tooltip.style.left = `${best.left}px`;
            position = best.pos;
            positioned = true;
            }
          
            if (!positioned) {
              tooltip.style.top = `${cardRect.bottom + spacing}px`;
              tooltip.style.left = `0px`;
              position = 'bottom';
            }
          
            tooltip.classList.remove("tooltip--top", "tooltip--bottom", "tooltip--left", "tooltip--right");
            tooltip.classList.add(`tooltip--${position}`);
          
            const caret = tooltip.querySelector('.tooltip-caret');
            if (caret) {
              caret.style.left = '';
              caret.style.top = '';
              caret.style.transform = '';
          
              if (position === 'top' || position === 'bottom') {
                const cardCenter = cardRect.left + cardRect.width / 2;
                const tooltipLeft = parseFloat(tooltip.style.left);
                const caretOffset = cardCenter - tooltipLeft;
                const safeOffset = Math.max(12, Math.min(caretOffset, tooltip.offsetWidth - 12));
                caret.style.left = `${safeOffset}px`;
                caret.style.transform = 'translateX(-50%)';
              } else if (position === 'left' || position === 'right') {
                const cardCenter = cardRect.top + cardRect.height / 2;
                const tooltipTop = parseFloat(tooltip.style.top);
                const caretOffset = cardCenter - tooltipTop;
                const safeOffset = Math.max(12, Math.min(caretOffset, tooltip.offsetHeight - 12));
                caret.style.top = `${safeOffset}px`;
                caret.style.transform = 'translateY(-50%)';
              }
            }
          
            tooltip.style.visibility = "visible";
            tooltip.style.opacity = "1";
        }
```


#3. Cập nhật CSS

```sh
    theme.css
    theme.min.css

```

Thêm CSS
```css
.tooltip-special .tooltiptext-special-card {   /* Có sẵn */
border: 2px solid #1c3662;                   /* Có sẵn */
border-radius: 2px;                            /* Có sẵn */
    position: fixed;                           /* THÊM MỚI */
}
```