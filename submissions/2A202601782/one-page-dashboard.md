# Operating Dashboard — iRSA

> Bản rút gọn trang 1. Mọi số khớp `operating-dashboard.md`. Phép `[MH]` ở trang 2.

**Model:** B2B · **Cập nhật:** 2026-08-28 · **Owner phiên họp:** Product Operations

**Chẩn đoán:** SME Việt Nam trả phí Hybrid; recruiter của DN dùng sản phẩm; iRSA không có bề mặt với ứng viên.

**North Star:** Median TTFV · hiện tại chưa đo · mục tiêu ≤30 ngày · 🔴

## Cây đèn 3 tầng

| Tầng · ID | Metric và định nghĩa ngắn | Hiện tại · 🟢 / 🟡 / 🔴 · Nguồn | Nhịp · Owner | Báo trước · Luật |
|---|---|---|---|---|
| L · L-01 | Median ngày kickoff → báo cáo đủ 3 phần trên CV thật | chưa đo · ≤7 / 8–30 / >30 · `[TB]` | Tuần · Product Ops | Usage + payback · R-01 |
| L · L-02 | Số hẹn HR đã chốt ÷ 5 | 0/5 · ≥1,0 / 0,60–0,99 / <0,60 · `[TB]` | Tuần · Product Ops | TTFV · R-02 |
| O · O-01 | Job hoàn tất ÷ job thử, theo account | chưa đo · ≥66,7 / 37,5–66,6 / <37,5 · `[TB]` | Tuần · Product Ops | Payback · R-03 |
| O · O-02 | Token + retry ÷ job hoàn tất | ~105đ ước · ≤1.805 / 1.806–2.605 / >2.605 · `[MH]` | Tuần · FinOps | GM · R-04 |
| O · O-03 | Giờ triển khai × 150kđ ÷ ACV 28,8tr | chưa đo · ≤5 / >5–10 / >10 · `[MH]` | Tuần · Product Ops | GM + payback · R-03 |
| G · G-01 | (ARPU − COGS biến đổi) ÷ ARPU | 81% mô hình / 62,5% D24 · ≥60 / 50–59 / <50 · `[MH]` | Tháng · Finance | Runway · R-04 |
| G · G-02 | CAC ÷ (ARPU × GM) | 2,4 tháng mô hình · ≤2,4 / 2,5–12 / >12 · `[MH]` | Tháng · Finance | NPV · R-05 |

## Luật quyết định

| ID | NẾU · TRONG · VÀ | THÌ | KHÔNG THÌ | Dừng? |
|---|---|---|---|---|
| R-01 | TTFV >30 ngày · 2 cohort · ≥2 DN hoặc 5 ca | Cắt 1 vị trí/folder; dừng SME mới 14 ngày | Không giảm giá; không tuyển sales | CÓ |
| R-02 | Coverage <0,60 · 14 ngày · sheet ≥7 ngày | Dừng outreach ngoài 5 DN; founder chỉ chốt hẹn | Không mở Sales-Led; không chạy ads | CÓ |
| R-03 | Triển khai/ACV >10% · 2 tuần · ≥1 timesheet đầy đủ | Dừng onboarding mới 7 ngày; còn 1 folder; trần hỗ trợ 19,2 giờ | Không bán overage; không thuê ngoài để giấu cost | CÓ |
| R-04 | AI cost >2.605đ · 2 tuần · ≥50 job hoàn tất | Giới hạn 2.800 token tươi; cắt retry 5%; đàm phán quota | Không bỏ QA; không tăng giá đại trà | KHÔNG |
| R-05 | Payback >12 tháng · 1 tháng · ≥1 khách trả phí hoặc CAC đã ghi | Đóng băng acquisition ngoài founder PLG | Không thuê AE; không tăng phí nền | CÓ |

## Cổng 90 ngày

| Ngày | Một metric · ngưỡng | Evidence | Đạt / Trượt |
|---:|---|---|---|
| 30 | Pain moment HR · 5/5 xác nhận thứ Hai, Drive+Sheets | Biên bản redacted + sheet 5 hẹn | GO / FIX |
| 60 | Usage depth · ≥66,7% trên ≥2 account, ≥20 job thử | Log completion theo account | GO / PIVOT |
| 90 | GM sau AI cost · ≥60% trên ≥1.000 CV hoàn tất | usage_metadata + Cost/Job tính lại | GO / KILL |

**Kill criteria:** KILL ngày 90 nếu GM <50% trên ≥1.000 CV hoàn tất sau hai vòng giới hạn token/retry, hoặc không SME nào hoàn tất 30 CV thật và chấp nhận gói 2,4 triệu đ / 300 CV.

**Chưa đo được:** TTFV khách · time-motion 5 ca + sheet kickoff · owner Tạ Thị Thu Huyền · 2026-09-11. Token thật · `usage_metadata` · 2026-09-30. NRR · cohort gia hạn · 2027-02-28.
