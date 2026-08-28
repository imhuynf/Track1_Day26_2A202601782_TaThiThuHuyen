# Operating Dashboard — iRSA (Intelligent Resume Screening Automation)

> Đây là **worksheet nguồn** để validator và rubric truy vết evidence. Sau khi
> hoàn tất, rút gọn phần vận hành sang
> `templates/one-page-dashboard-template.md`; không ép bảng 12 cột này lên một trang.

- Học viên: Tạ Thị Thu Huyền
- Mã học viên: 2A202601782
- Sản phẩm: iRSA
- Mô hình: B2B
- Cập nhật: 2026-08-28
- North Star: Time-to-first-value dưới 30 ngày

## Chẩn đoán mô hình

Chúng tôi là **B2B** vì tiền đến từ **doanh nghiệp SME Việt Nam (Trưởng HR hoặc chủ DN trả phí Hybrid)**, người dùng thật là **chuyên viên tuyển dụng của chính doanh nghiệp đó**, và chúng tôi **không có** bề mặt trực tiếp với người dùng cuối qua **ứng viên: iRSA không gửi tin, không có portal ứng viên, chỉ trả báo cáo trong Drive/Sheets của recruiter**.

Ba câu hỏi theo thực tế hôm nay, không theo kế hoạch quý sau: (1) Ai trả tiền — doanh nghiệp, chưa có cá nhân trả phí. (2) Ai dùng — recruiter trong tổ chức trả tiền. (3) Có trung gian ứng viên không — có người bị ảnh hưởng nhưng không chạm, không quan sát hành vi, không giữ quan hệ → không phải B2B2C. Đèn bật trước mặc định: Time-to-first-value.

| Dữ liệu đầu vào | Trạng thái | Nằm ở đâu hoặc cần gì để đo | Ngày có số |
|---|---|---|---|
| Unit economics Day 24 | Đo được | https://github.com/imhuynf/Track1_Day24_2A202601782_TaThiThuHuyen — README + `2A202601782_TaThiThuHuyen_Day24.xlsx` Tab 1–2 kịch bản Base (giả định mô hình, chưa quan sát vận hành) | 2026-08-27 |
| Value Metric và Cost/Job Day 25 | Đo được | https://github.com/imhuynf/Track1_Day25_2A202601782_TaThiThuHuyen — `2A202601782_TaThiThuHuyen_Day25_Monetization_Model.xlsx` tab 1–4; eval containment 8/12. Cost/Job tính lại từ ô vàng vì xlsx không cache công thức | 2026-08-27 |

Các số dưới đây đủ để neo ngưỡng [MH]. Chúng là **đầu vào mô hình Day 24–25**, không phải số vận hành đã quan sát. Ô nào chưa đo được ghi rõ ở bảng tiếp theo và section Chưa đo được — không gán [MH] cho ngưỡng cảm tính.

