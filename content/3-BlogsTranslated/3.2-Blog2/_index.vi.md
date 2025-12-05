---
title: "Blog 2"
date: "2025-09-30"
weight: 2
chapter: false
pre: " <b> 3.2. </b> "
---

# Hiện đại hóa việc phòng chống gian lận: GraphStorm v0.5 cho suy luận thời gian thực

Gian lận tiếp tục gây ra thiệt hại tài chính đáng kể trên toàn cầu, chỉ riêng người tiêu dùng Hoa Kỳ đã mất 12,5 tỷ USD vào năm 2024—tăng 25% so với năm trước theo Ủy ban Thương mại Liên bang. Sự gia tăng này không xuất phát từ các cuộc tấn công thường xuyên hơn, mà từ sự tinh vi ngày càng tăng của những kẻ gian lận. Khi các hoạt động gian lận trở nên phức tạp và kết nối với nhau hơn, các phương pháp machine learning thông thường tỏ ra yếu kém do chỉ phân tích các giao dịch một cách riêng lẻ, không thể nắm bắt được mạng lưới các hoạt động phối hợp đặc trưng cho các âm mưu gian lận hiện đại.

Graph neural networks (GNNs) giải quyết hiệu quả thách thức này bằng cách mô hình hóa các mối quan hệ giữa các thực thể—chẳng hạn như người dùng chia sẻ thiết bị, vị trí hoặc phương thức thanh toán. Bằng cách phân tích cả cấu trúc mạng lưới và thuộc tính của thực thể, GNNs có hiệu quả trong việc xác định các âm mưu gian lận tinh vi nơi thủ phạm che giấu các hoạt động đáng ngờ riêng lẻ nhưng để lại dấu vết trong mạng lưới quan hệ của chúng. Tuy nhiên, việc triển khai phòng chống gian lận trực tuyến dựa trên GNN trong môi trường production đặt ra những thách thức đặc thù: đạt được phản hồi inference dưới một giây, mở rộng quy mô đến hàng tỷ nodes và edges, và duy trì hiệu quả vận hành cho các bản cập nhật mô hình. Trong bài đăng này, chúng tôi chỉ cho bạn cách vượt qua những thách thức này bằng cách sử dụng GraphStorm, đặc biệt là các khả năng real-time inference mới của GraphStorm v0.5.

Các giải pháp trước đây đòi hỏi sự đánh đổi (tradeoffs) giữa khả năng và sự đơn giản. Cách tiếp cận DGL ban đầu của chúng tôi cung cấp các khả năng real-time toàn diện nhưng đòi hỏi việc điều phối dịch vụ (service orchestration) phức tạp—bao gồm việc cập nhật thủ công các cấu hình endpoint và định dạng payload sau khi huấn luyện lại với các hyperparameters mới. Cách tiếp cận này cũng thiếu sự linh hoạt của mô hình, đòi hỏi phải tùy chỉnh các mô hình và cấu hình GNN khi sử dụng các kiến trúc ngoài relational graph convolutional networks (RGCN). Các triển khai DGL in-memory sau đó đã giảm bớt sự phức tạp nhưng gặp phải những hạn chế về khả năng mở rộng (scalability) với khối lượng dữ liệu cấp doanh nghiệp. Chúng tôi đã xây dựng GraphStorm để thu hẹp khoảng cách này, bằng cách giới thiệu distributed training và các APIs cấp cao giúp đơn giản hóa việc phát triển GNN ở quy mô doanh nghiệp (enterprise scale).

