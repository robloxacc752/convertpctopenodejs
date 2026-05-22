# Minecraft Resource Pack Converter (Node.js) – Java → Bedrock

**Công cụ chuyển đổi Resource Pack từ Java sang Bedrock, hỗ trợ ItemsAdder (contents + storage), GeyserMC Mappings V3/V4, viết bằng Node.js.**

[![Node.js](https://img.shields.io/badge/Node.js-18%2B-green)](https://nodejs.org)
[![License](https://img.shields.io/badge/License-MIT-blue)](LICENSE)

> **Phiên bản:** & Node.js 1.0.0  
> **Cập nhật:** Tháng 5, 2026  
> **Tác giả:** HoangNam

---

## 📌 MỤC LỤC

1. [Giới thiệu chung](#-giới-thiệu-chung)
2. [Cài đặt và yêu cầu](#-cài-đặt-và-yêu-cầu)
3. [Cấu trúc ItemsAdder (contents + storage)](#-cấu-trúc-itemsadder-contents--storage)
4. [Hướng dẫn sử dụng – Python](#-hướng-dẫn-sử-dụng--python)
5. [Hướng dẫn sử dụng – Node.js](#-hướng-dẫn-sử-dụng--nodejs)
6. [Các tùy chọn nâng cao](#-các-tùy-chọn-nâng-cao)
7. [Kiểm tra và sửa lỗi pack tự động](#-kiểm-tra-và-sửa-lỗi-pack-tự-động)
8. [Triển khai lên GeyserMC](#-triển-khai-lên-geysermc)
9. [Xử lý sự cố](#-xử-lý-sự-cố)
10. [Giấy phép](#-giấy-phép)

---

## 🎯 GIỚI THIỆU CHUNG

Công cụ chuyển đổi **Java Resource Pack → Bedrock Edition** dành cho Minecraft, tối ưu cho plugin **GeyserMC** (cầu nối Java-Bedrock). Hỗ trợ:

- ✅ Items & Custom Model Data (CMD) – kể cả cấu trúc mới 1.21.4+
- ✅ Armor 2D/3D – tạo attachables Bedrock
- ✅ Blocks – blocks.json + terrain_texture.json (kể cả mô hình 3D)
- ✅ Fonts & Emojis – tự động quét glyph (Pillow/sharp)
- ✅ Sounds – chuyển đổi sounds.json → sound_definitions.json
- ✅ Cosmetics – mũ, wings, particle từ ItemsAdder
- ✅ Animated textures – flipbook từ .mcmeta

**Hai phiên bản:** Python (đầy đủ tính năng) và Node.js (nhẹ, dễ chạy trên server).

---

## ⚙️ CÀI ĐẶT VÀ YÊU CẦU

### Phiên bản Python

- **Python** 3.8+ (tải tại python.org)
- Cài thư viện:
  ```bash
  pip install pyyaml pillow
Kiểm tra: python -c "import yaml, PIL; print('OK')"

Phiên bản Node.js
Node.js 18+ (tải tại nodejs.org)

Tạo thư mục dự án, cài dependencies:

bash
npm init -y
npm install fs-extra js-yaml sharp
Lưu script converter.js vào thư mục.

🧩 CẤU TRÚC ITEMSADDER (CONTENTS + STORAGE)
Để converter nhận diện và merge đúng, bạn cần có thư mục contents và (tùy chọn) storage từ ItemsAdder. Cấu trúc chuẩn (theo wiki ItemsAdder):

text
/đường/dẫn/contents/
└── ten_namespace/               # ví dụ: "my_items"
    ├── configs/                 # file .yml định nghĩa item, block, font, cosmetic
    │   ├── items.yml
    │   ├── blocks.yml
    │   └── fonts.yml
    └── resourcepack/
        └── assets/
            └── ten_namespace/   # cùng tên namespace
                ├── textures/
                │   ├── items/
                │   ├── blocks/
                │   └── font/
                └── models/
Ví dụ items.yml:

yaml
items:
  my_sword:
    display_name: "Kiếm Thần"
    texture: "my_sword"
    lore: ["Sức mạnh vô địch"]
Ví dụ fonts.yml:

yaml
font_images:
  icon_heart:
    char: "❤"
    texture: "icon_heart.png"
    width: 8
    height: 8
Thư mục storage (không bắt buộc) thường chứa các file ảnh font bổ sung, script sẽ copy vào textures/font.

🐍 HƯỚNG DẪN SỬ DỤNG – PYTHON
Chạy tương tác (dễ nhất)
bash
python converter.py
Sau đó nhập:

Đường dẫn tuyệt đối đến Java Resource Pack

Đường dẫn output (mặc định ./bedrock_packs_v2)

Tên pack (không dấu cách)

Đường dẫn đến contents của ItemsAdder (có thể để trống nếu không dùng)

Đường dẫn đến storage (để trống nếu không)

Hệ thống sẽ kiểm tra tính hợp lệ của contents và yêu cầu nhập lại nếu sai cấu trúc.

Chạy với dòng lệnh
bash
python converter.py "C:\Users\Admin\Desktop\MyPack" "./output" "my_pack"
Kết quả
Thư mục Bedrock pack được tạo tại ./bedrock_packs_v2/my_pack/ với cấu trúc:

text
my_pack/
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
├── font/
└── CONVERSION_REPORT.txt
🟢 HƯỚNG DẪN SỬ DỤNG – NODE.JS
Chạy tương tác
bash
node converter.js
Các bước nhập tương tự như Python.

Chạy dòng lệnh
bash
node converter.js "D:\JavaPacks\MyPack" "./bedrock_output" "my_pack"
Lưu ý
Phiên bản Node.js nhẹ hơn, không hỗ trợ một số tính năng phức tạp như 3D model parsing, nhưng đủ dùng cho hầu hết resource pack thông thường.

Dùng sharp thay cho Pillow để xử lý ảnh.

🎛️ CÁC TÙY CHỌN NÂNG CAO
Khi chạy, bạn được chọn:

Tùy chọn	Mô tả
GeyserMC Mappings V3	Ổn định, phù hợp hầu hết server.
GeyserMC Mappings V4	Thử nghiệm, hỗ trợ components NBT, animation nâng cao.
Animation frames	Chuyển texture động từ .png.mcmeta sang flipbook_textures.json (Bedrock).
Cosmetics	Chuyển mũ, wings, particle từ ItemsAdder sang attachables Bedrock.
Log chi tiết	Hiển thị từng file được xử lý (hữu ích để debug).
🔍 KIỂM TRA VÀ SỬA LỖI PACK TỰ ĐỘNG
Sau khi merge ItemsAdder, script chạy PackValidator để:

Tạo thư mục assets/minecraft nếu thiếu.

Tạo file pack.mcmeta mặc định (pack_format: 15 cho Java 1.21).

Sửa đường dẫn texture trong các file .json – tự động thêm .png nếu thiếu.

Xóa các thư mục rỗng.

Phát hiện JSON lỗi cú pháp → tạo file .bak và báo cáo.

Tất cả các thay đổi được ghi lại trong log.

🚀 TRIỂN KHAI LÊN GEYSERMC
Copy toàn bộ thư mục Bedrock pack vào:

text
plugins/Geyser-Spigot/packs/
Reload Geyser bằng lệnh:

text
/geyser reload
Kết nối từ Bedrock client – pack sẽ tự động tải về.

Kiểm tra log Geyser để đảm bảo không có lỗi:

text
[Geyser] Loading custom resource pack: my_converted_pack
[Geyser] Successfully loaded 45 custom mappings.
Nếu pack không load, hãy xóa thư mục cũ trong packs/ và copy lại.

🐛 XỬ LÝ SỰ CỐ
Lỗi	Nguyên nhân	Cách khắc phục
KeyError: 'items' (Python)	Phiên bản script cũ	Tải phiên bản 2.3.0 trở lên.
pip không được nhận diện	Python chưa trong PATH	Dùng python -m pip install ... hoặc cài lại Python (chọn Add to PATH).
Error: Cannot find module 'fs-extra' (Node.js)	Chưa cài dependencies	Chạy npm install trong thư mục dự án.
Không tìm thấy contents hợp lệ	Đường dẫn sai hoặc thiếu resourcepack/assets	Nhập lại đường dẫn tuyệt đối, kiểm tra thư mục con.
Pack không load trong Geyser	manifest.json lỗi hoặc thiếu UUID	Xóa pack cũ, chạy lại converter. Nếu vẫn lỗi, kiểm tra log Geyser.
Font bị lệch, icon không hiển thị	Thiếu config glyph width	Cài Pillow (Python) hoặc sharp (Node.js) để tự động quét glyph; hoặc sửa thủ công file font/*.json.
Lỗi YAML khi parse ItemsAdder	File .yml sai cú pháp	Kiểm tra thủ công file bằng editor hỗ trợ YAML.
📄 GIẤY PHÉP
MIT License. Xem file LICENSE để biết thêm chi tiết.

🙏 CẢM ƠN
GeyserMC – Cầu nối hoàn hảo.

ItemsAdder – Plugin tạo nội dung tùy chỉnh.

Cộng đồng Minecraft Việt Nam.

Chúc bạn thành công! 🎮✨
