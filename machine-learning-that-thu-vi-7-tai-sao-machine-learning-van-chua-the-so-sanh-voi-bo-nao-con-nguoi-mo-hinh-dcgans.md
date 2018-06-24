

[Source](https://viblo.asia:443/p/machine-learning-that-thu-vi-7-tai-sao-machine-learning-van-chua-the-so-sanh-voi-bo-nao-con-nguoi-mo-hinh-dcgans-4dbZNgJ8lYM "Permalink to Machine Learning thật thú vị (7): Tại sao Machine Learning vẫn chưa thể so sánh với bộ não con người? - mô hình DCGANs")

# Machine Learning thật thú vị (7): Tại sao Machine Learning vẫn chưa thể so sánh với bộ não con người? - mô hình DCGANs

> Một trong những khả năng tuyệt vời nhất của con người chính là khả năng tưởng tượng. Có những người ta chưa gặp bao giờ, nhưng nếu ai đó đưa ta ảnh của nửa khuôn mặt người đó, ta hoàn toàn có thể dự đoán ra hình ảnh của cả khuôn mặt. Thậm chí, con người hoàn toàn có thể tưởng tượng ra toàn bộ một câu chuyện không có thật, hay hình dung về một câu chuyện mà ai đó kể cho chúng ta. Đó là common sense (trí thông minh cơ bản) mà bất kể ai trong chúng ta cũng có - theo [Yann Lecun - tại sao bộ não học nhanh như vậy][1]  
Nhưng điều đó không/ chưa tồn tại trong bộ não của máy tính. Để nhận diện, phân tích bất cứ điều gì, máy tính cần hàng triệu dữ liệu, và khả năng dự đoán, chỉ gói gọn trong tập dữ liệu đó mà thôi. Và đó là lý do, một mô hình[ Deep Learning ][2]mới phát triển - mô hình sinh mẫu.

Hơn một năm trước, bài báo khoa học của Alec Radford đã thay đổi cách mọi người nghĩ về mô hình sinh mẫu trong học máy. Hệ thống mới được gọi là **Deep Convolutional Generative Adversarial Networks** (**DCGANs** \- mình tạm dịch là mạng đối kháng sinh mẫu tích chập đa lớp).

> Đối kháng (Adversarial): trong quá trình xây dựng mô hình có 2 mạng chạy đối lập nhau, cố gắng làm tốt hơn mạng còn lại. Sinh mẫu (Generative): tìm hiểu cách thức dữ liệu được hình thành, sau đó đặt câu hỏi: dựa vào giả định cách thức tạo ra dữ liệu, dữ liệu sẽ được phân vào nhóm nào?. Đối nghịch với sinh mẫu là Phân biệt (Discriminative): không quan tâm đến cách thức dữ liệu được tạo ra, chỉ phân loại dựa trên đầu vào dữ liệu. Tích chập (Convolutional): quá trình trích lọc đặc trưng dữ liệu thông qua cửa sổ trượt với trọng số không đổi

DCGANs đã giúp tạo ra những bức ảnh rất giống với thực tế bằng cách sử dụng tổ hợp 2 mạng nơron đa lớp đối kháng nhau. Tất cả những bức ảnh phòng ngủ dưới đây đều được vẽ lên bởi DCGAN: ![][3] Những nhà nghiên cứu AI (trí thông minh nhân tạo) quan tâm tới **generative model** \- mô hình sinh mẫu, bởi chúng dường như là cánh cửa tiếp theo để xây dựng hệ thống AI có sự hiểu biết về dữ liệu được tiếp nhận.

Chúng ta sẽ cùng nhau sử dụng mô hình sinh mẫu để làm điều gì đó tưởng chừng hơi điên khùng - tạo hình ảnh cho trò chơi điện tự 8-bit. ![][4]

## Mục đích của mô hình sinh mẫu

Tại sao các nhà nghiên cứu AI lại muốn xây dựng hệ thống để tạo ra hình ảnh không được rõ của phòng ngủ? Ý tưởng chính là: **nếu bạn biết cách tạo ra bức ảnh, bạn phải hiểu về nó chứ.**

Hãy nhìn vào bức ảnh sau: ![][5] Ngay lập tức, bạn biết đây là hình ảnh của một chú chó - một loài vật có 4 chân và có đuôi. Nhưng với máy tính, hình ảnh chỉ là mạng lưới các con số đại diện cho màu sắc từng pixel. Máy tính không có khả năng hiểu rằng bức ảnh đại diện cho một khái niệm.

Nhưng hãy tưởng tượng, sau khi chúng ta chỉ ra cho máy tính hàng nghìn bức ảnh của chó, máy tính có khả năng tự tạo ra những bức ảnh mới với nhiều loài chó và góc độ khác nhau. Chúng ta thậm chí còn có thể yêu cầu nó tạo ra một bức hình cụ thể: "góc nhìn ngang của chó Becgie".

Nếu máy tính có thể làm điều đó, và bức ảnh tạo ra có đúng số chân, đuôi, tai, điều đó chứng tỏ máy tính biết phần nào bức ảnh là của con chó. Một cách nào đó, mô hình sinh mẫu tốt là **bằng chứng của sự hiểu cơ bản** của học máy - với trình độ đứa trẻ tập đi.

Đó là lý do tại sao các nhà khoa học lại rất phấn khích xây dựng mô hình sinh mẫu. Dường như có một cách để đào tạo máy tính mà không cần dạy chúng ý nghĩa của những khái niệm, và đó là một bước đi dài so với hệ thống hiện tại - học từ dữ liệu được gán nhãn trước bởi con người.

Nếu tất cả nghiên cứu này tạo ra chương trình tự động tạo ra ảnh loài chó, sau bao nhiêu năm nữa, chúng ta có thể có một cuốn lịch về các loài chó hoàn toàn được tạo bởi máy tính? ![][6] Và nếu như chúng ta xây dựng chương trình có thể hiểu về loài chó, tại sao không hiểu về những điều khác nữa. Sao không phải là một chương trình tạo ra kho ảnh vô hạn mọi người bắt tay nhau? Tôi tin là ai đó sẽ trả tiền cho kho ảnh: ![][7] Dựa vào tốc độ phát triển của mô hình sinh mẫu trong vài năm vừa qua, ai biết được điều gì sẽ chờ đợi trong 5 hay 10 năm nữa. Chuyện gì sẽ xảy ra nếu ai đó phát minh ra hệ thống tạo ra một bộ phim hoàn chỉnh? hay âm nhạc? hay trò chơi điện tử?

Nếu bạn nghĩ về 20-30 năm nữa, bạn có thể tưởng tượng một thế giới mà 100% hoạt động giải trí đều được tạo ra nhờ máy tính. ![][8]

> Dịch: Rồi một ngày, chúng ta nói về những bộ phim do con người thủ vai của những ngày xưa cũ; trong khi đang theo dõi những bộ phim máy tính tạo ra dựa trên nhu cầu.

Ngành trò chơi điện tử là lĩnh vực đầu tiên bắt đầu [nghiêm túc thử nghiệm với AI để tạo ra nội dung][9]. Chúng ta vẫn đang ở thuở sơ khai của mô hình mẫu sinh dựa trên học máy, và tính ứng dụng thực tế vẫn khá hẹp. Tuy thế, vẫn rất thú vị khi chúng ta tìm hiểu về nó. Hãy cùng nhau bắt đầu thôi nào!

## DCGANs hoạt động như thế nào?

Để tạo ra DCGAN, chúng ta cần tạo ra 2 mạng nơron đa lớp, cho chúng cạnh tranh với nhau, và cố gắng làm tốt hơn đối thủ. Trong quá trình đó, cả 2 mạng sẽ cùng trở lên tốt hơn.

Tưởng tượng rằng mạng nơron đa lớp thứ 1 là một viên cảnh sát được đào tạo để phát hiện tiền giả. Nhiệm vụ của cảnh sát là nhìn vào bức tranh và nói cho chúng ta biết bức ảnh chứa tiền thật hay không.

Vì mục tiêu năm ở trong ảnh, chúng ta sử dung [**Convolutional Neural Network][10] (CNN)**. Ý tưởng chính là mạng nơron nhận vào hình ảnh, xử lý qua nhiều lớp ẩn và lấy ra đặc trưng phức tạp từ ảnh, rồi trả ra một giá trị. Trong trường hợp này, giá trị biểu thị liệu ảnh có chứa tiền thật hay không.

Mạng thứ 1 có tên gọi là **Discriminator** \- người phân biệt: ![][11] Ta lại tưởng tượng mạng thứ 2 là kẻ lừa đảo, cố gắng học cách tạo tiền giả. Với mạng thứ 2, ta đảo ngược các lớp ẩn ở mạng thứ nhất để chương trình chạy ngược. Mạng này nhận một chuỗi các giá trị và trả kết quả là một bức ảnh. Mạng thứ 2 có tên gọi là **Generator** \- người sinh mẫu: ![][12]

Ở vòng thứ 1, **người sinh mẫu - kẻ lừa đảo** tạo ra đồng tiền chẳng giống tiền, bởi vì nó không có chút ý tưởng nào về đồng tiền thật. ![][13] Nhưng bởi vì giờ đây, **người phân biệt - viên cảnh sát** cũng rất kém trong việc nhận diện tiền thật, nên nó cũng không nhận ra sự sai khác. ![][14] Lúc này, chúng ta can thiệp và nói với viên cảnh sát rằng tờ tiền là giả. Chúng ta đưa ra đồng tiền thật và yêu cầu nó tìm điểm khác biệt. Cảnh sát cố gắng tìm chi tiết để phân biệt 2 tờ tiền.

Ví dụ: cảnh sát có thể chú ý rằng tiền thật có mặt người và tiền giả không. Sử dụng kiến thức này, cảnh sát biết phân biệt tiền giả, và làm tốt công việc hơn: ![][15] Ở vòng 2, kẻ lừa đảo biết rằng tờ tiền ban đầu đã bị từ chối, và biết rằng cảnh sát tìm kiếm khuôn mặt trên bức ảnh, cách tốt nhất để lừa viên cảnh sát là tạo khuôn mặt trên bức ảnh. ![][16] Tờ tiền giả lại được chấp nhận lần nữa! Cảnh sát lại phải nhìn vào tờ tiền thật để tìm ra điểm nhận dạng khác biệt so với tiền giả.

Quá trình này lặp lại liên tục hàng nghìn lần cho tới khi cả viên cảnh sát và kẻ lừa đảo đều trở thành chuyên gia. Cuối cùng, kẻ lừa đảo tạo ra tiền giả giống như thật, và cảnh sát cũng rất giỏi tìm ra những lỗi dù là bé nhất.

Đến thời điểm này, cả cảnh sát và kẻ lừa đảo đều đã được đào tạo đủ đến nỗi mà con người cũng khó phát hiện ra tờ tiền giả từ máy tính.

> Mọi người có thấy quá trình này giống quá trình mà loài người đã trải qua đế tiến hóa không? Mình thì thấy nó giống với thuyết tiến hóa của Darwin, khi những cá thể yếu kém bị loại bỏ, những cá thể mạnh tiếp tục phát triển. Tương tự, trong mạng sinh mẫu, những nhận diện chuẩn xác được giữ lại, những phân biệt kém không được chấp nhận. Con người đã trải qua hàng chục ngàn năm để được như bây giờ. Thế mới thấy, sự tiến hóa của máy tính nhanh đến mức nào!!!

## Áp dụng vào trò chơi điện tử

Giờ chúng ta đã biết cách thức DCGANs hoạt động, hãy xem cách chúng ta dùng nó để tạo ra trò chơi điện tử thế hệ 1980s.

Ý tưởng là chúng ta cố gắng tạo ra ảnh chụp màn hình của trò chơi điện tử tưởng tượng, rồi sao chép từng bits trong những ảnh chụp này để tạo ra những trò chơi theo phong cách cổ xưa. Vì trò chơi máy tạo chưa bao giờ tồn tại, nó có vẻ sẽ không bị coi là ăn cắp bản quyền ![😃][17]).

