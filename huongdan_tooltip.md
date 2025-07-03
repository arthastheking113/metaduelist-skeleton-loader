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
            <div class="tooltip-caret"></div>
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
            position = 'top';
        
            if (!cardEl || !tooltip) return;
        
            // Reset tooltip để đo đạc chính xác
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
        
            // Các vị trí để thử
            const belowTop = cardRect.height + spacing + 26;
            const aboveTop = -tooltipHeight - spacing + 0;
        
            // Vị trí trái để căn giữa
            let centerLeft = (cardRect.width - tooltipWidth) / 2;
        
            // Tính vị trí tuyệt đối của tooltip nếu đặt giữa
            let absoluteLeft = cardRect.left + centerLeft;
        
            // Điều chỉnh nếu tooltip tràn trái hoặc phải khỏi màn hình
            if (absoluteLeft < 0) {
                centerLeft -= absoluteLeft; // đẩy sang phải để không tràn trái
            } else if (absoluteLeft + tooltipWidth > window.innerWidth) {
                const overflowRight = absoluteLeft + tooltipWidth - window.innerWidth;
                centerLeft -= overflowRight; // đẩy sang trái để không tràn phải
            }
        
            const canShowBelow = (cardRect.bottom + spacing + tooltipHeight <= window.innerHeight);
            const canShowAbove = (cardRect.top - spacing - tooltipHeight >= topNavBottom);
        
            let positioned = false;
        
            // 1. Hiển thị bên dưới nếu đủ
            if (canShowBelow) {
                tooltip.style.top = `${belowTop}px`;
                tooltip.style.left = `${centerLeft}px`;
                positioned = true;
                position = 'bottom';
            }
            // 2. Nếu không, thử hiển thị bên trên
            else if (canShowAbove) {
                tooltip.style.top = `${aboveTop}px`;
                tooltip.style.left = `${centerLeft}px`;
                positioned = true;
                position = 'top';
            }
            // 3. Không đủ trên/dưới → sang phải
            else {
                const rightSpace = window.innerWidth - cardRect.right - spacing;
                const leftSpace = cardRect.left - spacing;
                const verticalCenter = (cardRect.height - tooltipHeight) / 2;
        
                if (rightSpace >= tooltipWidth) {
                    tooltip.style.left = `${cardRect.width + spacing}px`;
                    tooltip.style.top = `${verticalCenter}px`;
                    positioned = true;
                    position = 'right';

                } else if (leftSpace >= tooltipWidth) {
                    tooltip.style.right = `${cardRect.width + spacing}px`;
                    tooltip.style.top = `${verticalCenter}px`;
                    positioned = true;
                    position = 'left';
                }

            }
        
            // Nếu không định vị được, fallback đặt dưới lệch trái
            if (!positioned) {
                tooltip.style.top = `${belowTop}px`;
                tooltip.style.left = `0px`;
                position = 'bottom';
            }

            tooltip.classList.remove("tooltip--top", "tooltip--bottom", "tooltip--left", "tooltip--right");
            // Thêm class tương ứng
            if (position === "bottom") {
                tooltip.classList.add("tooltip--bottom");
            } else if (position === "top") {
                tooltip.classList.add("tooltip--top");
            } else if (position === "right") {
                tooltip.classList.add("tooltip--right");
            } else if (position === "left") {
                tooltip.classList.add("tooltip--left");
            }
        
            // Hiển thị tooltip
            tooltip.style.visibility = "visible";
            tooltip.style.opacity = "1";
        }
```


#3. Cập nhật CSS

```sh
    theme.css
    theme.min.css

```