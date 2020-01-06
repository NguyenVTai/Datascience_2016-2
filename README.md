# Datascience_2016-2
Đồ án cuối kỳ môn Data Science

Bài toán đặt ra: Dự đoán thời tiết của 3 tiếng sau dựa trên thông tin thời tiết hiện tại.

Input: Giờ, Tháng, Nhiệt độ,Nhiệt độ cảm nhận được, Tốc độ gió,Hướng gió,Gió giật,Độ che phủ của mây, Độ ẩm,lượng mưa, áp suất và giá trị thời tiết hiện tại. Input có thể lấy từ các cảm biến tự lắp đặt hoặc các trang mạng có cập nhật tình hình thời tiết. Ví dụ: https://www.worldweatheronline.com/ho-chi-minh-city-weather-history/vn.aspx

Output: Giá trị thời tiết của 3 giờ sau

# 1. Lấy dữ liệu:

Dữ liệu được lấy từ trang: https://www.worldweatheronline.com/ho-chi-minh-city-weather-history/vn.aspx

Hỉnh ảnh mô phỏng các thao tác lấy dữ liệu:
https://media.discordapp.net/attachments/513323420428271647/655663026313232407/unknown.png

Code lấy dữ liệu trong file crawl_data.ipynb
Để code hoạt động, cần cài đặt thư viện selenium, request html, và để file chromedriver.exe (hoặc bất cứ driver của browser nào bạn muốn sử dụng) chung với thư mục của file chứa code

Mô tả dữ liệu:
- Dữ liệu được lưu trong file weather_data.csv
- Dữ liệu được thu thập từ ngày 01/01/2017 đến 31/11/2019
- Dữ liệu gồm 12 cột 8512 dòng
- Ý nghĩa của các cột:
    - time: thời gian (str, format hh:mm)
    - month: tháng (float) (tạm thời, có thể chuyển về str)
    - temperature: nhiệt độ môi trường (float, độ C)
    - feelslike: nhiệt độ cảm nhận được (float, độ C)
    - wind: tốc độ gió (float, km/h)
    - direction: hướng gió (str)
    - gust: tốc độ gió tối đa (float, km/h)
    - cloud: độ che phủ của mây (float, %)
    - humidity: độ ẩm không khí (float, %)
    - precipitation: lượng mưa (float, mm)
    - pressure: áp suất không khí (float, bm)
    - weather: tình hình thời tiết hiện tại (str)   
# 2. Lấy nhãn:
- Lấy giá trị của cột weather (lưu vào 1 list)
- Chuyển giá trị về tập {norain,ltrain,rain}(không mưa, mưa nhỏ, mưa) (phải tự phân loại do cần đọc hiểu)
- Vì giá trị của của nhãn chính là giá trị của weather của dòng dữ liệu sau nên ta xóa đi nhãn đầu tiên (do nó không thuộc về dòng dữ liệu nào cả)
- Đồng thời xóa đi dòng cuối cùng trong tập dữ liệu (do không có nhãn tương ứng)
 
# 3. Tiền xử lý dữ liệu:
   # 3.1: Dữ liệu trống
    
- Về vấn đề dữ liệu thiếu, dữ liệu thiếu thu được có giá trị là 0
- Vắn đề này đặt ra câu hỏi là có nên giữ lại cột precipitation (lượng mưa) hay không, do số lượng 0 rất lớn nhưng nó có thể không phải dữ liệu trống
    - Nhóm quyết định bỏ cột, do có quá nhiều só 0 dễ gây overfitting 
- Dù cho giá trị thiếu trong mô hình là 0 tuy nhiên vẫn phải xét trường hợp dữ liệu thiếu là nan vì khi truyền tập test, mọi chuyện đều có thể xảy ra
- Các cột có giá trị số sẽ điền vào giá trị trống là giá trị trung bình của cột đó
- Các cột có giá trị kiểu category thì điền vào giá trị xuất hiện nhiều nhất
    # 3.2: Category:
