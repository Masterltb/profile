---
title: "Blog 3"
date: 2025-09-30
weight: 3
chapter: false
pre: " <b> 3.3. </b> "
---

# Thông báo ra mắt Amazon ECS Managed Instances cho các ứng dụng được container hóa

Hôm nay, chúng tôi công bố Amazon ECS Managed Instances, một tùy chọn tính toán (compute option) mới cho [Amazon Elastic Container Service (Amazon ECS)](https://aws.amazon.com/ecs/) cho phép các nhà phát triển sử dụng toàn bộ các khả năng của [Amazon Elastic Compute Cloud (Amazon EC2)](https://aws.amazon.com/ec2) đồng thời chuyển giao trách nhiệm quản lý cơ sở hạ tầng cho [Amazon Web Service](https://aws.amazon.com/) (AWS). Sản phẩm mới này kết hợp sự đơn giản trong vận hành của việc chuyển giao cơ sở hạ tầng với sự linh hoạt và khả năng kiểm soát của Amazon EC2, điều này có nghĩa là khách hàng có thể tập trung vào việc xây dựng các ứng dụng thúc đẩy sự đổi mới, đồng thời giảm tổng chi phí sở hữu (TCO) và duy trì các thông lệ tốt nhất của AWS (AWS best practices).

Amazon ECS Managed Instances cung cấp một môi trường tính toán container (container compute environment) được quản lý hoàn toàn, hỗ trợ nhiều loại instance EC2 và tích hợp sâu với các dịch vụ AWS. Theo mặc định, nó tự động chọn các instance EC2 được tối ưu hóa chi phí nhất cho các workloads của bạn, nhưng bạn có thể chỉ định các thuộc tính hoặc loại instance cụ thể khi cần. AWS xử lý mọi khía cạnh của việc quản lý cơ sở hạ tầng, bao gồm việc cấp phát (provisioning), mở rộng quy mô (scaling), vá lỗi bảo mật (security patching), và tối ưu hóa chi phí, giúp bạn có thể tập trung vào việc xây dựng và vận hành các ứng dụng của mình.

## Hãy dùng thử nào

Nhìn vào trải nghiệm trên [AWS Management Console](https://aws.amazon.com/console/) để tạo một cluster Amazon ECS mới, tôi có thể thấy tùy chọn mới để sử dụng ECS Managed Instances. Hãy cùng xem qua một lượt tất cả các tùy chọn mới.

Sau khi tôi đã chọn Fargate và Managed Instances, tôi được cung cấp hai tùy chọn. Nếu tôi chọn **Use ECS default**, Amazon ECS sẽ chọn các loại instance đa dụng (general purpose) dựa trên việc nhóm các Task đang chờ xử lý lại với nhau, và chọn loại instance tối ưu dựa trên các chỉ số về chi phí và khả năng phục hồi (resilience). Đây là cách đơn giản nhất và được khuyến nghị để bắt đầu. Việc chọn **Use custom – advanced** sẽ mở ra các tham số cấu hình bổ sung, nơi tôi có thể tinh chỉnh các thuộc tính của các instance mà Amazon ECS sẽ sử dụng.

Theo mặc định, tôi thấy CPU và Bộ nhớ (Memory) là các thuộc tính, nhưng tôi có thể chọn từ 20 thuộc tính bổ sung để tiếp tục lọc danh sách các loại instance có sẵn mà Amazon ECS có thể truy cập.

Sau khi tôi đã thực hiện các lựa chọn thuộc tính của mình, tôi thấy danh sách tất cả các loại instance phù hợp với các lựa chọn đó.

Từ đây, tôi có thể tạo cluster ECS của mình như bình thường và Amazon ECS sẽ thay mặt tôi cấp phát (provision) các instance dựa trên các thuộc tính và tiêu chí mà tôi đã xác định trong các bước trước đó.

## Các tính năng chính của Amazon ECS Managed Instances

Với Amazon ECS Managed Instances, AWS chịu hoàn toàn trách nhiệm về việc quản lý cơ sở hạ tầng, xử lý mọi khía cạnh của việc cấp phát (provisioning), mở rộng quy mô (scaling), và bảo trì (maintenance) instance. Điều này bao gồm việc triển khai các bản vá bảo mật (security patches) định kỳ được bắt đầu mỗi 14 ngày (do việc rút cạn kết nối của instance (instance connection draining), vòng đời thực tế của instance có thể dài hơn), với khả năng lên lịch cho các cửa sổ bảo trì (maintenance windows) sử dụng các cửa sổ sự kiện của Amazon EC2 (Amazon EC2 event windows) để giảm thiểu sự gián đoạn cho các ứng dụng của bạn.

Dịch vụ này cung cấp sự linh hoạt vượt trội trong việc lựa chọn loại instance. Mặc dù theo mặc định, nó tự động chọn các loại instance được tối ưu hóa chi phí, bạn vẫn duy trì quyền chỉ định các thuộc tính instance mong muốn khi các workloads của bạn yêu cầu các khả năng cụ thể. Điều này bao gồm các tùy chọn cho việc tăng tốc GPU, kiến trúc CPU, và các yêu cầu về hiệu năng mạng, mang lại cho bạn sự kiểm soát chính xác đối với môi trường tính toán (compute environment) của mình.

Để giúp tối ưu hóa chi phí, Amazon ECS Managed Instances quản lý việc sử dụng tài nguyên một cách thông minh bằng cách tự động đặt nhiều task trên các instance lớn hơn khi thích hợp. Dịch vụ này liên tục theo dõi và tối ưu hóa việc sắp xếp task, dồn các workloads vào ít instance hơn để rút cạn, sử dụng và chấm dứt các instance không hoạt động (rỗng), cung cấp cả tính sẵn sàng cao (high availability) và hiệu quả về chi phí cho các ứng dụng được container hóa của bạn.

Việc tích hợp với các dịch vụ AWS hiện có là liền mạch, đặc biệt là với các tính năng của Amazon EC2 như các tùy chọn định giá EC2 (EC2 pricing options). Sự tích hợp sâu này có nghĩa là bạn có thể tối đa hóa các khoản đầu tư vào dung lượng hiện có trong khi vẫn duy trì sự đơn giản trong vận hành của một dịch vụ được quản lý hoàn toàn.

Bảo mật vẫn là ưu tiên hàng đầu với Amazon ECS Managed Instances. Dịch vụ này chạy trên Bottlerocket, một hệ điều hành container được xây dựng chuyên dụng, và duy trì tình trạng bảo mật của bạn thông qua các bản vá bảo mật và cập nhật tự động. Bạn có thể xem tất cả các bản cập nhật và vá lỗi được áp dụng cho image hệ điều hành Bottlerocket trên [trang web của Bottlerocket](https://bottlerocket.dev/en/os/). Cách tiếp cận toàn diện về bảo mật này giữ cho các ứng dụng được container hóa của bạn luôn hoạt động trong một môi trường an toàn, được bảo trì.

## Hiện đã có mặt

Amazon ECS Managed Instances hiện đã có mặt tại các Khu vực AWS (AWS Regions) sau: US East (North Virginia), US West (Oregon), Europe (Ireland), Africa (Cape Town), Asia Pacific (Singapore), và Asia Pacific (Tokyo). Bạn có thể bắt đầu sử dụng Managed Instances thông qua AWS Management Console, AWS Command Line Interface (AWS CLI), hoặc các công cụ cơ sở hạ tầng dưới dạng mã (infrastructure as code - IaC) như AWS Cloud Development Kit (AWS CDK) và AWS CloudFormation. Bạn trả tiền cho các instance EC2 mà bạn sử dụng cộng với một khoản phí quản lý cho dịch vụ.

Để tìm hiểu thêm về Amazon ECS Managed Instances, hãy truy cập [tài liệu](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/ManagedInstances.html) và bắt đầu đơn giản hóa cơ sở hạ tầng container của bạn ngay hôm nay.

---

**Author**
**Micah Walter** — Micah Walter là một Kiến trúc sư giải pháp cấp cao (Sr. Solutions Architect) hỗ trợ các khách hàng doanh nghiệp tại khu vực Thành phố New York và xa hơn nữa. Anh ấy tư vấn cho các giám đốc điều hành, kỹ sư, và kiến trúc sư trong mỗi bước trên hành trình lên đám mây (cloud) của họ, với sự tập trung sâu sắc vào tính bền vững và thiết kế thực tiễn. Trong thời gian rảnh, Micah thích các hoạt động ngoài trời, nhiếp ảnh, và đuổi bắt cùng các con quanh nhà.
