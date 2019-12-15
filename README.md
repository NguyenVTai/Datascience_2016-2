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
- Dữ liệu gồm ? cột ? dòng
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
-   Dữ liệu có một vài giá trị bằng 0 ở cột "temperature" và điều đó là vô lý. Nêu nhóm mình đã thay thế bằng dữ liệu của cột feelslike vào thời điểm đó
# 2. Gán nhãn:
Gán nhãn:
- Thêm cột label vào dữ liệu
- Giá trị của của cột label đó chính là giá trị của weather của dòng dữ liệu sau
- Dòng dữ liệu cuối, do không thể gán nhãn nên bỏ đi
 
# 3. Tiền xử lý dữ liệu và áp dụng mô hình:
# ??. Các vấn đề cần giải quyết:
- Giảm số lượng class ở label (và weather)
- Mô hình hóa dữ liệu, tìm mô hình phù hợp
- Tiền xử lý các thuộc tính về mặt kiểu dữ liệu và giá trị để phù hợp với mô hình
- Áp dụng mô hình và đi đến kết luận
