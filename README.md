#  STM32 Timer Kesmeli Akıllı Geri Sayım & Röle Kontrol Sistemi

Bu proje, gömülü sistemlerde zamanlama işlemlerinin Polling (Sürekli Sorgulama) yöntemi yerine, donanımsal **Timer (Zamanlayıcı)** birimi ve **Interrupt (Kesme)** mekanizması kullanılarak nasıl gerçekleştirileceğini göstermektedir[cite: 4]. STM32F407 mikrodenetleyicisi üzerinde çalışan bu sistem, süre bitiminde bir röleyi tetikleyerek yüksek güçlü bir yükün anahtarlanmasını simüle eder[cite: 4].

## 🚀 Öne Çıkan Özellikler (Highlights)

*   **Donanımsal Zamanlayıcı (Timer 2):** 16 MHz sistem saati kullanılarak, Prescaler (15999) ve Period (999) değerleriyle tam 1 Hz (1 Saniye) aralıklarla donanımsal kesme (Interrupt) üretecek şekilde yapılandırılmıştır[cite: 4].
*   **Non-Blocking (Bloklamayan) Mimari:** İşlemci (CPU), zamanı sürekli sorgulamak yerine kendi rutin işlerini yaparken, Timer'dan gelen `HAL_TIM_PeriodElapsedCallback` kesmesiyle uyarılır[cite: 4].
*   **Röle (Relay) Kontrolü:** Süre dolduğunda, yüksek akımlı devreleri (Örn: 220V Motor, Lamba) kontrol etmek üzere elektromekanik röle tetiklenir[cite: 4].
*   **Görsel ve İşitsel Geri Bildirim:** Geri sayım süreci ve alarm durumu, özel olarak yazılmış kütüphane ile sürülen 16x2 Karakter LCD, durum LED'leri ve Buzzer üzerinden kullanıcıya anlık olarak bildirilir[cite: 4].

## 🛠️ Donanım ve Pin Yapılandırması

Sistemdeki dış birimlerin STM32 üzerindeki pin atamaları aşağıdaki gibidir[cite: 4]:

| Bileşen | Pin Kodu | Mod (Mode) | Açıklama |
| :--- | :--- | :--- | :--- |
| **LCD RS & EN** | `PD2`, `PD3` | GPIO_Output | LCD Komut/Veri Seçimi ve Enable Sinyali[cite: 4]. |
| **LCD Veri (D4-D7)** | `PC4`, `PC13-15` | GPIO_Output | 4-bit paralel LCD Veri Hattı[cite: 4]. |
| **Röle (Relay)** | `PC7` | GPIO_Output | Yük Kontrol Anahtarı (Logic 1 tetiklemeli)[cite: 4]. |
| **Buzzer** | `PA8` | GPIO_Output | Alarm durumunda sesli uyarı çıkışı[cite: 4]. |
| **Durum LED'leri** | `PE0`, `PE1` | GPIO_Output | Sistem çalışma durumu ve Alarm LED'leri[cite: 4]. |

## 📂 Yazılım Mimarisi

*   `Core/Inc/lcd.h` & `Core/Src/lcd.c`: 16x2 karakter LCD ekranı 4-bit modunda sürmek için pin seviyesinde sıfırdan oluşturulmuş sürücü dosyaları[cite: 4].
*   **Durum Makinesi:** 
    *   **Normal Mod:** Timer kesmesinden gelen bayrak kontrol edilir. Süre azaltılarak LCD'ye ve "Sistem Çalışıyor" efektini veren LED'e (`PE0`) yansıtılır[cite: 4].
    *   **Alarm Modu:** Sayaç 0'a ulaştığında Röle (`PC7`) aktif edilir, LCD'ye uyarı mesajı basılır ve Buzzer ile Alarm LED'i (`PE1`) flaşör modunda çalışmaya başlar[cite: 4].

## 💻 Nasıl Çalıştırılır?

1.  Projeyi `STM32CubeIDE` ile açın.
2.  Kodu derleyin (`Build`) ve ArmApp-18 üzerindeki STM32F407 kartına yükleyin[cite: 4].
3.  Enerji verildiğinde LCD başlatılarak 1 saniye boyunca "SISTEM HAZIR" mesajı görünür, bu esnada Röle ve Buzzer pasif durumdadır[cite: 4].
4.  Geri sayım başlar. Süre 00 olduğunda röle kontağının çekme sesini ("ÇIT") duyabilir, LCD üzerindeki alarm uyarısını ve flaşör efektini gözlemleyebilirsiniz[cite: 4].

<img width="1024" height="683" alt="image" src="https://github.com/user-attachments/assets/65f5687d-564e-42e5-8fcc-1522864d47e0" />
