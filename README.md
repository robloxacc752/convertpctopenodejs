# 🔄 Minecraft Resource Pack Converter – Java → Bedrock

Công cụ chuyển đổi Resource Pack từ Java sang Bedrock, hỗ trợ ItemsAdder (contents + storage), GeyserMC Mappings V3/V4, hỗ trợ cả Python và Node.js.

Phiên bản: Node.js 1.0.0  
Cập nhật: Tháng 5, 2026  
Tác giả: HoangNam

--------------------------------------------------------------------------------

## 📌 MỤC LỤC

1. Giới thiệu chung
2. Cài đặt và yêu cầu
3. Cấu trúc ItemsAdder (contents + storage)
4. Hướng dẫn sử dụng – Python
5. Hướng dẫn sử dụng – Node.js
6. Các tùy chọn nâng cao
7. Kiểm tra và sửa lỗi tự động
8. Triển khai lên GeyserMC
9. Xử lý sự cố
10. Giấy phép & Lời cảm ơn

--------------------------------------------------------------------------------

## 🎯 1. GIỚI THIỆU CHUNG

Công cụ chuyển đổi Java Resource Pack → Bedrock Edition dành cho Minecraft, được thiết kế tối ưu cho plugin GeyserMC (cầu nối Java-Bedrock). Hệ thống hỗ trợ toàn diện:

- Items & Custom Model Data (CMD): Tương thích cả cấu trúc mới 1.21.4+.
- Armor 2D/3D: Tự động tạo attachables cho Bedrock.
- Blocks: Đăng ký blocks.json + terrain_texture.json (kể cả mô hình khối 3D).
- Fonts & Emojis: Tự động quét và căn chỉnh glyph thông minh (dùng Pillow/sharp).
- Sounds: Chuyển đổi chuẩn xác sounds.json → sound_definitions.json.
- Cosmetics: Hỗ trợ mũ, wings, particle từ ItemsAdder.
- Animated Textures: Đọc .mcmeta để tạo flipbook animations.

Cung cấp 2 phiên bản: Python (đầy đủ tính năng, mạnh mẽ) và Node.js (nhẹ nhàng, dễ tích hợp trên server).

--------------------------------------------------------------------------------

## ⚙️ 2. CÀI ĐẶT VÀ YÊU CẦU

[ Phiên bản Python ]
- Yêu cầu: Python 3.8+ (tải tại python.org)
- Cài đặt thư viện cần thiết qua CMD/Terminal:
  pip install pyyaml pillow
- Kiểm tra cài đặt:
  python -c "import yaml, PIL; print('OK')"

[ Phiên bản Node.js ]
- Yêu cầu: Node.js 18+ (tải tại nodejs.org)
- Thiết lập thư mục dự án và cài dependencies:
  npm init -y
  npm install fs-extra js-yaml sharp
(Lưu script converter.js vào thư mục dự án sau khi cài đặt).

--------------------------------------------------------------------------------

## 🧩 3. CẤU TRÚC ITEMSADDER (CONTENTS + STORAGE)

Để converter nhận diện và gộp dữ liệu (merge) chính xác, bạn cần trỏ đến thư mục 'contents' và 'storage' (tùy chọn) từ plugin ItemsAdder. 

Cấu trúc chuẩn (theo wiki ItemsAdder):

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

--------------------------------------------------------------------------------

## 🐍 4. HƯỚNG DẪN SỬ DỤNG – PYTHON

[ Chạy chế độ tương tác (Khuyên dùng) ]
Gõ lệnh: python converter.py
Hệ thống sẽ lần lượt yêu cầu bạn nhập:
1. Đường dẫn tuyệt đối đến Java Resource Pack.
2. Đường dẫn output (mặc định: ./bedrock_packs_v2).
3. Tên pack (viết liền không dấu cách).
4. Đường dẫn đến 'contents' của ItemsAdder (để trống nếu không dùng).
5. Đường dẫn đến 'storage' của ItemsAdder (để trống nếu không dùng).

[ Chạy bằng dòng lệnh (CLI) ]
Gõ lệnh: python converter.py "C:\Users\Admin\Desktop\MyPack" "./output" "my_pack"

--------------------------------------------------------------------------------

## 🟢 5. HƯỚNG DẪN SỬ DỤNG – NODE.JS

[ Chạy chế độ tương tác ]
Gõ lệnh: node converter.js
(Các bước nhập thông tin tương tự như bản Python)

[ Chạy bằng dòng lệnh (CLI) ]
Gõ lệnh: node converter.js "D:\JavaPacks\MyPack" "./bedrock_output" "my_pack"