Trong một bài đăng blog gần đây, chúng tôi đã minh họa khả năng và sự đơn giản trong việc huấn luyện mô hình GNN quy mô doanh nghiệp và offline inference của GraphStorm. Mặc dù phát hiện gian lận bằng GNN ngoại tuyến (offline) có thể xác định các giao dịch gian lận sau khi chúng xảy ra—việc ngăn chặn tổn thất tài chính đòi hỏi phải chặn đứng gian lận trước khi nó xảy ra. GraphStorm v0.5 hiện thực hóa điều này thông qua hỗ trợ real-time inference gốc qua Amazon SageMaker AI. GraphStorm v0.5 mang đến hai cải tiến: triển khai endpoint được tinh giản giúp giảm nhiều tuần kỹ thuật tùy chỉnh—lập trình các tệp entry point của SageMaker, đóng gói các model artifacts, và gọi các APIs triển khai của SageMaker—xuống còn một thao tác lệnh duy nhất, và đặc tả payload được tiêu chuẩn hóa giúp đơn giản hóa việc tích hợp của client với các dịch vụ real-time inference. Những khả năng này cho phép thực hiện các tác vụ node classification dưới một giây như phòng chống gian lận, giúp các tổ chức chủ động chống lại mối đe dọa gian lận bằng các giải pháp GNN có khả năng mở rộng và vận hành đơn giản.

Để giới thiệu những khả năng này, bài đăng này trình bày một giải pháp phòng chống gian lận. Thông qua giải pháp này, chúng tôi chỉ ra cách một data scientist có thể chuyển đổi một mô hình GNN đã được huấn luyện sang các inference endpoints sẵn sàng cho production với chi phí vận hành tối thiểu. Nếu bạn quan tâm đến việc triển khai các mô hình dựa trên GNN để phòng chống gian lận thời gian thực hoặc các trường hợp kinh doanh (business cases) tương tự, bạn có thể điều chỉnh các phương pháp được trình bày ở đây để tạo ra giải pháp của riêng mình.

## Tổng quan về giải pháp

Giải pháp được đề xuất của chúng tôi là một quy trình (pipeline) 4 bước như được hiển thị trong hình sau. Quy trình bắt đầu ở bước 1 với việc xuất biểu đồ giao dịch từ cơ sở dữ liệu đồ thị xử lý giao dịch trực tuyến (OLTP) sang bộ nhớ có khả năng mở rộng (Amazon Simple Storage Service (Amazon S3) hoặc Amazon EFS), tiếp theo là huấn luyện mô hình phân tán ở bước 2. Bước 3 là quy trình triển khai đơn giản hóa của GraphStorm v0.5, tạo ra các real-time inference endpoints của SageMaker bằng một lệnh duy nhất. Sau khi SageMaker AI đã triển khai endpoint thành công, một ứng dụng client tích hợp với cơ sở dữ liệu đồ thị OLTP để xử lý các luồng giao dịch trực tiếp ở bước 4. Bằng cách truy vấn cơ sở dữ liệu đồ thị, client chuẩn bị các đồ thị con (subgraphs) xung quanh các giao dịch cần dự đoán, chuyển đổi đồ thị con thành định dạng payload được tiêu chuẩn hóa, và gọi endpoint đã được triển khai để dự đoán thời gian thực.

Để cung cấp chi tiết triển khai cụ thể cho từng bước trong giải pháp real-time inference, chúng tôi minh họa luồng công việc hoàn chỉnh bằng cách sử dụng nhiệm vụ phát hiện gian lận IEEE-CIS có sẵn công khai.

Lưu ý: Ví dụ này sử dụng một Jupyter notebook làm bộ điều khiển cho toàn bộ quy trình bốn bước để cho đơn giản. Để có thiết kế sẵn sàng cho production hơn, hãy xem kiến trúc được mô tả trong Build a GNN-based real-time fraud detection solution.

## Các điều kiện tiên quyết

