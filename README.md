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

**Thu thập dữ liệu**

Dự án sử dụng tập dữ liệu BraTS 2020 (Brain Tumor Segmentation Challenge 2020), một bộ dữ liệu y tế chuyên dùng cho nghiên cứu phát hiện và phân đoạn khối u não trên ảnh MRI.

BraTS 2020 là một trong những benchmark quan trọng trong lĩnh vực Medical Image Segmentation với các mục tiêu chính:

•	Phân đoạn khối u não từ ảnh MRI đa chuỗi (multi-modal MRI)

•	Hỗ trợ dự đoán tiên lượng

•	Đánh giá độ không chắc chắn của mô hình phân đoạn

**Tiền xử lý dữ liệu**

Trước khi huấn luyện, dữ liệu được tiền xử lý để đảm bảo tính nhất quán và cải thiện chất lượng đầu vào:

•	Chuẩn hóa kích thước và định dạng ảnh

•	Lọc nhiễu và cân bằng cường độ sáng

•	Giảm sai lệch giữa các lớp dữ liệu

**Xây dựng mô hình**

Hệ thống sử dụng các kiến trúc CNN 3D để xử lý dữ liệu MRI dạng thể tích:

•	U-Net3D

•	V-Net3D

•	SegNet3D

•	ResUNet3D

**Đánh giá mô hình**

Hiệu suất mô hình được đánh giá thông qua các chỉ số:
•	Accuracy

•	Loss

•	IoU (Intersection over Union)

•	Mean IoU

Các chỉ số này phản ánh khả năng phân đoạn chính xác vùng khối u so với ground truth.

**Trực quan hóa kết quả 3D**

Kết quả phân đoạn từ các mô hình được tái tạo dưới dạng mô hình 3D nhằm:

•	Hiển thị kích thước và hình dạng khối u

•	Xác định vị trí và biên giới tổn thương

•	Hỗ trợ bác sĩ trong chẩn đoán và lập kế hoạch điều trị

**Sơ đồ phương pháp đề xuất**
<img width="950" height="533" alt="image" src="https://github.com/user-attachments/assets/ed72d20b-71c5-4c4d-979d-6d75a23eec3c" />



**4. Kết quả**

4.1	Kết quả huấn luyện và kiểm thử

<img width="949" height="344" alt="image" src="https://github.com/user-attachments/assets/34e419a3-2cdc-49a7-9c36-de7959554e49" />

<img width="957" height="360" alt="image" src="https://github.com/user-attachments/assets/8784ed09-bc80-45b6-ac6f-6dbb5c643fdc" />

<img width="963" height="345" alt="image" src="https://github.com/user-attachments/assets/29b7d69d-44f5-4595-8d8e-0ac6e65d2e15" />


4.2	Kết quả trên tập test

<img width="960" height="324" alt="image" src="https://github.com/user-attachments/assets/14904f87-5e69-42c4-be36-fdfe4a2b3c66" />

<img width="961" height="351" alt="image" src="https://github.com/user-attachments/assets/eb36eab2-64cd-4e43-9309-d5413b8e1fe3" />

<img width="964" height="400" alt="image" src="https://github.com/user-attachments/assets/cc4d7579-2bb8-4ac5-906f-d9e432983cd2" />


4.1.3	Kết quả trực quan hóa 3D

**ẢNH GỐC**

<img width="1056" height="1104" alt="image" src="https://github.com/user-attachments/assets/4116be98-9d4f-4421-8c14-c479982a7472" />

Lớp 1: 28051 voxel.

Lớp 2: 85954 voxel.

Lớp 3: 13031 voxel

**UNET**

<img width="1031" height="1101" alt="image" src="https://github.com/user-attachments/assets/1e8b3746-05a6-4afd-b16a-823e28a29289" />

Lớp 1: thể tích 33506.00 đơn vị khối.

Lớp 2: thể tích 98616.00 đơn vị khối.

Lớp 3: thể tích 26009.00 đơn vị khối.

**Segnet**

<img width="950" height="1130" alt="image" src="https://github.com/user-attachments/assets/dae21ee2-87f4-4e4c-b06a-204e92302a1f" />

Lớp 1: thể tích 28634.00 đơn vị khối.

Lớp 2: thể tích 78918.00 đơn vị khối.

Lớp 3: thể tích 18971.00 đơn vị khối.


**Resnet3D**

<img width="1055" height="1134" alt="image" src="https://github.com/user-attachments/assets/a127a694-f80b-430d-9ffe-c7068f144314" />
Lớp 1: thể tích 17664.00 đơn vị khối.

Lớp 2: thể tích 159088.00 đơn vị khối.

Lớp 3: thể tích 9680.00 đơn vị khối.

**Vnet3D**

<img width="1048" height="1151" alt="image" src="https://github.com/user-attachments/assets/87910d04-7996-49d0-8566-3b4d8f5da1fb" />

Lớp 1: thể tích 29208.00 đơn vị khối.

Lớp 2: thể tích 81224.00 đơn vị khối.

Lớp 3: thể tích 24648.00 đơn vị khối.


<img width="435" height="187" alt="image" src="https://github.com/user-attachments/assets/3f0712e0-c896-4e8b-87e5-f977051215f7" />
