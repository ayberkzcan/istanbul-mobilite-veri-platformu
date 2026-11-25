# 0001 Mimari: batch, streaming ve tablo formatı

Durum: Önerildi
Tarih: 23 Kasım 2026

## Bağlam

Platformda iki farklı hızda veri var. Aylık CSV raporları 1 gün gecikebilir. Durak yoğunluğu ise en geç 5 dakikada güncellenmeli. Aynı tabloları hem DuckDB hem Spark kullanacak.

## Karar

Aylık dosyalar batch ile, geçiş olayları Kafka ve Structured Streaming ile işlenecek; tüm tablolar Polaris kataloğu altında Iceberg formatında tutulacak.

## Gerekçe

Aylık raporlar 1 gün gecikmeyi kabul eder. Bu yüzden batch işleme yeterlidir. Durak yoğunluğu için 5 dakika hedefi vardır. Bu hedefi ancak akış işleme karşılar. DuckDB'nin yazdığı Iceberg tablosunu Spark kopyalamadan okur.

Tüm veriyi streaming ile işleme fikrini eledik. Aylık dosyalar için gün boyu akış çalıştırmak maliyeti artırır. Ham Parquet klasörlerini de eledik. Snapshot desteği yoktur ve eşzamanlı yazmada veri bozulabilir.

## Sonuçlar

Hem batch hem streaming hattı kurulacak. Bu durum ekstra iş yükü getirir. Streaming sürekli çalışacağı için sabit maliyet oluşturur. Batch ise sadece çalıştığı süre kadar kaynak tüketir. İki yolun aynı tablolara yazması şema ve dosya takibi gerektirir.