| Chỉ số lab yêu cầu | Giá trị dùng cho ngưỡng | Nguồn / phép tính | Gắn nhãn khi dùng | Ghi chú trung thực |
|---|---|---|---|---|
| ARPU hoặc doanh thu / Value Metric | Day 25 vận hành: 5.333.000 đ/khách/tháng (≈ 205,13 USD). Gói Hybrid 2,4 triệu đ gồm 300 CV hoàn tất + 8.000 đ/CV overage. Day 24 Base khác: 2.000.000 đ/khách/tháng (1,6 triệu đ / 800 CV + overage 2.000 đ/CV) | Day 25 tab 4_Channel_Fit B5: 2,4 triệu + (1.000 × 66,7% − 300) × 8.000 đ. Day 24 Tab 1 D7 | [MH] khi lấy đúng một dòng và tính lại. Không trộn 2,0 triệu với Cost/Job Day 25 | Hai bài dùng hai gói giá. Ngưỡng vận hành/Cost/Job lấy Day 25. Ngưỡng LTV–CAC–runway lấy Day 24 Base và ghi rõ |
| CAC và CAC payback mục tiêu | CAC giả định Day 24 Base: 3.000.000 đ/khách mới. Payback Base tính được 2,4 tháng. Mục tiêu phân khúc SMB: dưới 12 tháng | Day 24 Tab 1 D22 và Tab 2 D22. Mốc 12 tháng: Bessemer Scaling to $100 Million (2024), Day 25 tab 6 | CAC 3.000.000 đ là giả định mô hình → [MH] nếu suy ngân sách từ ARPU×GM×12. Mốc 12 tháng là [BM] khi dẫn URL. Chưa có fully-loaded CAC từ funnel | Chưa có khách trả phí, chưa có hóa đơn marketing/sales. ICONIQ cost-per-opportunity 6.300 USD / win rate 25% = CAC sales ước 25.200 USD — chỉ để loại Sales-Led, không phải CAC đã đo |
| Gross margin mục tiêu | Mục tiêu sống còn Day 25: ≥ 60%. Day 24 Base tính được 62,5% trên COGS 750.000 đ / ARPU 2.000.000 đ. Day 25 GM trên Cost/Job thuần = 81,25% | Day 25 tab 2_Pricing B32 = 0,6. Day 24 Tab 2 D11 = 0,625. Day 25: (8.000 − 1.500) / 8.000 | 60% là ngưỡng bài / mục tiêu mô hình → [MH] khi gắn với Cost/Job. 81,25% là kết quả tính, không phải số kế toán. Không lấy 81% làm [BM] | Day 25 GM cao vì HITL biến thể A (escalate do khách chịu), overhead = 0, token chưa đo. Lab Day 25 đã cảnh báo GM > 85% thường là quên chi phí — 81% sát ngưỡng đó |
| Cost/Job, tách token/inference | Cost/Job (API+infra+retry+HITL) / job hoàn tất = 0,0577 USD ≈ **1.500 đ/CV hoàn tất**. Trong đó token LLM ≈ 100 đ; retry ≈ 5 đ; infra ≈ 225 đ; HITL QA ≈ 1.170 đ | Tính lại công thức tab 1_Cost_Job vì ô kết quả không cache: B11 = 1.000 × 8/12 = 666,67 job hoàn tất; LLM B31 = 0,00256 USD/job thử; B66 = (8,457 + 30) / 666,67 | [MH] cho trần Cost/Job và trần token. Token 2.800/1.200 và retry 5% là ước lượng → không nói "đã đo usage" | Chưa có `usage_metadata`, chưa có hóa đơn infra, cache = 0. Containment mẫu số là eval keyword 8/12, không phải production. Stack Day 24 (GPT-4.1 mini + OCR) khác Day 25 (Gemini 2.5 Flash) |
| Giả định retention / renewal | Day 24 Base: churn 5%/tháng → vòng đời 20 tháng. Chưa có renewal hay NRR | Day 24 Tab 1 D18 = 0,05; Tab 2 D14 = 20 tháng | Dùng để suy LTV thì là [MH] trên giả định. Retention/NRR thật = chưa đo được → [TB] khi có cohort | Không khách trả phí. Day 25 tháng 4+ đặt mục tiêu churn dưới 5%/tháng — đó là kế hoạch, chưa baseline |
| Kênh phân phối và người trả tiền | Kênh 90 ngày: **PLG** (founder ngồi cạnh, không self-serve thuần). Người trả tiền: Trưởng HR / chủ SME Việt Nam. Ngân sách CAC Day 25 ≈ 2.000 USD/khách = ARPU × GM × 12 | Day 25 tab 4_Channel_Fit B38 = PLG; B9 ≈ 205,13 × 81,25% × 12 ≈ 2.000 USD. Day 24 người mua = Trưởng phòng Nhân sự / TA Lead / chủ DN | Kênh đã chốt bằng Affordability Test → dùng khi viết luật GTM. Ngân sách CAC là [MH]. Cost-per-opportunity ICONIQ là [BM] | Partner-Led chưa có tên công ty. Sales-Led loại vì CAC ước lệch ~12,6 lần ngân sách. PLG tháng 1 chưa có Drive Picker |
| Người dùng trực tiếp và end-user bị ảnh hưởng | User trực tiếp: chuyên viên tuyển dụng sàng lọc CV trên Drive/Gmail/Sheets. End-user bị ảnh hưởng: ứng viên, nhưng iRSA không chạm trực tiếp (không gửi tin, human duyệt 100% quyết định tuyển) | Day 24 README mục Target Customer. Day 25 Pain Moment: 8h30–10h00 thứ Hai, recruiter đang ở Google Drive. Procurement note: AI không gửi tin cho ứng viên | Chẩn đoán B2B, không B2B2C. Đèn mặc định: Time-to-first-value | TTFV, sales cycle, NRR, usage depth, chi phí triển khai/ACV chưa có khách thật |

