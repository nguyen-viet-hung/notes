# Cơ bản về Hadoop
MapReduce 5
hadoop 11
hardcore 18

nguyenduyhao1111 viết ngày 04/10/2016
## Hadoop là cái gì vậy?

“Hadoop là một framework nguồn mở viết bằng Java cho phép phát triển các ứng dụng phân tán có cường độ dữ liệu lớn một cách miễn phí. Nó cho phép các ứng dụng có thể làm việc với hàng ngàn node khác nhau và hàng petabyte dữ liệu. Hadoop lấy được phát triển dựa trên ý tưởng từ các công bố của Google về mô hình MapReduce và hệ thống file phân tán Google File System (GFS).”
Như vậy mô hình lập trình Map Reduce là nền tảng ý tưởng của Hadoop. Bản thân Hadoop là một framework cho phép phát triển các ứng dụng phân tán phần cứng thông thường . Các phần cứng này thường có khả năng hỏng hóc cao. Khác với loại phần cứng chuyên dụng đắt tiền, khả năng xảy ra lỗi thấp như các supermicrocomputer chẳng hạn.
Hadoop viết bằng Java. Tuy nhiên, nhờ cơ chế streaming, Hadoop cho phép phát triển các ứng dụng phân tán bằng cả java lẫn một số ngôn ngữ lập trình khác như C++, Python, Pearl.
Cấu trúc Hadoop gồm những gì vậy?

## Để biết về Hadoop ta làm quen các khái niệm sau:

* Core: cung cấp các công cụ và giao diện cho hệ thống phân tán và các tiện ích I/O. Đây là phần lõi để xây dựng nên HDFS và MapReduce.
* MapReduce (MapReduce Engine): một framework giúp phát triển các ứng dụng phân tán theo mô hình MapReduce một cách dễ dàng và mạnh mẽ, ứng dụng phân tán MapReduce có thể chạy trên một cluster lớn với nhiều node.
* HDFS: hệ thống file phân tán, cung cấp khả năng lưu trữ dữ liệu khổng lồ và tính năng tối ưu hoá việc sử dụng băng thông giữa các node. HDFS có thể được sử dụng để chạy trên một cluster lớn với hàng chục ngàn node.
* HBase: một cơ sở dữ liệu phân tán, theo hướng cột (colunm-oriented). HBase sử dụng HDFS làm hạ tầng cho việc lưu trữ dữ liệu bên dưới và cung cấp khả năng tính toán song song dựa trên MapReduce.
* Hive: một data warehouse phân tán. Hive quản lý dữ liệu được lưu trữ trên HDFS và cung cấp một ngôn ngữ truy vấn dựa trên SQL.
* Chukwa: một hệ thống tập hợp và phân tích dữ liệu. Chukwa chạy các collector (các chương trình tập hợp dữ liệu), các collector này lưu trữ dữ liệu trên HDFS và sử dụng MapReduce để phát sinh các báo cáo.

## Bản thân Hadoop vận hành ra sao?

Bao gồm MapReduce job chia các task, tổng hợp chúng và các trình nền daemon quản lý bộ nhớ, quản lý đọc ghi file, gíám sát trạng thái HDFS.

### MapReduce job

là một đơn vị của công việc mà client muốn được thực hiện: nó bao gồm dữ liệu đầu vào, chương trình MapReduce, và thông tin cấu hình. Hadoop chạy các công việc (job) này bằng cách chia nó thành các nhiệm vụ (task), trong đó có hai kiểu chính là: các bản đồ nhiệm vụ (map task) và các nhiệm vụ thu gọn (reduce task).

Có hai loại node điều kiển quá trình thực hiện công việc (job): một jobtracker và một số tasktracker. Jobtracker kết hợp tất cả các công việc trên hệ thống bằng cách lập lịch công việc chạy trên các tasktracker. Tasktracker chạy các nhiệm vụ (task) và gửi báo cáo thực hiện cho jobtracker, cái lưu giữ các bản ghi về quá trình xử lý tổng thể cho mỗi công việc (job).
Hadoop chia đầu vào cho mỗi công việc MapReduce vào các mảnh (piece) có kích thước cố định gọi là các input split hoặc là các split. Hadoop tạo ra một task map cho mỗi split, cái chạy mỗi nhiệm vụ map do người sử dụng định nghĩa cho mỗi bản ghi (record) trong split.
Có rất nhiều các split , điều này có nghĩa là thời gian xử lý mỗi split nhỏ hơn so với thời gian xử lý toàn bộ đầu vào. Vì vậy, nếu chúng ta xử lý các split một cách song song, thì quá trình xử lý sẽ tốt hơn cân bằng tải, nếu các split nhỏ, khi đó một chiếc máy tính nhanh có thể xử lý tương đương nhiều split trong quá trình thực hiện công việc hơn là một máy tính chậm. Ngay cả khi các máy tính giống hệt nhau, việc xử lý không thành công hay các công việc khác đang chạy đồng thời làm cho cần bằng tải như mong muốn, và chất lượng của cân bằng tải tăng như là chia các splits thành các hạt mịn hơn.
Mặt khác, nếu chia tách quá nhỏ, sau đó chi phí cho việc quản lý các split và của tạo ra các map task bắt đầu chiếm rất nhiều tổng thời gian của quá trình xử lý công việc. Đối với hầu hết công việc, kích thước split tốt nhất thường là kích thước của một block của HDFS, mặc định là 64MB, mặc dù nó có thể thay đổi được cho mỗi cluster ( cho tất cả các file mới được tạo ra) hoặc định rõ khi mỗi file được tạo ra....

