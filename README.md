# UI skeleton loading template for Meta Duelist

Open `index.html` to see all components need to have a loading skeleton.

Every component has its own component section.

Below each component is its skeleton section. Its skeleton need to be added into that section.

List of components:
- Tierlist Component
- Homepage Component
- Menu Component
- Guide Menu Component
- Blog Menu Component
- Blog Component
- Deck Component
- Guide Component

Skeleton need to be responsive as same as its component.

Skeleton color is undefined right now, you can propose your idea on what is the good color pattern for this website.


# Solution
Xây dựng khung xương cho các thẻ sau: 

### Tất cả item được bao trong thẻ `skeleton-block`
    ```
        <div class="skeleton-block" aria-label="Loading...">
            <!-- content here -->
        </div>
    ```
### Style chung được thêm vào thẻ `skeleton-loading`
    ```
        <div class="skeleton-block">
            <div class="skeleton-block other-class" aria-label="Loading...">&nbsp;</div>
        </div>
    ```

### Image ( thẻ `<img>`):
 * Sử dụng div class `image` để làm khung
 * Option1: yêu cầu các style bổ sung như width, height, border-radius
 * Option2: Yêu cầu thẻ cha có width, height cố định. Mặc định thẻ image sẽ width: 100%; height: 100%
    ```
        <div class="skeleton-block" aria-label="Loading...">
            <div class="skeleton-dimension-holder" style="width: 100%; height: 200px;">
                <div class="skeleton-loading image">&nbsp;</div>
            </div>

            <!-- Hoặc -->

            <div class="skeleton-loading image" style="width: 300px; height: 150px;">&nbsp;</div>
        </div>
    ```

### Heading: ( thẻ h `h1`-`h6`)
* Sử dụng thẻ tương ứng với class `text-block`
* Yêu cầu các style bổ sung thẻ hiện chiều rộng thẻ
* border-radius mặc định là 5px
* Các thẻ H có chiều cao lần lượt là 24px; 22px; 20px; 18px; 16px; 14px;
    ```
        <div class="skeleton-block" aria-label="Loading...">
            <h2 class="skeleton-dimension-holder text-block" style="width: 80px;">
            </h2>>
        </div>
    ```

### Text: ( tất cả các thẻ hiển thị text) 
* Sử dụng thẻ tùy trường hợp ( span, div, p, h ...) với class  `text-block`
* Yêu cầu các style bổ sung thẻ hiện chiều rộng thẻ
* border-radius mặc định là 5px
    ```
        <div class="skeleton-block" aria-label="Loading...">
            <p class="skeleton-dimension-holder text-block" style="width: 80px;">
            </p>
            <!-- Inline span -->
            <div class="d-flex gap-2 mb-3">
                <span class="col-2 grow-0 skeleton-loading text-block"></span>
                <span class="col-2 grow-0 skeleton-loading text-block"></span>
                <span class="col grow-1 skeleton-loading text-block"></span>
            </div>
        </div>
    ```

### Các thẻ inline: 
* Sử dụng `span` hoặc `div` với class xác định chiều rộng
* Các thẻ inline có thể có các hiệu ứng tuần tự bằng cách sử dụng các class skeleton-delay-$i ( $i từ 1 đến 5 ) để xác định thẻ sẽ có hiệu ứng delay `200*$i` mili giây
    *Ví dụ thẻ skeleton-delay-2 sẽ có hiệu ứng chậm hơn thẻ bình thường 2*200 = 400 ms*

    ```
        <div class="skeleton-block" aria-label="Loading...">
            <div class="d-flex gap-2 mb-3">
                <span class="col-2 grow-0 skeleton-loading skeleton-delay-1 text-block"></span>
                <span class="col-2 grow-0 skeleton-loading skeleton-delay-2 text-block"></span>
                <span class="col grow-1 skeleton-loading skeleton-delay-3 text-block"></span>
            </div>
        </div>
    ```

### Khoảng trống được xác định bằng class `skeleton-padding`
* Yêu cầu các style bổ sung thẻ hiện chiều cao thẻ
* Mặc định là cao 30px ( padding: 0 15px )

    ```
        <div class="skeleton-block" aria-label="Loading...">
            <h2 class="skeleton-dimension-holder text-block" style="width: 80px;" aria-label="Heading" > </h2>

            <!-- Padding 45px -->
            <div class="skeleton-padding" style="padding: 45px 0 0 0;"></div>

            <div class="d-flex gap-2 mb-3">
                <span class="col-2 grow-0 skeleton-loading skeleton-delay-1 text-block"></span>
                <span class="col-2 grow-0 skeleton-loading skeleton-delay-2 text-block"></span>
                <span class="col grow-1 skeleton-loading skeleton-delay-3 text-block"></span>
            </div>
        </div>
    ```
