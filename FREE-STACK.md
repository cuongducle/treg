# FREE-STACK — bản phân tách (fork) "miễn phí tối đa" của treg

Bản fork này giữ nguyên treg (máy chủ + danh mục + CLI) và bổ sung những nhà cung cấp (provider) **miễn phí hoặc có mức dùng miễn phí lớn nhất** mà bản gốc chưa liệt kê, kèm hướng dẫn dùng tầng miễn phí của các nhà cung cấp đã có sẵn trong danh mục.

Nguyên tắc cốt lõi của treg khiến việc này khả thi: **gọi bằng key của team bạn thì không bao giờ bị tính phí qua treg** (không đo đếm, không markup). Vậy "miễn phí" = tự đăng ký mức miễn phí (free tier) của từng nhà cung cấp rồi kết nối (connect) vào — tổng chi phí hàng tháng: **0 đô la**.

Chạy máy chủ cục bộ (local server) của bạn:

```bash
scripts/dev-local.sh up          # máy chủ tại http://localhost:18790
scripts/dev-local.sh cli login   # CLI dùng thử
```

Kết nối một key miễn phí (cách nhanh):

```bash
treg secret add BRAVE_TOKEN --value <token>
# hoặc qua bảng điều khiển (dashboard) → Marketplace → Brave Search → Connect
```

---

## 1. Fork bổ sung — 10 nhà cung cấp mới (đã kiểm tra trực tiếp hành vi từ chối khóa rác ngày 2026-09-25)

| Nhà cung cấp | Mức miễn phí / giá | Điểm mạnh | Cách lấy khóa |
|---|---|---|---|
| **Brave Search** | 2.000 truy vấn/tháng, 1 truy vấn/giây, không cần thẻ | tìm kiếm web trên chỉ mục độc lập | brave.com/search/api (gói Free) → bảng điều khiển sao chép token |
| **Google Custom Search** | 100 truy vấn/ngày vĩnh viễn | kết quả Google thật, 10 kết quả/trang | Google Cloud bật Custom Search API + tạo programmable engine "Search the entire web" để lấy `cx` |
| **TwitterAPIs** | tặng 0,50 đô la (~625 lệnh gọi), sau 0,0008 đô la/lệnh gọi | hồ sơ / tìm kiếm / người theo dõi / chuỗi đăng X | twitterapis.com (không thẻ). Máy chủ API: `api.twitterapis.com` |
| **SocialCrawl** | tặng 100 tín dụng không hết hạn | **44 nền tảng một giản đồ**: TikTok, Instagram, YouTube, X, Reddit, **tìm kiếm Douyin, tìm kiếm Xiaohongshu**, LinkedIn, Hacker News, Google, cửa hàng ứng dụng, TMDT + `/search/everywhere` quét mọi mạng xã hội một lệnh gọi | socialcrawl.dev → bảng điều khiển |
| **SocialData** | 0,0002 đô la/kết quả (rẻ nhất X theo từng mục) | tìm kiếm bài đăng X, hồ sơ, dòng thời gian | socialdata.tools |
| **twitterapi.io** | ~0,00015 đô la/lần đọc (trả theo lượng dùng) | 5 điểm cuối X: thông tin, bài gần đây, tìm kiếm nâng cao, người theo dõi, chi tiết | twitterapi.io |
| **RedditAPIs** | 0,002 đô la/lệnh gọi, không thuê bao | tìm kiếm Reddit (q, subreddit, limit) | redditapis.com |
| **TikAPI** | dùng thử 5 ngày; 29 đô la/tháng 300 yêu cầu/ngày | `/public/search/{category}`, `/public/followers`; duy nhất có OAuth cấp tài khoản TikTok (tin nhắn, nhắc đến) | tikapi.io |
| **Firecrawl** | 500 tín dụng miễn phí, không cần thẻ | `/v2/scrape` (trang → Markdown sạch), `/v2/map` | firecrawl.dev |
| **Reddit (chính thức)** | miễn phí dưới 100 truy vấn/phút (OAuth) | tìm kiếm, subreddit nóng/giới thiệu, chi tiết bài đăng | reddit.com/prefs/apps tạo ứng dụng web, chuyển hướng `<url>/auth/reddit/callback`, đặt biến `TREG_REDDIT_CLIENT_ID/_SECRET` |

