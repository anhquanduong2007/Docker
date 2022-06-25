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
  
# Các khái niệm cơ bản 

- Docker Engine: Docker là tên của công ty còn phầm mềm mà công ty đó cung cấp để cài vào máy thì chúng ta ta gọi là docker engine. Docker engine bao gồm 2 module chính là Docker Daemon là module đầu tiên và cũng là module cốt lõi đóng vai trò là server chạy ngầm bên dưới hệ thống, bộ câu lệnh cli là module thứ 2 đóng vai trò là client và chúng ta sẽ sử dụng bộ câu lệnh này để giao tiếp với cục Docker Daemon thông qua giao thức https. Như đã trình bày hồi nãy, các container sử dụng chung kernel với hệ điều hành gốc được cài trên máy chủ vật lý nên chúng bị phụ thuộc vào hệ điều hành đó. Hệ điều hành window chỉ có thể chay được container giành cho window như là .net core, .net framework, do bị giới hạn như vậy nên là hầu như mỗi lần cài docker trên window, docker sẽ yêu cầu chúng ta kích hoạt hyperV hoặc virtualbox và cài lên nó một máy ảo linux và lúc này cục docker daemon sẽ được cài vào trong cái máy ảo linux đó còn cài bộ câu lệnh cli vẫn được cài trên máy window của chúng ta. (xem ảnh ở slide)
  
- Registry: Docker đóng gói ứng dụng thành cái image vậy thì phải có cách nào đó để chúng ta tải image này lên mạng và chia sẻ với các máy khác thì cái nơi mà chúng ta dùng để lưu trữ các image này chúng ta gọi là container registry. Thì nay trên thị trường có những nhà cung cấp dịch vụ lưu trữ image khác nhau như là Docker Hub do chính docker làm ra, Amazion Elatic Container Registry (ECR), ...

# Một số lệnh cơ bản

Syntax: docker (component) (command)

- (component): tên đối tượng cần tương tác và có 4 đối tượng phổ biến là:
  + image
  + container
  + network
  + volume
  + ...
  + 
- (command): mã lệnh gồm các mã lệnh phổ biến như:
  + ls: list
  + run
  + exec
  + stop
  + pull
  + prune
  + ...

- Với đối tượng image chúng ta có các lệnh sau
  
+ docker image pull (image) - docker image pull (image):(tag) => dùng để tải image từ registry về, thì mỗi image nó sẽ có nhiều tag khác nhau dùng để đánh version của các image đó nếu không để tag mặc định là lastest.
  
+ docker image push (image):(tag) => dùng để taỉ image đã build ở local lên registry

+ docker image ls || docker images => dùng để liệt kê image đang có trong máy local
  
+ docker image prune => dùng để xóa toàn bộ image không còn được dùng nữa thì thỉnh thoảng nên chạy lệnh này để dọn dẹp image có trong máy của mình
  
!! Short-hand: 
+ docker pull
+ docker push
  
- Đối tượng container

+ docker container run (image) => để chạy 1 container dùng lệnh run kèm theo tên của image

+ docker container ls || docker container ls -a => để list ra các container đang chạy, tuy nhiên muốn list ra kể cả container đã shut down thì chúng ta phải thêm cờ a => cách viết ngắn gọn hơn docker ps || docker ps -a

+ docker container stop (container_id) => dùng để stop container đang chạy với tham số là id or tên của container đó

+ docker container prune => tương tự như trên lệnh prune có tác dụng xóa toàn bộ container đã được shutdown không còn được dùng nữa

+ docker container exec (container_id) (command) => dùng để chạy 1 câu lệnh bên trong container bất kỳ.

!! Short-hand:
+ docker run
+ docker stop
+ docker exec

# Chu trình sống của container

