# Intel Image Classification: CNN & Transfer Learning

Projede amaç, **Intel Image Classification veri seti üzerinde CNN ve Transfer Learning modelleri ile görüntü sınıflandırması yapmak**, model performansını değerlendirmek ve Grad-CAM gibi görselleştirme teknikleri ile modelin karar mekanizmasını analiz etmektir.  

---

## 1. Kullanılan Veri Seti: Intel Image Classification
- **Tür:** Multiclass Image Classification  
- **Sınıf Sayısı:** 6  
  - Buildings, Forest, Glacier, Mountain, Sea, Street  
- **Boyut:** ~25.000 eğitim, 14.000 test görüntüsü  
- **Açıklama:** Farklı doğal ve yapay ortamların sınıflandırılması amaçlanmıştır.  
- [Kaggle Linki](https://www.kaggle.com/datasets/puneet6060/intel-image-classification)  

---

## 2. Seçilen Model ve Yaklaşım
- **CNN (Custom Convolutional Neural Network)**  
- **Transfer Learning:** MobileNetV2  
- Her iki model de **6 sınıf** üzerinde eğitilmiştir.  

### 2.1 CNN Modeli
- 4 konvolüsyon bloğu (Conv2D + BatchNorm + MaxPooling)
- GlobalAveragePooling2D → Dense(256) → Dropout(0.3) → Softmax output
- L2 regularizasyonu ve ReLU aktivasyonu kullanıldı.

### 2.2 Transfer Learning Modeli
- **MobileNetV2**, `include_top=False`  
- GlobalAveragePooling2D → Dense(128, ReLU, L2) → Dropout(0.5) → Softmax output  
- Base model ilk etapta donduruldu, fine-tuning opsiyonel.  

---

## 3. Teknik Detaylar

### 3.1 Veri Hazırlığı
- Görseller **150x150** boyutuna yeniden boyutlandırılmıştır.  
- `train_ds`, `val_ds`, `test_ds` olarak `tf.data.Dataset` formatında hazırlandı.  
- Batch normalization ve one-hot encoding uygulanmıştır.  

### 3.2 Hiperparametre Optimizasyonu
- **Keras Tuner** ile CNN modelinin:
  - Konvolüsyon blok sayısı  
  - Filtre sayıları  
  - Kernel boyutları  
  - Dropout oranları  
  - Learning rate  
  parametreleri optimize edildi.  
- Val_accuracy üzerinden en iyi kombinasyon seçildi.  

### 3.3 Eğitim ve Callback’ler
- **EarlyStopping:** `val_loss` izlenerek patience=5  
- **ReduceLROnPlateau:** learning rate otomatik azaltma  
- Optimizer: Adam, learning rate=1e-4  
- Loss: Categorical Crossentropy  

---

## 4. Model Değerlendirme

### 4.1 Accuracy / Loss Grafikleri
- Train ve validation setleri için accuracy ve loss çizimleri yapıldı.  
- CNN ve TL modellerinin performansı karşılaştırıldı.  
- Validation loss’un minimum olduğu epoch’lar işaretlendi.  

### 4.2 Confusion Matrix
- Test seti üzerinde her iki model için **confusion matrix** oluşturuldu.  
- Hangi sınıfların daha zor sınıflandırıldığı görselleştirildi.  

### 4.3 Grad-CAM Görselleştirmesi
- Test setinden rastgele örnekler seçildi.  
- Her sınıf için:
  - Orijinal görsel  
  - CNN Grad-CAM  
  - Transfer Learning Grad-CAM  
  görselleri yan yana gösterildi.  
- Modelin karar verirken hangi bölgeleri dikkate aldığı analiz edildi.  

---

## 5. Sonuçlar
- Transfer Learning modeli çoğu durumda CNN’den daha hızlı ve yüksek doğruluk sağladı.  
- Grad-CAM analizleri, modelin sınıflandırma kararlarını hangi bölgelerden aldığı konusunda fikir verdi.  

---

## 6. Gelecek Çalışmalar
- EfficientNet veya ResNet gibi farklı Transfer Learning modelleri ile karşılaştırma.  
- Veri artırma (data augmentation) ile model genelleme yeteneğinin geliştirilmesi.  
- Web veya mobil uygulama üzerinden gerçek zamanlı sınıflandırma deploy edilmesi.  
- Daha büyük ve çeşitli veri setleri ile modelin genelleme kapasitesinin artırılması.  

---

## 7. Link
- Kaggle Notebook: [https://www.kaggle.com/code/ecemkorkmaz/image-classification-cnn-tl](https://www.kaggle.com/code/ecemkorkmaz/image-classification-cnn-tl)  
