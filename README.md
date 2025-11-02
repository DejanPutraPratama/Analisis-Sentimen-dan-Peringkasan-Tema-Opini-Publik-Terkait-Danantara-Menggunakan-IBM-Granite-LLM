# Analisis Sentimen dan Peringkasan Tema Opini Publik Terkait Danantara Menggunakan IBM Granite LLM

## 1. Project Overview

Proyek ini bertujuan untuk menganalisis sentimen dan persepsi publik terhadap "Danantara", sebuah entitas yang baru-baru ini menjadi topik diskusi hangat di media sosial (X/Twitter). Seiring dengan banyaknya diskusi, muncul polarisasi opini.

Tujuan dari analisis ini adalah untuk:
1.  Mengklasifikasikan sentimen publik (positif, negatif, atau netral) dari tweet-tweet terkait "Danantara".
2.  Mengidentifikasi dan meringkas tema-tema utama atau isu-isu yang dibicarakan, baik dari sisi positif maupun keluhan (negatif).
3.  Memberikan wawasan (insight) berbasis data mengenai bagaimana "Danantara" dipersepsikan oleh masyarakat.

## 2. Raw Dataset Link

Dataset yang digunakan adalah "Analisis Sentimen Publik Danantara" yang bersumber dari Kaggle. Dataset ini berisi kumpulan tweet dalam file `data_clean_danantara.csv` yang menyebutkan kata kunci "Danantara".

* **Link Kaggle Dataset:** `https://www.kaggle.com/datasets/rezkanorhafizah/analisis-sentimen-publik-danantara`

## 3. Insight & Findings

Analisis dilakukan pada sampel acak sebanyak 20 tweet dari total 127 data.

### Temuan 1: Distribusi Sentimen
Hasil klasifikasi sentimen menggunakan AI menunjukkan sentimen publik yang cenderung negatif dan terpolarisasi.

* **Negatif:** 40.0%
* **Positif:** 30.0%
* **Netral:** 25.0%
* **(Error Parsing):** 5.0%


### Temuan 2: Ringkasan Tema Utama
AI juga digunakan untuk meringkas tema-tema dari kelompok tweet positif dan negatif.

**Tema Positif (Pujian/Harapan):**
* **Manajemen Aset & Kesejahteraan:** Publik melihat Danantara sebagai entitas yang berpotensi mengoptimalkan aset dan investasi untuk kesejahteraan masyarakat.
* **Pentingnya Integritas:** Adanya harapan agar Danantara dikelola secara profesional dan bersih untuk membawa dampak positif bagi BUMN dan ekonomi.
* **Dukungan Proyek Strategis:** Terdapat sentimen positif terkait 21 proyek strategis yang akan didanai oleh Danantara.

**Tema Negatif (Keluhan/Kekhawatiran):**
* **Kekhawatiran Korupsi:** Isu utama adalah ketakutan bahwa Danantara akan menjadi sarana korupsi baru atau tempat salah kelola aset BUMN.
* **Dampak pada Pegawai Honorer:** Muncul keluhan terkait efisiensi dan restrukturisasi yang berdampak pada penundaan gaji pegawai honorer.
* **Kurangnya Transparansi:** Ada keraguan publik terhadap transparansi dan akuntabilitas, terutama terkait siapa yang mengawasi Danantara.
* **Alokasi Sumber Daya:** Terdapat kritik terhadap alokasi sumber daya (misalnya, dana Pertamina) yang dinilai tidak tepat sasaran.

## 4. AI Support Explanation

Proyek ini menggunakan AI (Large Language Model) untuk dua tugas utama:

1.  **Model yang Digunakan:** `ibm-granite/granite-3.3-8b-instruct` melalui API Replicate.
2.  **Tugas 1: Klasifikasi Sentimen**
    * **Metode:** Setiap tweet dikirim ke LLM satu per satu menggunakan *prompt* terstruktur yang meminta klasifikasi 'positif', 'negatif', atau 'netral' dengan format `- Sentiment: [sentimen]`.
    * **Parsing:** Sebuah fungsi Python kustom (`parse_sentiment_respone`) dibuat untuk mengekstrak jawaban AI dari respons mentah. Fungsi ini juga memiliki logika *fallback* untuk menangkap jawaban 'parse error' jika format AI tidak terduga.
3.  **Tugas 2: Peringkasan Tema (Summarization)**
    * **Metode:** Tweet yang sudah diklasifikasi (positif dan negatif) digabungkan menjadi dua blok teks besar.
    * **Prompt:** Masing-masing blok teks dikirim ke LLM dengan *prompt* yang berbeda; satu untuk meringkas "tema-tema positif" dan satu lagi untuk "tema-tema negatif (keluhan)".
4.  **Manajemen API:** Jeda `time.sleep(11)` diterapkan setelah *setiap* panggilan API (baik untuk klasifikasi maupun peringkasan) untuk menghormati *rate limit* API Replicate dan mencegah *error*.
