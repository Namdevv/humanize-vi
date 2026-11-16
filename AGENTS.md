# Hướng dẫn cho agent

File này nói cách sửa Humanizer tiếng Việt mà không làm hỏng gói hay prompt.

## Repo này có gì

Humanizer tiếng Việt là một agent skill viết bằng Markdown, chuyển thể từ [Humanizer](https://github.com/blader/humanizer). `SKILL.md` là prompt mà agent đọc. Repo không có bước build.

Giữ skill dùng được ở mọi nơi. Không viết chỉ dẫn chỉ chạy được trên một hai công cụ agent.

## Các file chính

- `SKILL.md` là nguồn gốc và là file skill duy nhất của repo. Nó có metadata YAML, phần giải thích vì sao văn AI nghe như vậy, và các mẫu đánh số chia trong bảy nhóm, xếp theo độ mạnh và độ phổ biến.
- `README.md` giải thích cách cài, cách dùng và các mẫu.
- `CHANGELOG.md` ghi chú phát hành, mới nhất ở trên. Ghi chú cũ giữ số mẫu của bản phát hành đó.
- `.claude-plugin/plugin.json` mô tả Claude plugin và trỏ bộ nạp skill vào `SKILL.md` ở gốc repo.
- `.claude-plugin/marketplace.json` cho phép thêm repo này làm Claude marketplace.
- `.cursor-plugin/plugin.json` mô tả Cursor plugin. Không ghi đường dẫn `skills` để Cursor nạp `SKILL.md` ở gốc.
- `package.json` biến repo thành gói pi. Mục `pi.skills` trỏ vào gốc repo. Không thêm trường `version`; pi cài từ git.
- `agents/openai.yaml` chứa tên hiển thị, mô tả ngắn và prompt mặc định cho agent tương thích OpenAI.
- `scripts/validate-package.py` kiểm tra các file của gói và các giá trị dùng chung.

## Quy tắc khi sửa

Giữ `SKILL.md` và `README.md` khớp nhau.

- **Mẫu:** Đánh số mẫu từ 1, không bỏ số, mạnh và phổ biến nhất trước. Chỉ thêm mẫu mới khi chưa mẫu nào bao hàm nó; ưu tiên gộp vào mẫu có sẵn. Khi thêm, bỏ hay đánh số lại, phải sửa bảng trong README, tiêu đề mục mẫu trong README (`## 26 mẫu`) và mọi tham chiếu §. Validator đếm mẫu theo tiêu đề và kiểm tra tên mẫu trong README khớp `SKILL.md`.
- **Phiên bản:** Dùng cùng một phiên bản ở `metadata.version` trong `SKILL.md`, tiêu đề đầu tiên của `CHANGELOG.md`, `.claude-plugin/plugin.json` và `.cursor-plugin/plugin.json`. Không thêm trường `version` ở cấp cao nhất của skill.
- **Tương thích:** Giữ hướng dẫn cài và dùng trung lập giữa các agent. Claude Code, Cursor, OpenCode và Codex chỉ là ví dụ.
- **Mô tả:** Các manifest plugin dùng câu đầu tiên trong mô tả của `SKILL.md`.
- **Độ dài:** Agent đọc mọi chữ trong `SKILL.md` mỗi lần dùng. Validator giới hạn 6.500 từ (tính theo dấu cách, nên mỗi âm tiết tiếng Việt là một từ). Thay đổi nào thêm chữ phải đáng số chữ đó.
- **Lịch sử:** Thêm một ghi chú ngắn vào `CHANGELOG.md` cho mọi thay đổi hành vi hay bản sửa không hiển nhiên.
- **Kiểm tra:** Trước khi phát hành, chạy `python3 scripts/validate-package.py`, `npx skills add . --list` và `claude plugin validate .`.

## Cách viết

Viết giản dị trong comment, prompt, tài liệu, mô tả, thông báo lỗi và báo cáo tiến độ. Chính skill này là danh sách những điều cần tránh.

- Nói ý chính trước.
- Dùng từ thường và câu chủ động.
- Câu và đoạn ngắn.
- Một thứ chỉ dùng một tên gọi.
- Dùng `phải` cho yêu cầu bắt buộc.
- Dùng tiêu đề, danh sách và bảng khi chúng giúp người đọc.
- Bỏ chữ lặp và chữ thừa.
- Hạn chế viết tắt và giải thích thuật ngữ.
- Tránh phủ định kép.
- Giữ nguyên định danh, lệnh, đường dẫn, trường schema, trích dẫn, cụm từ bị theo dõi và ví dụ mang hành vi.
- Giữ đủ nghĩa kỹ thuật.

## Sửa skill

- Giữ metadata YAML hợp lệ.
- Coi phần prompt bên dưới metadata là sản phẩm.
- Ưu tiên một chỉ dẫn ngắn, rõ hơn là thêm ngoại lệ hay giải thích lặp lại.
- Văn bản phải là tiếng Việt tự nhiên. Ví dụ Trước/Sau phải là văn tiếng Việt thật, không dịch từng chữ từ bản tiếng Anh.