**Hai phép tính [MH] đã sẵn sàng neo ngưỡng** (chi tiết ở phụ lục khi dựng đèn):

- MH-01 Cost/Job tối đa để GM ≥ 60%: 8.000 đ × (1 − 0,60) = **3.200 đ/CV hoàn tất**. Hiện 1.500 đ. Trần token+retry sau khi trừ infra 225 đ và HITL 1.170 đ = **1.805 đ** so với ước hiện tại 105 đ.
- MH-02 Ngân sách CAC SMB 12 tháng trên số Day 25: 205,13 USD × 81,25% × 12 = **2.000 USD/khách** (≈ 52 triệu đ). CAC Day 24 Base 3 triệu đ nằm trong ngân sách này; không dùng để biện minh Sales-Led.

## Kiểm kê đèn ứng viên

Bảng đèn B2B handbook §3.2. ✅ số có hôm nay · 🔧 làm được trong 2 tuần (trước 2026-09-11) · ❌ chưa đo và chưa có đường ra số trong 2 tuần. Không giấu ❌.

| Đèn ứng viên từ handbook | Tầng | Trạng thái | Bằng chứng hiện có hoặc kế hoạch đo |
|---|---|---|---|
| Time-to-first-value (TTFV) | L | 🔧 | Chưa có khách. First value = recruiter nhận báo cáo sàng lọc đủ điểm/căn cứ/cảnh báo trên CV thật. Trong 2 tuần: bấm giờ 5 ca thứ Hai (Day 25), ghi ngày bắt đầu folder và ngày báo cáo đầu tiên. File: time-motion sheet |
| Pipeline coverage | L | 🔧 | Chưa CRM. Target 90 ngày đầu = 5 phỏng vấn HR (Day 25 tháng 1), không phải quota AE. Trong 2 tuần: sheet tên DN, ngày hẹn, CV/tuần, trạng thái. Coverage = số hẹn đã chốt ÷ 5 |
| % deal chết ở khâu security/procurement | L | ❌ | Chưa có deal. Cách đo: closed-lost reason khi có phản hồi mua hàng. Ngày xem lại 2026-11-30. Không bịa mẫu |
| POC → paid | O | ❌ | Tháng 1 không lấy khách trả phí. Cách đo: SME ký gói 2,4 triệu ÷ pilot go-live. Ngày xem lại 2026-11-30 |
| Sales cycle (ngày) | O | ❌ | PLG founder-led, chưa định nghĩa opportunity trên CRM. Không đo chu kỳ AE. Ngày xem lại 2026-11-30 chỉ nếu đổi khỏi PLG |
| Usage depth trong tài khoản | O | 🔧 | Portal test chưa log. Trong 2 tuần: event completion, retry, lỗi, human override trên CV test; depth = % job có báo cáo hoàn tất / job thử trong tuần |
| Chi phí triển khai ÷ ACV | O | 🔧 | Founder ngồi cạnh, chưa timesheet. Trong 2 tuần: ghi giờ onboarding ÷ ACV niêm yết 2,4 triệu đ/tháng × 12. Mẫu nội bộ, chưa hợp đồng |
| Tập trung doanh thu | O | ❌ | Doanh thu trả phí = 0. Cách đo: ARPU khách lớn nhất ÷ tổng ARPU khi ≥2 khách. Ngày xem lại 2026-12-15 |
| NRR | G | ❌ | Công thức biết; cần cohort gia hạn. Ngày có số sớm nhất 2027-02-28. Không dùng churn 5% Day 24 như NRR đã đo |
| Gross Margin | G | ✅ | Day 24 Tab 2 D11 Base = 62,5%. Day 25 Cost/Job GM = 81,25% (mô hình, chưa hóa đơn). File Excel Day 24–25 |
| CAC payback | G | ✅ | Day 24 Tab 2 D22 Base = 2,4 tháng trên CAC giả định 3 triệu đ. Chưa fully-loaded từ funnel. File Excel Day 24 |

