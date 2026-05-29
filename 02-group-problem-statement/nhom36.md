# 02 — Group Problem Statement

## Group convergence

Nhóm 3 người, mỗi người share top 3. Tổng cộng **9 candidates**.

| Cluster | Candidate examples | Pattern chung |
|---|---|---|
| AI Application / Lab workflow | Kiểm tra bài nộp lab còn thiếu phần nào, Setup môi trường lab AI bị lỗi, Tìm lại câu trả lời cũ trong nhóm chat | Học viên cần hoàn thành lab đúng yêu cầu, xử lý lỗi kỹ thuật và truy xuất lại tri thức lớp học |
| AI Daily for Life | Không biết ăn gì hôm nay, Không biết mặc gì ra đường, So sánh sản phẩm trước khi mua | Các quyết định nhỏ lặp lại hằng ngày, tốn năng lượng lựa chọn và có thể được gợi ý theo context cá nhân |
| RAG / AI Reliability | Manual RAG Test, Loop Agent đốt tiền, JSON Parsing Error | Kiểm soát chất lượng, chi phí và độ ổn định khi đưa AI/RAG vào workflow kỹ thuật |

## Shortlist và score

| Candidate | Actor rõ | Workflow rõ | Pain có evidence | Impact đo được | Làm trong lab | So sánh R/W/A được | Nhóm hiểu domain | Tổng |
|---|---:|---:|---:|---:|---:|---:|---:|---:|
| Automated RAG Evaluation | 5 | 5 | 4 | 5 | 5 | 5 | 5 | 34 |
| AI for Daily Life | 4 | 4 | 3 | 4 | 4 | 4 | 5 | 28 |
| AI Lab Submission | 5 | 5 | 4 | 4 | 5 | 5 | 4 | 32 |

Nhóm chọn: **Automated RAG Evaluation**.

Vì sao chọn:

- Sát nhất với bối cảnh của Khánh: Senior Fullstack Developer đang dành 6 tháng học sâu GenAI/RAG.
- Workflow rõ: tạo RAG prototype → chuẩn bị test set → chạy query → kiểm retrieved chunks/citation/answer → so sánh cấu hình.
- Bottleneck cụ thể: đánh giá thủ công mất thời gian và khó biết lỗi đến từ retrieval, chunking, prompt hay dữ liệu.
- Impact đo được bằng thời gian một vòng eval, retrieval hit rate, citation correctness, answer quality và latency.
- Có thể làm trong lab bằng scope nhỏ: dùng một tập tài liệu mẫu, 10-20 câu hỏi kiểm thử và bảng chấm điểm đơn giản.
- Dễ so sánh Rule / Workflow / Agent: spreadsheet/rubric thủ công, workflow chạy batch eval, hoặc agent debug nhiều bước.

Vì sao không chọn các bài khác:

- AI for Daily Life: gần gũi và có nhiều quyết định nhỏ lặp lại, nhưng scope dễ bị rộng; cần chọn một workflow cá nhân thật cụ thể trước khi đo impact.
- AI Lab Submission: workflow rõ và làm trong lab rất tốt, nhưng impact hẹp hơn; phù hợp làm candidate cá nhân hơn là bài nhóm chính nếu nhóm muốn đào sâu RAG.

## Quick validation

Nhóm hỏi nhanh 3 người có trải nghiệm học/build AI hoặc backend:

| Nguồn | Số người | Tín hiệu xác nhận | Tín hiệu phản bác | Nhóm sửa problem thế nào |
|---|---:|---|---|---|
| Quick interview | 3 | 3/3 từng test RAG hoặc chatbot bằng cách hỏi thủ công từng câu; đều khó biết lỗi do retrieval, prompt hay dữ liệu | 1 người nói prototype nhỏ có thể test tay tạm đủ | Thu hẹp problem: không phải "đánh giá AI nói chung", mà là "lặp lại evaluation cho RAG prototype nhỏ" |
| Mini poll trong lớp | 6 | 4/6 từng gặp câu trả lời AI đúng bề mặt nhưng citation/chunk không chắc | Một số bạn chưa build RAG đủ sâu để thấy pain | Giữ scope cho người đang build RAG hoặc đang học RAG nâng cao |
| Log / notes cá nhân | 1 prototype | Có nhiều lần thay chunk size/prompt nhưng không có bảng so sánh rõ | Chưa có golden dataset chuẩn | Thêm bước tạo 10-20 câu hỏi chuẩn và lưu baseline trước khi tối ưu |

Insight sau validation:

```text
Pain thật không nằm ở việc "AI trả lời sai" chung chung. Pain nằm ở việc mỗi lần chỉnh RAG pipeline, developer không có cách đo lặp lại để biết cấu hình mới tốt hơn hay chỉ cảm giác tốt hơn.
```

## Research giải pháp

Nhóm tìm các hướng đã có sẵn, không giả định phải tự build từ đầu.