Hadoop làm tốt nhất các công việc của nó chạy các map task trên một node khi mà dữ liệu đầu vào của nó cư trú ngay trong HDFS. Nó được gọi là tối ưu hóa dữ liệu địa phương. Bây giờ chúng ta sẽ làm rõ tại sao kích thước split tối ưu lại bằng kích thước của block: nó là kích thước lớn nhất của một đầu vào mà có thể được đảm bảo để được lưu trên một node đơn. Nếu split được chia thành 2 block, nó sẽ không chắc là bất cứ node HDFS nào lưu trữ cả hai block, vì vậy một số split phải được chuyển trên mạng đến node chạy map tast, như vậy rõ ràng là sẽ ít hiệu quả hơn việc chạy toàn bộ map task sử dụng dữ liệu cục bộ.

Các map task ghi đầu ra của chúng trên đĩa cục bộ, không phải là vào HDFS. Tại sao lại như vậy? Đầu ra của map là đầu ra trung gian, nó được xử lý bởi reduce task để tạo ra đầu ra cuối cùng , và một khi công việc được hoàn thành đầu ra của map có thể được bỏ đi. Vì vậy việc lưu trữ nó trong HDFS, với các nhân bản, là không cần thiết. Nếu các node chạy maptask bị lỗi trước khi đầu ra map đã được sử dụng bởi một reduce task, khi đó Hadoop sẽ tự động chạy lại map task trên một node khác để tạo ra một đầu ra map.

Khi “chạy Hadoop” có nghĩa là chạy một tập các trình nền - daemon, hoặc các chương trình thường trú, trên các máy chủ khác nhau trên mạng của bạn. Những trình nền có vai trò cụ thể, một số chỉ tồn tại trên một máy chủ, một số có thể tồn tại trên nhiều máy chủ.

Các daemon bao gồm:

* NameNode
* DataNode
* SecondaryNameNode
* JobTracker
* TaskTracker

### NameNode

Là một trình nền quan trọng nhất của Hadoop - các NameNode. Hadoop sử dụng một kiển trúc master/slave cho cả lưu trữ phân tán và xử lý phân tán. Hệ thống lưu trữ phân tán được gọi là Hadoop File System hay HDFS. NameNode là master của HDFS để chỉ đạo các trình nền DataNode slave để thực hiện các nhiệm vụ I/O mức thấp. NameNode theo dõi HDFS, cách các tập tin của bạn được phân chia thành các block, những node nào lưu các khối đó, và “kiểm tra sức khỏe” tổng thể của hệ thống tệp phân tán.
Chức năng của NameNode là nhớ (memory) và I/O chuyên sâu. Như vậy, máy
chủ lưu trữ NameNode thường không lưu trữ bất cứ dữ liệu người dùng hoặc thực hiện bất cứ một tính toán nào cho một ứng dụng MapReduce để giảm khổi lượng công việc trên máy. Điều này có nghĩa là máy chủ NameNode không gấp đôi (double) như là DataNode hay một TaskTracker.

Có điều đáng tiếc là có một khía cạnh tiêu cực đến tầm quan trọng của NameNode nó có một điểm của thất bại của một cụm Hadoop của bạn. Đối với bất cứ một trình nền khác, nếu các nút máy của chúng bị hỏng vì lý do phần mềm hay phần cứng, các Hadoop cluster có thể tiếp tục hoạt động thông suốt hoặc bạn có thể khởi động nó một cách nhanh chóng. Nhưng không thể áp dụng cho các NameNode.

### DataNode