Thiết kế trò chơi thời kì ban đầu rất đơn giản. Vì hệ thống trò chơi của Nintendo (**NES**) có rất ít bộ nhớ (thậm chí còn ít bộ nhớ hơn bài viết này), những lập trình viên phải sử dụng rất nhiều thủ thuật để có thể lưu thiết kế vào bộ nhớ. Để tối đa không gian, trò chơi sử dụng các mảnh ghép đồ họa 16x16 pixel, và tái sử dụng nhiều lần. Ví dụ, màn hình trò chơi "Huyền thoại Zelda" được tạo nên bởi 8 mảnh: ![][18] Và đây là những mảnh cho toàn bộ sơ đồ trò chơi. Thỉnh thoảng, lập trình viên thay đổi màu sắc để tạo ra các khu vực khác nhau, nhưng thực ra thiết kế mảnh ghép không đổi. ![][19]

Chúng ta sẽ để máy tính tạo ra các mảnh ghép cho trò chơi, và không quan tâm liệu bố cục bức ảnh được tạo có giống với thực tế không. Thay vào đó, chúng ta tìm hình dáng và cấu trúc mảnh 16x16 mà chúng ta có thể sử dụng trong trò chơi như hòn đá, nước, cầu... Sau đó, chúng ta có thể sử dụng các mảnh này để tạo nên các mức độ khác nhau của trò chơi.