- Khi chạy 1 image chúng ta có container, container lúc này ở trạng thái sống là up, khi stop or kill nó đi thì container ở trạng thái exited, khi dùng lệnh start để chạy lại container đã exited thì nó nó sống lại ở trạng thái up, khi mà container đã exited và ta remove hoặc là ta dùng lệnh prune thì nó sẽ rơi vào trạng thái dead => thực ra chúng ta sẽ không thấy container nào nữa bởi vì lúc đó docker sẽ xóa sạch để giải phóng bộ nhớ.

- Để tìm hiểu sâu hơn về container life cyle thì chúng ta sẽ tìm hiểu về một thứ là PID 1. Chu kỳ sống của container được quyết định bởi tiến trình có PID = 1 chạy ở bên trong nó hay còn được goi là tiến trình chủ đạo. CHừng nào tiến trình PID 1 còn sống thì container vẫn còn sống. Khi sử dụng lệnh docker stop thì thực chất là docker đang gửi 1 tín hiệu SIGTERM (terminating signal) đến để kết thúc tiến trình PID =  1 ở bên trong container, container sau đó sẽ kết thúc với exited code = 0.
  
- Trong vòng 10s sau khi chạy lệnh docker stop mà nếu vì một lỗi nào đó mà container chưa chịu dừng lại thì container sẽ gửi đi một tín hiệu thứ 2 là SIGKILL (kill signal) để ép buộc tiến trình PID 1 phải dừng ngay lập tức khi đó exited code của container sẽ là 137

# Đôi chút về alpine (xem qua)

- alpine là hệ điều hành linux đã được rút gọn đến mức tồi giản, cực kỳ nhẹ chỉ vài mgb nếu như là container, nếu như là hệ điều hành bình thường thì là 100 mấy m đổ lại thì điều đó phù hợp với triết lý của docker là càng nhẹ càng tốt và không những thế nó còn có tính bảo mật cao.
  
- Tại sao khi chạy docker run alpine và khi check status trong lệnh docker ps -a chúng ta thấy exited = 0. Tại sao có chuyện này, tại sao container lại kết thúc ngay lập tức mà nó không ở trạng thái up đúng như những gì chúng ta nói hồi nãy. thì hay đễ ý ở phần docker ps -a, ở cột command thì cột command này chính là lệnh mà docker sẽ chạy lệnh đầu tiên khi mà khởi tạo container, ở đây ta thấy "/bin/sh", vì sh chính là 1 terminal, shell cũng như 1 command line như window là phần mềm dùng để tương tác với hệ điều hành bằng các dòng lệnh, thì bản chất shell terminal là tiến trình cần có sự tương tác với người dùng, nhưng vì bên trong container của chúng ta đang khởi tạo ở đây không có user nào cả nên là cái shell terminal này sẽ kết thúc ngay lập tức mà shell terminal này là tiến trình PID chủ đạo, tiến trình PID = 1 là chủ đạo nên là dẫn đến việc nó kết thúc thì sẽ dẫn đến việc container sẽ kết thúc này lập tức => vậy thì phải làm sao, vậy thì chúng ta cần phải gắn thêm cờ -it: docker run -it alpine và khi chạy lệnh này, chính chúng ta đang là người sử dụng shell terminal ở ngay bên trong container. Vậy thì nến nhớ mỗi khi mà chúng ta cần chạy 1 cái container nào MÀ CÓ SỰ TƯƠNG TÁC VỚI NGỪOI DÙNG thì chúng ta phải thêm 2 cái cơ i,t này vào
  
 + -i or --interactive
 + -t or --tty
 + Để thoát khỏi container này thì là Ctrl + D or gõ lệnh exit => khi ấn xong gõ lệnh docker ps -a check status thì thấy nó có exited = 130 mà không phải bằng 0 có nghĩa là nó là do chúng ta kêt thúc chứ không phải do docker kêt thúc và điều này có nghĩa là sao, có nghĩa là khi chúng ta nhấn Ctrl + D chúng ta đã thoát khỏi cái shell termninal của container mà chúng ta hãy nhớ shell termnial chính là cái PID = 1, cái tiến trình chủ đạo bên trong container khi mà tiến trình chủ đạo kết thúc thì đồng nghĩa với việc là container nó kết thúc và chấm dứt luôn đó chính là lý do vì sao khi ấn ctrl + D thì container này nó cũng thoát

