# DepthOfMarket - Smart DOM Lmax
Display DOM on MT4 (MT5) platform

1. Giới thiệu
Công cụ Smart DOM LMAX hỗ trợ hiển thị DOM (Depth Of Market) của sàn LMAX (Một sàn lớn và đồng thời là nhà cung cấp thanh khoản Liquidity Provider (LP), do vậy khối lượng giao dịch lớn, thể hiện tốt mức độ thanh khoản của thị trường. LMAX hỗ trợ DOM đến level 20 (Top of order book).
Công cụ này hiện thị khối lượng theo mức giá trên Chart của MT4/5, đồng thời tính tổng khối lượng (Volume) các lệnh theo ASK và BID, từ đó giúp người dùng thấy được sự mất cân bằng (Imbalance - Disequilibrium) của Market hiện tại, giá luôn có xu hướng dịch chuyển về mức cân bằng. Đồng thời công cụ thể hiện mức giá có khối lượng lớn nhất trong DOM (Đó sẽ kháng cự/hỗ trợ cứng).
Smart DOM sử dụng thông qua một máy chủ trung gian để lấy dự liệu từ sàn LMAX (https://www.lmax.com/) và truyền dữ liệu (chuyển tiếp) về MT Client thông qua giao thức tcp, udp, pgm, norm, ipc, inproc, gssapi bằng thư viện (Lib) ZeroMQ.

============
2. Cài đặt
 * Xác định thư mục Data của MT 4(MT5): Vào Menu File -> Open Data Folder (Ctrl + Shift + D)
 
2.1 Cài đặt các thư viện hỗ trợ ZeroMQ
 - Thư viện ZeroMQ hỗ trợ ngôn ngữ MQL4/5 dành cho MetaTrader4/5 tên là mql-zmq, được phát triển bởi https://github.com/dingmaotu/mql-zmq. Các bạn có thể download trên đó, hoặc trực tiếp trên repo này (mục release).
 - Copy các file tương ứng vào thư mục Data của MT4/5 (Mục MQL4/MQL5)
  + Include/Zmq
  + Include/Mql
  + Library/MT4 (32 bits) - MT5 32 bits sử dụng chung Lib này với MT4. Copy các file dll vào MQL4\Libraries (Không phải cả thư mục MT4)
  + Library/MT5 dành cho MT5 64 bits.
  * Lưu ý: các thư viện DLL trong thư mục Library được xây dựng trên nền tảng Visual C++ runtime (2015), vì vậy phải cài đặt Microsoft Visual C++ 2015 trước, tải và cài đặt tại https://www.microsoft.com/en-us/download/details.aspx?id=53587 hoặc trong bộ Release này (Cài đặt vc_redist.x64.exe cho bản 64 Bits (MT5) và vc_redist.x86.exe cho bản 32 Bits (MT4).
  
2.2 Cài đặt Indicator Smart DOM LMAX trên MT
 - Copy file LMAXSmartDOM.ex4 vào thư mục MQL4\Indicators
 - Cấu hình MT4 cho phép Indicator sử dụng thư viện ZeroMQ DLL: Vào menu Tools -> Options -> Expert Advisors -> Tích chọn Allow DLL Imports
3. Hướng dẫn sử dụng
 3.1 Cấu hình các thông số:
  - Protocol: Giao thức để cập nhật dữ liệu từ DOM Server (Máy chủ chuyển tiếp dữ liệu DOM của LMAX)
  - DOM Server: Địa chỉ của máy chủ DOM Server (dùng domain hoặc ip address)
  - DOM Port: Cổng kết nối đến máy chủ DOM Server
  - Symbol (Currency Pair): Tên của cặp tiền tệ hoặc tài sản muốn lấy dữ liệu DOM, trong trường hợp tên khác với danh sách các cặp hỗ trợ, hoặc tên có cấu trúc khác biệt nên chỉ định rõ ràng, VD một số sàn có tên gọi khác nhau cho Spot Gold chẳng hạn (mặc định chuẩn là XAUUSD), vui lòng tham khảo trong danh sách các cặp hỗ trợ, nếu có sự khác biệt vui lòng chỉnh tham số này.
  - Auto Symbol (Currency Pair): Tự động lấy giá trị tên cặp tiền tệ/tài sản trên Chart mà Indicator Smart DOM được tích hợp vào (Tự động lấy 6 chữ cái đầu, VD XAUUSDm thì tool sẽ tự lấy giá trị XAUUSD).
  - Update Rate: Tốc độ cập nhật dự liệu về (tính theo ms), giá trị càng nhỏ thì dữ liệu lấy về càng nhiều, và cũng sẽ tốn tài nguyên hơn (nên sử dụng ở mức 500 ms hoặc cao hơn nếu không có nhu cầu cập nhật liên tục).
  - Max Volume: Giới hạn khối lượng của lệnh tối đa được cập nhật, nên để mặc định là 0 (không giới hạn)
  - Width (% of chart): Giới hạn vùng hiển thị trên chart, tool sẽ vẽ vùng BID, ASK và các thanh bar thể hiện độ lớn của Volume lệnh, vị trí lệnh tương ứng với giá của lệnh.
  - Draw Background: Vẽ các thành phần mô phỏng DOM trên chart dưới dạng ảnh nền (sẽ ko che đi các thành phần khác trên Chart)
  - Color Bid Bar: Mầu sắc hiển thị thanh bar thể hiện độ lớn Volume của lệnh tại mức giá Bid
  - Color Ask Bar: Mầu sắc hiển thị thanh bar thể hiện độ lớn Volume của lệnh tại mức giá Ask
  - Color Bid Area: Mầu sắc hiển thị thanh bar thể hiện vùng của lệnh Bid
  - Color Ask Area: Mầu sắc hiển thị thanh bar thể hiện vùng của lệnh Ask
  - Width Of Histogram Bar: Độ rộng của thanh bar thể hiện Volume.
  
  ============
  DANH SÁCH CÁC CẶP TIỀN TỆ/TÀI SẢN HỖ TRỢ BỞI SMART DOM LMAX:
  XAUUSD, XAGUSD, GBPUSD, EURUSD, AUDUSD, NZDUSD, USDCAD, USDJPY, USDCHF, AUDCAD, EURCAD, EURCHF, EURGBP, EURJPY, AUDJPY, AUDNZD, CADJPY, EURAUD, EURNZD, GBPAUD, GBPCAD, GBPCHF, GBPJPY, GBPNZD, NZDCAD, NZDCHF, NZDJPY, XTIUSD, XAUAUD, XAUEUR, XRPUSD (XRP), XBTUSD (Bitmex BTC), XETUSD (Bitmex ETH), AUS200, UK100, SPX (SP500), WS30 (US30).
*** Lưu ý: Cấu hình chính xác giá trị Symbol (Currency Pair) đúng như trong danh sách này để Tool hoạt động ổn định.
*** Không nên bật quá nhiểu Chart để tránh quá tải cho MT
*** Nếu chất lượng đường truyền Internet hạn chế, có thể làm chậm hoặc treo đơ MT
*** Nếu MT4 bị treo đơ không thể tiếp tục, hãy tắt MT4 và xóa chart (File: .chr trong thư mục Profile của MT4) rùi bật lại MT4.
5. DOWNLOAD
 5.1 Download bộ cài đặt: https://github.com/siwtom/DepthOfMarket/releases
 5.2 Download hướng dẫn: https://github.com/siwtom/DepthOfMarket/blob/master/GuideSmartDoMLmax.docx

Mọi thắc mắc vui lòng liên hệ Telegram: https://t.me/siwtom
