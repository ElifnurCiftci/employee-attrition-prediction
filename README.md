# CTU Çalışan Ayrılması, Maaş ve Terfi Tahminleme Modeli

Bu proje, **TOBB Ekonomi ve Teknoloji Üniversitesi Bilgisayar Mühendisliği Bölümü (YAP470/BIL470)** kapsamında, **Beyza Değirmenci** ve **Elifnur Çiftçi** tarafından geliştirilmiştir. 

Proje, CTU İnsan Kaynakları İlişkisel Veritabanı'nı (CTU Employee Relational Database) kullanarak çalışanların **işten ayrılma (attrition)**, **güncel maaş (salary)** ve **yöneticiliğe terfi (promotion)** durumlarını makine öğrenmesi modelleri ile analiz etmeyi ve tahminlemeyi amaçlamaktadır[cite: 1].

---

## Proje Özeti ve Bulgular

Yaklaşık 300.000 çalışan ve 2.8 milyon maaş kaydını içeren ilişkisel SQL tabloları birleştirilerek tek bir analitik veri seti (`employee_snapshot_data.parquet`) oluşturulmuştur.

- **Maaş Tahmini (Salary Prediction):**
  - **Model:** LightGBM, Random Forest Regressor, Linear Regression
  - **En İyi Performans:** **LightGBM** ($R^2 = 0.418$, $\text{RMSE} = 13,465$)
- **İşten Ayrılma Tahmini (Attrition Prediction):**
  - **Model:** XGBoost, Random Forest, Logistic Regression
  - **En İyi Performans:** **XGBoost** (ROC-AUC: 0.889, Ayrılan çalışanlar için en yüksek Duyarlılık/Recall: 0.74)
  - **Doğruluk (Accuracy):** Random Forest en yüksek doğruluğu (0.889) sağlarken, risk altındaki çalışanları tespit etmek için Recall başarısı nedeniyle XGBoost tercih edilmiştir.
- **Terfi Tahmini (Promotion Prediction):**
  - Yönetici pozisyonları toplam çalışanların %0.01'inden azını oluşturduğu için aşırı sınıf dengesizliği (extreme class imbalance) gözlemlenmiş ve analiz seviyesinde değerlendirilmiştir.

---

## Veri Özellikleri (Feature Engineering)

Projede kullanılan ve türetilen temel değişkenler:
- `tenure`: Çalışanın şirketteki kıdemi (yıl cinsinden).
- `age`: Çalışanın yaşı.
- `has_resigned` *(Hedef Değişken)*: Çalışanın ayrılma durumu (1: Ayrıldı, 0: Aktif).
- `is_manager` *(Hedef Değişken)*: Yöneticilik durumu (1: Yönetici, 0: Değil).
- `current_salary` *(Hedef Değişken)*: Güncel maaş miktarı.

---
## Kurulum ve Çalıştırma

### 1. Gerekli Kütüphaneler

Projenin çalışması için aşağıdaki Python kütüphanelerinin yüklü olması gerekmektedir:

bash
pip install pandas numpy matplotlib seaborn scikit-learn xgboost lightgbm sqlalchemy pymysql pyarrow

### 2. Veri Hazırlığı (SQL'den Parquet'ye)

Eğer veritabanından veri çekilecekse, Jupyter Notebook içerisindeki ilgili kod bloğunun yorum satırları kaldırılarak MySQL bağlantısı ile çalıştırılabilir:
1. **Altı farklı SQL tablosu** (`employees`, `departments`, `dept_emp`, `dept_manager`, `salaries`, `titles`) yüklenir.
2. Çalışanların en güncel durumları filtrelenir.
3. Tüm tablolar birleştirilerek `employee_snapshot_data.parquet` dosyası olarak kaydedilir.

### 3. Model Eğitimi ve Analiz

Hazırlanan `employee_snapshot_data.parquet` dosyası yüklendikten sonra notebook adımları sırasıyla çalıştırılarak Keşifçi Veri Analizi (EDA) grafikleri oluşturulabilir ve tahmin modelleri eğitilebilir.