=> Câu hỏi đặt ra là vậy làm sao mà mình chỉ muốn thoát ra tạm thời thôi, mình thoát ra tạm thời container đó và lát sau mình muôn và mình có thể quay lại được để tiếp tục làm công việc dở dang của mình ở bên trong container đó thì ở đây chúng ta có khái niệm mới đó là chế độ của container là cái mode của nó. Chúng ta có 2 chế độ là attach và -detach. Khi mà chúng ta có thể tương tác trực tiếp với container ngay bên trong cái shell terminal của nó như hồi nãy thì chúng ta gọi là chúng ta đang attach cái container đó vào cái command line của chúng ta nó đang ở chế độ attach. Khi muốn thoát ra tạm thời khỏi cái container đấy để làm cộng việc gì đó thì ấn ctrl + P + Q thì lúc này cái container sẽ vẫn chạy ngầm ở bên dưới nhưng mà nó không còn hiện lên trên màn hình nữa thì chế độ đó chúng ta gọi là detach là chúng ta đã detach cái container đó ra khỏi cái màn hình command line của chúng ta, và khi chúng ta muốn hiện lại container đó lên lại màn hình command line thì chạy lệnh docker attach (container_id) or (name) của container đó và từ đây chúng ta sẽ có thêm 1 cái option mới khi mà khởi tạo container đó chính là khi mà chúng ta muốn nó bắt đầu chạy ở ngay trạng thái detach luôn, là nó chạy ngầm bên dưới luôn không còn hiện lên màn hình nữa thì chúng ta gắn cho 1 cái cờ d docker run -d (image).

# Thực thi câu lệnh bên trong container

Syntax: docker exec (container_id) (command)

- docker exec là một lệnh đặc biệt dùng để thực hiện 1 câu lệnh bên trong container đang chạy, bất cứ container nào đang chạy. Ví dụ
  
  + docker exec (container_id) echo Hello World
  + docker exec (container_id) echo $PATH
  + ...

- Nếu như ai chưa biết thì SSH là tên 1 loại giao thức bảo mật tuy nhiên trong thuật ngữ của lập trình viên thì SSH cũng có nghĩa là cái thao tác kết nối từ xa tới một cái máy tính cho dù là máy ảo or máy vật lý, có nghĩa là các bạn đứng ở 1 cái máy của mình kết nối tới 1 cái máy khác nó nằm ở trên môi trường cloud hoăc kết nối tới một cái máy của đồng nghiệp các bạn đang ở cách các bạn 1 khoảng khá xa thì chúng ta gọi đó là thao tác SSH. thì đây là câu lệnh docker exec -it (container_id) sh => đễ chúng ta có thể SSH vào bất kỳ 1 container nào đang chạy. Như các bạn biết là container nó hoạt động như 1 cái máy ảo và nó độc lập với máy local của chúng ta, nó độc lập với cái máy chúng ta đang chạy, thao tác SSH vào là 1 thao tác quan trọng nếu như chẳng may container của chúng ta đang gắp sự cố thì bản chất cái việc SSH này nó y hệt như cái việc mà chúng ta tương tác với cái shell terminal. Như chúng ta thấy trong câu lệnh có sh bản chất của sh là chúng ta đang mở 1 cái shell terminal ở bên trong container này và bởi vì cái shell đó nó cần sự tương tác nên chúng ta cần cung cấp cái cờ -it ở đằng trước. Việc mà chúng ta thao tác ssh khác với lại cái việc mà chúng ta attach cái việc container lại màn hình, khi mà chúng ta attach lại container vào màn hình thì chúng ta sử dụng chính cái tiến trình có PID = 1 là câu lệnh mặc định, tiến trình chủ đạo của container đó, còn khi chúng ta ssh vào bằng câu lệnh exec là chúng ta đang mở 1 cái tiến trình thứ cấp, một cái tiến trình phụ chạy song song với tiến trình chủ đạo ở bên trong container đó nên khi chúng ta ấn ctrl + D or exit thì khi check status qua lệnh docker ps -a thì vẫn thấy status là up do đây chỉ là 1 tiến trình thứ cấp nên nó có bị kết thúc đi chăng nữa nó không bị ảnh hưởng đến tiến trình chủ đạo và trừng nào tiến trình chủ đạo đang chạy thì cái container vẫn tiếp tục sống.