## Đèn báo sớm

Cây tuần này (7 đèn, không copy cả bảng handbook): L-02 coverage 5 hẹn HR → L-01 TTFV → O-01 usage depth và O-03 triển khai/ACV → G-02 CAC payback. Song song O-02 chi phí AI → G-01 Gross Margin. North Star là L-01.

| ID | Đèn | Định nghĩa và công thức | Nhịp · Owner | Hiện tại | 🟢 | 🟡 | 🔴 | Nguồn | Ngày kiểm tra | Báo trước cho | Luật |
|---|---|---|---|---:|---|---|---|---|---|---|---|
| L-01 | Time-to-first-value | Số ngày lịch từ kickoff (recruiter chọn folder CV hoặc founder ngồi cạnh lần đầu) đến báo cáo sàng lọc đầu tiên đủ 3 phần: điểm phù hợp, căn cứ từ CV, nhận xét hoặc cảnh báo, trên CV thật của vị trí đó. Không đếm CV test nội bộ, không đếm ngày cài đặt, không đếm quyết định tuyển. Công thức: median số ngày trên các tài khoản go-live trong kỳ | Tuần · Product Operations | chưa đo; 5 ca nội bộ từ 2026-09-11 | ≤7 ngày | 8–30 ngày | >30 ngày | [TB] Xanh = một vòng thứ Hai (pain moment Day 25). Đỏ = thủng North Star 30 ngày. Chốt baseline sau 5 ca và 2 SME, 2026-10-15 | 2026-08-28 | O-01 Usage depth rồi G-02 CAC payback (khách không thấy giá trị thì không gia hạn, payback không đóng) | R-01 |
| L-02 | Coverage 5 hẹn HR | Số cuộc hẹn HR SME đã có ngày và tên DN trong kế hoạch tháng 1 chia 5. Không đếm tin nhắn chưa chốt lịch, không đếm demo nội bộ team. Công thức: số hẹn còn hiệu lực ÷ 5 | Tuần · Product Operations | 0/5 = 0 | ≥1,0 (đủ 5 hẹn) | 0,60–0,99 (3–4 hẹn) | <0,60 (dưới 3 hẹn) | [TB] Mẫu số 5 lấy kế hoạch Day 25 tháng 1, không dùng quy ước pipeline 3× của AE. Xem lại sau 2026-09-11 | 2026-08-28 | L-01 TTFV (không có hẹn thì không có khách để đo giá trị) | R-02 |

## Đèn vận hành

