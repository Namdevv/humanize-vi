# Humanizer tiếng Việt

Humanizer tiếng Việt sửa văn bản tiếng Việt do AI viết để nó đọc như người viết, mà không đổi nội dung. Skill chuyển thể từ [Humanizer](https://github.com/blader/humanizer) của blader, vốn dựa trên trang [Signs of AI writing](https://en.wikipedia.org/wiki/Wikipedia:Signs_of_AI_writing) mà biên tập viên Wikipedia dùng để nhận ra văn AI. Bản này thêm các dấu hiệu riêng của văn AI tiếng Việt, như văn dịch, "một cách" và từ hành chính. Skill chạy trong Claude Code, Codex, Cursor và mọi agent hỗ trợ skill.

**Trước:**
> Chúng tôi vô cùng hào hứng thông báo tính năng soạn thảo chung đã chính thức ra mắt! 🚀 Suốt nhiều tháng, chính đội ngũ của chúng tôi cũng "chìm" trong những file tên bao_cao_final_v7.docx — và chúng tôi biết phải có cách tốt hơn. Giờ đây, hai người có thể chỉnh sửa cùng một tài liệu một cách đồng thời, với mọi thay đổi được hiển thị theo thời gian thực. Đây không chỉ là một tính năng, mà là cả một cách cộng tác hoàn toàn mới. Và điều tuyệt vời nhất? Tính năng có sẵn ngay hôm nay cho mọi gói, hoàn toàn miễn phí.

**Sau:**
> Soạn thảo chung ra mắt hôm nay. Nhiều tháng liền, chính đội chúng tôi cũng gửi qua gửi lại những file tên bao_cao_final_v7.docx và phát ngán. Giờ hai người sửa cùng một tài liệu được, ai gõ gì người kia thấy ngay. Mọi gói đều dùng được, miễn phí.

Skill sửa văn cho người đọc. Nó không nhằm vượt qua công cụ dò AI, và các công cụ đó vẫn có thể đánh dấu kết quả.

## Năm dấu hiệu mạnh nhất

Đây là những dấu hiệu văn AI hay gặp nhất. Skill sửa ngay khi gặp một lần:

1. **Không phải X mà là Y:** "Đây không chỉ là một tính năng, mà là cả một cuộc cách mạng."
2. **Câu chốt một dòng:** "Đó mới là điều quan trọng." "Tóm lại, ..."
3. **Câu nghe sâu sắc:** "Suy cho cùng, chìa khóa nằm ở..."
4. **Dẫn dắt trước khi vào ý:** "Trong thời đại công nghệ 4.0 ngày nay..." "Hãy cùng tìm hiểu!"
5. **Cãi với người không có mặt:** "Tôi không nói rằng X, nhưng..."

Skill kiểm tra [26 mẫu](#26-mẫu) tất cả.

## Cài đặt

Cài xong, gọi skill bằng `/humanizer-vi`.

### Claude Code

```text
/plugin marketplace add Namdevv/humanize-vi
/plugin install humanizer-vi@humanizer-vi
```

Plugin cần Claude Code 2.1.142 trở lên. Với bản cũ hơn, dùng `npx skills add Namdevv/humanize-vi --global --agent claude-code`.

### Codex

```bash
npx skills add Namdevv/humanize-vi --global --agent codex
```

### pi

```bash
pi install git:github.com/Namdevv/humanize-vi
```

Thêm `-l` để chỉ cài cho project hiện tại. `pi update --extensions` kéo bản mới.

### Claude.ai và Claude Desktop

Tải repo này dạng ZIP (**Code → Download ZIP**) rồi tải lên làm skill trong Settings.

### Agent khác

```bash
npx skills add Namdevv/humanize-vi --global --agent '*'
```

Lệnh này cài skill cho mọi agent mà Skills CLI hỗ trợ, gồm Gemini CLI, GitHub Copilot và Windsurf. Bỏ `--global` ở bất kỳ lệnh nào phía trên để chỉ cài cho project hiện tại. Với agent mà Skills CLI chưa biết, chép `SKILL.md` vào thư mục skill của agent đó.

## Cách dùng

Gọi skill trực tiếp:

```
/humanizer-vi

[dán văn bản vào đây]
```

Hoặc nhờ bằng lời thường:

```
Sửa đoạn này cho bớt giọng AI: [văn bản]
```

Để sửa một file, đưa đường dẫn:

```
Sửa phần văn xuôi trong docs/bai-viet.md cho bớt giọng AI
```

Skill đọc hết file hiện tại trước khi sửa, kể cả file nó vừa viết trong cùng phiên. Bản tóm tắt cuối nói đã rà bao nhiêu phần và phần nào chưa rà được.

### Theo giọng của bạn

Muốn bản viết lại giống giọng bạn hơn, gửi kèm một đoạn mẫu:

```
/humanizer-vi

Đây là mẫu văn của tôi để theo giọng:
[dán 2-3 đoạn bạn tự viết]

Giờ sửa đoạn này:
[dán văn bản AI]
```

Skill theo nhịp câu, cách chọn từ, dấu câu và thói quen riêng trong mẫu, kể cả gạch ngang nếu bạn hay dùng. Skill cũng giữ cách xưng hô của văn bản gốc (tôi, mình, em, chúng tôi) và giữ thuật ngữ tiếng Anh quen dùng trong văn kỹ thuật (deploy, cache, API).

## Cách hoạt động

Mô hình ngôn ngữ viết ra thứ dễ xuất hiện tiếp theo nhất, nên mặc định nó chọn cách viết hợp với nhiều người đọc và nhiều chủ đề nhất. Người viết thật chọn cho một người đọc và một chủ đề. Mỗi dấu hiệu skill tìm là một dạng của lựa chọn mặc định đó: câu báo hiệu điều quan trọng thay vì đưa thêm thông tin, nhịp câu và định dạng theo khuôn, cấu trúc tiếng Anh khoác chữ tiếng Việt, sự việc thường bị tô thành bước ngoặt, đồ thừa từ cuộc trò chuyện, hay câu trả lời giải thích lại điều người đọc đã biết.

Skill đánh dấu mọi dấu hiệu tìm thấy, mạnh trước. Nó viết nháp, có thể gộp, tách hay đổi thứ tự câu và đoạn, rồi đối chiếu bản nháp với các mẫu và với các nhận định gốc trước khi viết bản cuối. Chỗ nào chung chung, nó thêm chi tiết cụ thể mà nó biết chắc là đúng, và giữ mọi nhận định của bản gốc.

Khi bạn dán văn bản, skill trả về bản nháp, danh sách ngắn những chỗ còn nghe như AI, và bản cuối. Khi sửa file, nó chỉ sửa văn xuôi, giữ nguyên code, dữ liệu, frontmatter và đích liên kết. Văn cá nhân giữ ý kiến và nét riêng của người viết. Văn kỹ thuật và văn tham khảo giữ giọng trung tính, giản dị.

## 26 mẫu

Các mẫu xếp từ mạnh và phổ biến nhất đến yếu nhất. Skill sửa năm mẫu đầu ngay khi gặp một lần. Mẫu ghi *yếu nếu đứng một mình* chỉ được sửa khi đoạn văn có thêm dấu hiệu khác, vì người viết cẩn thận có thể cố ý dùng từng mẫu đó.

### A. Dàn dựng thay vì nói thẳng

| # | Mẫu | Trước | Sau |
|---|-----|-------|-----|
| 1 | **Không phải X mà là Y** | "Không chỉ là X, mà còn là Y", "Điều này không có nghĩa là X. Nó có nghĩa là Y." | Nói thẳng ý chính |
| 2 | **Câu chốt một dòng và câu kết theo khuôn** | "Đó mới là điều quan trọng."; "Tóm lại, ..."; "Không màu mè. Không phô trương." | Cắt câu chốt lặp ý; gộp câu cụt thành nhận định cụ thể |
| 3 | **Câu nghe sâu sắc** | "Suy cho cùng, chìa khóa nằm ở...", "Về bản chất..." | Thay câu châm ngôn bằng nhận định cụ thể |
| 4 | **Dẫn dắt trước khi vào ý** | "Trong thời đại công nghệ 4.0 ngày nay...", "Hãy cùng tìm hiểu!", "Nói thật nhé?" | Bỏ phần dẫn và nói luôn ý chính |
| 5 | **Cãi với người không có mặt** | "Tôi không nói rằng...", "Một cách dễ nghĩ đến là..." | Bỏ phản bác hay phương án không ai nêu; giữ nhận định thật |

### B. Nhịp theo khuôn

| # | Mẫu | Trước | Sau |
|---|-----|-------|-----|
| 6 | **Bộ ba gượng ép** | "sáng tạo, đột phá và bền vững"; ba ví dụ rồi một bài học | Dùng đúng số mục nội dung cần |
| 7 | **Mở câu lặp lại** | "Điều này... Bên cạnh đó... Điều này..." | Gộp câu, đổi chủ ngữ, hoặc bỏ từ nối |
| 8 | **Gạch ngang làm từ nối vạn năng** (*yếu nếu đứng một mình*) | "Chính sách mới — được công bố bất ngờ — ảnh hưởng..." | Dùng dấu chấm, phẩy, hai chấm, ngoặc đơn; theo mẫu văn nếu mẫu có dùng gạch |
| 9 | **Rào đón chồng chất** (*yếu nếu đứng một mình*) | "có thể phần nào dường như có lẽ" | Chỉ giữ lời rào mà nguồn ủng hộ |

### C. Văn dịch

| # | Mẫu | Trước | Sau |
|---|-----|-------|-----|
| 10 | **Bị động kiểu dịch và câu thiếu chủ thể** (*yếu nếu đứng một mình*) | "Báo cáo được hoàn thành bởi nhóm phân tích." | "Nhóm phân tích đã làm xong báo cáo." |
| 11 | **"Một cách", "việc", "sự" và danh từ hóa** | "Việc tối ưu truy vấn một cách hiệu quả..." | "Tối ưu truy vấn..." |
| 12 | **Chuỗi "của", "đối với", "về mặt" và từ hành chính** | "Đối với vấn đề về mặt hiệu năng, nhóm đã tiến hành thực hiện..." | Dùng từ thường người ta nói hằng ngày |

### D. Thổi phồng và mượn uy tín

| # | Mẫu | Trước | Sau |
|---|-----|-------|-----|
| 13 | **Từ AI hay dùng** | "đóng vai trò quan trọng... đa dạng và phong phú... dấu ấn..." | Dùng từ giản dị |
| 14 | **Thổi phồng tầm quan trọng** | "đánh dấu một bước ngoặt", "mở ra kỷ nguyên mới", "tương lai đầy hứa hẹn" | Giữ sự việc, bỏ tầm vóc; kết ở sự việc cụ thể cuối cùng |
| 15 | **Liên hệ mơ hồ** | "gắn liền với", "trong khuôn khổ" | Gọi tên mối quan hệ nếu biết |
| 16 | **Đuôi "qua đó", "góp phần", "thể hiện"** | "..., qua đó thể hiện..., góp phần khẳng định..." | Chỉ giữ điều nguồn ủng hộ |
| 17 | **Giọng quảng cáo** | "nằm nép mình giữa lòng núi rừng hùng vĩ", "điểm đến không thể bỏ qua" | Nói thứ đó là gì |
| 18 | **Mượn uy tín** | "Các chuyên gia cho rằng...", "Nhiều nghiên cứu đã chỉ ra..." | Ghi nguồn thật và điều nguồn nói, hoặc bỏ nhận định |
| 19 | **Né chữ "là" và "có"** | "đóng vai trò là... tự hào sở hữu..." | "là... có..." |

### E. Định dạng theo khuôn

| # | Mẫu | Trước | Sau |
|---|-----|-------|-----|
| 20 | **In đậm để trang trí** | "**Hiệu năng:** Hiệu năng được nâng cao" | Bỏ in đậm; chuyển danh sách có nhãn thành văn xuôi |
| 21 | **Tiêu đề trang trí** | "Chiến Lược Đàm Phán Và Hợp Tác", "🚀 Giai Đoạn Ra Mắt:" | Chỉ viết hoa chữ đầu và tên riêng; bỏ emoji và mũi tên |

### F. Đồ thừa từ cuộc trò chuyện và bản nháp

| # | Mẫu | Trước | Sau |
|---|-----|-------|-----|
| 22 | **Câu thừa của chatbot** | "Câu hỏi rất hay! ... Hy vọng thông tin này hữu ích!" | Bỏ phần bọc, giữ nội dung |
| 23 | **Giới hạn kiến thức và phỏng đoán** | "Mặc dù thông tin cụ thể còn hạn chế, có lẽ..." | Nói rõ nguồn cho biết gì, hoặc bỏ câu |
| 24 | **Tiêu đề lặp lại ở câu đầu** | "## Hiệu năng" + "Tốc độ là yếu tố then chốt." | Để tiêu đề tự làm việc của nó |
| 25 | **Viết về tài liệu thay vì chủ đề** | "Hàm này được thêm vào để thay thế...", "Bảng dưới đây so sánh..." | Mô tả chủ đề; chỉ nêu quy ước khi người đọc không tự thấy |

### G. Viết cho sai người đọc

| # | Mẫu | Trước | Sau |
|---|-----|-------|-----|
| 26 | **Giải thích lại điều người đọc đã biết** | Câu trả lời đi qua chẩn đoán và bằng chứng rồi mới tới quyết định | Đưa quyết định lên đầu; để chẩn đoán và bằng chứng cho ticket sau đó |

## Ví dụ đầy đủ

**Trước (nghe như AI):**
> Tháng 12 vừa rồi, tôi đã có bốn ngày không thể nào quên tại Đà Lạt, và phải nói rằng thành phố này đã hoàn toàn chinh phục trái tim tôi. Ngay từ khoảnh khắc nhận phòng tại homestay xinh xắn nằm nép mình trên đồi thông, tôi biết mình đã đến một nơi thật đặc biệt.
>
> Không thể phủ nhận, những con dốc ở đây khá thử thách — leo lên leo xuống mỗi ngày khiến đôi chân tôi "biểu tình" không ít! — nhưng mỗi lần lên đến đỉnh, bạn sẽ được đền đáp bằng khung cảnh đẹp như tranh vẽ.
>
> Chuyến đi sẽ không trọn vẹn nếu thiếu chợ đêm Đà Lạt, nơi đông đúc du khách háo hức thưởng thức ẩm thực địa phương. Còn đồ ăn thì sao? Tuyệt vời. Dù bánh tráng nướng ở chợ là món ai cũng phải thử, món tôi thích nhất lại là bánh căn ở một quán nhỏ trong hẻm, nóng hổi vừa ra lò.
>
> Nhưng điều thực sự làm nên Đà Lạt không chỉ là cảnh đẹp — mà là cảm giác. Chỉ cần đi xa trung tâm vài con phố, bạn sẽ khám phá một Đà Lạt yên bình, chân thực hơn, mời gọi bạn chậm lại và tận hưởng từng khoảnh khắc.
>
> Tôi có quay lại không? Chắc chắn rồi — lần sau sẽ vào mùa mai anh đào, và mang theo đôi giày tốt hơn! Đà Lạt không chỉ là một điểm đến; đó là nơi để bạn yêu đi yêu lại. ✨

**Sau:**
> Tháng 12 vừa rồi tôi ở Đà Lạt bốn ngày. Homestay nằm trên một đồi thông, đi đâu cũng phải leo dốc. Chân tôi mỏi suốt mấy ngày, nhưng lên tới đỉnh thì nhìn được cả một vùng.
>
> Ai cũng bảo phải ra chợ đêm, nên tôi đi. Chợ đông, phần lớn là khách du lịch đi ăn. Bánh tráng nướng là món nổi tiếng ở đó, nhưng món ngon nhất tôi ăn lại là bánh căn ở một quán nhỏ trong hẻm, vừa ra khuôn còn nóng.
>
> Đoạn tôi thích nhất bắt đầu cách trung tâm vài con phố, nơi đường vắng và không ai vội.
>
> Tôi sẽ quay lại, lần sau vào mùa mai anh đào, và mang giày tốt hơn.

## Nguồn

- [Humanizer](https://github.com/blader/humanizer) của blader là bản gốc tiếng Anh mà skill này chuyển thể.
- [Wikipedia: Signs of AI writing](https://en.wikipedia.org/wiki/Wikipedia:Signs_of_AI_writing) là nguồn của danh sách mẫu gốc.
- [WikiProject AI Cleanup](https://en.wikipedia.org/wiki/Wikipedia:WikiProject_AI_Cleanup) duy trì trang đó.

## Lịch sử phiên bản

Xem [CHANGELOG.md](CHANGELOG.md).

## Giấy phép

MIT