- Chúng ta có thể thay lệnh sh bằng bash docker exec -it (container_id) bash, như các bạn biết thì cái bash cũng là 1 cái shell terminal luôn, tuy nhiên không phải terminal nào cũng cài sẵn bash bên trong nó.

# Port mapping

- Có một câu hỏi là sau này không lẽ mỗi lần mỗi tương tác với container thì chúng ta cứ phải chạy 1 câu lệnh dài như vậy để kết nối bên trong để có thể tương tác với các thứ thì nó hơi phức tạp thì port mapping chính là giải pháp. Thực tế chúng ta deloy ứng dụng lên web chúng ta không thể để cho user ssh trực tiếp vào bên trong container của chúng ta, tại vì thứ 1 user không biết về khái niệm ssh là cái gì? họ chỉ biết là tôi muốn vào địa chỉ trang web này thì tôi vào được ứng dụng của tôi và cái việc ssh vào container như vậy thì nó rất là kém bảo mật nên là cái việc port nào, port bao nhiêu và container nào là việc của chúng ta, làm sao để cho user chạy 1 cách dễ dàng nhất.

Syntax: docker run -p (target_port):(container_port) ...
  + target_port là port ở local
  + container_port là port của container

- Để xem logs của container thì có lệnh: docker logs -f (container_id), cờ f ở đây có nghĩa là keep following nghĩa là giữ cho màn hình in log liên tục theo thời gian

# Bind mount

- Các image có tính Immutable là bất biến, không thể chỉnh sửa được do vậy một khi mà chúng ta chạy 1 container bất kỳ cho dù chúng ta có chỉnh sửa cái dữ liệu nội dung bên trong của cái container đó cho đến đâu đi nữa thì nó cũng không ảnh hưởng tới cái image gốc ban đầu của chúng ta, và nó cũng như nó cũng chẳng ảnh hưởng tới container khác => điều này dẫn đến việc dữ liệu bên trong container không được bảo toàn là tắt cái container đi thì mọi dữ liệu mất hết do đó container nó có cái tính stateless, cái tính stateless này là hệ quả của thằng immutable. Giờ ví dụ mình muốn chạy 1 container bất kỳ để mình làm database thì mình phải làm sao để có thể keep lại dữ liệu bên trong nó, làm sao để khiến nó stateful thay vì stateless thì chúng ta sẽ sử dụng 1 cái thứ gọi là volume.
  
- Volume là gì? volume là cái phần bộ nhớ mà docker dùng để lưu trữ dữ liệu cho container, thì nói ngắn gọn volume là một cái thư mục ảo do docker tạo ra và quản lí việc mà chúng ta cần làm là lấy cái volume đó gắn vào container là xong thì cái thao tác mà đem cái volume này đi gắn vô container thì chúng ta gọi là bind mount.

- Syntax
  + docker volume create (volume_name)
  + docker run -v (local_dir/volume):(container_dir) ...

!! Nguyên tắc chung là cái gì của local đứng trước và cái gì của container đứng sau.

# Dockerfile

- Dockerfile là 1 file template giúp chúng ta xây dựng image riêng của riêng mình.

ví dụ: 
  FROM alpine:latest
  RUN apk add bask
  CMD ["bash"]