Ghi chú kiểm tra trực tiếp: Brave 422 `SUBSCRIPTION_TOKEN_INVALID` · Google CSE 400 `API key not valid` · TwitterAPIs 401 `unauthorized` · SocialCrawl 401 `MISSING_API_KEY` (kèm đặc tả OpenAPI công khai 636 đường dẫn — nguồn chép tham số) · SocialData 401 `Unauthenticated` · twitterapi.io 401 sạch · RedditAPIs 403 `Invalid token` · TikAPI 400 lỗi trường · Firecrawl 401 trên `/v2/team/credit-usage`.

## 2. Đã có sẵn trong danh mục — chỉ cần kết nối (connect) key miễn phí

| Nhà cung cấp (service treg) | Mức miễn phí | Dùng để làm gì |
|---|---|---|
| `akta` | miễn phí hoàn toàn (catalog đã xác thực) | tìm kiếm công ty — thay thế Lusha/PDL/Crunchbase |
| `google-search-console` | OAuth miễn phí | dữ liệu từ khóa (keyword) thật của trang web mình |
| `google-ads` | OAuth miễn phí | dữ liệu từ khóa + kế hoạch từ khóa (Keyword Planner) |
| `youtube` | 10.000 đơn vị/ngày (≈ 100 lượt tìm kiếm + hàng nghìn lượt đọc) | nghiên cứu nền tảng YouTube |
| `meta-ad-library` | miễn phí (cần xác minh danh tính) | nghiên cứu quảng cáo Facebook/Instagram |
| `serper` | tặng 2.500 truy vấn (dùng một lần) | trích xuất SERP (kết quả tìm kiếm) đầy đủ các chiều dọc (vertical) của Google |
| `tavily` | 1.000 tín dụng/tháng | công cụ tìm kiếm AI (AI search) cho các tác tử (agent) — tìm kiếm + trích xuất |
| `exa` | tín dụng dùng thử | tìm kiếm web theo ngữ nghĩa |
| `apify` | 5 đô la tín dụng/tháng (tự gia hạn) | trình thu thập dữ liệu (scraper) mọi trang |
| `brightdata` | 5.000 bản ghi/tháng (scraper TikTok) | dữ liệu TikTok |
| `hunter` | 25 lượt tìm kiếm + 50 lượt xác thực email/tháng | tìm và xác thực email |
| `tomba` | mức miễn phí dùng thử | tìm email |
| `coingecko` | API công cộng miễn phí | dữ liệu tiền mã hóa |
| `finnhub`, `twelvedata`, `marketstack`, `tiingo`, `eodhd` | mức miễn phí 60–800 lệnh gọi/ngày | dữ liệu thị trường |
| `millionverifier` | trả trước rẻ nhất mảng xác thực (verify) (~0,0004 đô la/lần) | làm sạch (clean) danh sách email |
| `dataforseo` | không có mức miễn phí (nạp tối thiểu 50 đô la, không mất hạn) | mức nền tảng SERP/thứ hạng (rank) rẻ nhất khi mở rộng quy mô |

## 3. API không cần key — dùng trực tiếp bằng curl/tác tử (agent), miễn phí 100%

Các đường dẫn API (endpoint) này không cần thông tin xác thực (credential), dùng thẳng (treg cần key nên không phù hợp đưa vào danh mục):

