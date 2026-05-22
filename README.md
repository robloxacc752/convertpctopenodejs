# Minecraft Resource Pack Converter (Node.js)

> Chuyển đổi Java Resource Pack → Bedrock Edition, tối ưu cho GeyserMC, hỗ trợ ItemsAdder (contents + storage)

![Node.js](https://img.shields.io/badge/Node.js-18%2B-brightgreen)
![License](https://img.shields.io/badge/License-MIT-blue)

---

## 🚀 Tính năng

- ✅ Chuyển đổi Items, Armor, Blocks, Fonts, Sounds, Cosmetics
- ✅ Tích hợp ItemsAdder – tự động merge `contents` & `storage`
- ✅ GeyserMC Mappings V3 / V4
- ✅ Xử lý animation (flipbook), kiểm tra & sửa lỗi pack
- ✅ Giao diện dòng lệnh (CLI) đẹp, log chi tiết

---

## 📦 Yêu cầu

- **Node.js** 18 trở lên (tải tại [nodejs.org](https://nodejs.org))

---

## 🔧 Cài đặt

```bash
# Clone dự án (hoặc tạo thư mục mới)
git clone https://github.com/your-repo/mc-converter-nodejs.git
cd mc-converter-nodejs

# Cài dependencies
npm install fs-extra js-yaml sharp
```

---

## 🎮 Cách sử dụng

### Chế độ tương tác (khuyến nghị)

```bash
node converter.js
```

Sau đó nhập:

1. Đường dẫn tuyệt đối đến Java Resource Pack (thư mục chứa `assets/` và `pack.mcmeta`)
2. Đường dẫn thư mục output (mặc định `./bedrock_packs_v2`)
3. Tên pack (không dấu cách)
4. Đường dẫn đến `contents` của ItemsAdder (bắt buộc nếu có, để trống nếu không)
5. Đường dẫn đến `storage` (tùy chọn)

### Chế độ dòng lệnh (batch)

```bash
node converter.js "C:\JavaPacks\MyPack" "./output" "my_bedrock_pack"
```

---

## 🧩 Tích hợp ItemsAdder

Để converter nhận diện đúng, thư mục `contents` phải có cấu trúc chuẩn:

```
/đường/dẫn/contents/
└── ten_namespace/
    ├── configs/           # file .yml (items, armor, blocks, fonts, cosmetics)
    └── resourcepack/
        └── assets/
            └── ten_namespace/   # textures, models, sounds
```

Script sẽ tự động:

- Merge `resourcepack/assets` vào Java pack tạm
- Parse file YAML để lấy mapping
- Copy `storage` (nếu có) vào thư mục font

---

## ⚙️ Các tùy chọn nâng cao

Khi chạy, bạn có thể chọn:

| Tùy chọn | Mô tả |
|---|---|
| GeyserMC Mappings V3 | Ổn định, dùng cho hầu hết server |
| GeyserMC Mappings V4 | Thử nghiệm, hỗ trợ components, NBT, animation phức tạp |
| Animation frames | Bật/tắt xử lý ảnh động từ `.png.mcmeta` |
| Cosmetics | Chuyển đổi mũ, wings, particle từ ItemsAdder |
| Log chi tiết | Hiển thị thông tin xử lý từng file |

---

## ✅ Kiểm tra & sửa lỗi pack tự động

Sau khi merge ItemsAdder, script tự động:

- Tạo `assets/minecraft` nếu thiếu
- Tạo `pack.mcmeta` mặc định (`pack_format: 15`)
- Sửa đường dẫn texture trong các file `.json` (thêm `.png`)
- Xóa thư mục rỗng
- Phát hiện JSON lỗi cú pháp → tạo file `.bak`

---

## 📤 Triển khai lên GeyserMC

Copy toàn bộ thư mục Bedrock pack vào:

```
plugins/Geyser-Spigot/packs/
```

Reload Geyser:

```
/geyser reload
```

Kết nối từ Bedrock client → pack tự động tải.

---

## 🐛 Xử lý lỗi thường gặp

| Lỗi | Nguyên nhân | Cách khắc phục |
|---|---|---|
| `npm not recognized` | Chưa cài Node.js | Tải và cài Node.js từ [nodejs.org](https://nodejs.org) (chọn "Add to PATH") |
| `Cannot find module 'fs-extra'` | Chưa cài dependencies | Chạy `npm install` trong thư mục dự án |
| Không tìm thấy contents hợp lệ | Đường dẫn sai hoặc thiếu `resourcepack/assets` | Nhập lại đường dẫn tuyệt đối, kiểm tra cấu trúc |
| Pack không load trong Geyser | `manifest.json` lỗi | Xóa pack cũ, chạy lại converter |
| Font bị lệch | Thiếu config glyph width | Cài `sharp` để tự động quét hoặc sửa thủ công file `font/*.json` |

---

## 📄 Giấy phép

MIT License. Xem file [LICENSE](./LICENSE).

---

*Phiên bản: 1.0.0 — Cập nhật: Tháng 5, 2026*