> Mục tiêu: sinh mẫu các mảnh ghép trong trò chơi

### Thu thập dữ liệu

Để đào tạo mô hình, chúng ta cần rất nhiều dữ liệu. May mắn là có hơn 700 trò chơi của **NES** mà ta có thể lấy về.

Tôi sử dụng wget để tải về toàn bộ màn hình trò chơi **NES** trên [website][20]. Sau vài phút tải, tôi có hơn 10,000 ảnh chụp. ![][21] Hiện tại, DCGANs chỉ hoạt động trên ảnh nhỏ - cỡ 256x256 pixels. Nhưng vì màn hình ảnh của NES là 256x224, để cho đơn giản, tôi cắt ảnh còn 224x224 pixels.

### Cài đặt DCGAN

Có rất nhiều cài đặt mã nguồn mở của DCGANs trên github mà bạn có thể thử. Tôi sử dụng bản [cài đặt trên Tensorflow][22] của Taehoon Kim. Vì DCGANs là thuật toán [học không giám sát][23], tất cả những gì bạn cần làm là đưa dữ liệu, khởi tạo tham số ban đầu, bắt đầu đào tạo và đợi kết quả.

Đây là mẫu của dữ liệu đào tạo gốc: ![][24] Quá trình đào tạo bắt đầu! Đầu tiên, kết quả từ **Generator** chứa nhiều nhiễu. Nhưng từng phần bắt đầu hình thành khi **Generator** học cách làm công việc tốt hơn: ![][25] Sau một vài vòng đào tạo, chúng ta đã nhận ra được cấu trúc phiên bản quen thuộc của game Nintendo: ![][26] Tiếp tục đào tạo, chúng ta có thể thấy tường và vật cản đã được hình thành. Chúng ta thậm chí còn thấy cả số máu hay những dòng chữ: ![][27]