```bash
# Reddit công khai (chỉ đọc) — nhớ đặt User-Agent mô tả riêng
curl -A "free-stack:research:1.0" "https://www.reddit.com/r/Entrepreneur/top.json?t=month&limit=25"

# Hacker News (Algolia) — tìm kiếm toàn bộ HN
curl "https://hn.algolia.com/api/v1/search?query=side%20hustle&tags=story"

# Wikipedia REST — tóm tắt sạch cho tác tử
curl "https://en.wikipedia.org/api/rest_v1/page/summary/affiliate_marketing"

# Google Trends qua pytrends (pip install pytrends) — xu hướng từ khóa
python -c "from pytrends.request import TrendReq; p=TrendReq(); p.build_payload(['ai side hustle']); print(p.interest_over_region() if 0 else p.interest_over_time().tail())"

# SEC EDGAR — hồ sơ doanh nghiệp Mỹ
curl -H "User-Agent: research your@email.com" "https://data.sec.gov/submissions/CIK0000320193.json"

# FRED — dữ liệu vĩ mô
curl "https://api.stlouisfed.org/fred/series/observations?series_id=UNRATE&api_key=abcdefghijklmnopqrstuvwxyz&file_type=json"  # key free tại fred.stlouisfed.org

# DeFiLlama — TVL toàn bộ DeFi, không cần key
curl "https://api.llama.fi/protocols"

# Binance công khai — giá tiền mã hóa
curl "https://api.binance.com/api/v3/ticker/24hr?symbol=BTCUSDT"
```

## 4. Mảng Trung Quốc (đặc thù dự án nghiên cứu ngách)

| Nguồn | Chi phí | Ghi chú |
|---|---|---|
| `tikhub` (trong treg) | 0,001 đô la/lệnh gọi (Douyin từ 0,001–0,01; Xiaohongshu phẳng 0,01) | ổn định, có ngay trong danh mục |
| MediaCrawler (GitHub, NanmiCoder) | 0 đô la tự lưu trữ (self-host) | thu thập dữ liệu (crawl) Xiaohongshu/Douyin/Kuaishou/Bilibili/Weibo/Zhihu; kho lưu trữ gốc từng bị gỡ — dùng nhánh phân nhánh (fork), cần nuôi tài khoản |
| Evil0ctal/Douyin_TikTok_Download_API | 0 đô la tự lưu trữ | API dữ liệu Douyin/TikTok tự dựng |
| bilibili-API-collect | 0 đô la | tài liệu API phi chính thức của Bilibili, khá đầy đủ |

Rủi ro pháp lý cần nhớ: LinkedIn kiện Proxycurl chết tháng 7/2025 (buộc xóa 401 triệu hồ sơ); các trình thu thập dữ liệu (crawler) nền tảng Trung Quốc và X đều từng bị gỡ kho. Luôn có phương án dự phòng (fork/sao lưu), không xây hạ tầng sống chết vào một trình thu thập dữ liệu (scraper).

## 5. Chưa đưa được vào danh mục

- **EnsembleData** — miễn phí 50 đơn vị/ngày (~1.500/tháng) cho TikTok/Instagram; máy chủ `app.ensembledata.com` không kết nối được từ máy dò (chặn ở tầng kết nối), điểm cuối chỉ hiển thị trong bảng điều khiển sau đăng ký — tự tạo tài khoản và dùng trực tiếp.

## 6. Bộ combo (stack) đề xuất cho nghiên cứu ngách (chi phí 0 đô la/tháng)

1. Phát hiện từ khóa/xu hướng (Keyword/trend discovery): Google CSE (100/ngày) + Brave (2.000/tháng) + pytrends + GSC
2. Theo dõi mạng xã hội (Social listening): Reddit JSON (không giới hạn thực tế khi giữ dưới 60 lệnh gọi/phút) + YouTube (quota chính thức) + TwitterAPIs (625 lệnh gọi tặng) + Tavily (1.000/tháng cho tác tử)
3. Nghiên cứu quảng cáo (Ads research): Meta Ad Library + Google Ads Transparency (web) + TikTok Creative Center (web)
4. Khai thác dữ liệu doanh nghiệp (Company enrichment): Akta (miễn phí, đã xác thực)
5. Quảng cáo/python khi cần cạo (scrape) sâu: Apify (5 đô la tín dụng/tháng) + Playwright tự viết
6. Trung Quốc: TikHub nạp nhỏ theo lệnh gọi — chỉ khi tự lưu trữ (self-host) MediaCrawler không đáng

So sánh: mua riêng Semrush + Hunter + Crunchbase + X API ≈ 471 đô la/tháng.
