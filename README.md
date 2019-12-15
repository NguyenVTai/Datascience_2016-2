# Datascience_2016-2
Đồ án cuối kỳ môn Data Science
Đề tài:
ĐỒ ÁN DỰ ĐOÁN THỜI TIẾT THỜI GIAN THỰC

Bài toán đặt ra: Dự đoán thời tiết của 3 tiếng sau dựa trên thông tin thời tiết hiện tại.

Input: Giờ, Tháng, Nhiệt độ,Nhiệt độ cảm nhận được, Tốc độ gió,Hướng gió,Gió giật,Độ che phủ của mây, Độ ẩm,lượng mưa, áp suất và giá trị thời tiết hiện tại

Output: Giá trị thời tiết của 3 giờ sau

# 1. Lấy dữ liệu:

Dữ liệu được lấy từ trang: https://www.worldweatheronline.com/ho-chi-minh-city-weather-history/vn.aspx

Hỉnh ảnh mô phỏng các thao tác lấy dữ liệu:
https://media.discordapp.net/attachments/513323420428271647/655663026313232407/unknown.png

Mô tả dữ liệu:
- Dữ liệu được lưu trong file weather_data.csv
- Dữ liệu gồm ? cột ? dòng
- Ý nghĩa của các cột:
    time: thời gian (str, format hh:mm)
    month: tháng (float) (tạm thời, có thể chuyển về str)
    temperature: nhiệt độ môi trường (float, độ C)
    feelslike: nhiệt độ cảm nhận được (float, độ C)
    wind: tốc độ gió (float, km/h)
    direction: hướng gió (str)
    gust: tốc độ gió tối đa (float, km/h)
    cloud: độ che phủ của mây (float, %)
    humidity: độ ẩm không khí (float, %)
    precipitation: lượng mưa (float, mm)
    pressure: áp suất không khí (float, bm)
    weather: tình hình thời tiết hiện tại (str)
# 2. Gán nhãn và tiền xử lý dữ liệu:

# 3. Áp dụng mô hình:
