# recycle-view

## Giới thiệu

`recycle-view` dùng để hiển thị một danh sách dữ liệu lớn, giúp tối ưu performance khi chỉ render một số lượng các item cần thiết trong phạm vi nhìn thấy của người dùng.

## Cách sử dụng

- recycle-view có height mặc định là window height, bạn có thể thay đổi thông qua style của recycle-view
- Có hai loại recycle-view: grid view và list view. Mặc định sẽ là list view. Mỗi loại recycle view sẽ có những thuộc tính khác nhau, một số thuộc tính có thể dùng chung. Các thuộc tính dùng chung bao gồm:

| Thuộc tính     | Kiểu dữ liệu | Bắt buộc | Mô tả                                                                                                                                                                                                                                   |
| -------------- | ------------ | :------: | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| totalCount     | string       |    ✓     | Tổ số lượng các phần tử có trong mảng.                                                                                                                                                                                                  |
| isGrid         | boolean      |          | Chỉ định hiển thị view dưới dạng grid, có thể có nhiều phần tử trên 1 dòng, mặc định là false.                                                                                                                                          |
| hasPlaceholder | boolean      |          | Khi scroll quá nhanh thì có thể sẽ bị trắng 1 lúc do quá trình tính toán, có thể dùng placeholder để hiển thị thay vùng trắng đó. Bắt buộc phải dùng chung với component `<recycle-footer>`. Mặc định là false                          |
| hasLoading     | boolean      |          | Do recycle view sử dụng cơ chế lazy load, nên có thể mất một khoảng thời gian để khởi tạo component, có thể dùng loading để hiển thị trong lúc chờ component được load xong. Bắt buộc phải dùng chung với component `<recycle-loading>` |
| onReady        | event        |          | Sự kiện được gọi khi recycle view được khởi tạo xong                                                                                                                                                                                    |
| onEndReached   | event        |          | Sự kiện được gọi khi scroll tới vị trí của phần tử cuối cùng trong danh sách                                                                                                                                                            |
| onScrolling    | event        |          | Sự kiện được gọi khi đang scroll, trả về detail `{isScrolling: boolean}`, với isScrolling là true nếu đang scroll, false nếu dừng scroll                                                                                                |
| onRangeChanged | event        |          | Sự kiện được gọi khi các phần tử phạm vi hiển thị bị thay đổi, trả về detail `{startIndex: number, endIndex: number}`                                                                                                                   |

## List view

| Thuộc tính              | Kiểu dữ liệu | Bắt buộc | Mô tả                                                                                                                                                                                                            |
| ----------------------- | ------------ | :------: | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| initialTopMostItemIndex | number       |          | Vị trí của phần tử được xuất hiện đầu tiên, chỉ có tác dụng ở lần đầu khi list được hiển thị, khi thay đổi giá trị này list sẽ không tự động scroll tới. Mặc định là 0, giá trị trong khoảng từ 0 tới totalCount |

Cấu trúc của list view:

```xml
<recycle-view>
  <recycle-item />
  <recycle-footer />
  <recycle-loading />
  <recycle-placeholder />
</recycle-view>
```

## Grid view

- Grid view và List view dùng chung component là `recycle-view`, tuy nhiên grid cho phép hiển thị nhiều item trên một dòng (số lượng item trên 1 dòng là giống nhau)
- Để dùng grid view thì cần khai báo `isGrid` = true.
- Các item cần có cùng chiều cao (height) với nhau, nếu không có thể gây ra việc scroll không đúng. Có thể thay đổi chiều cao qua style hoặc class của recycle-item.
- Tham khảo code trong [src/pages/grid](src/pages/grid)

| Thuộc tính              | Kiểu dữ liệu | Bắt buộc | Mô tả                                                                                                                                                                                                            |
| ----------------------- | ------------ | :------: | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| initialTopMostItemIndex | number       |          | Vị trí của phần tử được xuất hiện đầu tiên, chỉ có tác dụng ở lần đầu khi list được hiển thị, khi thay đổi giá trị này list sẽ không tự động scroll tới. Mặc định là 0, giá trị trong khoảng từ 0 tới totalCount |

Cấu trúc của list view:

```xml
<recycle-view isGrid="{{true}}">
  <recycle-item />
  <recycle-loading />
  <recycle-placeholder />
</recycle-view>
```

## Các component con

- `recycle-item`
  Danh sách các item được hiển thị. Bắt buộc phải có trong recycle view và phải đặt ở vị trí trên cùng so với các component con khác

- `recycle-footer`
  Chỉ hỗ trợ dạng list
- `recycle-loading`
  Hỗ trợ cả list và grid
- `recycle-placeholder`
  Hỗ trợ cả list và grid

| Thuộc tính | Kiểu dữ liệu | Bắt buộc | Mô tả                      |
| ---------- | ------------ | -------- | -------------------------- |
| class      | string       |          | Custom class cho component |
| style      | string       |          | Inline style cho component |

## createRecycleContext

Tham khảo code trong [src/pages/scroll](src/pages/scroll)

Dùng để thực hiện các tác vụ cho `recycle-view`. Hỗ trợ các hàm:

- scrollToIndex(location: number | IndexLocationWithAlign)
- scrollIntoView(location: ScrollIntoViewLocation)
- scrollTo(location: ScrollToOptions)
- scrollBy(location: ScrollToOptions)

Trong đó:

```typescript
type IndexLocationWithAlign = {
  /**
   * Vị trí của phần tử muốn scroll tới
   */
  index: number;
  /**
   * Vị trí của item trong viewport.
   */
  align?: 'start' | 'center' | 'end';
  /**
   * Sử dụng smooth nếu muốn scroll mượt hơn, mặc định là auto
   */
  behavior?: 'smooth' | 'auto';
};

type ScrollIntoViewLocation = {
  /**
   * Vị trí của phần tử muốn scroll tới
   */
  index: number;
  /**
   * Sử dụng smooth nếu muốn scroll mượt hơn, mặc định là auto
   */
  behavior?: 'auto' | 'smooth';
};

type ScrollToOptions = {
  /**
   * Top px muốn scroll tới
   */
  top?: number;
  /**
   * Sử dụng smooth nếu muốn scroll mượt hơn, mặc định là auto
   */
  behavior?: 'auto' | 'smooth';
};
```

Lưu ý: behavior smooth không hỗ trợ cho iOS https://caniuse.com/?search=scroll-behavior