=> dòng đầu tiền chỉ định image mà chúng ta muốn đung dề làm mẫu, dòng tiếp theo là lệnh cài đặt bash ở trên hệ điều hành alpine và dòng cuối dùng được gọi là câu lệnh mồi hook command lệnh này sẽ là lệnh đầu tiên sẽ được chạy khi mà ta khởi tạo container từ image này thì để tạo image thì chúng ta dùng câu lệnh docker build -t (image_name):(tag) . 
để ý đằng sau câu lệnh có 1 cái dầu chấm thì cái dấu chấm này là 1 tham số quan trọng không thể bỏ được, dấu chấm này được gọi là build context. build context là gì? đến cái thư mục mà bạn đang chứa docker file toàn bộ nội dung bên trong các thư mục chứa docker file đó được gọi là build context bao gồm cả cái Dockerfile đó luôn. Khi chạy lệnh docker build trên window client sẽ gửi toàn bộ build context đến docker daemon đang nằm trong máy ảo linux. Và hãy cẩn thận nếu chúng ta vô tình để những file không liên quan bên trong thư mục build context thì dữ liệu gửi đến docker daemon sẽ quá nặng khiến cho việc build image trở nên lâu hơn để ngăn ngừa chuyện đó thì chúng ta có thể sử dụng 1 file là .dockerignore

- From (image): chỉ định image gốc ban đầu mà bạn muốn tạo ra sự thay đổi, lưu ý là chúng ta không thay đổi image gốc ban đầu mà là chúng tạo một cái image mới dựa trên cái image đó 
- RUN (command): giúp chúng ta chạy các câu lệnh trong container
- WORKDIR (directory): sẽ tạo 1 thư mục và đặt thư mục đó là thư mục mặc định của container khi mà được khởi tạo
- COPY (src) (dest): sẽ copy file từ máy local đi vào bên trong cái image đó
- ADD (src/URL) (dest): tương tự lệnh copy nhưng xịn hơn 1 xíu là nó có thể tải file từ internet về và nếu như nó là file nén nó sẽ tự động giải nén
- EXPOSE (port): lệnh này thông báo cái port mà ứng dụng bên trong container đang chạy, lệnh này có tác dụng như là 1 dòng comment cho những cái người đọc tài liệu sau này, đọc cái file docker file của bạn, họ đọc và họ hiểu chứ nó không có tác dụng thực thi, không có tác dụng cấu hình gì cả.
- CMD command argument1 argument2... : câu lệnh mồi, nó chỉ thực thi khi nào mà chúng ta khởi chạy container mà thôi. cách viết thứ 2 là CMD ["command","argument1","argument2",...]. Dạng đầu tiên là dạng Shell form dạng thứ 2 gọi là exec form thì dạng shell form thì toàn câu lệnh CMD sẽ được bọc vào 1 cái shell và chạy bên trong đó còn dạng thứ 2 là dạng exec form thì container sẽ chạy trực tiếp câu lệnh này mà không thông qua shell (recommend hơn vì nó hạn chế khả năng tấn công shell injection).
- Quy trình docker tạo ra image từ docker file, image là 1 tập hợp nhiều layer. Môi layer tương ứng với 1 câu lệnh trong dockerfile. khi build image docker sẽ duyệt qua từng câu lệnh đó trong dockerfile, qua mỗi câu lệnh docker sẽ khởi tạo 1 container tạm thời từ cái image của layer trước đó tạo ra những thay đổi bên trong container đúng theo mô tả của câu lệnh và sau đó nó sẽ sao lưu cái container này lại thành 1 layer mới và xong xuôi docker sẽ loại bỏ container tạm thời này và tiếp tục lặp lại quy trình cho đến khi nó duyệt qua cái dòng cuối cùng của dockerfile
- Để có thể giảm cái kích thước của cái image cũng như giảm số lượng layer thì chúng ta có thể kết hợp các câu lệnh trên một dòng và toán tử && đồng thời xóa cache sau khi thực thi câu lệnh.
- Sau khi build xong có thể thì có thể tải image lên registry login docker trước với docker login sau đó dùng lệnh docker push (image):(tag).
- Docker cung cấp chức năng luư cache theo từng của image nghĩa là trong lúc build mỗi layer sẽ có 1 cái mã hash tạo ra, lần đầu tiên khi mà tạo lên registry thì toàn bộ layer sẽ được tải lên nhưng mà những lần sau docker sẽ tính toán xem nếu cái mã hash của cáy layer này nó không thay đổi, nghĩa là layer nó không có 1 sự thay đổi nào về mặt data hay là về mặt meta data ở bên trong thì docker sẽ chỉ tải lên nhưng layer có sự thay đổi mà thôi do đó nó sẽ tích kiệm được thời gian tải lên nói riêng cũng như tiết kiệm được thời gian triển khai ứng dụng nói chung. vì một lý do nào đó mà chúng ta không muốn lưu cache thì chúng ta sẽ thêm cái cờ --no-cache vào câu lệnh build và các mã hash lúc này sẽ được sinh mới hoàn toàn

