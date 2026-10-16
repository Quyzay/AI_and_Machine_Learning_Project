LINK COLAB

https://colab.research.google.com/drive/1QWTvqB2rHimYg4jKWYVX_ndO4BBVGFTb?usp=sharing

-------------------------------------------------------------------------------------------------

1. Phân tích từng phần của dự án
Phần 1. Thu thập và đọc dữ liệu
Mục tiêu: Đọc file hmnist_28_28_RGB.csv và kiểm tra dữ liệu.
- Mỗi dòng đại diện cho một ảnh da.
- Các cột pixel mô tả màu sắc của ảnh RGB kích thước 28 × 28.
- Cột label cho biết ảnh thuộc nhóm tổn thương da nào.
Đánh giá sơ bộ: Dataset đã được chuyển thành dữ liệu số nên thuận tiện cho Machine Learning. Tuy nhiên, ảnh 28 × 28 khá nhỏ, có thể làm mất các chi tiết quan trọng của tổn thương da.


Phần 2. Khám phá dữ liệu (EDA)
Mục tiêu: Xem số lượng ảnh trong từng lớp và hiển thị một số ảnh mẫu.
- Biểu đồ phân bố lớp cho biết lớp nào có nhiều hoặc ít ảnh.
- Ảnh mẫu giúp kiểm tra dữ liệu có được đọc đúng hay không.
- Nếu số lượng ảnh giữa các lớp chênh lệch lớn, mô hình có thể học tốt lớp phổ biến nhưng dự đoán kém lớp hiếm.
Đánh giá sơ bộ: EDA giúp phát hiện sự mất cân bằng dữ liệu trước khi huấn luyện. Cần xem biểu đồ thực tế sau khi chạy để xác định lớp nào chiếm ưu thế.


Phần 3. Tiền xử lý dữ liệu
Mục tiêu: Chuẩn hóa giá trị pixel và chuyển dữ liệu về định dạng phù hợp.
- Giá trị pixel từ 0–255 được chia cho 255 để đưa về khoảng 0–1.
- Dữ liệu được chuyển thành dạng (số ảnh, 28, 28, 3).
- Tập dữ liệu được chia thành train và test theo tỷ lệ 80:20.
Đánh giá sơ bộ: Tiền xử lý giúp CNN nhận đúng cấu trúc ảnh và thường giúp quá trình học ổn định hơn. Chia tập có phân tầng (stratify) giúp tỷ lệ các lớp trong tập train và test tương đối đồng đều.


Phần 4. Mô hình SVM
Mục tiêu: Xây dựng mô hình Machine Learning để phân loại tổn thương da dựa trên pixel ảnh.
- Ảnh được chuyển từ ma trận thành vector một chiều.
- SVM sử dụng kernel RBF để học ranh giới giữa các lớp.
- class_weight="balanced" hỗ trợ các lớp có số lượng mẫu khác nhau.
Đánh giá sơ bộ: SVM là mô hình cơ sở để so sánh với CNN. Tuy nhiên, khi dữ liệu có nhiều pixel và nhiều ảnh, SVM có thể tốn thời gian huấn luyện và không tận dụng cấu trúc không gian của ảnh tốt như CNN.
Phần 5. Mô hình CNN
Mục tiêu: Để máy học các đặc trưng trực tiếp từ ảnh.
- Conv2D: phát hiện đặc trưng như cạnh, màu sắc và hoa văn.
- MaxPooling2D: giảm kích thước đặc trưng.
- BatchNormalization: hỗ trợ quá trình huấn luyện ổn định hơn.
- Dropout: giảm nguy cơ học thuộc dữ liệu.
- Softmax: đưa ra xác suất dự đoán cho 7 lớp.
Đánh giá sơ bộ: CNN phù hợp với bài toán ảnh vì có khả năng tự học đặc trưng. Tuy nhiên, hiệu quả phụ thuộc vào chất lượng dữ liệu, số lượng ảnh, cấu hình mô hình và thời gian huấn luyện.


Phần 6. Huấn luyện mô hình
Mục tiêu: Cho mô hình học từ tập train và theo dõi kết quả trên tập validation.
- accuracy thể hiện tỷ lệ dự đoán đúng.
- loss đo mức độ sai lệch của dự đoán so với nhãn thật.
- EarlyStopping dừng huấn luyện khi kết quả validation không cải thiện sau một số epoch.
Đánh giá sơ bộ:
- Nếu accuracy train và validation cùng tăng, mô hình có dấu hiệu học được đặc trưng hữu ích.
- Nếu accuracy train cao nhưng validation thấp, mô hình có thể bị overfitting.
- Nếu cả hai đều thấp, mô hình có thể chưa học đủ hoặc cần điều chỉnh dữ liệu và kiến trúc.

  
Phần 7. Đánh giá mô hình
Chỉ số	Ý nghĩa đơn giản
Accuracy	Tỷ lệ dự đoán đúng trên toàn bộ mẫu
Precision	Trong các mẫu được dự đoán thuộc một lớp, có bao nhiêu mẫu đúng
Recall	Trong các mẫu thực sự thuộc một lớp, mô hình tìm ra được bao nhiêu
F1-score	Cân bằng giữa Precision và Recall
Confusion Matrix	Cho biết mô hình nhầm lớp nào với lớp nào
Đánh giá sơ bộ: Không nên chỉ dựa vào Accuracy vì dataset có thể mất cân bằng. Cần xem thêm Recall và F1-score của từng lớp, đặc biệt là các lớp có ý nghĩa lâm sàng quan trọng.


Phần 8. So sánh SVM và CNN
Tiêu chí	SVM	CNN
Đầu vào	Vector pixel	Ảnh RGB 28 × 28
Cách học	Học ranh giới phân loại	Tự học đặc trưng ảnh
Tốc độ	Có thể chậm khi dữ liệu lớn	Phù hợp hơn khi dùng GPU
Ưu điểm	Làm mô hình cơ sở dễ so sánh	Khai thác cấu trúc ảnh
Hạn chế	Không học đặc trưng không gian trực tiếp	Cần điều chỉnh huấn luyện và tài nguyên
Kết luận sơ bộ: CNN có lợi thế về việc học đặc trưng từ ảnh, nhưng chưa thể khẳng định CNN tốt hơn SVM cho dataset này nếu chưa so sánh các chỉ số trên cùng tập test.


Phần 9. Dự đoán ảnh mới
Mục tiêu: Kiểm tra mô hình bằng cách đưa ảnh vào và nhận kết quả dự đoán.
- Ảnh phải được chuyển về RGB, kích thước 28 × 28.
- Giá trị pixel cần được chuẩn hóa giống lúc huấn luyện.
- Mô hình trả về lớp dự đoán và xác suất tương ứng.
Đánh giá sơ bộ: Chức năng này thể hiện khả năng ứng dụng mô hình. Tuy nhiên, ảnh tải lên cần có định dạng tương tự dữ liệu huấn luyện; ảnh điện thoại thông thường có thể khác ảnh dermatoscopy trong dataset.
Lưu ý: mô hình chỉ phục vụ mục đích học tập, không dùng để tự chẩn đoán ung thư da.