| ID | Đèn | Định nghĩa và công thức | Nhịp · Owner | Hiện tại | 🟢 | 🟡 | 🔴 | Nguồn | Ngày kiểm tra | Báo trước cho | Luật |
|---|---|---|---|---:|---|---|---|---|---|---|---|
| O-01 | Usage depth trong tài khoản | Với từng DN đang test hoặc pilot: số job có báo cáo hoàn tất chia số job thử trong tuần. Đây là tỷ lệ hoàn tất, không phải % seat đã mua. Không đếm lần gọi API. Không gộp nhiều DN. Công thức: job hoàn tất ÷ job thử, theo account | Tuần · Product Operations | chưa đo; log portal từ 2026-09-11 | ≥66,7% | 37,5–66,6% | <37,5% | [TB] Xanh = eval Day 25 keyword 8/12 = 66,7%. Đỏ = 30 CV activation ÷ 80 CV tối đa một ca thứ Hai = 37,5%. Không dùng 60% vì 60% là KPI số CV, không phải tỷ lệ hoàn tất. Chốt lại sau 4 tuần log, 2026-10-15 | 2026-08-28 | G-02 CAC payback (tài khoản không hoàn tất job thì CAC không thu hồi) | R-03 |
| O-02 | Chi phí AI trên mỗi job hoàn tất | Tổng chi phí token LLM cộng retry trong tuần, quy đổi 26.000 đ/USD, chia số job HOÀN TẤT. Không chia job thử. Không gồm HITL hay infra. Công thức: (USD token + USD retry) × 26.000 ÷ số job hoàn tất | Tuần · FinOps | ~105 đ (ước 2.800/1.200 token, chưa usage_metadata) | ≤1.805 đ | 1.806–2.605 đ | >2.605 đ | [MH] MH-01: 8.000×(1−0,60)−225−1.170 = 1.805 đ giữ GM 60%; 8.000×(1−0,50)−225−1.170 = 2.605 đ là mép GM 50% | 2026-08-28 | G-01 Gross Margin | R-04 |
| O-03 | Chi phí triển khai ÷ ACV | Giờ founder hoặc kỹ sư ngồi cạnh khách trong kỳ × 150.000 đ/giờ, chia ACV niêm yết 2.400.000 đ/tháng × 12 = 28.800.000 đ. Một khách. Không gồm R&D chung. Công thức: chi phí giờ triển khai ÷ 28.800.000 | Tuần · Product Operations | chưa đo; timesheet từ 2026-09-11 | <15% | 15–30% | >30% | [TB] 15/30% là điểm khởi đầu handbook B2B, chưa có hóa đơn triển khai. Chốt sau 3 khách, 2026-10-15 | 2026-08-28 | G-01 Gross Margin (triển khai nặng biến iRSA thành dịch vụ) | R-03 |

## Đèn kết quả

| ID | Đèn | Định nghĩa và công thức | Nhịp · Owner | Hiện tại | 🟢 | 🟡 | 🔴 | Nguồn | Ngày kiểm tra | Báo trước cho | Luật |
|---|---|---|---|---:|---|---|---|---|---|---|---|
| G-01 | Gross Margin sau AI cost | Doanh thu thuần trừ COGS biến đổi (API + infra + retry + HITL trong COGS) chia doanh thu. Không lấy GM 81% Day 25 làm số kế toán. Công thức: (ARPU − Cost/Job × job hoàn tất) ÷ ARPU theo khách | Tháng · Finance | 81% mô hình Day 25; 62,5% Day 24 Base | ≥60% | 50–59% | <50% | [MH] MH-01: 60% = ô B32 Day 25, không phải trung vị AI-native ~53%. 50% = đèn NGUY HIỂM trên cùng tab giá. 62,5% Day 24 chỉ để đối chiếu | 2026-08-28 | Runway và khả năng chịu CAC | R-04 |
| G-02 | CAC payback | Số tháng Gross Profit để hoàn CAC. Công thức: CAC ÷ (ARPU × GM). SMB. Không dùng CAC ICONIQ như số đã đo | Tháng · Finance | 2,4 tháng trên CAC giả định Day 24 Base 3.000.000 đ | ≤2,4 tháng | 2,5–12 tháng | >12 tháng | [MH] MH-02: Base Day 24 = 2,4 tháng. Trần 12 tháng = 2.000 USD CAC trên ARPU 205,13 × GM 81,25%. Không lấy 18/24 tháng mid-market làm đích SME | 2026-08-28 | NPV / sống còn của mô hình Day 24 | R-05 |

## Lý do ranh giới xanh / vàng / đỏ

Một câu cho mỗi đèn. Ba vùng không chồng. Không lấy trung vị ngành làm đích.

