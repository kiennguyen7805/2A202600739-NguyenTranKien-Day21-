# Lab 21 — Evaluation Report

**Học viên**: Nguyễn Trần Kiên — 2A202600739  
**Ngày nộp**: 2026-06-25  
**Submission option**: A (lightweight)  

## 1. Setup
- **Base model**: `unsloth/Qwen2.5-3B-bnb-4bit`  
- **Dataset**: `5CD-AI/Vietnamese-alpaca-gpt4-gg-translated`, 200 samples (180 train + 20 eval)  
- **max_seq_length**: 1024 (p95 = 562, rounded up to 1024)  
- **GPU**: Tesla T4, 16 GB VRAM  
- **Training cost**: $0.07 (~11.3 phút @ $0.35/hr)  
- **HF Hub link** (nếu chọn Option B): N/A  

## 2. Rank Experiment Results

| Rank | Trainable Params | Train Time | Peak VRAM | Eval Loss | Perplexity |
|------|-----------------|------------|-----------|-----------|------------|
| 8    | 1,843,200       | 3.34 min   | 15.10 GB  | 1.5598    | 4.76       |
| 16   | 3,686,400       | 4.44 min   | 14.90 GB  | 1.4970    | 4.47       |
| 64   | 14,745,600      | 3.56 min   | 15.02 GB  | 1.4758    | 4.37       |
| Base | -               | -          | -         | N/A       | N/A        |

## 3. Loss Curve Analysis
*(Chèn hình ảnh loss_curve.png được đặt tại thư mục results/loss_curve.png)*
- **Quan sát**: Đường cong loss của tập train giảm dần đều từ khoảng 1.9 xuống 1.3 trong suốt 3 epochs huấn luyện. Do tập dữ liệu mẫu nhỏ (200 mẫu) và số lượng bước huấn luyện ngắn (khoảng 67 steps), mô hình chưa có dấu hiệu bị overfitting (eval loss giảm ổn định và không bị tăng ngược lại). Tuy nhiên, nếu tăng số epoch hoặc dung lượng dữ liệu lên, cần tiếp tục giám sát chặt chẽ đường loss của tập eval.

## 4. Qualitative Comparison (5 examples)

### Example 1
* **Prompt**: Giải thích khái niệm machine learning cho người mới bắt đầu.
* **Base Model**: Machine learning là một phân khúc của trí tuệ nhân tạo, nó tập trung vào việc thiết lập các mô hình máy móc để học từ dữ liệu và từ đó dự đoán hoặc phân loại dữ liệu mới. Nhìn vào khía cạnh đơn giản, nó có nghĩa là máy học, hoặc máy tự học. Mô hình học được từ dữ liệu trước, sau đó dự đoán hoặc phân
* **Fine-tuned Model (r=16)**: Machine learning là một bộ môn công nghệ máy tính dựa trên việc học tập và cải thiện các dự đoán dựa trên dữ liệu mà không có sự hướng dẫn trực tiếp từ người dùng. Nó là một phần của AI (nền tảng trí thông minh nhân tạo). Machine learning sử dụng thuật toán và mô hình máy tính để học từ dữ liệu và t
* **Nhận xét**: **improved** - Cả hai mô hình đều trả về định nghĩa chính xác. Tuy nhiên, mô hình Fine-tuned hành văn tự nhiên và chuyên nghiệp hơn, diễn giải thuật ngữ rõ ràng hơn về hệ thống AI so với bản Base model.

### Example 2
* **Prompt**: Viết đoạn code Python tính số Fibonacci thứ n.
* **Base Model**: Để tính số Fibonacci thứ n, bạn có thể sử dụng hàm đệ quy hoặc vòng lặp. Đây là một đoạn mã Python cho phép bạn tính số Fibonacci thứ n bằng cách sử dụng vòng lặp. ```python def fibonacci(n): if n <= 0: return None if n == 1: return 0 elif n == 2: return 1 else: a, b = 0, 1 for _ in range(2, n): a, ```
* **Fine-tuned Model (r=16)**: Để tính số Fibonacci thứ n, bạn có thể sử dụng đệ quy hoặc vòng lặp. Đây là một ví dụ về cách tính số Fibonacci thứ n bằng vòng lặp: \n\n```python\ndef fibonacci(n):\n    if n <= 0:\n        return 0\n    elif n == 1:\n        return 1\n    else:\n        a, b = 0, 1\n        for _ in range(2, n+1):\n```
* **Nhận xét**: **improved** - Bản Fine-tuned xử lý các trường hợp cơ bản (base cases) như `n <= 0` trả về 0 và `n == 1` trả về 1 chuẩn xác hơn bản Base. Cả hai mô hình đều bị cắt ngắn ở dòng lặp chính do giới hạn token sinh ra nhưng mô hình sau fine-tune thụt lề code (indentation) rất chuẩn.

