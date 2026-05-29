Case ví dụ: **Automated RAG Evaluation**

Khánh là Senior Fullstack Developer (7 năm kinh nghiệm). 
Khánh vừa nghỉ việc tại một công ty Outsourcing lớn để dành 6 tháng 
tự học chuyên sâu về GenAI và kiến trúc 
RAG (Retrieval-Augmented Generation - hệ thống truy xuất dữ liệu nâng cao cho AI).

## Vì sao đây là ví dụ tốt?

- Có actor cụ thể (Senior Engineer chuyển dịch sang AI).
- Có workflow lặp lại mỗi khi tinh chỉnh hệ thống.
- Có bottleneck kỹ thuật rất rõ (Đánh giá chất lượng đầu ra bất định của LLM).
- Có metric thời gian và rủi ro tài chính rõ ràng.
- Có sự phân tách sắc nét giữa tư duy lập trình truyền thống (Rule-based) và AI Agent.

---

# 01 — Individual Problem Scan

## Scan rộng

| # | Lăng kính | Problem quan sát được | Ai đang đau? | Dấu hiệu thật |
|---|---|---|---|---|
| 1 | Lặp lại | Phải ngồi test tay (Manual Test) lại hàng chục câu hỏi mỗi khi thay đổi một câu Prompt hoặc đổi thuật toán cắt chữ (Chunking size) để xem AI có bị trả lời tệ đi không. | Bản thân | Mất 2 - 3 tiếng/lần chỉnh sửa; không dám đẩy code mới vì sợ làm hỏng câu trả lời cũ. |
| 2 | Tốn thời gian | Đọc, lọc và xử lý thủ công các file tài liệu PDF bị lỗi định dạng (loại bỏ headers, footers, bảng biểu phức tạp) trước khi đưa vào Vector Database để tránh làm AI bị "nhiễm độc" thông tin. | Bản thân |Mất cả buổi chiều chỉ để viết script Regex bóc tách chữ cho 50 file PDF pháp lý. |
| 3 | Rủi ro tài chính | Chuỗi các Agent tự gọi nhau (Prompt Chaining) bị rơi vào vòng lặp vô hạn (Infinite Loop) hoặc bốc nhầm dữ liệu quá dài khiến hóa đơn API OpenAI/Anthropic tăng vọt không kiểm soát. | Founder/Bootstrapper | Tài khoản bị trừ sạch 150 USD chỉ sau một đêm ngủ quên khi chạy script thử nghiệm. |
| 4 | Tốn thời gian | Ngồi cấu hình hạ tầng, cài đặt Vector DB (Pinecone/Milvus), cấu hình Docker, và quản lý môi trường ảo Python local thay vì tập trung vào tối ưu thuật toán RAG. | bản thân | 3 ngày đầu tuần trôi qua chỉ để sửa lỗi xung đột thư viện pip và cấu hình GPU trên máy. |
| 5 | AI có thể tốt hơn | Cơ chế tìm kiếm ngữ nghĩa (Semantic Search) thuần túy bốc nhầm các văn bản cũ đã hết hiệu lực vì chúng có từ khóa tương đồng với văn bản mới, khiến AI trả lời sai luật hiện hành. | Người dùng cuối, bản thân | AI liên tục trích dẫn Luật Doanh Nghiệp năm 2020 thay vì bản cập nhật năm 2026. |
| 6 | Tốn thời gian | Hệ thống Multi-Agent xử lý tuần tự qua quá nhiều bước khiến người dùng phải đợi rất lâu mới thấy chữ xuất hiện trên màn hình, Khánh loay hoay cấu hình Server-Sent Events (SSE) để stream kết quả. | Người dùng cuối, bản thân | Thời gian phản hồi (Latency) lên tới hơn 30 giây cho một câu hỏi tóm tắt văn bản. |
| 7 | AI có thể tốt hơn | Mô hình LLM bị hiện tượng "Mất tích ở giữa" (Lost in the Middle), bỏ qua hoàn toàn các bằng chứng pháp lý quan trọng nếu chúng nằm ở giữa một Prompt dài chứa nhiều tài liệu nhồi nhét. | Người dùng cuối | AI trả lời "Không tìm thấy thông tin" mặc dù tài liệu đó đã được bốc lên và truyền vào Prompt. |
| 8 | Pain từ người khác | Mô hình AI hứng lên tự trả về thêm vài câu thoại thừa như "Here is your JSON:" hoặc thiếu một dấu đóng ngoặc } làm sập hàm JSON.parse() ở tầng Backend và gây trắng xóa giao diện Frontend. | Bản thân | Lỗi 500 Internal Server Error xuất hiện ngẫu nhiên 2-3 lần sau mỗi 10 lần chatbot phản hồi. |
| 9 | Lặp lại | Mỗi lần chuyển đổi giữa các mô hình (ví dụ từ GPT-4o sang Llama-3 chạy local để tiết kiệm tiền), Khánh lại phải viết lại toàn bộ code gọi API và cấu hình lại tham số định dạng đầu ra. | Bản thân | Mất cả ngày chỉ để "refactor" lại tầng API Gateway cho AI khi nhà cung cấp đổi phiên bản mô hình. |
| 10 | Áp lực thời gian | Bị ngợp trước hàng chục bài báo nghiên cứu (Research Papers) và thư viện mới ra mắt mỗi tuần (LangChain, LlamaIndex, CrewAI), không biết nên học cái nào để kịp ra mắt sản phẩm trong 6 tháng. | Bản thân | 10+ tabs arXiv luôn mở; cảm giác FOMO (sợ bỏ lỡ) và hoang mang về lộ trình học tập tăng cao. |

