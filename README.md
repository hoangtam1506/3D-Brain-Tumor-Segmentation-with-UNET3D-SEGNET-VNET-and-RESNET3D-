**3D Brain Tumor Segmentation with UNET3D SEGNET VNET and RESNET3D -  SỬ DỤNG MÔ HÌNH UNET3D, SEGNET, VNET, RESNET3D TRỤC QUAN HÓA 3D KHỐI U NÃO**

**1. Giởi thiệu**
Chẩn đoán và phân loại khối u não luôn là một thách thức lớn trong y học hiện đại, do sự đa dạng về loại hình, kích thước và vị trí của các khối u. Những yếu tố này không chỉ ảnh hưởng nghiêm trọng đến sức khỏe và tính mạng bệnh nhân mà còn đòi hỏi các phương pháp chẩn đoán tiên tiến và hiệu quả hơn. Trong thực hành lâm sàng, các bác sĩ thường dựa vào hình ảnh y học như cộng hưởng từ (MRI) hoặc chụp cắt lớp vi tính (CT) để đánh giá tình trạng bệnh. Tuy nhiên, việc phân tích và đánh giá thủ công các hình ảnh này không chỉ tiêu tốn nhiều thời gian mà còn dễ xảy ra sai sót do sự phức tạp của dữ liệu.

Các mô hình học sâu như UNet3D, SegNet, VNet và ResUNet3D đã chứng minh khả năng phân đoạn hình ảnh y học nhanh chóng và chính xác. Tuy nhiên, để các công nghệ này có thể ứng dụng sâu rộng hơn trong thực tế lâm sàng, việc tích hợp các công cụ hỗ trợ trực quan hóa 3D là cần thiết. Trực quan hóa 3D không chỉ giúp làm rõ cấu trúc và đặc điểm của khối u mà còn tạo điều kiện thuận lợi cho bác sĩ trong việc lập kế hoạch điều trị.

Ví dụ của 3D trong việc chuẩn đoán
<img width="861" height="337" alt="image" src="https://github.com/user-attachments/assets/3a6d87aa-e6b8-40de-915e-98c52d3de9a0" />



**2. DATASET**

BraTS 2020 là một bộ dữ liệu tiêu chuẩn trong lĩnh vực xử lý ảnh y tế, đặc biệt dành cho việc phân đoạn khối u não (gliomas). Bộ dữ liệu này bao gồm các ảnh MRI được chụp trước phẫu thuật 

<img width="829" height="372" alt="image" src="https://github.com/user-attachments/assets/3a8e8c80-d5da-4bad-8cf0-d848cb6c2aab" />

2.1 Mô tả tập dữ liệu MRI:  Bao gồm các ảnh MRI chụp trước phẫu thuật từ nhiều bệnh nhân và được lưu trữ dưới định dạng npy (mảng NumPy)

Các loại ảnh MRI cung cấp:
  
•	T1-Weighted with Contrast Enhancement (T1ce): Sau khi tiêm chất tương phản gadolinium, chế độ này làm nổi bật các khu vực có sự tăng cường tín hiệu, thường liên quan đến sự hiện diện của khối u hoặc viêm nhiễm.

•	T2-Weighted (T2): Chế độ này nhạy cảm với sự hiện diện của nước, giúp phát hiện các khu vực phù nề hoặc tổn thương, nơi có sự tích tụ dịch.

•	T2 Fluid Attenuated Inversion Recovery (FLAIR): Bằng cách triệt tiêu tín hiệu từ dịch não tủy, chế độ FLAIR giúp làm nổi bật các tổn thương gần não thất, thường được sử dụng để phát hiện các tổn thương liên quan đến bệnh lý như đa xơ cứng.

Tiền xử lý dữ liệu:

•	Ảnh đã được đồng bộ hóa (co-registered) với cùng mẫu giải phẫu.

•	Nội suy về cùng độ phân giải không gian 1mm31 mm^31mm3.

•	Loại bỏ hộp sọ (skull-stripped) để tập trung vào vùng não.

2.2 Mô tả dữ liệu nhãn phân đoạn (Mask)

Tất cả dữ liệu MRI đều đi kèm với nhãn phân đoạn 3D (mặt nạ phân đoạn), được tạo thủ công bởi các chuyên gia thần kinh học và được kiểm duyệt.	

Các vùng được phân đoạn:

•	Vùng u tăng cường tín hiệu (GD-enhancing tumor - ET, label 4).

•	Vùng phù nề quanh khối u (Peritumoral edema - ED, label 2).

•	Lõi u hoại tử hoặc không tăng tín hiệu (Necrotic and Non-enhancing tumor core - NCR/NET, label 1).
•	Nền (background - label 0).

Nguồn gốc của nhãn

•	Được phân đoạn bởi từ 1 đến 4 chuyên gia, theo quy trình chuẩn.

•	Đảm bảo chất lượng cao nhờ kiểm duyệt của các bác sĩ thần kinh học giàu kinh nghiệm.



Số lượng tập dữ liệu: tập dữ liệu gồm 334 trường hợp, khi chia theo tỷ lệ 70% cho tập train, ~15% cho val và ~15% cho test:

<img width="902" height="329" alt="image" src="https://github.com/user-attachments/assets/5e4c1c98-ee4e-43fc-8b82-7c43be7260fe" />

**3. Phương pháp đề xuất**
**4. Kết quả**
