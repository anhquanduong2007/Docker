# Docker là gì ?

- Khi phát triển 1 ứng dụng bằng bất kỳ ngôn ngữ nào, chúng ta đều phải cài đặt những phần mềm đi kèm theo nó bao gồm các thư viện, nền tảng thực thi. Ví
dụ như khi lập trình java chúng ta phải cài jdk, maven java,... Vấn đề sẽ xảy ra khi ban muốn đem ứng dụng đi deploy lên một máy khác ở một môi trường khác, không phải bất kỳ ngôn ngữ nào cũng build file thực thi, chạy trực tiếp bằng ngôn ngữ máy, do đó khả năng cao là chúng ta phải cài lại từ đầu các phân mềm hỗ trợ này mà phải cài đúng phiên bản, đúng version của project mà bạn đang làm và nó sẽ càng vấn đề hơn nếu bạn phải cài trên nhiều máy khác nhau chứ không phải 1 máy, việc triển khai ứng dụng trở lên khó khăn hơn và kém hiệu quả -> Docker mang đến giải pháp cho vấn đề này, Docker là công nghệ giúp đóng gói ứng dụng, cùng toàn bộ thư viện và nền tảng thực thi lại, thành 1 gói duy nhất do đó có thể đem chạy bất kỳ ở đâu, bất kỳ môi trường nào cho dù là linux, macos hay là window miễn là cài đặt docker là nó luôn chạy được.

# Khái niệm về container và image ?

- Docker đóng gói toàn bộ ứng dụng cùng thư viện và nền tảng thực thi của chúng thành một gói, 1 package duy nhất thì những package mà docker tạo ra ở đây chính là images và khi CHẠY 1 images chúng ta có container. Về cơ bản thì container hoạt động chính xác như 1 máy ảo với đẩy đủ tính năng để cài đặt và chạy bất cứ phần mềm nào bên trong nó như là file system, network interface, process tree,... đều có đẩy đủ trong container, tuy nhiên tuy là chức năng giống nhau nhưng mà container KHÔNG thực sự là máy ảo, sự khác biệt lớn nhất đó là container nó không có bộ kernel, nó không có bộ nhânhệ điều hành của riêng nó mà tất cả các container đều sử dụng chung bộ nhân của máy chủ vật lý nên suy cho cùng container chỉ đơn thuần là các tiến trình chạy trên hệ điều hành và được docker quản lý.

# Một chút giới thiệu về OS kernel

- Kernel không phải là phần cứng, nó là phần mềm, kernel là một phần của hệ điều hành, nó cốt lõi, là hạt nhân, trái tim của hệ điều hành, kernel điều khiển mọi thứ bên trong máy tính của bạn, kernel nằm ở giữa và điều khiển sự giao tiêp giữa phần cứng và phần mềm (xem hình ở slide). Mỗi hệ điều hành khác nhau sẽ có phần kernel khác nhau, tuy nhiên nếu chúng có kernel giống nhau có nghĩa là nghĩa là chúng có cùng một phả hệ.

# So sánh container và VM (máy ảo) khác nhau như thế nào ? (xem hình ở slide)

- Máy ảo sử dụng công nghệ ảo hóa ở tầng hardware, tầng phần cứng của thiết bị, máy chủ vật lý sẽ cài lên trên 1 cái phần mềm ảo hóa chuyên dụng như là vmware,... Chúng có nhiệm vụ phân chia tài nguyên cho các máy ảo được cài bên trên, mỗi máy ảo này có hẳn 1 hệ điều hành độc lập ở bên trong nó dung lượng lên đến hàng chục gb, chúng sử dụng phần nhân, phần kernel của riêng nó cho nên bạn có thể cài bất kỳ, có thể cài máy ảo ubuntu chạy trên window or macos chạy lên window mà không gặp bất kỳ vấn đề gì.
  
- Container sử dụng công nghệ ảo hóa ở tầng software application, bản thân docker chính là công nghệ ảo hóa đó, sự khác biệt ở đây là container tận dụng lại phần nhân, phần lõi kernel của hệ điều hành máy chủ vật lý do đó nó không tốn dung lượng để cài thêm hệ điều hành nào cả, điều này dẫn đến việc dung lượng của container chỉ tầm vài chục mgb đổ lại lên cực kỳ nhẹ nhưng đổi lại nó không có sự tự do như là máy ảo, chúng ta không thể chạy được container ubuntu ở trên hệ điều hành window được, điều này lý giải tại sao khi mà cài docker lên window thì chúng ta cần kích hoạt hyperV và virtualbox và setup máy ảo linux trên đó để cài docker chạy được trên window.

# Docker làm thế nào để có được sự khác biệt đó

- Về bản chất công nghệ bên dưới Docker được viết bằng Golang và docker đã tận dụng được những tính năng được cung cấp bởi nhân linux như là:
  
+ Namespaces: để phân chia tách biệt các container với nhau.
  
+ Control groups: để tối ưu hóa tài nguyên phần cứng.
  
+ ...

# Tại sao phải cần Docker ?

- Việc deloy và ứng dụng sẽ dễ dàng hơn rất nhiều với docker. Chúng ta không cần phải nhớ là hệ thống nào, chạy bằng ngôn ngữ gì, phải cài đặt những thư viện gì, nền tảng gì, version bao nhiêu, chúng ta chỉ cần cài docker và mọi thứ đễu xong.
  
- Ngoài ra khi nói tới docker chúng ta không thể không nói tới Microservies. Docker có 3 tính chất sau:

+ Immutable: tính bất biến, nghĩa là ứng dụng của mình khi mà chạy máy ở local thế nào thì khi đóng gói nó lại và mang đi đâu chạy thì nó cũng y chang như vậy không khác được.
  
+ Lightweight: Vì chỉ vài chục mgb nhẹ hơn máy ảo rất là nhiều nên việc tạo container là rất nhanh chóng

+ Stateless: container không tạo ra thay đổi dữ liệu bên trong nó, điều này có được là do tính bất biến ở bên trên, có thể hiểu đơn giản bởi vì không chứa dữ liệu ở bên trong nên các container giờ đây cực kỳ tự do và linh hoạt, chúng ta có thể nhân bản các container lên 100, container giống hệt nhau để đáp ứng đủ traffic người dùng vào giờ cao điểm hoặc bạn có thể shutdow nó ở máy này và dựng nó lên ở máy khác mà vẫn chẳng ảnh hưởng gì đến hệ thống ban đầu.
  
# Slide xem hình: https://drive.google.com/file/d/1LXxBCmt2EpbuAStiVsOeHfYzl0jy4BeF/view?usp=sharing