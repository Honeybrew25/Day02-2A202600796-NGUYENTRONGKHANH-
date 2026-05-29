
# 03 — Individual Reflection Example

## Đóng góp của Khánh trong nhóm

- Định vị bài toán: Giúp nhóm không bị sa đà vào việc build "thêm một con chatbot nữa" mà tập trung vào phần Tooling/M&E (Measurement & Evaluation) - một kỹ năng rất thiếu của các dev làm AI phong trào.

- Technical Input: Giới thiệu framework Ragas và tư duy đưa "Human-in-the-loop" vào đúng chỗ (Dev chỉ xem dashboard lỗi chứ không ngồi đọc lại các câu trả lời đúng).

## Bảng dùng AI trong reflection

| Phase | Tôi dùng AI để làm gì? | AI hữu ích ở đâu? | AI sai/hời hợt ở đâu? | Tôi sửa gì |
|---|---|---|---|---|
| Brainstorming | Gợi ý các metric toán học để đánh giá NLP. | Gợi ý nhanh các khái niệm như BLEU, ROUGE, BERTScore. | Các metric truyền thống này (BLEU/ROUGE) không hiệu quả với RAG hiện đại. AI không tự cập nhật được pattern "RAG Triad". | Tôi tự bỏ qua các metric cũ, ép AI đi theo hướng LLM-as-a-judge (Ragas). |
| Workflow Design | Chuyển đổi text flow thành cấu trúc Markdown trực quan. | Định dạng nhanh, tiết kiệm thời gian gõ phím. | AI cố tình nhét bước "AI tự fix code" vào để biến nó thành Agent. | Tôi cắt bỏ phần tự sửa code, giữ ranh giới con người ở bước Code Optimization. |


## Bài học của Khánh

- Muốn làm AI Master, phải biết đo lường: Code một pipeline RAG cơ bản chỉ mất 15 phút nhờ LangChain, nhưng để tối ưu nó lên mức Production thì 90% thời gian là ở bước Evaluation.
- Hiểu rõ giới hạn của AI-as-a-judge: AI chấm điểm dựa trên Prompt, do đó prompt chấm điểm phải có tiêu chí cực kỳ khắt khe (Few-shot examples rõ ràng) thì điểm số mới đáng tin cậy.
- Kế hoạch tiếp theo: Thay vì học lan man tất cả các mô hình mới ra mắt, tôi sẽ hoàn thiện workflow test tự động này. Có nó làm "la bàn", tôi có thể tự tin thử nghiệm mọi kỹ thuật RAG nâng cao khác trong 5 tháng còn lại.

---