- Cột time: chuyển [6:00, 9:00, 12:00, 15:00] về "sang" [18:00,21:00,0:00,3:00] về "toi"
- Cột month: chuyển [5,6,7,8,9,10,11] thành "mua" và [12,1,2,3,4] thành "kho"
- Cột direction: Giá trị biểu diễn 16 hướng, nhóm bỏ đi ký tự đầu tiên nếu mã là 3 ký tự để chuyển về 8 hướng 
    - Ví dụ: "NNE" sẽ chuyển thành "NE" còn "SE" hoặc "E" sẽ vẫn giữ nguyên
- Cột weather: Chuyển giá trị hiện tại thành 1 trong 3 giá trị "rain"(có mưa) "ltrain"(ít mưa) và "norain"(không mưa)
    - Nhóm tự tạo ra 3 list để lưu giá trị (bước này làm bằng tay do phải đọc hiểu)
        list_norain = ['Clear', 'Cloudy', 'Mist', 'Sunny', 'Partly cloudy', 'Thundery outbreaks possible']
        list_ltrain = ['Light drizzle','Light rain','Light rain shower', 'Patchy light drizzle', 'Patchy light rain', 'Patchy light rain with thunder','Patchy rain possible']
        list_rain = ['Heavy rain','Heavy rain at times','Moderate or heavy rain shower','Moderate rain', 'Moderate rain at times', 'Overcast','Torrential rain shower']
    - Bước này giống với lúc lấy nhãn dữ liệu
- Cuối cùng là chuẩn hóa lại giá trị trong bảng bằng StandardScaler
# 4. Áp dụng mô hình
- Nhóm khảo sát 2 mô hình là multi-layer perceptron, và support vector machine (sử dụng gbf kernel)
- Ở mô hình multi-layer perceptron nhóm thử nghiệm với alpha = [0.001,0.01, 0.1, 1, 10, 100] và hidden layer sizes = [1,10,20,50,100]
    - Kết quả: độ lỗi thấp nhất trong validation set là 19.582245430809397 khi alpha = 10, là hidden layer sizes là 50
- Ở mô hình SVM nhóm thử nghiệm với C=[0.1,1,10, 20, 50,100] và gamma  = [0.0001,0.001,0.01,0.1]
    - Kết quả: 19.647519582245433 khi C = 50 và gamma = 0,01
- Về độ chính xác, MLP nhỉnh hơn 1 chút, tuy nhiên nhóm không dựa vào đó mà cần thêm đánh giá về precision và recall:
- Report của MLP:

              precision    recall  f1-score   support

      ltrain       0.70      0.49      0.58      1105
      
      norain       0.87      0.97      0.92      5776
      
        rain       0.72      0.43      0.54       778


    accuracy                           0.85      7659
    
   macro avg       0.76      0.63      0.68      7659
   
weighted avg       0.83      0.85      0.83      7659

- Report của SVM:

              precision    recall  f1-score   support

      ltrain       0.71      0.35      0.47      1105
      
      norain       0.84      0.98      0.90      5776
      
        rain       0.75      0.35      0.47       778


    accuracy                           0.82      7659
    
   macro avg       0.76      0.56      0.61      7659
   
weighted avg       0.81      0.82      0.80      7659

- Nhóm thấy nên quyết định dựa trên recall do điều quan trọng là tìm được đúng nhãn là mưa hoặc ít mưa
- Mô hinh được chọn sau cùng là MLP

# 5. Áp dụng trên tập test và kết luận:
- Kết quả của mô hình trên tập test:
    
                  precision    recall  f1-score   support


      ltrain       0.57      0.38      0.46       123
      
      norain       0.86      0.95      0.90       643
      
        rain       0.66      0.43      0.52        86


    accuracy                           0.81       852
    
   macro avg       0.69      0.59      0.63       852
   
weighted avg       0.79      0.81      0.80       852

- Kết luận: mô hình có thể sẽ không hoạt động tốt khi áp dụng vào thực tế do cả precision và recall của 2 nhãn ltran và rain đều khá thấp. Tuy nhiên, nếu không yêu cầu độ chính xác quá cao, mô hình có thể xem như một thông tin tham khảo trong trường hợp bất đắc dĩ
