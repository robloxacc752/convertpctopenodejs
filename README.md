# Minecraft Resource Pack Converter (Node.js) – Java → Bedrock

**Công cụ chuyển đổi Resource Pack từ Java sang Bedrock, hỗ trợ ItemsAdder (contents + storage), GeyserMC Mappings V3/V4, viết bằng Node.js.**

[![Node.js](https://img.shields.io/badge/Node.js-18%2B-green)](https://nodejs.org)
[![License](https://img.shields.io/badge/License-MIT-blue)](LICENSE)

---

## 🎯 Tính năng

- **Chuyển đổi toàn diện** – Items (CMD), Armor, Blocks, Fonts, Cosmetics, Sounds.
- **Tích hợp ItemsAdder** – Tự động merge `contents` + `storage`, parse YAML, tạo mapping.
- **GeyserMC Ready** – Xuất `custom_mappings.json` phiên bản V3 hoặc V4.
- **Xử lý thông minh** – Animation frames (flipbook), kiểm tra & sửa lỗi pack tự động.
- **Log đẹp mắt** – Tiếng Việt, thanh tiến trình, báo cáo chi tiết.

---

## 📦 Yêu cầu

- **Node.js** 18 trở lên (tải tại [nodejs.org](https://nodejs.org))
- Các gói npm: `fs-extra`, `js-yaml`, `sharp`

---

## 🚀 Cài đặt & Sử dụng

### 1. Tải dự án và cài dependencies ###
```bash
git clone https://github.com/your-repo/minecraft-converter-js.git
cd minecraft-converter-js
npm install
2. Chạy tương tác
bash
node converter.js
Nhập các thông tin theo hướng dẫn:

Đường dẫn tuyệt đối đến Java Resource Pack

Đường dẫn thư mục output (mặc định ./bedrock_packs_v2)

Tên pack (không dấu cách)

Đường dẫn đến contents của ItemsAdder – bắt buộc nếu có (kiểm tra cấu trúc hợp lệ)

Đường dẫn đến storage – tùy chọn

3. Chạy với dòng lệnh
bash
node converter.js "D:\JavaPacks\MyPack" "./output" "my_bedrock_pack"
4. Kết quả
Thư mục Bedrock pack được tạo với cấu trúc:

text
my_bedrock_pack/
├── manifest.json
├── custom_mappings.json
├── blocks.json
├── sound_definitions.json
├── attachables/
├── textures/
│   ├── items/
│   ├── blocks/
│   └── font/
├── sounds/
└── CONVERSION_REPORT.txt
🛠️ Cấu hình nâng cao
Khi chạy, bạn chọn:

GeyserMC Mappings – V3 (ổn định) hoặc V4 (hỗ trợ components, NBT).

Animation frames – Bật/tắt xử lý ảnh động từ .png.mcmeta.

Cosmetics – Chuyển mũ, wings, particle từ ItemsAdder sang attachables Bedrock.

Log chi tiết – Hiển thị từng file được xử lý.

🧩 Tích hợp ItemsAdder
Để converter merge đúng cấu trúc contents của ItemsAdder, thư mục của bạn phải theo mẫu:

text
plugins/ItemsAdder/contents/
└── my_namespace/
    ├── configs/               # file .yml (items, armor, blocks, fonts, cosmetics)
    │   ├── items.yml
    │   └── fonts.yml
    └── resourcepack/
        └── assets/
            └── my_namespace/  # cùng tên namespace
                ├── textures/
                ├── models/
                └── sounds/
Script sẽ:

Merge resourcepack/assets vào pack Java tạm thời.

Parse các file YAML trong configs/ để lấy metadata.

Copy nội dung storage (nếu có) vào textures/font.

✅ Kiểm tra & sửa lỗi pack
Sau khi merge, PackValidator tự động:

Tạo thư mục assets/minecraft nếu thiếu.

Tạo pack.mcmeta với pack_format: 15 (Java 1.21).

Sửa đường dẫn texture trong các file .json (thêm .png).

Xóa thư mục rỗng.

Nếu phát hiện lỗi không tự sửa được (ví dụ JSON sai cú pháp), script sẽ báo cáo và tạo file .bak.

🚀 Deploy lên GeyserMC
Copy toàn bộ thư mục Bedrock pack vào:

text
plugins/Geyser-Spigot/packs/
Reload Geyser:

text
/geyser reload
Kết nối từ Bedrock client – pack sẽ tự động tải.

🐛 Xử lý sự cố
Vấn đề	Nguyên nhân	Khắc phục
Error: Cannot find module 'fs-extra'	Chưa cài dependencies	Chạy npm install
Lỗi YAML: ...	File config ItemsAdder sai cú pháp	Kiểm tra thủ công file .yml
Không tìm thấy contents hợp lệ	Đường dẫn sai hoặc thiếu resourcepack/assets	Nhập lại đường dẫn tuyệt đối
Pack không load trong Geyser	manifest.json lỗi hoặc thiếu UUID	Xóa pack cũ, chạy lại converter
Font bị lệch	Thiếu config glyph width	Cài sharp để tự động quét, hoặc sửa thủ công file font/*.json
📄 Giấy phép
MIT License – được phép sử dụng, sửa đổi, phân phối.

Phiên bản Node.js: 1.0.0
Phiên bản Python: 2.3.0 (tham khảo)
Cập nhật: Tháng 5, 2026
