# Prototype · FlyingPigs · T3 Du lịch

**Mở:** tải repo về rồi mở `index.html` bằng Chrome hoặc Edge. Một file, không cần cài đặt, không có backend.

**Ca khó:** khách **xác nhận mơ hồ** ("ừ ok lấy chuyến đó đi") và **trợ lý vừa đổi nhầm** ("sao lại 7h??"), trên đặt chỗ dựng TRV-7Q2K (2 hành khách, có bé 5 tuổi, BM 217 chỉ còn 1 chỗ).

## Bấm qua ca khó (≈2 phút)

Trang tự dẫn đường:
- **Bắt đầu:** khung chat trống có thẻ viền cam **"▶ Bắt đầu ở đây"** với nút *Gửi tin đầu tiên của khách*.
- **Đang ở đâu:** thanh **7 bước + 🏁 Kết thúc** ở đầu trang. Bước hiện tại tô cam, bước đã qua có dấu ✓, dòng **👉 BƯỚC x/7** nói cần bấm gì và sẽ thấy gì.
- **Bấm gì tiếp:** nút cần bấm luôn có **viền cam nhấp nháy**, và khung chat tự cuộn tới nút đó.
- **Kết thúc:** thẻ xanh **🏁** trong chat tóm tắt điều vừa thấy, kèm nút *Làm lại từ đầu* và *Xem nhánh 2b*.

Đường chính (7 bước):
1. Gửi tin đầu tiên *"Bị huỷ à?? Cho mình bay tối nay…"* → AI không giữ BM 217 (thiếu chỗ), tự giữ 2 chỗ BM 201.
2. 💬 *"ừ ok lấy chuyến đó đi"* → nhãn "Mình chưa chắc" ②, 3 thẻ chọn ④, nhật ký policy có dòng `CHẶN doi_chuyen_bay`.
3. Chọn **BM 201**.
4. **Xác nhận đổi sang BM 201** trên thẻ vàng ⑥.
5. 💬 *"Ơ sao lại 7h??…"* → AI giữ sẵn BM 205, hiện **Hoàn tác** ⑦ có đồng hồ.
6. Bấm **Hoàn tác** → 3 nút "sai ở đâu?" ⑨.
7. **Xác nhận đổi sang BM 205** → 🏁.

Nhánh 2b (quá hạn): ở bước 5 bấm **⏩ Tua +10 phút** trước khi nhắn → vé đã xuất, AI tự chuyển nhân viên ⑧, panel "Màn hình nhân viên" hiện tóm tắt → 🏁.

Tin ở mục *"Thử thêm (ca phụ)"* rẽ khỏi đường chính (hoàn vé, trẻ bay một mình, tin tự xưng nhân viên); thanh bước sẽ ghi **CA PHỤ**.

Menu **⚙ Công cụ cho người trình bày** (thu gọn): nhảy thẳng tới bước 3 / bước 6 / nhánh 2b, tua giờ, bật/tắt số chú thích.

Các tin gợi ý khác để thử: hoàn vé (#6), cho bé bay một mình (#7), tin tự xưng nhân viên hãng (#9), hỏi bồi thường (#2).

## Ảnh đánh số

- `screen-ambiguous.png`: Ca 1 · xác nhận mơ hồ
- `screen-wrong.png`: Ca 2 · đổi nhầm, còn hạn hoàn tác
- `screen-late.png`: Ca 2b · đổi nhầm, vé đã xuất → nhân viên

Ý nghĩa từng số ①–⑨ (nguyên tắc HAX/PAIR và mức tự chủ): xem `spec.md` §5 hoặc panel "Chú thích" trong trang.

## Lời gọi AI thật (tuỳ chọn, bonus)

- Không có ô nhập key: trang **tự đọc key** khi mở, theo thứ tự — `fetch('../.env')` (hoạt động khi chạy qua server nhỏ: `python -m http.server` ở gốc repo rồi mở `http://localhost:8000/prototype/`), rồi `prototype/env.local.js` (hoạt động cả khi mở trực tiếp bằng file://). Cả hai file **đã gitignore**, không nằm trong repo nộp. Biến nhận: `GEMINI_API_KEY` hoặc `ANTHROPIC_API_KEY`/`CLAUDE_API_KEY`; key `sk-ant-…` → Claude, còn lại → Gemini (key Gemini lấy miễn phí tại aistudio.google.com, không cần thẻ).
- Gõ tin tự do vào khung chat → trang gọi Claude (`claude-opus-5-5`, Anthropic JS SDK `@anthropic-ai/sdk@1.3.0` tải từ jsDelivr, structured output JSON, có bật server-side fallback khi model từ chối) hoặc Gemini (`gemini-3.5-flash`, REST `generateContent`, structured output JSON, tự huỷ sau 20 giây nếu model quá tải) để **phân loại ý định và đề xuất tool** — cùng một system prompt và schema. JSON trả về hiện trong panel "Nhật ký quyết định".
- **Lớp policy trong code** quyết định việc gì được chạy: `doi_chuyen_bay` và `gui_yeu_cau_hoan_ve` đề xuất từ AI luôn bị chặn, chỉ chạy khi khách bấm nút trên thẻ.
- Chỉ gửi đi dữ liệu dựng của track và tin bạn gõ. Không có key thì tin gõ tự do được nhận diện bằng từ khoá, không gửi gì ra ngoài.
- Bằng chứng bonus +3: **`ai-call-log.png`** trong thư mục này — tin gõ tự do "ừ ok đổi luôn chuyến đó cho mình đi" được Gemini phân loại (`mo_ho: true`, JSON đầy đủ trong panel "Nhật ký quyết định") và lớp policy vẫn chặn `doi_chuyen_bay`. Key không xuất hiện trong ảnh.