# docker-compose

- Giúp mình có thể triển khai được nhiều container cùng lúc. khai báo trong file docker-compose.yaml
- Viết dấu # để comment
- Cú pháp của file docker-compose.yaml
  version '3'
  services:
    pg: 
      image: postgres:9.6-alpine
      ports: 
        - 5432:5432
      volumes:
        - pgdata:/var/lib/postgresql/data
      enviroment:
        POSTGRES_DB: postgres
    frontend:
      image: nambach/frontend:lastest
      build: 
        context: .
  volumes:
    pgdata:

  + version '3': version của file, version đi theo version của docker engine nên cần chú ý lên trang chủ docker xem cho chính xác thông thường là 3
  + services: khai báo các container, ở đây không gọi container mà mình sẽ gọi là services theo định dạng yaml thì servies sẽ là 1 key của object services này
  + đầu tiên mình có services postgres thì cái này là tên của services lên đặt sao cũng được
  + trong mỗi services thì mình sẽ lần lượt khai báo thông tin để docker biết cách tạo container cho nó, thì đầu tiên tên của image, tiếp đến là port mapping, tiếp đến là bind mount volumes, phải lưu ý là khi mà mình khai báo volumes thì mình sẽ phải khai báo volumes này ở cái phần riêng dưới cuối file compose và nó cùng cấp với services, tiếp đến là cấu hình biến môi trường và khi chạy file này biến môi trường sẽ được chuyền vào bên trong container. Đó là xong 1 services pg thì mình sẽ khai báo thêm services front-end của mình.
  + docker-compose up -d có flag -d là để chạy ngầm
  + docker-compose down sẽ shutdow toàn bộ container, dọn dẹp hiện trường giải phòng bộ nhớ

# Network
- Ví dụ chúng ta có 2 servies là backend và frontend thì bây giờ cục frontend của mình muốn gọi đến cái dục backend theo lý thuyết mình phải có địa chỉ ip của cục backend đển gọi đến cho đùng nhưng mà trong trường hợp này ví lí do nào đó cục backend bị restart và ip nó bị thay đổi thì lúc này mình phải cấu hình lại cục front-end thì nó rất là phiền phức thì docker cung cấp giải pháp là network, chúng ta sẽ tạo ra 1 đối tượng network và gán tất cả container vào trong cái network đó thì nó cũng giống như là việc chúng ta cho các máy tính của văn phòng vào cùng 1 cái mạng lan vậy thì lúc này các container sẽ giao tiếp với nhau thông qua cái mạng lan đó và đặc biệt hơn thông qua tên của services thay vì địa chỉ ip, app front-end sẽ gọi đến app backend bằng chính cái tên services.
  
# Slide xem hình: https://drive.google.com/file/d/1LXxsBCmt2EpbuAStiVsOeHfYzl0jy4BeF/view?usp=sharing