| ID | Lý do ranh giới |
|---|---|
| L-01 | Xanh ≤7 ngày vì pain moment là một buổi sáng thứ Hai và founder ngồi cạnh; vàng 8–30 ngày còn kịp North Star; đỏ >30 ngày thì khách đã qua một chu kỳ tuyển mà chưa thấy báo cáo. |
| L-02 | Xanh = đủ 5 hẹn đã viết trong Day 25 tháng 1; vàng 3–4 hẹn (0,60–0,99) còn học được; đỏ dưới 3 hẹn thì không đủ pain moment để đo TTFV. |
| O-01 | Xanh ≥66,7% lấy eval 8/12 Day 25, không phải 60% activation; vàng 37,5–66,6%; đỏ <37,5% = 30 CV ÷ 80 CV một ca thứ Hai thì không đạt activation trong đúng pain moment. |
| O-02 | Xanh ≤1.805 đ giữ GM 60% khi infra 225 đ và HITL 1.170 đ đứng yên; vàng đến 2.605 đ là mép GM 50%; đỏ trên 2.605 đ thì COGS biến đổi đã ăn hơn nửa giá 8.000 đ. |
| O-03 | Xanh <15% = dưới 28,8 giờ × 150.000 đ trên ACV 28,8 triệu đ; vàng 15–30% (28,8–57,6 giờ) còn là sản phẩm; đỏ >30% biến iRSA thành dịch vụ mặc áo SaaS. |
| G-01 | Xanh ≥60% lấy đúng ô B32 Day 25, không lấy ~53% AI-native làm đích; vàng 50–59% trùng đèn CẢNH BÁO trên tab giá; đỏ <50% trùng NGUY HIỂM. |
| G-02 | Xanh ≤2,4 tháng là payback Base Day 24 đã tính; vàng đến 12 tháng còn nằm trong trần CAC 2.000 USD (MH-02); đỏ >12 tháng vượt ngân sách SMB của chính mô hình, không phải mốc 18 tháng mid-market. |

## Luật quyết định

Đúng 5 luật, đủ năm vế. Luật dừng = ngừng một việc đang làm. Phản xạ B2B bị cấm: giảm giá và tuyển sales khi khách chưa thấy giá trị.

| ID | NẾU | TRONG | VÀ | THÌ | KHÔNG THÌ | Luật dừng? |
|---|---|---|---|---|---|---|
| R-01 | Median TTFV >30 ngày | 2 cohort liên tiếp | Mỗi cohort ≥2 tài khoản go-live hoặc ≥5 ca time-motion | Cắt pilot còn một vị trí một folder CV và dừng nhận SME mới trong 14 ngày | Không giảm giá gói 2,4 triệu và không tuyển sales để bù TTFV chậm | CÓ |
| R-02 | Coverage 5 hẹn HR <0,60 | 14 ngày liên tiếp | Sheet tháng 1 đã mở ≥7 ngày | Dừng mọi outreach ngoài danh sách 5 DN và chuyển founder sang chốt cho đủ 5 hẹn | Không mở Sales-Led và không chạy ads để bù số hẹn | CÓ |
| R-03 | Usage depth <37,5% hoặc chi phí triển khai ÷ ACV >30% | 2 tuần liên tiếp | Account có ≥20 job thử hoặc ≥1 khách có timesheet | Cắt onboarding còn một folder CV trên Drive, founder ngồi cạnh và ghi timesheet trong tuần tới | Không bán overage và không nhận khách mới cùng mức giá khi tài khoản hiện tại chưa dùng hết | KHÔNG |
| R-04 | Chi phí AI / job hoàn tất >2.605 đ hoặc Gross Margin <50% | 2 tuần liên tiếp | ≥50 job hoàn tất trong cửa sổ | Giới hạn token tươi còn 2.800/lượt, cắt retry xuống tối đa 5%, đàm phán lại quota Gemini trước kỳ billing | Không bỏ HITL/QA để cost trông thấp hơn và không tăng giá đại trà | KHÔNG |
| R-05 | CAC payback >12 tháng | 1 tháng lịch | Đã có ≥1 khách trả phí hoặc đã ghi fully-loaded CAC | Đóng băng mọi chi tiêu acquisition ngoài PLG founder ngồi cạnh | Không thuê AE và không tăng phí nền để bù CAC | CÓ |

## Cổng gác 90 ngày

Đúng một metric mỗi cổng. FIX chỉ một lần cho cùng một lỗ hổng; lần hai phải PIVOT hoặc KILL. Ngày 30 không đo doanh thu.

