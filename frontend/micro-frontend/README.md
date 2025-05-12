## Mục lục

[1. Các chiến lược triển khai MicroFrontend](#1-các-chiến-lược-triển-khai-microfrontend)

**Lợi ích của Micro-Frontend**

✅ **Khả năng mở rộng** – Nhiều team làm việc song song mà không xung đột.

✅ **Dễ bảo trì** – Codebase nhỏ giúp debug và cập nhật dễ dàng hơn.

✅ **Release nhanh** – Triển khai độc lập giúp đẩy nhanh tốc độ phát triển.

✅ **Linh hoạt công nghệ** – Không bị khóa vào một framework duy nhất.

✅ **Khả năng chịu lỗi** – Lỗi ở một micro-frontend không làm sập cả ứng dụng.

---

**Thách thức khi Áp dụng**

⚠ **Độ phức tạp tăng** – Quản lý nhiều repo, build pipeline đòi hỏi công sức.

⚠ **Ảnh hưởng hiệu năng** – Tải nhiều bundle có thể làm chậm trang.

⚠ **Xử lý nghiệp vụ chung** – Authentication, theming, state management cần được đồng bộ.

⚠ **Rủi ro tích hợp** – Đảm bảo trải nghiệm người dùng mượt mà giữa các thành phần độc lập.

### 1. Các chiến lược triển khai MicroFrontend

**1. Tích hợp tại Build Time (NPM Packages)**

- Mỗi micro-frontend được đóng gói thành NPM package.
- App chính import chúng lúc build.

✔ **Ưu điểm**: Đơn giản, hiệu năng tốt (gộp chung một bundle).

❌ **Nhược điểm**: Liên kết chặt, cần redeploy app chính khi cập nhật.

```json
// package.json của app chính
{
  "dependencies": {
    "product-list": "1.0.0",
    "shopping-cart": "2.1.0"
  }
}
```

**2. Tích hợp tại Runtime qua Iframe**

- Mỗi micro-frontend chạy trong một `<iframe>` riêng.

✔ **Ưu điểm**: Cách ly tốt, đa công nghệ dễ dàng.

❌ **Nhược điểm**: UX kém (khó chia sẻ state/styling), khó SEO.

```html
<div>
  <iframe src="https://product-list.example.com"></iframe>
  <iframe src="https://cart.example.com"></iframe>
</div>
```

**3. Tích hợp qua Web Components**

- Micro-frontend được xây dựng thành custom elements (`<my-component>`).

✔ **Ưu điểm**: Hỗ trợ sẵn bởi trình duyệt, không phụ thuộc framework.

❌ **Nhược điểm**: Khó chia sẻ state, cần polyfill cho trình duyệt cũ.

```tsx
// Micro-frontend (dùng LitElement)
@customElement("product-list")
class ProductList extends LitElement {
  render() {
    return html`<h2>Danh sách sản phẩm</h2>`;
  }
}
```

```tsx
<!-- App chính -->
<product-list></product-list>
```

**4. Tích hợp qua JavaScript Modules (Module Federation)**

- Webpack 5 **Module Federation** tải micro-frontend động lúc runtime.

✔ **Ưu điểm**: Lazy loading, chia sẻ dependencies, linh hoạt.

❌ **Nhược điểm**: Cấu hình phức tạp, quản lý dependency khó.

```tsx
// webpack.config.js (Micro-frontend)
new ModuleFederationPlugin({
  name: "productList",
  filename: "remoteEntry.js",
  exposes: { "./ProductList": "./src/ProductList" },
});
```

```tsx
// App chính tải động
const ProductList = await import("productList/ProductList");
```

**5. Edge Side Includes (ESI) – Tích hợp ở CDN**
- CDN ghép các fragment từ micro-frontend khác nhau.
    
✔ **Ưu điểm**: Tốc độ cao (cache tại CDN), đơn giản.
    
❌ **Nhược điểm**: Chỉ phù hợp nội dung tĩnh, cần CDN hỗ trợ.

```ts
<esi:include src="https://product-list.example.com" />
```

 **So sánh Các Chiến lược**

| Chiến lược | Tự chủ Team | Đa công nghệ | Hiệu năng | Độ phức tạp |
| --- | --- | --- | --- | --- |
| **Build-Time (NPM)** | Thấp | Thấp | Cao | Thấp |
| **Iframe** | Cao | Cao | Thấp | Thấp |
| **Web Components** | Trung bình | Trung bình | Trung bình | Trung bình |
| **Module Federation** | Cao | Cao | Trung bình | Cao |
| **ESI (CDN)** | Thấp | Thấp | Cao | Thấp |

**Kinh nghiệm Thực tế khi Áp dụng**

🔹 **Dùng Design System thống nhất** – Đảm bảo giao diện đồng bộ.

🔹 **Chuẩn hóa nghiệp vụ chung** – Authentication, logging, error handling.

🔹 **Tối ưu hiệu năng** – Lazy loading, chia sẻ thư viện chung.

🔹 **Kiểm soát dependencies** – Tránh xung đột phiên bản.

🔹 **Test end-to-end** – Đảm bảo tích hợp mượt mà giữa các micro-frontend.