Đây là lúc mọi thứ trở nên phức tạp. Làm thế nào chúng ta biết máy tính đang tạo ra bức ảnh mới, thay vì chỉ tái tạo lại trực tiếp từ ảnh đào tạo. Ở 2 trong những bức ảnh trên, bạn có thể nhìn thấy rõ thanh menu từ game Mario cùng thanh tiêu đề và hòn đá từ bản nguyền gốc.

Tái tạo lại dữ liệu đào tạo hoàn toàn có thể xảy ra. Sử dụng tập dữ liệu đào tạo lớn và không đào tạo quá lâu, ta có thể giảm khả năng điều này xảy ra. Đây là vấn đề nhức nhối vẫn đang được các nhà khoa học nghiên cứu.

Vì muốn hình ảnh đẹp hơn, tôi tinh chỉnh mô hình cho tới khi nó tạo được thiết kế có vẻ giống nguyên gốc. Nhưng tôi _**không thể biết**_ được đây là thiết kế hoàn toàn mới hay không, trừ phi so sánh với tất cả ảnh trong tập đào tạo.

Sau một vài giờ huấn luyện, ảnh máy tạo chứa các mảnh 16x16 trông khá ổn. Tôi thêm một chút thay đổi vào vật cản, mẫu đá, nước, bụi cây...