| Ngày | Metric gác cổng | Ngưỡng | Bằng chứng vật lý | Nếu đạt | Nếu trượt |
|---:|---|---|---|---|---|
| 30 | Phỏng vấn HR xác nhận cùng pain moment | 5/5 recruiter xác nhận 8h30–10h00 thứ Hai, đang ở Drive plus Sheets, chưa ATS | Biên bản phỏng vấn đã redacted plus sheet 5 hẹn (tên DN, giờ, app đang mở) | GO | FIX |
| 60 | Usage depth trên tài khoản kickoff | ≥66,7% job hoàn tất ÷ job thử, trên ≥2 tài khoản có ≥20 job thử | Log completion theo account trên portal test hoặc pilot | GO | PIVOT |
| 90 | Gross Margin sau AI cost | ≥60% trên ≥1.000 CV hoàn tất | Export token/usage_metadata plus bảng Cost/Job tính lại trên mẫu 1.000 job hoàn tất | GO | KILL |

## Kill criteria

KILL hướng Hybrid-PLG của iRSA vào ngày 90 nếu Gross Margin dưới 50% trên ít nhất 1.000 CV hoàn tất sau hai vòng giới hạn token và retry, hoặc không SME nào hoàn tất 30 CV thật và chấp nhận gói 2,4 triệu đ / 300 CV.

## Chưa đo được

Mỗi khoảng trống có cách đo và ngày xem lại. Không để trống cho đẹp.

| Đèn hoặc giả định | Cần gì để đo | Ai chịu trách nhiệm | Ngày có số |
|---|---|---|---|
| Time-to-first-value nội bộ (🔧) | Time-motion 5 ca thứ Hai: ngày mở folder CV → ngày recruiter nhận báo cáo đủ điểm/căn cứ/cảnh báo. File sheet | Tạ Thị Thu Huyền | 2026-09-11 |
| Pipeline coverage (🔧) | Sheet 5 hẹn HR: tên DN, ngày, CV/tuần, trạng thái. Coverage = số hẹn đã chốt ÷ 5 | Tạ Thị Thu Huyền | 2026-09-11 |
| Usage depth trên portal test (🔧) | Event completion, retry, lỗi, human override. Depth = job có báo cáo hoàn tất ÷ job thử trong tuần | Tạ Thị Thu Huyền + kỹ sư | 2026-09-11 |
| Chi phí triển khai ÷ ACV (🔧) | Timesheet giờ founder ngồi cạnh ÷ ACV niêm yết 2,4 triệu đ/tháng × 12. Chưa hợp đồng | Tạ Thị Thu Huyền | 2026-09-11 |
| Token input/output thật / job | Bật `usage_metadata` trên Gemini, log theo job hoàn tất, đối chiếu hóa đơn API | Tạ Thị Thu Huyền + kỹ sư | 2026-09-30 |
| Retry rate production | Log timeout và max_retries; mẫu tối thiểu 200 job | Tạ Thị Thu Huyền + kỹ sư | 2026-09-30 |
| TTFV khách pilot và phút/CV trước–sau | Pilot 4 tuần / SME: ngày kickoff → ngày 30 CV hoàn tất trên CV thật | Tạ Thị Thu Huyền | 2026-10-15 |
| Infra / logging / egress theo hóa đơn | Invoice hosting, DB, storage; không bịa dòng chi phí trước khi có bill | Tạ Thị Thu Huyền | 2026-10-15 |
| Pilot Report | 4 tuần / 1.000 CV được phép với 1 SME; đo completion, lỗi, override, phút tiết kiệm | Tạ Thị Thu Huyền | 2026-10-15 |
| Containment trên CV khách | Eval mở ≥20 CV cùng 1 JD, rồi log family match production. Không dùng 8/12 làm số khách | Tạ Thị Thu Huyền | 2026-10-15 |
| % deal chết ở khâu security/procurement (❌) | Cột closed-lost reason trên sheet hẹn HR. Tỷ lệ = deal chết procurement ÷ deal đã có phản hồi mua hàng. Mẫu = 0 cho đến khi có phản hồi | Tạ Thị Thu Huyền | 2026-11-30 |
| POC → paid (❌) | Số SME ký gói 2,4 triệu đ ÷ số pilot go-live. Tháng 1 không bán; xem lại khi có pilot | Tạ Thị Thu Huyền | 2026-11-30 |
| Sales cycle (❌) | Không đo chu kỳ AE khi kênh là PLG. Ngày xem lại: chỉ bật nếu đổi khỏi PLG; khi đó cần ngày qualified → ngày ký trên CRM | Tạ Thị Thu Huyền | 2026-11-30 |
| CAC funnel thật (❌) | CRM: nguồn lead, cost-per-opportunity, win/loss. Không dùng ICONIQ như CAC đã đo | Tạ Thị Thu Huyền | 2026-11-30 |
| Tập trung doanh thu (❌) | ARPU khách lớn nhất ÷ tổng ARPU. Cần ≥2 khách trả phí | Tạ Thị Thu Huyền | 2026-12-15 |
| NRR (❌) | Doanh thu cohort cuối kỳ ÷ đầu kỳ sau expansion và churn. Cần một cohort gia hạn | Tạ Thị Thu Huyền | 2027-02-28 |