Mỗi máy slave trong cluster của bạn sẽ lưu trữ (host) một trình nền DataNode
để thực hiện các công việc nào đó của hệ thống file phân tán - đọc và ghi các khối HDFS tới các file thực tế trên hệ thống file cục bộ (local filesytem). Khi bạn muốn đọc hay ghi một file HDFS, file đó được chia nhỏ thành các khối và NameNode sẽ nói cho các client của bạn nơi các khối trình nền DataNode sẽ nằm trong đó. Client của bạn liên lạc trực tiếp với các trình nền DataNode để xử lý các file cục bộ tương ứng với các block. Hơn nữa, một DataNode có thể giao tiếp với các DataNode khác để nhân bản các khối dữ liệu của nó để dự phòng.
Các DataNode thường xuyên báo cáo với các NameNode. Sau khi khởi tạo, mỗi DataNode thông báo với NameNode của các khối mà nó hiện đang lưu trữ. Sau khi Mapping hoàn thành, các DataNode tiếp tục thăm dò ý kiến NameNode để cung cấp thông tin về thay đổi cục bộ cũng như nhận được hướng dẫn để tạo, di chuyển hoặc xóa các blocks từ đĩa địa phương (local).

### Secondary NameNode

Các Secondary NameNode (SNN) là một trình nền hỗ trợ giám sát trạng thái của các cụm HDFS. Giống như NameNode, mỗi cụm có một SNN, và nó thường trú trên một máy của mình. Không có các trình nền DataNode hay TaskTracker chạy trên cùng một server. SNN khác với NameNode trong quá trình xử lý của nó không nhận hoặc ghi lại bất cứ thay đổi thời gian thực tới HDFS. Thay vào đó, nó giao tiếp với các NameNode bằng cách chụp những bức ảnh của siêu dữ liệu HDFS (HDFS metadata) tại những khoảng xác định bởi cấu hình của các cluster.

Như đã đề cập trước đó, NameNode là một điểm truy cập duy nhất của lỗi (failure) cho một cụm Hadoop, và các bức ảnh chụp SNN giúp giảm thiểu thời gian ngừng (downtime) và mất dữ liệu. Tuy nhiên, một NameNode không đòi hỏi sự can thiệp của con người để cấu hình lại các cluster sẻ dùng SSN như là NameNode chính.

### jobTracker

Trình nền JobTracker là một liên lạc giữa ứng dụng của bạn và Hadoop. Một khi bạn gửi mã nguồn của bạn tới các cụm (cluster), JobTracker sẽ quyết định kế hoạch thực hiện bằng cách xác định những tập tin nào sẽ xử lý, các nút được giao các nhiệm vụ khác nhau, và theo dõi tất cả các nhiệm vụ khi chúng đang chạy. Nếu một nhiệm vụ (task) thất bại (fail), JobTracker sẽ tự động chạy lại nhiệm vụ đó, có thể trên một node khác, cho đến một giới hạn nào đó được định sẵn của việc thử lại này.

Chỉ có một JobTracker trên một cụm Hadoop. Nó thường chạy trên một máy chủ như là một nút master của cluster.
### TaskTraker

Như với các trình nền lưu trữ, các trình nền tính toán cũng phải tuân theo kiến trúc master/slave: JobTracker là giám sát tổng việc thực hiện chung của một công việc MapRecude và các taskTracker quản lý việc thực hiện các nhiệm vụ riêng trên mỗi node slave.
Mỗi TaskTracker chịu trách nhiệm thực hiện các task riêng mà các JobTracker giao cho. Mặc dù có một TaskTracker duy nhất cho một node slave, mỗi TaskTracker có thể sinh ra nhiều JVM để xử lý các nhiệm vụ Map hoặc Reduce song song.

Một trong những trách nhiệm của các TaskTracker là liên tục liên lạc với JobTracker. Nếu JobTracker không nhận được nhịp đập từ một TaskTracker trong vòng một lượng thời gian đã quy định, nó sẽ cho rằng TaskTracker đã bị treo (cashed) và sẽ gửi lại nhiệm vụ tương ứng cho các nút khác trong cluster.

Cấu trúc liên kết này có một node Master là trình nền NameNode và JobTracker và một node đơn với SNN trong trường hợp node Master bị lỗi. Đối với các cụm nhở, thì SNN có thể thường chú trong một node slave. Mặt khác, đối với các cụm lớn, phân tách NameNode và JobTracker thành hai máy riêng. Các máy slave, mỗi máy chỉ lưu trữ một DataNode và Tasktracker, để chạy các nhiệm vụ trên cùng một node nơi lưu dữ liệu của chúng

(Bài viết khá dài mong bạn thông cảm - Bài sau nói về HDFS)
Bài viết có tham khảo từ nhiều nguồn