Điểm mạnh của phần Scan này:

- Đúng tầm Senior: Các vấn đề không phải là "Làm sao để cài Python" hay "Prompt là gì", mà tập trung vào các bài toán kiến trúc sâu cấp hệ thống (Latency, JSON Parsing, Token Cost, Evaluation).
- Xung đột tư duy rõ ràng: Thể hiện rõ nỗi đau của một Fullstack Dev khi mang tư duy "Chính xác tuyệt đối" (Deterministic) sang thế giới "Xác suất bất định" (Probabilistic) của AI (ví dụ ở Problem #1, #5, #8).
- Metric thực tế: Sử dụng các chỉ số đo lường sát sườn với dân làm AI (Latency tính bằng giây, Token tính bằng USD, Context window tính bằng Tokens, Chunk size).
- Tuyệt đối không giải pháp: Tập trung hoàn toàn vào việc bóc tách "nỗi đau" và triệu chứng thật, chưa hề đề cập đến việc dùng công cụ hay xây dựng con Bot nào để giải quyết.

## Top 3

| Rank | Problem | Vì sao chọn | Điều còn chưa chắc |
|---|---|---|---|
| 1 | Manual RAG Test | Workflow rõ, triệt tiêu tốc độ code, mang đúng tư duy Unit Test từ Fullstack sang. | Khó tạo tập câu hỏi chuẩn (Golden Dataset); điểm số AI chấm liệu có chuẩn? |
| 2 | Loop Agent đốt tiền | Sát sườn tài chính của người nghỉ việc; có thể chặn ở tầng API Gateway. | Làm sao phân biệt vòng lặp "lỗi" và vòng lặp "Agent đang cố sửa sai"? |
| 3 | JSON Parsing Error | Điểm gãy chí mạng giữa thế giới bất định (AI) và thế giới chính xác (Fullstack). | Ép khuôn chặt quá có làm AI bớt thông minh hoặc tăng Latency không? |


## Problem Card #1 — Automated RAG Evaluation

**Problem 1 câu:**  
Mỗi khi chỉnh sửa Prompt hoặc cấu trúc RAG, Khánh mất 2-3 tiếng để test tay lại 50 câu hỏi bằng mắt nhằm kiểm tra chất lượng phản hồi, gây nghẽn tiến độ và tạo tâm lý ngại cải tiến code.

**Actor:**  
Khánh, Senior Fullstack Engineer đang tự học và làm sản phẩm AI một mình.

**Thời điểm / bối cảnh:**  
Mỗi khi tối ưu thuật toán RAG hoặc refactor câu lệnh hệ thống (System Prompt).

**Current workflow:**

```text
1. Chỉnh sửa code Python (thuật toán RAG hoặc Prompt) trong IDE
2. Deploy code thử nghiệm lên môi trường Local
3. Mở file Excel chứa danh sách 50 câu hỏi test chuẩn (Golden Dataset)
4. Copy-paste từng câu hỏi vào giao diện Chatbot, đợi 5-10 giây phản hồi
5. Dùng mắt đọc, đối chiếu thủ công với tài liệu gốc để check lỗi "bịa đặt" (Hallucination)
6. Ghi chú kết quả Đạt/Không đạt vào file Excel thủ công
7. Lặp lại cho đến khi hết 50 câu
```

**Bottleneck:**  
Bước 5 — Đọc hiểu và thẩm định chất lượng bằng mắt tốn quá nhiều năng lượng nhận thức, dễ sai sót mang tính cảm tính sau khi đọc quá nhiều chữ.

**Impact:**  
 Mất 150 phút/lần test. Một tuần sửa code 3 lần $\rightarrow$ Mất gần 8 tiếng/tuần chỉ để test tay. Khánh bị tâm lý "sợ sửa code" vì ngại quy trình test quá mệt mỏi.

**Success metric:**  
Giảm tổng thời gian chạy và đánh giá tập Test xuống dưới 5 phút, xuất ra được chỉ số chất lượng cụ thể (Score 0.0 - 1.0) một cách tự động.

**Non-AI alternative:**  
Viết Unit Test (PyTest) kiểm tra xem câu trả lời có chứa các từ khóa cố định (Keywords matching). Cách này thất bại vì AI có thể đổi từ đồng nghĩa nhưng ý nghĩa vẫn đúng, hoặc câu văn mượt nhưng thông tin bên trong là bịa đặt.

**AI hypothesis:**  
Áp dụng kiến trúc "AI đánh giá AI" (LLM-as-a-Judge). Dùng một mô hình độc lập để tự động đối chiếu câu trả lời với ngữ cảnh gốc và chấm điểm theo tiêu chí nghiêm ngặt.

**Quick gut:**  
Workflow (Tích hợp CI/CD tự động).

### Draft current workflow
```text
CURRENT STATE — 150 phút

[1 Sửa code/Prompt: 10']
→ [2 Chạy local: 5']
→ [3 Copy-paste 50 câu hỏi: 25']
→ [4 Chờ AI sinh câu trả lời: 15']
→ [5 Đọc bằng mắt & Thẩm định: 80']  <-- bottleneck
→ [6 Ghi log vào Excel: 15']

### Draft future workflow

```text
FUTURE STATE — 21 phút

[1 Sửa code/Prompt: 10']
→ [2 Chạy CLI Script tự động gọi API: 1']
→ [3 AI Judge chạy ngầm chấm điểm 50 câu: 0'] (Hệ thống tự chạy trong 1 phút)
→ [4 Khánh review các câu bị AI Judge gắn tag lỗi trên Dashboard: 1'] <-- human boundary

Fallback: AI Judge chấm sai -> Khánh cấu hình lại Prompt cho Judge hoặc đọc lại các câu có điểm số mập mờ (0.5 - 0.7).
```

## Problem Cards #2 và #3 — tóm tắt

| Card | Actor | Bottleneck | Metric | Quick gut | Vì sao chưa chọn làm #1 |
|---|---|---|---|---|---|
| Loop Agent đốt tiền | Khánh (Bootstrapper) | Không có cơ chế ngắt tự động khi Agent tự chat tuần hoàn | 150 USD mất sạch/đêm -> 0 USD rủi ro | Rule (Middleware) | Tần suất xảy ra ít hơn (chỉ khi code lỗi luồng Agent phức tạp) |
| JSON Parsing Error | Khánh (Backend) | AI trả về sai định dạng cấu trúc JSON mong muốn | 2-3 lỗi sập/10 lần chat → 0 lỗi | Workflow / Rule | Thuộc tầng xử lý lỗi (Error handling), không giúp tăng tốc độ R&D lõi như bài test |

---