## Phụ lục ngưỡng suy từ mô hình

| ID | Metric | Input Day 24–25 | Phép tính | Kết quả và ngưỡng áp dụng |
|---|---|---|---|---|
| MH-01 | Trần token+retry để GM 60% và 50% | Giá bán = 8.000 đ/CV hoàn tất; GM mục tiêu B32 = 60%; infra = 225 đ/job; HITL QA = 1.170 đ/job; Cost/Job hiện ≈ 1.500 đ | Trần GM 60%: 8.000 × (1 − 0,60) = 3.200; 3.200 − 225 − 1.170 = 1.805. Trần GM 50%: 8.000 × (1 − 0,50) = 4.000; 4.000 − 225 − 1.170 = 2.605 | O-02: xanh ≤1.805 đ, vàng 1.806–2.605 đ, đỏ >2.605 đ. G-01: xanh ≥60%, vàng 50–59%, đỏ <50% |
| MH-02 | Trần CAC và payback SMB | ARPU Day 25 = 205,13 USD/khách/tháng; GM Day 25 = 81,25%; payback Base Day 24 = 2,4 tháng; CAC Base = 3.000.000 đ | Ngân sách: 205,13 × 0,8125 × 12 = 2.000 USD/khách. Payback tại trần: 2.000 ÷ (205,13 × 0,8125) = 12 tháng. Base: 3.000.000 ÷ (2.000.000 × 0,625) = 2,4 tháng | G-02: xanh ≤2,4 tháng, vàng 2,5–12 tháng, đỏ >12 tháng |

## Ghi nhận AI critique

Chạy prompt 5.1–5.4 trên dashboard đã viết sẵn. Không dán dữ liệu khách, hợp đồng hay API key. Tôi tự chọn nhận/bác; không copy nguyên văn AI vào đèn. Bác bỏ ý hạ trần O-02 xuống gần 105 đ: MH-01 1.805/2.605 là mép GM, không phải p95; 105 đ còn là ước.

| Phản biện | Chấp nhận hay bác bỏ | Thay đổi đã thực hiện | Lý do |
|---|---|---|---|
| O-01 lấy 60% từ KPI activation (đủ 30 CV) gắn lên tỷ lệ hoàn tất — hai khái niệm, VIBES | Chấp nhận | 🟢 ≥66,7% (8/12); 🟡 37,5–66,6%; 🔴 <37,5% (30÷80). Cổng 60 = 66,7% | 8/12 = 66,7%. 30 CV ÷ 80 CV một ca thứ Hai = 37,5%. Không bịa 60% |
| R-04 đòi ≥200 job hoàn tất — tháng 1 PLG không đủ mẫu nên GM thủng mà luật không bắn | Chấp nhận | Đổi mẫu R-04 thành ≥50 job hoàn tất | Pain moment Day 25: 50–80 CV/sáng thứ Hai; 50 là một ca |
| R-03 THÌ prototype Drive Picker cả sprint — đội 2 người không làm được thứ Hai | Chấp nhận | THÌ còn cắt onboarding một folder Drive, ngồi cạnh, ghi timesheet | Hành động nhỏ hơn, vẫn cấm bán overage |