Lưu ý: Phiên bản Node.js được thiết kế để chạy nhẹ nhàng trên máy chủ. Nó sử dụng thư viện sharp để xử lý ảnh, tuy nhiên có thể sẽ lược bỏ một số tính năng parsing 3D phức tạp so với bản Python.

--------------------------------------------------------------------------------

## 🎛️ 6. CÁC TÙY CHỌN NÂNG CAO

Trong quá trình chạy, bạn sẽ được cung cấp các tùy chọn sau:

- GeyserMC Mappings V3: Chế độ ổn định nhất, tương thích tốt với hầu hết các máy chủ hiện nay.
- GeyserMC Mappings V4: Chế độ thử nghiệm, hỗ trợ tính năng đọc components NBT và animation nâng cao.
- Animation frames: Chuyển đổi các texture động từ tệp .png.mcmeta của Java sang flipbook_textures.json của Bedrock.
- Cosmetics: Tự động quét và chuyển đổi mũ, wings, particle từ cấu trúc ItemsAdder sang attachables Bedrock.
- Log chi tiết: Hiển thị chi tiết từng tệp được xử lý trên console (Rất hữu ích khi cần debug).

--------------------------------------------------------------------------------

## 🔍 7. KIỂM TRA VÀ SỬA LỖI TỰ ĐỘNG

Sau khi tiến hành gộp dữ liệu ItemsAdder, hệ thống sẽ chạy module PackValidator để làm sạch và sửa lỗi:
- Tự động tạo thư mục assets/minecraft nếu bị thiếu.
- Tạo tệp pack.mcmeta mặc định (vd: pack_format: 15 cho Java 1.21).
- Quét và sửa lỗi đường dẫn texture trong các file JSON (tự động điền đuôi .png).
- Dọn dẹp, xóa bỏ các thư mục rỗng.
- Cảnh báo các file JSON sai cú pháp, tự động tạo tệp backup .bak để tránh crash hệ thống.

--------------------------------------------------------------------------------

## 🚀 8. TRIỂN KHAI LÊN GEYSERMC

1. Sao chép toàn bộ thư mục Bedrock pack vừa được tạo vào thư mục plugin của Geyser:
   plugins/Geyser-Spigot/packs/
   
2. Tải lại cấu hình Geyser trong game hoặc console:
   /geyser reload
   
3. Kết nối vào máy chủ bằng Bedrock Client – Resource Pack sẽ tự động tải xuống.

4. Kiểm tra Console để xác nhận mappings đã nhận:
   [Geyser] Loading custom resource pack: my_converted_pack
   [Geyser] Successfully loaded 45 custom mappings.

(Mẹo: Nếu pack không hoạt động, hãy xóa hẳn thư mục pack cũ trong thư mục packs/ rồi dán thư mục mới vào).

--------------------------------------------------------------------------------

## 🐛 9. XỬ LÝ SỰ CỐ

[ Lỗi: KeyError: 'items' (Python) ]
- Nguyên nhân: Đang sử dụng script phiên bản quá cũ.
- Khắc phục: Vui lòng tải bản Converter từ 2.3.0 trở lên.

[ Lỗi: pip không được nhận diện ]
- Nguyên nhân: Python chưa được thêm vào biến môi trường (PATH).
- Khắc phục: Chạy bằng python -m pip install... hoặc cài đặt lại Python và nhớ tick ô "Add to PATH".

[ Lỗi: Error: Cannot find module 'fs-extra' (Node.js) ]
- Nguyên nhân: Chưa cài đặt gói thư viện phụ thuộc.
- Khắc phục: Chạy lệnh npm install ngay trong thư mục dự án.

[ Lỗi: Không tìm thấy contents hợp lệ ]
- Nguyên nhân: Khai báo sai đường dẫn hoặc thiếu thư mục resourcepack/assets.
- Khắc phục: Cung cấp lại đường dẫn tuyệt đối, kiểm tra kỹ lại cây thư mục ItemsAdder.

[ Lỗi: Font bị đè, lệch, mất icon ]
- Nguyên nhân: Chưa xác định được độ rộng (glyph width) của ký tự.
- Khắc phục: Cài đặt Pillow (đối với Python) hoặc sharp (Node.js) để script tự quét. Hoặc phải chỉnh sửa thông số trong font/*.json bằng tay.

--------------------------------------------------------------------------------

## 📄 10. GIẤY PHÉP & LỜI CẢM ƠN

Dự án được phân phối dưới giấy phép MIT License.

Lời cảm ơn:
- GeyserMC – Cầu nối hoàn hảo mang Bedrock và Java lại gần nhau.
- ItemsAdder – Framework tùy chỉnh nội dung xuất sắc.
- Cộng đồng Minecraft Việt Nam đã đóng góp ý tưởng.

Chúc bạn chuyển đổi thành công! 🎮✨