| Nguồn / tool / case | Link | Họ giải quyết phần nào? | Điểm mạnh | Khoảng trống / rủi ro | Bài học cho nhóm |
|---|---|---|---|---|---|
| Ragas | https://docs.ragas.io/en/v0.3.4/ | Metrics/evaluation cho LLM app và RAG pipeline | Có sẵn hướng đo retrieval/generation quality | Cần hiểu metric; kết quả LLM-as-judge vẫn phải review | Dùng như reference metric, không xem điểm số là chân lý tuyệt đối |
| LangSmith RAG Evaluation | https://docs.langchain.com/langsmith/evaluate-rag-tutorial | Tạo dataset, chạy eval, so sánh kết quả RAG | Hợp với workflow experiment và regression test | Có thể gắn với ecosystem LangChain; cần setup project | Pattern tốt: dataset cố định + chạy eval lặp lại sau mỗi thay đổi |
| Arize Phoenix | https://arize.com/docs/phoenix | Trace, eval và phân tích LLM/RAG runs | Tốt cho quan sát retrieved chunks, spans, scores | Có thể quá nhiều tính năng cho lab nhỏ | Lấy ý tưởng trace từng query để debug retrieval/prompt rõ hơn |
| RAGAS paper | https://arxiv.org/abs/2309.15217 | Đề xuất framework đánh giá RAG | Giúp hiểu RAG cần đo cả retrieval và generation | Paper không phải plug-and-play product | Problem statement nên tách lỗi retrieval và lỗi answer generation |

Research takeaway:

```text
Không nên build một agent tự debug toàn bộ RAG ngay. Hướng hợp lý hơn là Workflow: tạo golden question set nhỏ, chạy batch eval, lưu retrieved chunks/citation/answer/latency, rồi để developer review bảng kết quả và quyết định chỉnh gì tiếp theo.
```

## Workflow before/after

File nhóm nộp kèm:

```text
02-group-problem-statement-workflow.png/pdf/md
```

Nội dung workflow:

**Current workflow**

