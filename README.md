# Fashion-MNIST Giyim Eşyası Sınıflandırma Projesi

Bu proje, Fashion-MNIST veri seti üzerinde derin öğrenme tabanlı bir giyim eşyası sınıflandırma modeli içermektedir. Model, 1T-shirt/top, Trouser, Pullover, Dress, Coat, Sandal, Shirt, Sneaker, Bag, Ankle boot kategorilerini sınıflandırmak için bir Evrişimli Sinir Ağı (CNN) kullanmaktadır.

## Proje Yapısı

```
FashionMNIST-Classification/
│
├── FashionMNIST_Classification.ipynb       # Ana Jupyter Notebook (tüm analiz ve kodlar)
├── README.md                        # Proje dokümantasyonu (bu dosya)
```

## Model Mimarisi

- Çok katmanlı CNN yapısı kullanılmıştır:
  - İki evrişim bloğu (Conv2D + BatchNormalization + MaxPooling + Dropout)
  - Flatten katmanı ile özelliklerin düzleştirilmesi
  - 256 nöronlu fully connected (Dense) katman + BatchNormalization + Dropout
  - 10 nöronlu Softmax çıkış katmanı

### Katman Seçim Gerekçeleri

- **ReLU Aktivasyonu**: Hesaplama verimliliği ve gradyan kaybını önleme
- **Nöron Sayıları ve Filtre Miktarları**:
   - İlk Conv2D katmanlarında 32 filtre ile başlanmış, ardından 64 filtreye çıkarak modelin öğrenme kapasitesi artırılmıştır.
   - Dense katmanda 256 nöron kullanılmıştır; bu katman karmaşık ilişkilerin öğrenilmesini sağlar.
   - Kademeli olarak filtre sayısının artırılması ve dropout kullanımı aşırı öğrenmeyi engellemeye yöneliktir.
- **BatchNormalization Katmanları**: Eğitim sürecini hızlandırır ve ağın daha stabil öğrenmesini sağlar.
- **Dropout Katmanları**: %25 ve %50 oranlarında dropout ile aşırı öğrenme (overfitting) önlenmiştir.
- **Softmax**: Çok sınıflı sınıflandırma için ideal çıkış aktivasyonu

## Eğitim Parametreleri

- **Optimizer**: Adam (learning_rate=0.001)
- **Loss Fonksiyonu**: Categorical Crossentropy
- **Batch Size**: 64 (bellek ve performans dengesi)
- **Epoch**: 40 (erken durdurma (EarlyStopping) ile val_loss iyileşmezse eğitim durdurulur)
- **Validation Split**: 0.2 (eğitim verisinin %20'si doğrulama için)

## Sonuçlar ve Performans

Model test setinde **%93.26 doğruluk** elde etmiştir. Detaylı performans metrikleri:

### Karışıklık Matrisi Analizi

- En yüksek performans Trouser, Sandal ve Bag sınıflarında (F1-score > 0.98)
- En çok karıştırılan rakam çiftleri: Coat-Shirt, T-shirt/top-Shirt, Pullover-Coat
- Tüm sınıflarda dengeli bir performans (precision(hassasiyet) ve recall(geri çağırma) değerleri birbirine yakın)

### Sınıflandırma Raporu

```
              precision    recall  f1-score   support

           0     0.8940    0.8770    0.8854      1000
           1     0.9950    0.9920    0.9935      1000
           2     0.8884    0.9230    0.9053      1000
           3     0.9245    0.9300    0.9272      1000
           4     0.9108    0.8780    0.8941      1000
           5     0.9890    0.9860    0.9875      1000
           6     0.7937    0.8080    0.8008      1000
           7     0.9681    0.9700    0.9690      1000
           8     0.9940    0.9880    0.9910      1000
           9     0.9721    0.9740    0.9730      1000

    accuracy                         0.9326     10000
   macro avg     0.9329    0.9326    0.9327     10000
weighted avg     0.9329    0.9326    0.9327     10000
```

## Kullanım

### Gereksinimler

```bash
pip install numpy matplotlib tensorflow scikit-learn seaborn
```

### Çalıştırma

1. Jupyter Notebook'u başlatın:
```bash
jupyter notebook FashionMNIST_Classification.ipynb
```

2. Alternatif olarak, Python scriptleriyle çalıştırabilirsiniz:
```bash
python -m models.fashion_mnist_model
```

## İyileştirme Önerileri

1. **Veri Artırma (Data Augmentation)**: Yanlış sınıflandırılan örnekler için dönüşümler uygulama

2. **Model Mimarisi**: Daha karmaşık Konvolüsyonel Sinir Ağları (CNN) deneme

3. **Optimizasyon**:
   - Öğrenme oranı zamanlaması (Learning Rate Scheduling)
   - Farklı optimizerlar (RMSprop, Nadam) test etme

4. **Detaylı Analiz**:
   - Yanlış sınıflandırılan örnekleri inceleme
   - Model kararlılığını anlama teknikleri (ör. Grad-CAM)

## Katkı

Katkıda bulunmak isterseniz lütfen fork edip pull request gönderin. Özellikle:

- Model performansını artıracak öneriler
- Kod iyileştirmeleri
- Yeni özellikler ve analizler

## Lisans

Bu proje MIT lisansı altında lisanslanmıştır. Daha fazla bilgi için `LICENSE` dosyasına bakınız.