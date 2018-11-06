

[Source](https://kipalog.com/posts/Bat-dau-voi-Machine-Learning-thong-qua-Tensorflow--Phan-I-2 "Permalink to Bắt đầu với Machine Learning thông qua Tensorflow (Phần I.2)")

# Bắt đầu với Machine Learning thông qua Tensorflow (Phần I.2)

#### TL;DR

Qua bài viết trước, chúng ta đã biết được đến sự tồn tại của một vài khái niệm cơ bản trong Machine Learning. Tiếp theo chúng ta sẽ đi sâu vào cách để giải những bài toán này. Cũng như mình đã nói trong bài viết trước. Machine Learning sử dụng rất nhiều đến kiến thức toán cũng như xác suất thống kê. Việc này làm cho việc tiếp cận với Machine Learning trở nên không được phổ thông. Chính vì vậy, mình muốn giới thiệu đến các bạn những bộ công cụ hỗ trợ việc thực hành Machine Learning. Mình không nói rằng toán trở nên bớt quan trọng nhưng theo quan điểm của mình, việc làm đơn giản hóa một vài phần để học từng bước từng bước một thì việc học sẽ trở nên bớt đau đớn hơn.

Hiện tại với công việc của mình, vì lí do dân tộc mà người Nhật họ yêu cầu bắt buộc là phải dùng _Chainer_ [[Official site]][1]. (_Hay ít nhất là công ty mình yêu cầu phải dùng_) Thư viện này có tốc độ training và predict khá nhanh. Hỗ trợ _graph_ (Biểu đồ) rất trực quan. Tuy nhiên cấu trúc API cá nhân mình thấy là trúc trắc, cộng đồng nhỏ, hẹp lại thiên về hướng Nhật ngữ nên không phù hợp để làm giảm đau đớn trong quá trình học.

Thư viện được mình lựa chọn là Tensorflow [[Official site]][2]. Đây là một thư viện có năng lực không hề thua kém Chainer, cấu trúc API rõ ràng hơn cộng với sử dụng rất nhiều kĩ thuật trong Machine Learning nên rất phù hợp trong quá trình học ở mức cơ bản.  
Trong bài viết này sẽ giới thiệu Tensorflow là gì và thực hành một bài toán nhập môn với Tensorflow.

## Tensorflow là gì?

Theo định nghĩa của Google thì:

> TensorFlow™ is an open source software library for numerical computation using data flow graphs. Nodes in the graph represent mathematical operations, while the graph edges represent the multidimensional data arrays (tensors) communicated between them. The flexible architecture allows you to deploy computation to one or more CPUs or GPUs in a desktop, server, or mobile device with a single API. TensorFlow was originally developed by researchers and engineers working on the Google Brain Team within Google's Machine Intelligence research organization for the purposes of conducting machine learning and deep neural networks research, but the system is general enough to be applicable in a wide variety of other domains as well.

Quá dài và quá khó hiểu. Nếu hiểu theo cách của mình thì Tensorflow là một thư viện mã nguồn mở cung cấp khả năng xử lí tính toán số học dựa trên biểu đồ mô tả sự thay đổi của dữ liệu. Tensor được sử dụng khi bạn cần giải quyết các bài toán supervised learning.

![](https://s3-ap-southeast-1.amazonaws.com/kipalog.com/i1ucnftq6_image.png)  
Hình 1.1: Ví dụ về một graph trong Tensorflow

### Khái niệm cơ bản trong Tensorflow

Khi thực hành với Tensorflow, sẽ có rất nhiều khái niệm phức tạp. Tuy nhiên chỉ ở mức cơ bản, chúng ta sẽ đi đến khái niệm quan trọng nhất trong Tensorflow là **Tensor** [[Tensor]][3].

#### Node

Vì Tensorflow mô tả lại dòng chảy của dữ liệu thông qua graph nên mỗi một điểm giao cắt trong graph thì được gọi là Node. Tại sao điều này quan trọng thì là vì các Node chính là điểm đại diện cho việc thay đổi của dữ liệu nên việc lưu trữ lại tham chiếu của các Node này là rất quan trọng.

#### Tensor

Như trong bài viết trước mình có đề cập, để giải được các bài toán Machine Learning, cần phải làm cho máy tính có thể hiểu được dữ liệu của tập nguồn và dữ liệu của tập đích. Tensorflow cung cấp một loại dữ liệu mới được gọi là _Tensor_ [[Tenensorflow]][4]. Trong thế giới của Tensorflow, mọi kiểu dữ liệu đều được quy về một mối được gọi là Tensor hay trong Tensorflow, tất cả các loại dữ liệu đều là Tensor. Vậy nên có thể hiểu được phần nào cái tên Tensorflow là một thư viện mô tả, điều chỉnh dòng chảy của các Tensor.

Tensor là một kiểu dữ liệu dạng mảng có nhiều chiều được mô tả dạng `Tensor = [[[1,1,1],[178,62,74]],[[45,2,2],[19,0,17]],[[7,5,2],[0,11,4]],[[8,13,5],[1,6,7]]]`. Mảng nhiều chiều này được đính kèm thêm một vài thuộc tính tham chiếu khác. Các thuộc tính của Tensor được mô tả trong tài liệu bao gồm:

* **device**: Tên của thiết bị mà Tensor hiện tại sẽ được xuất bản. Có thể None.
* **graph**: Đồ thị chứa Tensor hiện tại.
* **name**: Tên của Tensor hiện tại.
* **shape**: Trả về TensorShape mô tả lại Shape của Tensor hiện tại.
* **op**: _Operation_(Toán tử / Phép toán) được sử dụng để xuất bản Tensor hiện tại.
* **dtype**: Kiểu của các _elements_(Phần tử) trong Tensor hiện tại.

##### Rank

Rank là bậc hay độ sâu của một Tensor. Ví dụ như `Tensor = [1]` sẽ có rank là 1, `Tensor = [[[1,1,1],[178,62,74]]]` sẽ có rank bằng 3, `Tensor = [[1,1,1],[178,62,74]]` sẽ có rank bằng 2. Cách nhanh nhất để xác định rank của một Tensor là đếm số lần mở ngoặc vuông cho đến giá trị khác ngoặc vuông đầu tiên. Việc phân rank này khá quan trọng vì nó đồng thời cũng giúp phân loại dữ liệu của Tensor. Khi ở cách rank đặc biệt cụ thể, Tensor có những tên gọi riêng như sau:

* **Scalar**: Khi Tensor có rank bằng 0, Tensor đại diện cho một số hoặc một chuỗi cụ thể. Ví dụ: `scalar=123`.
* **Vector**: Vector là một Tensor rank 1. Trong python thì Vector là một _list hay mảng một chiều_ [[Pyton]][5] chứa các số. Ví dụ: `list=[123,345]`.
* **Matrix**: Đây là một Tensor rank 2 hay mảng hai chiều theo khái niệm của Python. Ví dụ: `matrix=[[1,2],[2,1]]`.
* **N-Tensor**: Khi rank của Tensor tăng lên lớn hơn 2, chúng được gọi chung là N-Tensor.

![](https://s3-ap-southeast-1.amazonaws.com/kipalog.com/nqj1uqylgd_image.png)  
Hình 1.2: Ví dụ minh họa về Tensor

###### Lưu ý: Khái niệm về chiều trong Tensorflow và Python có sự sai khác lẫn nhau. Chiều trong python chính là bậc trong Tensorflow. Chiều trong Tensorflow là số lượng elements có trong bậc cuối cùng của Tensor tương ứng. Ví dụ `Tensor = [[[1,1,1],[178,62,74]]]` có chiều bằng 3, `Tensor = [[1,1,1],[178,62,74]]` vẫn có chiều bằng 3.

##### Shape

Shape là một tuple[[Python]][6] có _dimention_ (Số chiều) bằng với rank của Tensor tương ứng dùng để mô tả lại cấu trúc của Tensor đó. Dưới đây là ví dụ về Shape.

* `Tensor = 1` sẽ có `Shape = ()`.
* `Tensor = [1]` sẽ có `Shape = (1)`.
* `Tensor = [[[1,1,1],[178,62,74]]]` sẽ có `Shape = (1,2,3)`.
* `Tensor = [[1,1,1],[178,62,74]]` sẽ có `Shape = (2,3)`.

Dựa vào cấu trúc của Shape, ta dễ dàng thấy rằng ràng buộc cơ bản của Tensor là chiều của các elements trong Tensor tại mỗi bậc phải bằng nhau. Dưới đây là ví dụ báo lỗi của Tensorflow.
    
    
    >>> test = tf.constant([[1], [1,2]])
    Traceback (most recent call last):
      File "", line 1, in 
      File "/usr/local/lib/python2.7/dist-packages/tensorflow/python/framework/constant_op.py", line 102, in constant
        tensor_util.make_tensor_proto(value, dtype=dtype, shape=shape, verify_shape=verify_shape))
      File "/usr/local/lib/python2.7/dist-packages/tensorflow/python/framework/tensor_util.py", line 383, in make_tensor_proto
        _GetDenseDimensions(values)))
    ValueError: Argument must be a dense tensor: [[1], [1, 2]] - got shape [2], but wanted [2, 1].
    >>> test = tf.constant([[1,2], [1]])
    Traceback (most recent call last):
      File "", line 1, in 
      File "/usr/local/lib/python2.7/dist-packages/tensorflow/python/framework/constant_op.py", line 102, in constant
        tensor_util.make_tensor_proto(value, dtype=dtype, shape=shape, verify_shape=verify_shape))
      File "/usr/local/lib/python2.7/dist-packages/tensorflow/python/framework/tensor_util.py", line 383, in make_tensor_proto
        _GetDenseDimensions(values)))
    ValueError: Argument must be a dense tensor: [[1, 2], [1]] - got shape [2], but wanted [2, 2].
    >>> test = tf.constant([[1,2], [1,2]])
    >>> test
    
    

