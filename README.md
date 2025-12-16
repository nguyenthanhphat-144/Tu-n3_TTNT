# Tuần 03: GIẢI THUẬT TÌM KIẾM ĐỐI KHÁNG

## 1. Tóm tắt Các Yêu cầu Đã Triển khai

| STT | Bài tập | Thuật toán | Ma trận Ứng dụng | Ghi chú |
| :--- | :--- | :--- | :--- | :--- |
| **BTTL 1** | Cài đặt Minimax | Minimax | 3x3 | Thuật toán cốt lõi. |
| **BTTL 2** | Cài đặt Cắt tỉa Alpha-beta | Alpha-Beta Pruning | 3x3 | Tối ưu hóa tốc độ tìm kiếm. |
| **BTVN 3 & 4** | Mở rộng Minimax & Alpha-beta | Minimax & Alpha-Beta Pruning | NxN (5x5, 10x10) | Yêu cầu phải sử dụng Hàm Lượng Giá Heuristic. |
| **BTVN 5** | Ứng dụng Alpha-beta | Alpha-Beta Pruning | Cờ Tướng, Cờ Vua, Cờ Vây | Phân tích lý thuyết về hàm Heuristic và Move Generator. |
| **BTVN 6** | Xây dựng Giao diện Đồ họa | Tkinter/OpenCV | 3x3 | Tích hợp AI vào giao diện người dùng. |

---

## 2. Phân tích Thuật toán Cốt lõi

### 2.1. Cấu trúc Trò chơi (Adversarial Search Components)

Thuật toán được xây dựng dựa trên các thành phần cơ bản của lý thuyết trò chơi:

* **Trạng thái ban đầu (Initial State):** Ma trận trò chơi rỗng + Xác định người chơi bắt đầu (X hoặc O).
* **Hàm chuyển trạng thái (Successor):** Trả về trạng thái mới sau khi thực hiện một nước đi hợp lệ.
* **Hàm lợi ích (Utility Function):** Đánh giá kết quả cuối cùng: +1 (X thắng), -1 (O thắng), 0 (Hòa).
* **Hàm Lượng giá Heuristic E(n):** Được sử dụng cho các ma trận lớn (NxN) khi tìm kiếm tuyệt đối không khả thi: $E(n) = X(n) - O(n)$ (Số khả năng thắng của X trừ đi số khả năng thắng của O).

### 2.2. Thuật toán Minimax

* **Nguyên tắc:** MAX (người chơi hiện tại) luôn cố gắng tối đa hóa Hàm Lợi ích, trong khi MIN (đối thủ) luôn cố gắng tối thiểu hóa Hàm Lợi ích.
* **Triển khai:**
    * Hàm `maxValue(state)`: Tìm nước đi tốt nhất cho MAX (tìm giá trị lớn nhất).
    * Hàm `minValue(state)`: Tìm nước đi tốt nhất cho MIN (tìm giá trị nhỏ nhất).
    * Thuật toán duyệt toàn bộ cây trò chơi cho đến trạng thái kết thúc (`terminal(state)`).

### 2.3. Thuật toán Cắt tỉa Alpha-beta (Alpha-Beta Pruning)

Alpha-beta là một tối ưu hóa của Minimax, cho phép loại bỏ các nhánh cây tìm kiếm không cần thiết mà không ảnh hưởng đến kết quả cuối cùng.

* **Nguyên tắc cắt tỉa:** Việc cắt tỉa xảy ra khi $\alpha \ge \beta$. 
    * $\alpha$ (alpha): Giá trị tốt nhất mà MAX có thể đảm bảo được tính đến thời điểm hiện tại. (Luôn tăng)
    * $\beta$ (beta): Giá trị tốt nhất mà MIN có thể đảm bảo được tính đến thời điểm hiện tại. (Luôn giảm)
* **Giải thích Vấn đề Cắt tỉa:** Nếu tại một nút MIN, giá trị $\beta$ hiện tại (ví dụ: $\beta \le 3$) nhỏ hơn hoặc bằng giá trị $\alpha$ đã được đảm bảo ở nút MAX cha (ví dụ: $\alpha \ge 10$), thì nhánh đó sẽ không bao giờ được chọn, và việc tìm kiếm ở các nút con còn lại sẽ bị dừng (cắt tỉa).

---

## 3. Triển khai Mở rộng (Bài tập về nhà)

### 3.1. Ứng dụng cho Ma trận $N \times N$ (Bài 3 & 4)

* **Vấn đề:** Khi kích thước $N$ tăng ($N=5, 10$), không gian trạng thái bùng nổ, Minimax vét cạn không khả thi.
* **Giải pháp:** Phải sử dụng **Cắt tỉa Alpha-beta** và giới hạn độ sâu (Depth-Limited Search), kết hợp với **Hàm Lượng giá Heuristic $E(n)$** để ước lượng giá trị của các trạng thái không kết thúc.

### 3.2. Ứng dụng vào Game Cờ phức tạp (Bài 5)

Việc ứng dụng Alpha-beta cho Cờ Tướng/Vua/Vây đòi hỏi các thành phần phức tạp hơn:

* **Bộ tạo Nước đi Hợp lệ (Move Generator):** Phải mã hóa toàn bộ luật chơi phức tạp (như luật chiếu, bắt quân).
* **Hàm Đánh giá (Evaluation Function):** Hàm Heuristic phức tạp, tính toán không chỉ lợi thế vật chất mà còn lợi thế vị trí, khả năng tấn công/phòng thủ, và các yếu tố chiến thuật khác.

---

## 4. Giao diện Đồ họa (Bài 6)

* **Công cụ:** Sử dụng thư viện `Tkinter` (hoặc `OpenCV` như trong ví dụ `Thuật giải Minimax 2`) để xây dựng GUI.
* **Tích hợp AI:** Lớp `GUI` hoặc hàm `mainLoop` gọi hàm `minimax` hoặc `FindBestMove` (sử dụng Alpha-beta) để xác định nước đi của máy tính và cập nhật trạng thái bảng.