Để chạy ví dụ này, bạn cần có một [tài khoản AWS](https://signin.aws.amazon.com/signup?request_type=register) mà mã [AWS Cloud Development Kit (AWS CDK)](https://aws.amazon.com/cdk/) của ví dụ sử dụng để tạo các tài nguyên cần thiết, bao gồm [Amazon Virtual Private Cloud (Amazon VPC)](https://aws.amazon.com/vpc/), cơ sở dữ liệu [Amazon Neptune](https://aws.amazon.com/neptune/), Amazon SageMaker AI, [Amazon Elastic Container Registry (Amazon ECR)](https://aws.amazon.com/ecr/), Amazon S3, cùng các roles và permission liên quan.

Lưu ý: Các tài nguyên này sẽ phát sinh chi phí trong quá trình thực thi (khoảng 6 USD mỗi giờ với cài đặt mặc định). Hãy theo dõi việc sử dụng một cách cẩn thận và xem lại các trang giá cho những dịch vụ này trước khi tiếp tục. Làm theo hướng dẫn dọn dẹp ở cuối để tránh các khoản phí phát sinh liên tục.

## Ví dụ thực hành: Phòng chống gian lận thời gian thực với bộ dữ liệu IEEE-CIS

Toàn bộ mã triển khai cho ví dụ này, bao gồm các Jupyter notebook và các tệp kịch bản Python hỗ trợ, hiện có sẵn trong kho lưu trữ (repository) công khai của chúng tôi. Kho lưu trữ này cung cấp một bản triển khai end-to-end hoàn chỉnh mà bạn có thể trực tiếp thực thi và điều chỉnh cho các trường hợp sử dụng (use cases) phòng chống gian lận của riêng bạn.

### Tổng quan về bộ dữ liệu và nhiệm vụ

Ví dụ này sử dụng bộ dữ liệu phát hiện gian lận IEEE-CIS, chứa 500.000 giao dịch đã được ẩn danh với khoảng 3,5% là các trường hợp gian lận. Bộ dữ liệu bao gồm 392 thuộc tính dạng phân loại (categorical) và dạng số (numerical), với các thuộc tính chính như loại thẻ, loại sản phẩm, địa chỉ, và tên miền email tạo thành cấu trúc đồ thị (graph structure) được hiển thị trong hình sau. Mỗi giao dịch (với một nhãn isFraud) kết nối đến các thực thể Loại thẻ, Vị trí, Loại sản phẩm, và tên miền email của Người mua và Người nhận, tạo ra một đồ thị không đồng nhất (heterogeneous graph) cho phép các mô hình GNN phát hiện các mẫu gian lận thông qua các mối quan hệ giữa các thực thể.

Khác với bài đăng trước của chúng tôi đã minh họa về GraphStorm cùng với [Amazon Neptune Analytics](https://aws.amazon.com/neptune/) cho các luồng công việc phân tích ngoại tuyến (offline), ví dụ này sử dụng cơ sở dữ liệu Neptune làm kho lưu trữ đồ thị OLTP, được tối ưu hóa cho việc trích xuất đồ thị con (subgraph) nhanh chóng cần thiết trong quá trình real-time inference. Dựa trên thiết kế đồ thị, dữ liệu IEEE-CIS dạng bảng được chuyển đổi thành một bộ tệp CSV tương thích với định dạng của cơ sở dữ liệu Neptune, cho phép tải trực tiếp vào cả cơ sở dữ liệu Neptune và quy trình huấn luyện mô hình GNN của GraphStorm chỉ với một bộ tệp duy nhất.

### Bước 0: Thiết lập môi trường

Bước 0 thiết lập môi trường chạy cần thiết cho quy trình (pipeline) phòng chống gian lận bốn bước. Hướng dẫn thiết lập hoàn chỉnh có sẵn trong kho lưu trữ (repository) triển khai.

Để chạy giải pháp ví dụ, bạn cần triển khai một [AWS CloudFormation stack](https://aws.amazon.com/cloudformation/) thông qua AWS CDK. Stack này tạo ra Neptune DB instance, VPC để đặt nó vào, và các roles và security groups phù hợp. Nó còn tạo thêm một SageMaker AI notebook instance, từ đó bạn chạy các notebook ví dụ đi kèm với repository.

```bash
git clone [https://github.com/aws-samples/amazon-neptune-samples.git](https://github.com/aws-samples/amazon-neptune-samples.git)

cd neptune-database-graphstorm-online-inference/neptune-db-cdk

# Ensure you have CDK installed and have appropriate credentials set up

cdk deploy
Khi việc triển khai (deployment) hoàn tất (mất khoảng 10 phút để các tài nguyên cần thiết sẵn sàng), AWS CDK sẽ in ra một vài kết quả đầu ra (outputs), một trong số đó là tên của SageMaker notebook instance mà bạn sử dụng để chạy qua các notebooks:

Bash

# Example output

NeptuneInfraStack.NotebookInstanceName = arn:aws:sagemaker:us-east-1:012345678912:notebook-instance/NeptuneNotebook-9KgSB9XXXXXX
Bạn có thể điều hướng đến giao diện người dùng (UI) của SageMaker AI notebook, tìm notebook instance tương ứng, và chọn liên kết Open Jupyterlab của nó để truy cập notebook.

Một cách khác là, bạn có thể sử dụng AWS Command Line Interface (AWS CLI) để nhận một URL đã được ký trước (pre-signed URL) để truy cập notebook. Bạn sẽ cần thay thế <notebook-instance-name> bằng tên notebook instance thực tế.

Bash

aws sagemaker create-presigned-notebook-instance-url --notebook-instance-name <notebook-instance-name>
Khi bạn đang ở trong giao diện web console của notebook instance, hãy mở notebook đầu tiên, 0-Data-Preparation.ipynb, để bắt đầu đi qua ví dụ.

Bước 1: Xây dựng đồ thị
Trong Notebook 0-Data-Preparation, bạn chuyển đổi bộ dữ liệu IEEE-CIS dạng bảng thành cấu trúc đồ thị không đồng nhất (heterogeneous graph structure) được hiển thị trong hình ở đầu phần này. Jupyter Notebook được cung cấp sẽ trích xuất các thực thể từ các thuộc tính (features) của giao dịch, tạo ra các node Loại thẻ từ các thuộc tính card1–card6, các node Người mua và Người nhận từ các tên miền email, các node Loại sản phẩm từ mã sản phẩm, và các node Vị trí từ thông tin địa lý. Quá trình chuyển đổi này thiết lập các mối quan hệ giữa các giao dịch và những thực thể này, tạo ra dữ liệu đồ thị ở định dạng nhập của Neptune (Neptune import format) để nhập trực tiếp vào kho lưu trữ đồ thị OLTP. Hàm create_neptune_db_data() điều phối quá trình trích xuất thực thể và tạo mối quan hệ này trên tất cả các loại node (mất khoảng 30 giây).

Python

GRAPH_NAME = "ieee-cis-fraud-detection"

PROCESSED_PREFIX = f"./{GRAPH_NAME}"

ID_COLS = "card1,card2,card3,card4,card5,card6,ProductCD,addr1,addr2,P_emaildomain,R_emaildomain"

CAT_COLS = "M1,M2,M3,M4,M5,M6,M7,M8,M9"

# Lists of columns to keep from each file

COLS_TO_KEEP = {

"transaction.csv": (

ID_COLS.split(",")

+ CAT_COLS.split(",")

+

# Numerical features without missing values

[f"C{idx}" for idx in range(1, 15)]

+ ["TransactionID", "TransactionAmt", "TransactionDT", "isFraud"]

),

"identity.csv": ["TransactionID", "DeviceType"],

}

create_neptune_db_data(

data_prefix="./input-data/",

output_prefix=PROCESSED_PREFIX,

id_cols=ID_COLS,

cat_cols=CAT_COLS,

cols_to_keep=COLS_TO_KEEP,

num_chunks=1,

)
Notebook này cũng tạo ra tệp cấu hình JSON cần thiết cho lệnh GConstruct của GraphStorm và thực thi quy trình xây dựng đồ thị. Lệnh GConstruct này chuyển đổi dữ liệu có định dạng Neptune thành một định dạng đồ thị nhị phân phân tán (distributed binary graph format) được tối ưu hóa cho quy trình (pipeline) huấn luyện của GraphStorm, có chức năng phân chia cấu trúc đồ thị không đồng nhất (heterogeneous graph structure) trên các nút tính toán (compute nodes) để cho phép huấn luyện mô hình có khả năng mở rộng trên các đồ thị quy mô công nghiệp (được đo bằng hàng tỷ nodes và edges). Đối với dữ liệu IEEE-CIS, lệnh GConstruct mất 90 giây để hoàn thành.

Trong Notebook 1-Load-Data-Into-Neptune-DB, bạn tải dữ liệu CSV vào Neptune database instance (mất khoảng 9 phút), việc này giúp chúng có sẵn cho online inference. Trong quá trình online inference, sau khi chọn một node giao dịch, bạn truy vấn cơ sở dữ liệu Neptune để lấy vùng lân cận đồ thị (graph neighborhood) của node mục tiêu, truy xuất các thuộc tính (features) của mọi node trong vùng lân cận và cấu trúc đồ thị con (subgraph structure) xung quanh mục tiêu.

Bước 2: Huấn luyện mô hình
Sau khi bạn đã chuyển đổi dữ liệu sang định dạng đồ thị nhị phân phân tán (distributed binary graph format), đã đến lúc huấn luyện một mô hình GNN. GraphStorm cung cấp các kịch bản dòng lệnh (command-line scripts) để huấn luyện một mô hình mà không cần viết mã. Trong Notebook 2-Model-Training, bạn huấn luyện một mô hình GNN sử dụng lệnh phân loại node (node classification) của GraphStorm với cấu hình được quản lý thông qua các tệp YAML. Cấu hình cơ bản xác định một mô hình RGCN hai lớp với các lớp ẩn 128 chiều, huấn luyện trong 4 epochs với learning rate là 0.001 và batch size là 1024, mất khoảng 100 giây cho 1 epoch huấn luyện và đánh giá mô hình trên một instance ml.m5.4xlarge. Để cải thiện độ chính xác của việc phát hiện gian lận, notebook cung cấp các cấu hình mô hình nâng cao hơn như lệnh dưới đây.

Bash

!python -m graphstorm.run.gs_node_classification \

--workspace ./ \

--part-config ieee_gs/ieee-cis.json \

--num-trainers 1 \

--cf ieee_nc.yaml \

--eval-metric roc_auc \

--save-model-path ./model-simple/ \

--topk-model-to-save 1 \

--imbalance-class-weights 0.1,1.0
Các đối số trong lệnh này giải quyết thách thức về sự mất cân bằng nhãn (label imbalance) của bộ dữ liệu, nơi chỉ có 3,5% giao dịch là gian lận, bằng cách sử dụng AUC-ROC làm thước đo đánh giá (evaluation metric) và sử dụng trọng số lớp (class weights). Lệnh này cũng lưu mô hình hoạt động tốt nhất cùng với các tệp cấu hình thiết yếu cần thiết cho việc triển khai endpoint. Các cấu hình nâng cao có thể nâng cao hơn nữa hiệu suất của mô hình thông qua các kỹ thuật như HGT encoders, multi-head attention, và hàm mất mát class-weighted cross entropy, mặc dù những tối ưu hóa này làm tăng yêu cầu tính toán. GraphStorm cho phép thực hiện những thay đổi này thông qua các đối số lúc chạy (run time arguments) và các cấu hình YAML, làm giảm nhu cầu sửa đổi mã.

Bước 3: Triển khai endpoint thời gian thực
Trong Notebook 3-GraphStorm-Endpoint-Deployment, bạn triển khai endpoint thời gian thực thông qua kịch bản khởi chạy (launch script) đơn giản của GraphStorm v0.5. Việc triển khai đòi hỏi ba model artifacts được tạo ra trong quá trình huấn luyện: tệp mô hình đã lưu chứa các trọng số, tệp JSON xây dựng đồ thị đã được cập nhật với metadata biến đổi thuộc tính, và tệp YAML cấu hình huấn luyện được cập nhật lúc chạy. Những artifacts này cho phép GraphStorm tái tạo lại chính xác các cấu hình huấn luyện và mô hình để có hành vi inference nhất quán. Đáng chú ý, tệp JSON xây dựng đồ thị và tệp YAML cấu hình huấn luyện đã được cập nhật chứa các cấu hình quan trọng, thiết yếu để khôi phục mô hình đã được huấn luyện trên endpoint và xử lý các payload yêu cầu đến. Việc sử dụng các tệp JSON và YAML đã được cập nhật cho việc triển khai endpoint là rất quan trọng.

GraphStorm sử dụng tính năng bring your own container (BYOC) của SageMaker AI để triển khai một môi trường inference nhất quán. Bạn cần xây dựng và đẩy (push) Docker image thời gian thực của GraphStorm lên Amazon ECR bằng cách sử dụng các kịch bản shell (shell scripts) được cung cấp. Cách tiếp cận container hóa này cung cấp các môi trường chạy (runtime environments) nhất quán tương thích với cơ sở hạ tầng được quản lý của SageMaker AI. Docker image chứa các phụ thuộc cần thiết cho các khả năng real-time inference của GraphStorm trên môi trường triển khai.

Để triển khai endpoint, bạn có thể sử dụng kịch bản (script) launch_realtime_endpoint.py do GraphStorm cung cấp, giúp bạn thu thập các artifacts cần thiết và tạo ra các tài nguyên SageMaker AI cần thiết để triển khai một endpoint. Script này chấp nhận URI của image trên Amazon ECR, IAM role, đường dẫn đến các model artifact, và cấu hình S3 bucket, tự động xử lý việc cấp phát (provisioning) và cấu hình endpoint. Theo mặc định, script sẽ đợi cho việc triển khai endpoint hoàn tất trước khi thoát. Khi hoàn tất, nó sẽ in ra tên và Khu vực AWS (AWS Region) của endpoint đã được triển khai cho các yêu cầu inference sau đó. Bạn sẽ cần phải thay thế các trường được đặt trong <> bằng các giá trị thực tế của môi trường của bạn.

Bash

!python ~/graphstorm/sagemaker/launch/launch_realtime_endpoint.py \

--image-uri <account_id>.dkr.ecr.<aws_region>[.amazonaws.com/graphstorm:sagemaker-endpoint-cpu](https://.amazonaws.com/graphstorm:sagemaker-endpoint-cpu) \

--role arn:aws:iam::<account_id>:role/<your_role> \

--region <aws_region> \

--restore-model-path <restore-model-path>/models/epoch-1/ \

--model-yaml-config-file <restore-model-path>/models/GRAPHSTORM_RUNTIME_UPDATED_TRAINING_CONFIG.yaml \

--graph-json-config-file <restore-model-path>/models/data_transform_new.json \

--infer-task-type node_classification \

--upload-tarfile-s3 s3://<cdk-created-bucket> \

--model-name ieee-fraud-detect
Bước 4: Real-time inference
Trong Notebook 4-Sample-Graph-and-Invoke-Endpoint, bạn xây dựng một ứng dụng client cơ bản tích hợp với endpoint GraphStorm đã được triển khai để thực hiện phòng chống gian lận thời gian thực trên các giao dịch đến. Quy trình inference chấp nhận dữ liệu giao dịch thông qua các payload JSON được tiêu chuẩn hóa, thực hiện các dự đoán phân loại node (node classification) trong vài trăm mili giây, và trả về điểm xác suất gian lận cho phép đưa ra quyết định ngay lập tức.

Một lời gọi inference end-to-end cho một node đã tồn tại trong đồ thị có ba giai đoạn riêng biệt:

Lấy mẫu đồ thị (Graph sampling) từ cơ sở dữ liệu Neptune. Đối với một node mục tiêu đã cho đã tồn tại trong đồ thị, truy xuất vùng lân cận k-hop của nó với một giới hạn fanout, tức là giới hạn số lượng hàng xóm được truy xuất ở mỗi hop bằng một ngưỡng.

Chuẩn bị payload cho inference. Neptune trả về các đồ thị bằng GraphSON, một định dạng dữ liệu giống JSON chuyên dụng được sử dụng để mô tả dữ liệu đồ thị. Ở bước này, bạn cần chuyển đổi GraphSON được trả về sang đặc tả JSON của riêng GraphStorm. Bước này được thực hiện trên client thực hiện inference, trong trường hợp này là một SageMaker notebook instance.

Thực hiện Model inference bằng một SageMaker endpoint. Sau khi payload được chuẩn bị, bạn gửi một yêu cầu inference đến một SageMaker endpoint đã tải một model snapshot đã được huấn luyện trước đó. Endpoint nhận yêu cầu, thực hiện bất kỳ phép biến đổi thuộc tính nào cần thiết (chẳng hạn như chuyển đổi các thuộc tính dạng phân loại (categorical) sang mã hóa one-hot (one-hot encoding)), tạo biểu diễn đồ thị nhị phân trong bộ nhớ, và đưa ra dự đoán cho node mục tiêu bằng cách sử dụng vùng lân cận đồ thị và các trọng số mô hình đã được huấn luyện. Phản hồi được mã hóa thành JSON và gửi lại cho client.

Một phản hồi ví dụ từ endpoint sẽ trông như sau:

JSON

{
"status_code": 200,

"request_uid": "877042dbc361fc33",

"message": "Request processed successfully.",

"error": "",

"data": {

"results": [

{

"node_type": "Transaction",

"node_id": "2991260",

"prediction": [0.995966911315918, 0.004033133387565613]

}

]

}

}
Dữ liệu quan tâm cho giao dịch đơn lẻ mà bạn đã dự đoán nằm trong khóa (key) prediction và node_id. tương ứng. Kết quả dự đoán cung cấp cho bạn điểm số thô (raw scores) mà mô hình tạo ra cho lớp 0 (hợp lệ) và lớp 1 (gian lận) tại các chỉ số (indexes) 0 và 1 tương ứng của danh sách predictions. Trong ví dụ này, mô hình đánh dấu giao dịch này là có khả năng cao là hợp lệ. Bạn có thể tìm thấy đặc tả phản hồi đầy đủ của GraphStorm trong tài liệu của GraphStorm.

Các ví dụ triển khai hoàn chỉnh, bao gồm mã client và các đặc tả payload, được cung cấp trong kho lưu trữ (repository) để hướng dẫn việc tích hợp với các hệ thống production.

Dọn dẹp
Để ngừng phát sinh chi phí trên tài khoản của bạn, bạn cần xóa các tài nguyên AWS mà bạn đã tạo bằng AWS CDK tại bước Thiết lập Môi trường (Environment Setup).

Trước tiên, bạn phải xóa SageMaker endpoint được tạo trong Bước 3 để lệnh cdk destroy có thể hoàn thành. Hãy xem mục Delete Endpoints and Resources để biết thêm các tùy chọn xóa một endpoint. Khi xong, bạn có thể chạy lệnh sau từ thư mục gốc của repository:

Bash

cd neptune-database-graphstorm-online-inference/neptune-db-cdk

cdk destroy
Xem tài liệu của AWS CDK để biết thêm thông tin về cách sử dụng cdk destroy, hoặc xem tài liệu của CloudFormation để biết cách xóa một stack từ giao diện người dùng console (console UI). Theo mặc định, lệnh cdk destroy không xóa các model artifacts và dữ liệu đồ thị đã xử lý được lưu trữ trong S3 bucket trong quá trình huấn luyện và triển khai. Bạn phải xóa chúng theo cách thủ công. Hãy xem mục Deleting a general purpose bucket để biết thông tin về cách làm trống và xóa một S3 bucket mà AWS CDK đã tạo.

Kết luận
Graph neural networks giải quyết các thách thức phòng chống gian lận phức tạp bằng cách mô hình hóa các mối quan hệ giữa các thực thể mà các phương pháp machine learning truyền thống bỏ lỡ khi phân tích các giao dịch một cách riêng lẻ. GraphStorm v0.5 giúp đơn giản hóa việc triển khai real-time inference của GNN với một lệnh duy nhất để tạo endpoint, việc mà trước đây đòi hỏi sự phối hợp của nhiều dịch vụ, và một đặc tả payload được tiêu chuẩn hóa giúp đơn giản hóa việc tích hợp của client với các dịch vụ real-time inference. Các tổ chức giờ đây có thể triển khai các endpoint phòng chống gian lận quy mô doanh nghiệp thông qua các lệnh được tinh giản, giúp giảm công sức kỹ thuật tùy chỉnh từ hàng tuần xuống còn các thao tác một lệnh duy nhất.

Để triển khai phòng chống gian lận dựa trên GNN với dữ liệu của riêng bạn:

Xem lại tài liệu của GraphStorm để biết các tùy chọn cấu hình mô hình và các đặc tả triển khai.

Điều chỉnh ví dụ IEEE-CIS này cho bộ dữ liệu phòng chống gian lận của bạn bằng cách sửa đổi các bước xây dựng đồ thị và kỹ thuật thuộc tính (feature engineering), sử dụng mã nguồn và các bài hướng dẫn hoàn chỉnh có sẵn trong kho lưu trữ GitHub của chúng tôi.

Truy cập hướng dẫn triển khai từng bước để xây dựng các giải pháp phòng chống gian lận sẵn sàng cho production với các khả năng nâng cao của GraphStorm v0.5 bằng cách sử dụng dữ liệu doanh nghiệp của bạn.

Author Jian Zhang — Nhà khoa học ứng dụng cấp cao, người đã sử dụng các kỹ thuật machine learning để giúp khách hàng giải quyết nhiều vấn đề khác nhau, chẳng hạn như phát hiện gian lận, tạo ảnh trang trí, và nhiều hơn nữa. Ông đã phát triển thành công các giải pháp machine learning dựa trên đồ thị, đặc biệt là graph neural network, cho các khách hàng ở Trung Quốc, Hoa Kỳ, và Singapore. Với vai trò là người truyền bá kiến thức về các khả năng đồ thị của AWS, Zhang đã có nhiều bài thuyết trình trước công chúng về GraphStorm, GNN, Deep Graph Library (DGL), Amazon Neptune, và các dịch vụ AWS khác.

Author Theodore Vasiloudis — Nhà khoa học ứng dụng cấp cao tại AWS, nơi ông làm việc về các hệ thống và thuật toán machine learning phân tán. Ông đã lãnh đạo việc phát triển GraphStorm Processing, thư viện xử lý đồ thị phân tán cho GraphStorm và là một nhà phát triển cốt lõi cho GraphStorm. Ông nhận bằng Tiến sĩ Khoa học Máy tính từ Viện Công nghệ Hoàng gia KTH, Stockholm, vào năm 2019.

Author Xiang Song — Nhà khoa học ứng dụng cấp cao tại AWS AI Research and Education (AIRE), nơi ông phát triển các framework deep learning bao gồm GraphStorm, DGL, và DGL-KE. Ông đã lãnh đạo việc phát triển Amazon Neptune ML, một khả năng mới của Neptune sử dụng graph neural networks cho các đồ thị được lưu trữ trong cơ sở dữ liệu đồ thị. Hiện ông đang lãnh đạo việc phát triển GraphStorm, một framework machine learning đồ thị nguồn mở cho các trường hợp sử dụng doanh nghiệp. Ông nhận bằng Tiến sĩ về hệ thống và kiến trúc máy tính tại Đại học Fudan, Thượng Hải, vào năm 2014.

Author Florian Saupe — Giám đốc Sản phẩm Kỹ thuật Cấp cao tại bộ phận nghiên cứu AI/ML của AWS, hỗ trợ các nhóm khoa học như nhóm machine learning đồ thị, và các nhóm Hệ thống ML làm việc về huấn luyện phân tán quy mô lớn, inference, và khả năng chịu lỗi. Trước khi gia nhập AWS, Florian đã lãnh đạo mảng quản lý sản phẩm kỹ thuật cho lĩnh vực lái xe tự động tại Bosch, là một nhà tư vấn chiến lược tại McKinsey & Company, và đã làm việc như một nhà khoa học về hệ thống điều khiển và robot—một lĩnh vực mà ông có bằng Tiến sĩ.

Author Ozan Eken — Giám đốc Sản phẩm tại AWS, đam mê xây dựng các sản phẩm Generative AI và Graph Analytics tiên tiến. Với việc tập trung vào đơn giản hóa các thách thức dữ liệu phức tạp, Ozan giúp khách hàng khai phá những hiểu biết sâu sắc hơn và thúc đẩy sự đổi mới. Ngoài công việc, anh ấy thích thử các món ăn mới, khám phá các quốc gia khác nhau, và xem bóng đá.
```