![Automated RAG Evaluation Current Workflow](https://drive.google.com/thumbnail?id=16PeSWnYFlkO5vIwsPYDBKTRuM3btUjI7&sz=w1600)

**Future workflow**

![Automated RAG Evaluation Future Workflow](https://drive.google.com/thumbnail?id=1JTXxl1kRY1UpKvjins3TEK2JQD9Q-KuS&sz=w1600)

```text
CURRENT STATE — 6 bước, 120 phút/vòng

[1 Chọn cấu hình RAG: 10']
→ [2 Chạy thử vài câu hỏi bằng tay: 25']
→ [3 Đọc answer/citation/retrieved chunks: 45']  <-- bottleneck
→ [4 Ghi notes/spreadsheet thủ công: 20']
→ [5 Đoán lỗi nằm ở retrieval/chunking/prompt/data: 10']
→ [6 Chỉnh config và chạy lại: 10']

FUTURE STATE — 6 bước, 30 phút/vòng

[1 Chọn config + eval set: 5']       -- Rule/checklist
→ [2 Workflow chạy batch Q&A: 5']    -- Workflow step
→ [3 Tự lưu answer/chunks/citation/latency: 5'] -- Workflow step
→ [4 AI gợi ý failure category: 5']  -- AI hỗ trợ phân loại
→ [5 Developer review bảng eval: 8'] -- Human boundary
→ [6 Chọn thay đổi tiếp theo: 2']

Fallback:
Nếu AI chấm không chắc hoặc citation mâu thuẫn → developer mở retrieved chunks và chấm thủ công theo rubric.

Bottleneck mới:
Developer review bảng eval. Đây là bottleneck chấp nhận được vì đó là điểm kiểm soát chất lượng.
```

Before/after impact:

| Metric | Trước | Sau kỳ vọng | Ghi chú |
|---|---:|---:|---|
| Tổng thời gian một vòng eval | 120 phút | Dưới 30 phút | Target chính |
| Số bước | 6 | 6 | Không giảm bước nhiều, nhưng giảm effort ở bước kiểm và ghi nhận |
| Bước thủ công | 6/6 | 2/6 | Developer vẫn review và quyết định thay đổi |
| Bottleneck chính | Đọc từng answer/chunk thủ công | Review bảng eval | Human boundary |
| Risk mới | Chủ yếu là cảm tính | LLM-as-judge có thể chấm sai | Cần rubric + sample review thủ công |

## Problem Statement v0

| Field | Nội dung |
|---|---|
| **Actor** | Khánh, Senior Fullstack Developer đang tự học GenAI/RAG và build RAG prototype. |
| **Workflow** | Mỗi lần đổi chunking, embedding, retrieval, rerank hoặc prompt, Khánh chạy thử câu hỏi thủ công, đọc answer/citation/chunks, ghi notes và tự đoán lỗi nằm ở đâu. |
| **Bottleneck** | Bước đọc từng answer/citation/retrieved chunks và so sánh nhiều cấu hình mất khoảng 1-2 giờ/vòng, nhưng kết quả vẫn cảm tính. |
| **Impact** | Khánh khó biết pipeline mới tốt hơn thật hay chỉ tốt hơn trên vài câu hỏi mẫu; tốc độ học/build demo RAG bị chậm. |
| **Success Metric** | Giảm thời gian một vòng eval từ khoảng 120 phút xuống dưới 30 phút; có bảng so sánh retrieval hit, citation correctness, answer quality và latency. |
| **Boundary** | Không để AI tự quyết kiến trúc cuối; không xem điểm LLM-as-judge là tuyệt đối; developer phải review các case quan trọng. |

## Rule / Workflow / Agent

| Mức | Phương án | Khi nào đủ | Rủi ro | Chọn? |
|---|---|---|---|---|
| **Rule** | Golden question set + rubric + spreadsheet thủ công | Đủ nếu chỉ test vài câu và ít thay đổi config | Vẫn tốn thời gian, khó scale khi nhiều cấu hình | Không chọn làm toàn bộ, nhưng dùng làm nền |
| **Workflow** | Eval set → batch query → lưu answer/chunks/citation/latency → AI phân loại lỗi → developer review | Hợp vì các bước rõ, lặp lại, có metric và human review | LLM-as-judge có thể chấm sai, cần kiểm mẫu | Chọn |
| **Agent** | Agent tự chạy nhiều cấu hình, tự phân tích lỗi, tự đề xuất và sửa pipeline | Chỉ cần khi muốn tự tối ưu nhiều nhánh/config | Dễ tốn chi phí, vòng lặp khó kiểm soát | Chưa chọn |

Mức chọn:

```text
Workflow.
```

Vì sao:

- Evaluation có các bước lặp lại rõ ràng.
- Rule/checklist đủ làm nền nhưng chưa giảm nhiều effort khi so sánh nhiều cấu hình.
- AI hữu ích ở bước phân loại lỗi và tóm tắt kết quả, nhưng developer vẫn phải review.
- Chưa cần agent vì chưa muốn AI tự tối ưu pipeline hoặc tự chạy nhiều vòng không kiểm soát.

## Problem Statement v1

| Field | Nội dung |
|---|---|
| **Actor** | Khánh, Senior Fullstack Developer đang học sâu GenAI/RAG trong 6 tháng. |
| **Workflow** | Chuẩn bị eval set → chạy RAG với một config → lưu answer/retrieved chunks/citation/latency → chấm theo rubric → so sánh config → chọn chỉnh sửa tiếp theo. |
| **Bottleneck** | Đọc và chấm thủ công answer/citation/chunks cho nhiều cấu hình mất 1-2 giờ/vòng và khó tái lập. |
| **Impact** | Tốc độ học/build RAG chậm; dễ chọn kiến trúc theo cảm tính; khó chứng minh demo tốt hơn sau mỗi thay đổi. |
| **Success Metric** | Một vòng eval dưới 30 phút; có bảng so sánh retrieval hit, citation correctness, answer quality, latency; ít nhất 10-20 câu hỏi chạy lặp lại được. |
| **Boundary** | AI không tự quyết kiến trúc cuối, không tự sửa pipeline, không thay thế review của developer ở các câu hỏi quan trọng. |
| **AI intervention point** | Sau khi batch query chạy xong, AI hỗ trợ phân loại lỗi và tóm tắt kết quả trước khi developer review. |
| **Mức chọn** | Workflow: rule/rubric làm nền, batch eval tự động, AI phân loại lỗi, developer review. |
| **Rủi ro & người thật kiểm tra** | Risk: LLM-as-judge chấm sai, metric bị lệch, overfit vào eval set nhỏ. Người thật review: developer kiểm mẫu các câu fail/pass quan trọng. |

## Final decision

Decision:

```text
Go với scope nhỏ.
```

Pilot nhỏ nhất:

- Dùng một tập tài liệu mẫu nhỏ cho RAG.
- Tạo 10-20 golden questions và expected evidence/source.
- Chạy 2 cấu hình RAG: baseline và config mới.
- Lưu answer, retrieved chunks, citation, latency vào bảng.
- AI hỗ trợ phân loại lỗi: retrieval miss, wrong chunk, weak prompt, hallucination, data missing.
- Developer đo thời gian review và số lỗi phát hiện được.

Exit / rollback:

- Nếu thời gian setup eval lâu hơn thời gian test tay trong scope nhỏ, quay về rubric + spreadsheet.
- Nếu AI chấm sai quá nhiều hoặc không giải thích được lý do chấm, chỉ giữ batch logging và human review.
- Nếu eval set quá nhỏ làm kết quả lệch, chưa dùng điểm số để quyết định kiến trúc cuối.

Decision rationale:

- Problem rõ, workflow rõ, metric rõ.
- Có non-AI components: golden dataset, rubric, spreadsheet/logging.
- AI nằm ở một bước cụ thể: phân loại lỗi và tóm tắt kết quả.
- Human review rõ: developer vẫn kiểm citation/chunks và quyết định config tiếp theo.