### Example 3
* **Prompt**: Liệt kê 5 nguyên tắc thiết kế UI/UX.
* **Base Model**: 1. **Giới thiệu**: 5 nguyên tắc thiết kế UI/UX bao gồm: 1. **Giới thiệu**: 1. **Giới thiệu**: 1. **Giới thiệu**: 1. **Giới thiệu**: 1. **Giới thiệu**: 1. **Giới thiệu**: 1. **Giới thiệu**: 1. **Giới thiệu**: 1. **Giới thiệu**: 1. **Giới thiệu**: 1. **Giới thiệu**: 1. **Giới thiệu**: 1. **Giới thiệu*
* **Fine-tuned Model (r=16)**: 1. **Chuẩn bị**: Tạo một kế hoạch trước khi bắt đầu thiết kế UI/UX. 2. **Thấu hiểu**: Thấu hiểu nhu cầu và mong muốn của người dùng là chìa khóa để tạo ra trải nghiệm người dùng tốt nhất. 3. **Tính dễ sử dụng**: Thiết kế UI/UX phải dễ sử dụng và dễ tiếp cận. Người dùng phải có thể sử dụng sản phẩm m...
* **Nhận xét**: **improved (Win lớn)** - Base model gốc bị dính lỗi lặp từ vô hạn (repetition loop bug) do chưa được huấn luyện định dạng câu trả lời (instruction-following). Bản Fine-tuned đã giải quyết hoàn toàn lỗi này và đưa ra câu trả lời liệt kê theo thứ tự rất rõ ràng và mạch lạc.

### Example 4
* **Prompt**: Tóm tắt sự khác biệt giữa LoRA và QLoRA.
* **Base Model**: LoRA (Low-Rank Adaptation) và QLoRA (Quantized LoRA) là hai phương pháp khác nhau được sử dụng để cải thiện hiệu suất của mô hình NLU (NLP) đã được huấn luyện. LoRA được sử dụng để giảm số lượng tham số của mô hình NLU đã được huấn luyện bằng cách thêm các tham số bổ sung để thay thế một phần của cá
* **Fine-tuned Model (r=16)**: LoRA (Low-Rank Adaptation) và QLoRA (Quantized LoRA) là hai phương pháp khác nhau được sử dụng để cải thiện hiệu suất của mô hình NLP (Nền tảng học máy NLP) bằng cách sử dụng kỹ thuật giảm kích thước. LoRA sử dụng kỹ thuật để giảm kích thước mô hình bằng cách thay thế các thành phần của nó bằng các 
* **Nhận xét**: **improved** - Mô hình sau fine-tune diễn đạt trôi chảy hơn và dịch đúng thuật ngữ chuyên ngành (như "NLP" thay vì dùng "mô hình NLU" chưa chính xác của Base model).

### Example 5
* **Prompt**: Phân biệt prompt engineering, RAG, và fine-tuning.
* **Base Model**: Prompt Engineering, RAG và Fine-tuning là ba kỹ thuật khác nhau trong AI để cải thiện tính hiệu quả và khả năng thực thi của mô hình ngôn ngữ. - **Prompt Engineering:** Prompt Engineering là kỹ thuật tạo ra các prompt (dòng lệnh) cho mô hình NLP để làm cho nó dễ tiếp cận và hiệu quả hơn. Các prompt 
* **Fine-tuned Model (r=16)**: Prompt Engineering, RAG (Retrieval-Augmented Generation), và Fine-tuning là ba kỹ thuật khác nhau được sử dụng trong AI để cải thiện hiệu suất của mô hình AI. 1. Prompt Engineering: Prompt Engineering là kỹ thuật sử dụng câu lệnh hoặc prompt để cung cấp thông tin cho mô hình AI, giúp nó có thể hiểu 
* **Nhận xét**: **improved** - Bản Fine-tuned có cấu trúc so sánh mạch lạc, chuyên nghiệp hơn và dịch đầy đủ thuật ngữ RAG (Retrieval-Augmented Generation).

## 5. Conclusion về Rank Trade-off
Đối với tập dữ liệu nhỏ (200 ví dụ) trong bài thực hành này, cấu hình rank `r=16` mang lại tỷ lệ hiệu năng trên tài nguyên (ROI) tối ưu nhất. Khi phân tích kết quả, chúng ta thấy hiện tượng hiệu suất bắt đầu bão hòa rõ rệt khi tăng rank (diminishing returns): việc tăng rank từ `r=16` lên `r=64` làm tăng số lượng tham số huấn luyện lên gấp 4 lần (từ 3.68M lên 14.7M), nhưng chỉ giúp cải thiện chỉ số perplexity rất nhỏ (từ 4.47 giảm xuống 4.37). Trong khi đó, rank `r=8` tuy tiết kiệm tham số nhất (1.84M) nhưng lại cho perplexity kém nhất (4.76). Khi triển khai thực tế (production), đề xuất lựa chọn cấu hình `r=16` để tối ưu hóa sự cân bằng giữa dung lượng lưu trữ adapter gọn nhẹ và độ chính xác của phản hồi.

## 6. What I Learned
- **Bài học 1**: Hiểu sâu hơn về cơ chế hoạt động của QLoRA và thư viện Unsloth giúp tối ưu hóa quá trình huấn luyện mô hình 3B trên GPU T4 miễn phí cực kỳ nhanh chóng và tiết kiệm bộ nhớ GPU VRAM.
- **Bài học 2**: Nhận thức rõ ràng về sự bão hòa hiệu quả (diminishing returns) khi tăng rank LoRA trong huấn luyện thực tế, từ đó biết cách chọn rank LoRA hợp lý mà không lãng phí tài nguyên.
- **Bài học 3**: Thấy được tác động to lớn của việc fine-tune trong việc triệt tiêu lỗi lặp từ vô hạn (repetition loop bug) của Base model gốc và cải thiện định dạng câu trả lời tiếng Việt.