![](https://s3-ap-southeast-1.amazonaws.com/kipalog.com/eqlrr6pxtx_image.png)  
Hình 1.3: Báo lỗi của Tensorflow khi khai báo một Tensor không hợp lệ

##### Op

Được viết tắt là op, khái niệm Operator là toán tử được dùng để thực thi Tensor tại node đó. Các toán tử này có thể là **_Const_** (Hằng số), **_Variable_** (Biến số), **_Add_** (Phép cộng), **_Mul_** (Phép nhân)... Đôi khi mình cảm thấy việc dịch là toán tử cũng không hợp lí bởi lẽ các toán tử này đôi khi lại mô tả Node là Constant hay Variable. Có thể nói, khái niệm operator trong Tensorflow là khái niệm dùng để mô tả lại trạng thái của Node nói chung.

##### DType

Đây là kiểu dữ liệu của các elements trong Tensor. Vì một Tensor chỉ có duy nhất một thuộc tính DType nên từ đó cũng suy ra là chỉ có duy nhất một kiểu DType duy nhất cho toàn bộ các elements có trong Tensor hiện tại. Bảng danh sách các Dtype khả dụng tra cứu [tại đây][7]. Việc tạo ra hơn một DType khác nhau cho các elements của Tensor là không khả dụng. Hiện có hack hay trick nào chưa thì mình chưa rõ nhưng kể cả khi bạn thực hiện các phép toán thì cũng không thể làm điều đó được.

## Hello world với Tensorflow

Phần quan trọng tiếp theo là bạn ít ra cũng phải thực hành được một bài toán nào đó với Tensorflow chứ. Nội dung bên dưới là cách thức cài đặt Tensorflow và cách thức giải một bài toán Supervised Learn đơn giản thông qua một thuật toán thuộc nhóm Regression. Vì đây là This Year I Learned nên phần bên dưới là nội dung mô tả về trải nghiệm của mình. Không phải là bài hướng dẫn.

### Hello world

Như đã quen thuộc với tất cả mọi lập trình viên trên thế giới. Bài toán Hellowork là bài toán đơn giản nhất nhằm chỉ ra cách cài đặt và chạy thử nghiệm một chương trình hoặc thuật toán bất kì. Để [install][8] được Tensorflow, thư viện đòi hỏi ta phải thực hiện vô cùng nhiều bước cài đặt nên cách mình lựa chọn là sử dụng **Docker** [[Kipalog]][9]. Docker sẽ cung cấp cho ta một máy ảo (Hiện tại máy ảo của Docker chỉ hỗ trợ Linux) có thiết lập sẵn môi trường làm việc cần thiết cho một dự án phát triển nào đó.

Thiết bị mình dùng là một con Workstion cài Ubuntu. Mình sử dụng câu lệnh sau để thiết lập môi trường cho Docker.
    
    
    $ sudo apt-get instatll docker
    $ docker run -it gcr.io/tensorflow/tensorflow bash
    

_Thông thường thì theo kinh nghiệm bản thân thì mỗi lần train cho project khá tốn thời gian. Mỗi lần đổi quyết định về model sử dụng, mình phải train lại con máy hết tầm khoảng hơn một ngày làm việc chỉ để nó tự blend ảnh theo phong cách cho mình. Vậy nên trong khoảng thời gian train mà có ông nào vô tình đi qua đạp dây mạng của mình chắc mình tức lòi mắt ra mất. Cách tốt nhất để hạn chế mấy rủi ro kiểu này là sự dụng tmux. Đây là tùy chọn, các bạn có thể làm theo hoặc không. Tuy nhiên dùng tmux thì Cr7 xong sẽ thấy nó cool hơn. :D_

Sau khi chạy lệnh để khởi động Docker, bạn sẽ được đưa đến con trỏ ở vị trí sau:
    
    
    root@aa4cde1f099f:/notebooks#
    

Mình chạy tiếp lệnh `python` để khởi động python ở _scritp mode_(Chế độ kịch bản).
    
    
    $ python
    Python 2.7.12 (default, Nov 19 2016, 06:48:10) 
    [GCC 5.4.0 20160609] on kipalog.com
    Type "help", "copyright", "credits" or "license" for more information.
    >>>
    

Vì Tensorflow không phải là thư viện mặc định của python nên để sử dụng được ta luôn phải import trước khi dùng. Tuy nhiên là mình thấy chữ Tensorflow khá dài và tốn nhiều không gian lệnh. Vậy nên mình _alias_ (Ánh xạ) nó thành `tf` để tiện dùng trong tương lai.
    
    
    >>> import tensorflow as tf
    

Giờ mình sẽ vẽ một graph trong Tensorflow bằng cách tạo ra một Node.
    
    
    >>> hello = tf.constant('Hello, TensorFlow!')
    

Node này có tên là `hello`, Operator là `constant`, DType là `tf.string`, Shape là `()`. Khá nhiều thông tin cho Node này. Làm như thế nào để biết. Chỉ cần type lại tên Node là `hello` vào trong dòng lệnh của python rồi gõ Enter là ổn.
    
    
    >>> hello
    
    

Quá đã, mình đã từng thích Python rất nhiều vì điều này. Ta có thể nhận thấy Operator được thiết lập cho Node bởi _method_(Phương thức)`constant()`. Vì dữ liệu được truyền vào có kiểu chuỗi nên DType được chuyển một cách tự động. Đây là một Scalar Node nên có `rank=0` đương nhiên `shape=()`. Tuy nhiên Node `hello` mặc dù đã xuất hiện thần kì trên đồ thị nhưng chẳng thể làm gì được cả. Hãy tưởng tượng ở thế giới nhân sinh của chúng ta, máy móc thì làm sao mà học được. Trong Tensorflow có một cõi thần tiên, nơi mà đã được triển khai hết các thuật toán Machine Learning cần thiết để train cho máy tính. Ta chỉ có thể liên kết đến nơi đó thông qua một ông thầy cúng gọi là _Session_ (Phiên)[[Tensorflow]][10]. Session sẽ có nhiệm vụ giao tiếp với các vị thần tiên tục gọi là Worker để thực thi các hoạt động mong muốn của ta.
    
    
    >>> sess = tf.Session()
    

Bỏ bao nhiêu là tiền ra mới thỉnh được ông thầy này về phải tận dụng cho đã. Ta sẽ yêu cầu ông ấy chạy thử qua Node `hello`.
    
    
    >>> print(sess.run (hello))
    'Hello, TensorFlow!'
    

Vậy là chắc chắn Tensorflow có chạy được. Yo!...

### Thuật toán Linear Regression

Thực ra là phần này mình viết cuối cùng nên là mình lười rồi. Các bạn chịu khó xem tạm ở đây nhé: 

### Triển khai thuật toán Linear Regression bằng Tensorflow

Là một trong những bài toán đơn giản nhất của Machine Learing. Triển khai một thuật toán Linear Regression là cách đơn giản nhất nhằm tiếp cận với Tensorflow. Bài toán được đưa ra là tìm ra Model quan hệ giữa tập nguồn [1,2,3,4] và tập đích [0,-1,-2,-3]. Sở dĩ lựa chọn tập này vì khi vẽ mô hình tương quan giữa hai tập số vào đồ thị, ta dễ dàng dùng một đường thẳng đi qua toàn bộ cả bốn điểm trên. Điều này cho thấy rằng Loss Function của Model trên có giá trị bằng 0. Vậy nên Model của cần tìm của chúng ta sẽ đủ tốt nếu giá trị của Loss Function trả về bằng 0. Đây là cách để thực hiện Unit test trong Machine Learning với các bài toán đơn giản.

#### Import Tensorflow

Giống như với bài toán hello word. Chúng ta sẽ tiến hành import Tensorflow vào python.
    
    
    >>> import tensorflow as tf
    >>> sess = tf.Session()
    

#### Xây dựng graph

##### Constant

Nhắc lại một chút về việc tạo Node dạng constant trong Tensorflow.
    
    
    >>> node1 = tf.constant(3.0, dtype=tf.float32)
    >>> node2 = tf.constant(4.0) # also tf.float32 implicitly
    >>> print(node1, node2)
    Tensor("Const:0", shape=(), dtype=float32) Tensor("Const_1:0", shape=(), dtype=float32)
    

Không chỉ là các Node tĩnh không bao giờ thay đổi kiểu dữ liệu, Tensorflow cho phép ta tạo ra các toán tử khác nữa của node thông qua việc tạo một node mới.
    
    
    >>> node3 = tf.add(node1, node2)
    >>> print("Node3: ", node3)
    Node3:  Tensor("Add:0", shape=(), dtype=float32)
    

Node mới hiện tại đã có Operator là `Add`. Cách thức hoạt động của Node này như sau.
    
    
    >>> print("sess.run(node3): ",sess.run(node3))
    sess.run(node3):  7.0
    

Trong TensorBoard, graph sẽ thể hiện như sau.

![](https://s3-ap-southeast-1.amazonaws.com/kipalog.com/n2ux2com3j_image.png)  
_Hình 2.1: Đồ thị của phép add hai constant_

##### Placeholder

Suy nghĩ một chút về bài toán, đồ thị mong muốn của chúng ta là một hàm bậc hai một ẩn có dạng `y = Wx + b` với `x` là dữ liệu tập nguồn, `y` là dữ liệu tập đích, `W` là _weight_(Trọng số) của `x` và `b` là _bias_(Độ lệch) của hàm mong muốn. Để xây dựng được hàm dưới dạng graph, ta không thể chỉ dùng các Node cố định mà Tensorflow mong muốn giá trị của `x` và `y` có thể thay đổi được trong quá trình Session yêu cầu Worker làm việc. Google đưa ra một khái niệm gọi là _placeholder_(Đặt gạch / Giữ chỗ) như sau.
    
    
    >>> a = tf.placeholder(tf.float32)
    >>> b = tf.placeholder(tf.float32)
    >>> adder_node = a + b  # + provides a shortcut for tf.add(a, b)
    

Khi một Node có Operator là `placeholder`, chúng không cần giá trị cụ thể mà chỉ yêu cầu một kiểu DType đặt trước. Giá trị của Node sẽ được xác định mỗi lần Worker chạy qua chúng thông qua một tập dữ liệu kiểu _Dictionaries_ (Kiểu dữ liệu từ điển)[[Python]][11]
    
    
    >>> print(sess.run(adder_node, {a: 3, b:4.5}))
    7.5
    >>> print(sess.run(adder_node, {a: [1,3], b: [2, 4]}))
    [ 3.  7.]
    

Trong TensorBoard, graph sẽ được thể hiện như sau.  
![](https://s3-ap-southeast-1.amazonaws.com/kipalog.com/o4yu2rfk24_image.png)  
_Hình 2.2: Đồ thị của phép add hai placeholder_  
Đồng thời cũng có thể dễ dàng tạo ra một phép toán kết hợp Node như sau.
    
    
    >>> add_and_triple = adder_node * 3.
    >>> add_mul_add = adder_node * node3
    

##### Variable

Vậy là xong, Model của chúng ta cần làm sẽ có dạng.
    
    
    >>> x = tf.placeholder(tf.float32)
    >>> W = tf.constant(1.)
    >>> b = tf.constant(1.)
    >>> linear_model = W * x + b
    >>> y = tf.placeholder(tf.float32)
    

Quên công thức bên trên ngay và luôn. Vì `W` và `b` là hai constant nên dẫn đến việc khi chúng ta cần phải thay đổi hai giá trị này để tạo ra Model mới, ta sẽ phải tạo ra hai biến mới thật phiền. Tensorflow cung cấp một Operator là _Variable_ (Biến số).
    
    
    >>> W = tf.Variable([.3], dtype=tf.float32)
    >>> b = tf.Variable([-.3], dtype=tf.float32)
    

Variable giống placeholder yêu cầu lập trình viên phải truyền vào DType của Node đồng thờitruyền vào một giá trị khởi tạo được gọi là _Seed_(Mầm). Trong quá trình chạy thuật toán, giá trị mới cho các Variable sẽ được tính toán và thay đổi để tạo nên một Model mới. Các giá trị này không được truyền vào sẵn như placeholder mà thay đổi thông qua quá trình chạy. Chính vì vậy mà chúng cần một giá trị được gọi là seed. Giá trị seed này có thể là bất cứ thứ gì. Thông thường thì mình sẽ lấy hai giá trị đầu trong tập train là [`[1,0],[2,-1]]` để giải hệ phương trình bậc hai hai ẩn ra được giá trị của `W` và `b` thỏa mãn hai giá trị đầu làm seed. Tuy nhiên trong trường hợp này thì giá trị đó chính là giá trị kì vọng luôn do hàm mong muốn của ta đi qua cả bốn điểm cho trước. Như vậy chúng ta sẽ xây dựng một model có giá trị bất kì như sau để đảm bảo seed luôn sai nhằm có cái để train.
    
    
    >>> W = tf.Variable([.3], dtype=tf.float32)
    >>> b = tf.Variable([-.3], dtype=tf.float32)
    >>> x = tf.placeholder(tf.float32)
    >>> linear_model = W * x + b
    

Các Variable cần phải được đưa về trạng thái seed trước khi thực hiện các tính toán đi qua Node đó.
    
    
    >>> init = tf.global_variables_initializer()
    >>> sess.run(init)
    

Giờ chúng ta sẽ thử chạy thử xem là giá trị khởi tạo chúng ta đã chọn.
    
    
    >>> print(sess.run(linear_model, {x:[1,2,3,4]}))
    [ 0.          0.30000001  0.60000002  0.90000004]
    

Chúng ta có thể thấy tập giá trị mới được trả ra bởi `linear_model` này hoàn toàn không giống với tập đích của tập train. Vậy ta cần huấn luyện lại Model này. Trước đó, ta sẽ xem thử giá trị của Loss Function là bao nhiêu đã.
    
    
    >>> y = tf.placeholder(tf.float32)
    >>> squared_deltas = tf.square(linear_model - y)
    >>> loss = tf.reduce_sum(squared_deltas)              
    >>> print(sess.run(loss, {x:[1,2,3,4], y:[0,-1,-2,-3]}))
    23.66
    

Ta tạo ra một `placeholder y`. `squared_deltas` có giá trị là `Wx + b - y`. Method reduce_sum sẽ tính tổng tất cả những giá trị khả dụng của squared_deltas trong toàn bộ quá trình chạy. Tổng được tính ra có giá trị là `1.69 + 6.76 + 15.21 ~ 23.66`. Một con số sai lệch quá lớn so với giá trị 0. Vậy chúng ta sẽ thử thay thế giá trị của các Variables để đạt được model mong muốn.
    
    
    >>> fixW = tf.assign(W, [-1.])
    >>> fixb = tf.assign(b, [1.])
    >>> sess.run([fixW, fixb])
    >>> print(sess.run(loss, {x:[1,2,3,4], y:[0,-1,-2,-3]}))
    0.0
    

Hàm mất mát bằng 0. Không còn gì để kì vọng hơn nữa. Vừa rồi chúng ta đã điểm qua các khái niệm cơ bản trong Tensorflow bao gồm **Tensor**, **Node**, **Const**, **Variable**, **placeholder**. Giờ chúng ta sẽ tiến tới xây dựng một Model có thể học được.

#### Xử dụng API tf.train Model

Để bắt đầu với việc training, chúng ta xây dựng một mẫu cho Model mong muốn.
    
    
    >>> import tensorflow as tf
    >>> 
    >>> # Model parameters
    >>> W = tf.Variable([.3], dtype=tf.float32)
    >>> b = tf.Variable([-.3], dtype=tf.float32)
    >>> # Model input and output
    >>> x = tf.placeholder(tf.float32)
    >>> linear_model = W * x + b
    >>> y = tf.placeholder(tf.float32)
    >>> # loss
    >>> loss = tf.reduce_sum(tf.square(linear_model - y)) # sum of the squares
    

Tiếp theo, chúng ta sẽ tìm trong package `tf.train` của Tensorflow một _Optimizer_(Trình tối ưu hóa) mong muốn. Optimizer chính là quy chuẩn để Worker biết được cách thức huấn luyện Model. Vì đang sử dụng thuật toán Linear Regression nên chúng ta cần phải làm cực tiểu hóa cho đạo hàm của Loss Function. Có một vài trình tối ưu hóa đạo hàm trong package `tf.train`. Mình sẽ chọn Optimizer đơn giản nhất là GradientDescentOptimizer. Đối số truyền vào cho Optimizer này là giá trị alpha của công thức tối ưu Loss Function. Ở đây mình chọn giá trị rất nhỏ là `0.01` để máy chạy cho nó lâu.
    
    
    >>> # optimizer
    >>> optimizer = tf.train.GradientDescentOptimizer(0.01)
    >>> train = optimizer.minimize(loss)
    

Kế đó là chuẩn bị tập train.
    
    
    >>> # training data
    >>> x_train = [1,2,3,4]
    >>> y_train = [0,-1,-2,-3]
    

Sau đó khởi tạo giá trị ban đầu cho Model để chọn được Model ban đầu.
    
    
    >>> # training loop
    >>> init = tf.global_variables_initializer()
    >>> sess = tf.Session()
    >>> sess.run(init) # reset values to wrong
    

Vì bản thân training method không biết được giá trị như thế nào thì là giá trị tối ưu cho Loss Function. Mặc dù ai cũng biết trong bài toán này thì 0 là giá trị tối ưu đó. Tuy nhiên một cách tổng quát là nếu ta mà biết được thì ta đã không cần đến Machine Learning. Công việc của ta là cho Model học đi học lại học tái học hồi. Đến lúc nào đó ta thấy nó ổn thì nó sẽ ổn. (_Khái niệm thế nào là ổn không được đề cập trong bài viết này. Người làm Machine Learning dùng cái này để kiếm tiền mà. Nên muốn biết phải đọc sách. Trong trường hợp đặc biệt này. Giá trị ổn đã được ta định trước bằng 0_)
    
    
    for i in range(1000):
    >>> sess.run(train, {x:x_train, y:y_train})
    >>> 
    >>> # evaluate training accuracy
    >>> curr_W, curr_b, curr_loss = sess.run([W, b, loss], {x:x_train, y:y_train})
    >>> print("W: %s b: %s loss: %s"%(curr_W, curr_b, curr_loss))
    W: [-0.9999969] b: [ 0.99999082] loss: 5.69997e-11
    

Ta có thể thấy là giá trị loss lúc này rơi vào khoảng `5 x 10^-11` là một khoảng rất rất nhỏ. Nếu ta tiếp tục train, giá trị sẽ giảm xuống nữa nhưng không bao giờ về 0 được vì Lim đang tiến tới vô cùng. Ok vậy là ta đã đi qua một bài toán Machine Learning dạng Regression. Nếu dùng thư viện thì việc thực hàng Machine Learning trở nên rất là đơn giản đúng không? Tuy nhiên nếu không có kiến thức căn bản về Machine Learning, ta cũng không thể biết được rằng mình nên làm gì với cái thư viện này. Đây là toàn bộ lời giản cho bài toán mẫu. Lời giải tối ưu hơn có thể [tham khảo tại đây][12].

#### Bài toán đầy đủ
    
    
    >>> import tensorflow as tf
    >>> 
    >>> # Model parameters
    >>> W = tf.Variable([.3], dtype=tf.float32)
    >>> b = tf.Variable([-.3], dtype=tf.float32)
    >>> # Model input and output
    >>> x = tf.placeholder(tf.float32)
    >>> linear_model = W * x + b
    >>> y = tf.placeholder(tf.float32)
    >>> # loss
    >>> loss = tf.reduce_sum(tf.square(linear_model - y)) # sum of the squares
    >>> # optimizer
    >>> optimizer = tf.train.GradientDescentOptimizer(0.01)
    >>> train = optimizer.minimize(loss)
    >>> # training data
    >>> x_train = [1,2,3,4]
    >>> y_train = [0,-1,-2,-3]
    >>> # training loop
    >>> init = tf.global_variables_initializer()
    >>> sess = tf.Session()
    >>> sess.run(init) # reset values to wrong
    >>> for i in range(1000):
    >>>   sess.run(train, {x:x_train, y:y_train})
    >>> 
    >>> # evaluate training accuracy
    >>> curr_W, curr_b, curr_loss = sess.run([W, b, loss], {x:x_train, y:y_train})
    >>> print("W: %s b: %s loss: %s"%(curr_W, curr_b, curr_loss))
    

![](https://s3-ap-southeast-1.amazonaws.com/kipalog.com/2r4jrv9bzo_image.png)  
_Hình 2.3: Đồ thị của toàn bộ bài toán_

Bài toán của chúng ta không bao giờ đơn giản và êm ả với chỉ một Model dạng linear. Model không chỉ tự thay đổi giá trị bản thân chúng để học mà còn thay đổi cả dạng của chúng nữa. Điều này dẫn đến Model phải có khả năng tự chuyển đổi nhờ vào một API gọi là `tf.contrib.learn`  
...

[1]: https://chainer.org/
[2]: https://www.tensorflow.org/
[3]: https://kipalog.com#tensor
[4]: https://www.tensorflow.org/api_docs/python/tf/Tensor
[5]: https://docs.python.org/2/tutorial/datastructures.html#more-on-lists
[6]: https://docs.python.org/2/tutorial/datastructures.html#tuples-and-sequences
[7]: https://www.tensorflow.org/api_docs/python/tf/DType
[8]: https://www.tensorflow.org/install/
[9]: https://kipalog.com/posts/Toi-da-dung-Docker-nhu-the-nao
[10]: https://www.tensorflow.org/api_docs/python/tf/Session
[11]: https://docs.python.org/2/tutorial/datastructures.html#dictionaries
[12]: https://github.com/aymericdamien/TensorFlow-Examples/blob/master/examples/2_BasicModels/linear_regression.py

  

