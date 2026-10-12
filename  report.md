# Báo cáo: Tối ưu CI bằng Gradle Dependency Caching

## Cấu hình
Bước `actions/setup-java@v5` được bổ sung tham số `cache: 'gradle'` (xem `.github/workflows/ci.yml`).

## Cách đo
Push code 2 lần liên tiếp. Vào tab **Actions** -> chọn run -> xem thời gian từng step và tổng thời gian Job.

## Kết quả

| Chỉ số | Lần 1 (chưa có cache) | Lần 2 (đã có cache) | Chênh lệch |
|--------|----------------------|---------------------|-----------|
| Bước `Set up JDK 17` (setup-java) | ... s | ... s | ... s |
| Bước `./gradlew test` | ... s | ... s | ... s |
| Tổng thời gian Job | ... s | ... s | ... s (...%) |

## Log xác nhận cache
- Lần 1: `Cache not found for input keys: gradle-...` (không có cache, cache được lưu ở bước Post setup-java)
- Lần 2: `Cache restored successfully` / `Cache restored from key: gradle-...`

> Dán ảnh chụp màn hình log và bảng thời gian vào đây:
> ![lan-1](images/run1.png)
> ![lan-2](images/run2.png)

## Nhận xét
Lần 2 không phải tải lại dependencies từ Internet nhờ cache khôi phục từ key `gradle-<hash của file gradle/wrapper và *.gradle*>`, nên bước `./gradlew test` nhanh hơn rõ rệt và Job giảm từ ... s xuống ... s.