Tiếp đến, tôi cần tiền xử lý ảnh được tạo để chắc chắn rằng chúng chỉ chứa 64 màu hiển thị được trên NES. ![][28] Sau đó, tôi mở ảnh 64 màu ra ở trong [Tiled Map Editor][29] và lấy ra các mảnh 16x16 phù hợp với các nhóm đối tượng khác nhau. Đây là những mảnh tôi đã lựa chọn sau khi bỏ đi những mảnh khó nhận diện. ![][30] Sử dụng Editor trên, tôi sắp xếp các mảnh 16x16 lựa chọn phía trên thành các lớp tương tự như trò chơi "[Castlevania][31]" ![][32]

> Lưu ý là ở bài này, tác giả cố gắng tìm thiết kế của các mảnh, và không quá quan tâm đến việc liệu học máy có tạo ra được bố cục trò chơi chính xác hay không.

Ảnh trông khá ổn. Hãy nhớ rằng chúng ta không chỉnh sửa bất cứ pixel nào trong từng mảnh 16x16 đó. Tất cả các mảnh đều được lấy ra trực tiếp từ mô hình DCGAN

Tiếp đến, hãy thêm nhân vật chính và một vài kẻ thù. Dưới đây là hình ảnh trong trò chơi khi đã được thêm cả thanh menu: ![][33] Khá giống trò chơi thực tế phải không nào!!

## Lời kết

Tôi thực sự cảm thấy phấn khích với mô hình sinh mẫu. Ý tưởng của một ngày làm việc nghệ thuật không ngừng rất cuốn hút tôi. Nhưng khi mà tôi nói với mọi người về vấn đề này, một số người trả lời: "Thế này vẫn quá đơn giản".

Có rất nhiều niềm hy vọng xung quanh mô hình sinh mẫu. **Generative Adversarial Networks - GANs** được gọi là tương lai của AI, mặc dầu mô hình khó đào tạo và chỉ giới hạn ở ảnh nhỏ. Thực tế, mô hình tốt nhất chỉ có thể tạo ra ảnh chó đột biến kích thước nhỏ như tem thư. ![][34] Nhưng chỉ một vài năm trước, chúng ta còn không thể làm được điều đó. Chúng tôi đã cực kỳ phấn khích chỉ với bức ảnh như thế này thôi: ![][35] Và công nghệ cải thiện từng ngày. Một vài bài báo xuất hiện gần đây đã sử dụng GANs để già hóa khuôn mặt con người. ![][36] Nếu mọi thứ cứ tiếp tục cải thiện ở tốc độ này, không lâu sau nữa, mô hình sinh mẫu sẽ có khả năng sáng tạo như con người. Đây chính là thời kỳ tuyệt vời để bắt tay vào thử nghiệm!!!

## Nguồn

> Bài gốc: 

