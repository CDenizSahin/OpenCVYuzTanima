# OpenCV Yüz Tanıma (WinForms + Emgu CV)

Windows Forms (.NET Framework 4.8) üzerinde Emgu CV ile gerçek zamanlı yüz tespiti ve LBPH tabanlı yüz tanıma uygulaması.

## Özellikler
- **Kamerayı Başlat/Durdur**: Canlı görüntü akışı
- **Yüz Kaydet**: İsim alıp kırpılmış yüzü `dataset/isim_index.jpg` şeklinde kaydeder
- **LBPH Tanıma**: Açılışta ve her kayıt sonrası verisetinden modeli eğitir
- **Çoklu Yüz**: Karedeki tüm yüzlere dikdörtgen ve isim yazar
- **Ayna Modu**: Ön izleme yatay çevrilmiş (mirror)
- **Hata Uyarıları**: MessageBox ile kullanıcı dostu uyarılar

## Ekran Görüntüsü
> Dilersen `assets/screenshots/` altına ekran görüntüsü ekleyip README’ye koyabilirsin.

## Proje Yapısı
```
OpenCVYuzTanima.csproj
Program.cs
MainForm.cs
MainForm.Designer.cs
assets/
  └─ haarcascade_frontalface_default.xml
dataset/  (çalışma anında otomatik oluşturulur)
```

## Bağımlılıklar
- .NET Framework 4.8 (Windows Forms)
- NuGet paketleri:
  - Emgu.CV (4.12.0.5764)
  - Emgu.CV.runtime.windows (4.12.0.5764)
- OpenCV Haar Cascade:
  - `assets/haarcascade_frontalface_default.xml`
  - Kaynak: https://github.com/opencv/opencv/blob/master/data/haarcascades/haarcascade_frontalface_default.xml

## Kurulum
1. Visual Studio (2019/2022) ile projeyi aç.
2. NuGet geri yükleme otomatik yapılır (paketler csproj’da tanımlı).
3. `assets/haarcascade_frontalface_default.xml` dosyasının gerçek içerik olduğundan emin ol.

Alternatif (komut satırı):
```
dotnet restore
dotnet build -c Debug
```

## Çalıştırma
- Visual Studio üzerinden F5
- veya komut satırı:
```
./bin/Debug/net48/OpenCVYuzTanima.exe
```

## Kullanım
- **Başlat**: Kamerayı açar ve yüz tespitini başlatır.
- **İsim Gir + Yüz Kaydet**: O andaki ilk tespit edilen yüzü griye çevirip 100x100 kırpar ve `dataset/isim_index.jpg` olarak kaydeder. Ardından LBPH modeli yeniden eğitilir.
- **Tanıma**: Canlı akışta her yüz için sınır kutusu ve isim yazılır. Eşik kontrolü `Distance < 80` ile yapılır.
- **Durdur**: Kamerayı kapatır.

## Yapılandırma
- Eşik değeri: `MainForm.cs` içindeki `RecognizeThreshold` (varsayılan `80.0`).
- Eğitim görüntüsü boyutu: `_trainSize = 100x100`.
- LBPH parametreleri: `new LBPHFaceRecognizer(1, 8, 8, 8, 80)`.
- Ayna Modu: Ön izleme `FlipType.Horizontal` ile yatay çevrilir.

## Dataset Formatı
- Dosya adı: `isim_index.jpg` (örn: `cemdeniz_1.jpg`)
- Bir kişiye ait birden fazla örnek kaydetmek doğruluğu artırır (farklı açı, ışık, ifade).

## Sorun Giderme
- **BadImageFormatException**: Proje `x64` hedefli (csproj’da ayarlı). Sisteminin 64-bit olduğundan emin ol.
- **Kamera siyah ekran**:
  - Başka uygulamalar (Zoom/Teams/OBS) kamerayı kapat.
  - Kod, DirectShow/MSMF/Any backend ve 0/1/2 indeks fallback ile açmayı dener.
  - UI güncellemesi `BeginInvoke` ile yapılıyor; yine de sorun varsa sürücüyü güncelle.
- **Cascade Hatası**: `assets` içindeki cascade dosyasının gerçek ve bozulmamış olduğundan emin ol.

## Geliştirme Fikirleri
- Kamera seçimi (ComboBox)
- Toplu yüz kaydetme (karedeki tüm yüzleri isimle kaydetme)
- Log/Status paneli (formda tanı ve hata mesajlarını gösterme)
- Veri temizleme aracı (dataset’teki bozuk görselleri tespit etme)

## Lisans
- Bu proje için uygun bir lisans ekleyebilirsin (ör. MIT). Emgu CV/OpenCV lisans koşullarına uy.

## Katkı
- Issue ve PR’lere açıktır. İyileştirme önerilerini bekliyorum.