[1]: https://www.youtube.com/watch?v=cWzi38-vDbE
[2]: https://viblo.asia/p/machine-learning-that-thu-vi-2-tao-sach-van-hoc-va-game-mario-WAyK81o9ZxX#viet-ra-mot-cau-chuyen-6
[3]: https://viblo.asia/uploads/c1c17bb3-187c-4771-8a49-d96826f5cc98.png
[4]: https://viblo.asia/uploads/7bcb99ef-3a82-4c50-91a4-1963a2930494.png
[5]: https://viblo.asia/uploads/b185e913-53a2-4a27-8eee-22b2a110ab80.jpeg
[6]: https://viblo.asia/uploads/eea8e21f-2b22-4447-b7fe-3a2732952203.jpeg
[7]: https://viblo.asia/uploads/c469f27e-f18e-459d-884b-12b9904baf87.jpeg
[8]: https://viblo.asia/uploads/b34f60b5-a672-4420-81dc-f31a650ccd7d.png
[9]: https://www.technologyreview.com/s/601258/artificial-intelligence-can-now-design-realistic-video-and-game-imagery/
[10]: https://viblo.asia/p/machine-learning-that-thu-vi-3-nhan-dien-chim-trong-anh-vyDZOX1xlwj#mang-cnn-hoat-dong-nhu-the-nao-7
[11]: https://viblo.asia/uploads/f62990f2-2f79-4e64-a63b-4fdd07754eb8.png
[12]: https://viblo.asia/uploads/6a811320-f7d4-4354-bec0-cb8138c80a26.png
[13]: https://viblo.asia/uploads/7946ef18-807f-4ad3-aaa5-8332a9caa1fa.png
[14]: https://viblo.asia/uploads/d075debf-61bd-4467-9229-5f0507abd0bf.png
[15]: https://viblo.asia/uploads/74415195-946c-4d02-87b8-fa4b3caae4f3.png
[16]: https://viblo.asia/uploads/fc1b6752-43ea-4c67-94d0-ecda3f99f980.png
[17]: https://twemoji.maxcdn.com/2/72x72/1f603.png
[18]: https://viblo.asia/uploads/65ceaaf3-46ae-47e0-bbdf-35c2e6926640.png
[19]: https://viblo.asia/uploads/4fa35458-58ef-4a4c-b130-bc703db7ba96.png
[20]: http://www.vgmuseum.com/nes.htm
[21]: https://viblo.asia/uploads/f2512916-ab2a-472e-a99f-189034f07356.png
[22]: https://github.com/carpedm20/DCGAN-tensorflow
[23]: https://viblo.asia/p/machine-learning-that-thu-vi-1-du-doan-gia-nha-dat-gAm5y91w5db#hoc-khong-giam-sat-3
[24]: https://viblo.asia/uploads/335aba49-082b-412f-9b0e-46c4d171f447.png
[25]: https://viblo.asia/uploads/full/c0f4e075-f016-482a-97a4-322af003c11c.gif
[26]: https://viblo.asia/uploads/full/0e9a69f0-de99-4df3-8026-77d50e009549.gif
[27]: https://viblo.asia/uploads/e7d2533c-d448-4792-a491-db152c4b8461.png
[28]: https://viblo.asia/uploads/90b8ea27-03bb-431b-aa68-d617c50d6043.png
[29]: http://www.mapeditor.org/
[30]: https://viblo.asia/uploads/b4ea9b8f-f20a-4533-8a36-168128323641.png
[31]: https://en.wikipedia.org/wiki/Castlevania
[32]: https://viblo.asia/uploads/2790f355-6be3-4b9c-8485-7ca1329797c5.png
[33]: https://viblo.asia/uploads/49946d28-e656-4b49-824e-afed825f23a9.png
[34]: https://viblo.asia/uploads/56f055be-0cd1-498a-a104-64b7bf9cf359.png
[35]: https://viblo.asia/uploads/1ba7a39e-9e90-4e62-b689-d8c5ff35799a.png
[36]: https://viblo.asia/uploads/8c1a36b3-00dd-4a5f-b363-004384debcd4.png

  

