<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Kuis Pengetahuan Umum Dasar</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
            background-color: #f0f0f0;
            margin: 0;
            padding: 20px;
            box-sizing: border-box;
        }

        .quiz-container {
            background-color: white;
            padding: 30px;
            border-radius: 10px;
            box-shadow: 0 0 15px rgba(0, 0, 0, 0.2);
            width: 100%;
            max-width: 600px;
            text-align: center;
        }

        h1 {
            color: #333;
            margin-bottom: 10px;
        }

        #completion-message {
            color: #28a745;
            font-size: 1.2em;
            font-weight: bold;
            margin-top: 5px;
            margin-bottom: 20px;
        }

        .question-counter-text {
            font-size: 0.9em;
            color: #666;
            margin-bottom: 20px;
        }

        #question-container {
            margin-bottom: 20px;
        }

        #question {
            font-size: 1.5em;
            font-weight: bold;
            margin-bottom: 25px;
            color: #444;
        }

        .btn-grid {
            display: grid;
            grid-template-columns: repeat(2, 1fr);
            gap: 10px;
            margin-bottom: 20px;
        }

        .btn {
            background-color: #007bff;
            color: white;
            border: none;
            padding: 12px 15px;
            border-radius: 5px;
            cursor: pointer;
            font-size: 1em;
            transition: background-color 0.2s ease, box-shadow 0.2s ease;
            word-wrap: break-word;
            min-height: 50px;
            display: flex;
            align-items: center;
            justify-content: center;
            outline: none;
            font-weight: bold;
        }

        .btn:not(.correct):not(.wrong):not(.skip-btn):not(.btn-prev-q) { background-color: #007bff; }
        .btn:not(.correct):not(.wrong):not(.skip-btn):not(.btn-prev-q):focus {
            background-color: #007bff;
            box-shadow: 0 0 0 3px rgba(0, 123, 255, 0.5);
        }
        .btn:not([disabled]):not(.correct):not(.wrong):not(.skip-btn):not(.btn-prev-q):hover {}
        .btn:not([disabled]):not(.correct):not(.wrong):not(.skip-btn):not(.btn-prev-q):focus:hover {
            background-color: #007bff;
            box-shadow: 0 0 0 3px rgba(0, 123, 255, 0.5);
        }

        .btn.correct { background-color: #28a745 !important; box-shadow: none; }
        .btn.correct:hover { background-color: #218838 !important; }
        .btn.correct:focus {
            background-color: #28a745 !important;
            box-shadow: 0 0 0 3px rgba(40, 167, 69, 0.6) !important;
        }

        .btn.wrong { background-color: #dc3545 !important; box-shadow: none; }
        .btn.wrong:hover { background-color: #c82333 !important; }
        .btn.wrong:focus {
            background-color: #dc3545 !important;
            box-shadow: 0 0 0 3px rgba(220, 53, 69, 0.6) !important;
        }

        .btn:disabled {
            cursor: not-allowed;
            opacity: 0.65;
        }
        /* Adjusted to not conflict with new button's disabled state if it's not a skip-btn or answer btn */
        .btn:disabled:not(.correct):not(.wrong):not(.skip-btn):not(.btn-prev-q) {
            background-color: #6c757d !important;
            color: #ccc !important;
        }


        .controls {
            display: flex;
            justify-content: center;
            gap: 10px;
        }

        #skip-navigation-controls {
            justify-content: space-between; /* Adjusted to space-around or similar if needed for 3 buttons */
            margin-top: 40px;
            margin-bottom: 10px;
        }

        .skip-btn { /* This style is for prev-50 and next-50 */
            background-color: #28a745; /* Green */
            color: white;
            padding: 8px 12px;
            font-size: 0.9em;
            min-width: 80px; /* Ensures same width for all skip-type buttons */
        }
        .skip-btn:hover {
            background-color: #218838; /* Darker Green */
            color: white;
        }
        .skip-btn:disabled { /* Default disabled for green skip buttons */
            background-color: #a3d8b0 !important;
            color: #e9f5ec !important;
            /* cursor: not-allowed; is inherited from .btn:disabled */
            /* opacity: 0.65; is inherited from .btn:disabled */
        }

        /* New button style for "Previous Question" */
        .btn-prev-q {
            background-color: #5F9EA0; /* CadetBlue - "biru terang" */
            color: white; /* Text color */
            padding: 8px 12px; /* Same padding as skip-btn */
            font-size: 0.9em; /* Same font size as skip-btn */
            min-width: 80px; /* Same min-width as skip-btn */
        }
        .btn-prev-q:hover:not([disabled]) {
            background-color: #4682B4; /* SteelBlue - darker for hover */
            color: white;
        }
        .btn-prev-q:disabled {
            background-color: #B0C4DE !important; /* LightSteelBlue - for disabled state */
            color: #666666 !important; /* Darker text for readability on light blue */
            /* opacity will be applied by .btn:disabled */
        }


        .hide { display: none !important; }
    </style>
</head>
<body>
    <div class="quiz-container">
        <h1>Pengetahuan Umum Dasar</h1>
        <p id="completion-message" class="hide">Selamat Kuis Sudah Selesai 🎉</p>
        <div id="initial-controls" class="controls">
            <button id="start-btn" class="btn">Mulai</button>
            <button id="continue-btn" class="btn hide">Lanjutkan</button>
        </div>
        <div id="question-counter" class="question-counter-text hide">0/0</div>
        <div id="question-container" class="hide">
            <div id="question">Kata Bahasa Inggris</div>
            <div id="answer-buttons" class="btn-grid">
            </div>
            <div id="skip-navigation-controls" class="controls hide">
                <button id="prev-50-btn" class="btn skip-btn">&laquo; 50</button>
                <button id="prev-question-btn" class="btn btn-prev-q">&lt;</button> <button id="next-50-btn" class="btn skip-btn">50 &raquo;</button>
            </div>
        </div>
    </div>

    <script>
        const startButton = document.getElementById('start-btn');
        const continueButton = document.getElementById('continue-btn');
        const initialControls = document.getElementById('initial-controls');
        const completionMessageElement = document.getElementById('completion-message');
        const questionContainerElement = document.getElementById('question-container');
        const questionElement = document.getElementById('question');
        const answerButtonsElement = document.getElementById('answer-buttons');
        const questionCounterElement = document.getElementById('question-counter');

        const skipNavigationControls = document.getElementById('skip-navigation-controls');
        const prev50Button = document.getElementById('prev-50-btn');
        const prevQuestionButton = document.getElementById('prev-question-btn'); // Referensi untuk tombol baru
        const next50Button = document.getElementById('next-50-btn');
        const JUMP_AMOUNT = 50;

        let orderedQuestions, currentQuestionIndex;
        let score = 0;
        let questionTimeout;

        // Daftar kata mentah dari PDF (Inggris: Indonesia) - Total 1580 kata
        const rawVocabularyList = [


  { "en": "Apa Fungsi Utama Rangkaian Penguat (Amplifier)?", "id": "Meningkatkan Kekuatan Atau Amplitudo Sinyal Listrik." },
  { "en": "Apa Yang Dimaksud Dengan Penguatan (Gain)?", "id": "Rasio Sinyal Output Terhadap Sinyal Input." },
  { "en": "Apa Itu Penguat Tegangan (Voltage Amplifier)?", "id": "Penguat Yang Meningkatkan Level Tegangan Sinyal." },
  { "en": "Apa Itu Penguat Arus (Current Amplifier)?", "id": "Penguat Yang Meningkatkan Level Arus Sinyal." },
  { "en": "Apa Itu Penguat Daya (Power Amplifier)?", "id": "Penguat Yang Meningkatkan Level Daya Sinyal." },
  { "en": "Apa Komponen Aktif Utama Dalam Penguat?", "id": "Transistor Atau Operational Amplifier (Op-Amp)." },
  { "en": "Apa Itu Operational Amplifier (Op-Amp)?", "id": "Penguat Diferensial Penguatan Sangat Tinggi." },
  { "en": "Sebutkan Dua Input Pada Operational Amplifier (Op-Amp)?", "id": "Input Inverting Dan Non-Inverting." },
  { "en": "Apa Ciri Penguat Kelas A?", "id": "Transistor Aktif Selama Seluruh Siklus Input." },
  { "en": "Berapa Efisiensi Maksimum Teoretis Penguat Kelas A?", "id": "Efisiensi Maksimumnya Adalah Dua Puluh Lima Persen." },
  { "en": "Apa Ciri Penguat Kelas B?", "id": "Dua Transistor Aktif Bergantian Setengah Siklus." },
  { "en": "Apa Masalah Utama Pada Penguat Kelas B?", "id": "Cacat Crossover (Crossover Distortion)." },
  { "en": "Apa Ciri Penguat Kelas AB?", "id": "Gabungan Kelas A Dan B." },
  { "en": "Apa Tujuan Dari Konfigurasi Kelas AB?", "id": "Menghilangkan Cacat Crossover Sambil Tetap Efisien." },
  { "en": "Apa Ciri Penguat Kelas C?", "id": "Transistor Aktif Kurang Dari Setengah Siklus." },
  { "en": "Di Mana Penguat Kelas C Biasa Digunakan?", "id": "Aplikasi Frekuensi Radio (RF) Seperti Pemancar." },
  { "en": "Apa Itu Bandwidth Sebuah Penguat?", "id": "Rentang Frekuensi Kerja Efektif Sebuah Penguat." },
  { "en": "Apa Itu Distorsi Harmonik?", "id": "Munculnya Frekuensi Harmonik Yang Tidak Diinginkan." },
  { "en": "Apa Itu Umpan Balik Negatif (Negative Feedback)?", "id": "Mengembalikan Sebagian Sinyal Output Ke Input." },
  { "en": "Apa Manfaat Umpan Balik Negatif Pada Penguat?", "id": "Meningkatkan Stabilitas Dan Mengurangi Distorsi." },
  { "en": "Apa Itu Penguat Diferensial?", "id": "Menguatkan Selisih Antara Dua Sinyal Input." },
  { "en": "Apa Itu Common-Mode Rejection Ratio (CMRR)?", "id": "Kemampuan Menolak Sinyal Yang Sama." },
  { "en": "Apa Itu Impedansi Input Penguat?", "id": "Hambatan Yang Dilihat Sinyal Input." },
  { "en": "Bagaimana Seharusnya Impedansi Input Penguat Tegangan?", "id": "Seharusnya Sangat Tinggi Agar Tidak Membebani." },
  { "en": "Bagaimana Seharusnya Impedansi Output Penguat Tegangan?", "id": "Seharusnya Sangat Rendah Untuk Transfer Daya." },
  { "en": "Apa Fungsi Utama Filter Analog?", "id": "Meloloskan Frekuensi Tertentu Dan Meredam Lainnya." },
  { "en": "Apa Itu Filter Lolos Bawah (Low-Pass Filter)?", "id": "Meloloskan Frekuensi Di Bawah Frekuensi Cutoff." },
  { "en": "Apa Itu Filter Lolos Atas (High-Pass Filter)?", "id": "Meloloskan Frekuensi Di Atas Frekuensi Cutoff." },
  { "en": "Apa Itu Filter Lolos Pita (Band-Pass Filter)?", "id": "Meloloskan Rentang Frekuensi Tertentu." },
  { "en": "Apa Itu Filter Tolak Pita (Band-Stop Filter)?", "id": "Meredam Rentang Frekuensi Tertentu." },
  { "en": "Apa Itu Frekuensi Cutoff?", "id": "Frekuensi Di Mana Daya Sinyal Setengah." },
  { "en": "Frekuensi Cutoff Dikenal Juga Sebagai Apa?", "id": "Frekuensi -3 Desibel (dB)." },
  { "en": "Apa Itu Filter Pasif?", "id": "Filter Yang Terbuat Dari Komponen Pasif." },
  { "en": "Sebutkan Contoh Komponen Untuk Filter Pasif?", "id": "Resistor, Kapasitor, Dan Induktor." },
  { "en": "Apa Itu Filter Aktif?", "id": "Filter Menggunakan Komponen Aktif Seperti Op-Amp." },
  { "en": "Apa Keuntungan Filter Aktif Dibanding Pasif?", "id": "Memiliki Penguatan Dan Tidak Membutuhkan Induktor." },
  { "en": "Bagaimana Membuat Filter Low-Pass Pasif Sederhana?", "id": "Dengan Rangkaian Resistor Dan Kapasitor Seri." },
  { "en": "Bagaimana Membuat Filter High-Pass Pasif Sederhana?", "id": "Dengan Rangkaian Kapasitor Dan Resistor Seri." },
  { "en": "Apa Itu Orde Filter?", "id": "Menentukan Kecuraman Transisi Dari Passband Stopband." },
  { "en": "Bagaimana Orde Filter Mempengaruhi Respon?", "id": "Orde Lebih Tinggi Memberikan Transisi Lebih Curam." },
  { "en": "Apa Itu Passband Dalam Sebuah Filter?", "id": "Rentang Frekuensi Yang Diloloskan Oleh Filter." },
  { "en": "Apa Itu Stopband Dalam Sebuah Filter?", "id": "Rentang Frekuensi Yang Diredam Oleh Filter." },
  { "en": "Apa Itu Riak Passband (Passband Ripple)?", "id": "Variasi Penguatan Di Dalam Daerah Passband." },
  { "en": "Filter Jenis Apa Yang Memiliki Riak Passband?", "id": "Filter Chebyshev Dan Filter Eliptik (Cauer)." },
  { "en": "Apa Keuntungan Utama Filter Butterworth?", "id": "Respon Frekuensi Yang Maksimal Datar (Flat)." },
  { "en": "Apa Itu Resonansi Dalam Rangkaian Filter?", "id": "Puncak Respon Pada Frekuensi Tertentu." },
  { "en": "Filter Apa Yang Menggunakan Rangkaian LC Seri?", "id": "Filter Lolos Pita (Band-Pass Filter)." },
  { "en": "Filter Apa Yang Menggunakan Rangkaian LC Paralel?", "id": "Filter Tolak Pita (Band-Stop Filter)." },
  { "en": "Apa Itu Faktor Kualitas (Q) Filter?", "id": "Ukuran Selektivitas Atau Ketajaman Sebuah Filter." },
  { "en": "Apa Pengaruh Faktor Q Yang Tinggi?", "id": "Bandwidth Filter Menjadi Semakin Sempit." },
  { "en": "Apa Fungsi Utama Rangkaian Osilator?", "id": "Menghasilkan Sinyal Periodik Tanpa Sinyal Input." },
  { "en": "Sebutkan Dua Komponen Utama Rangkaian Osilator?", "id": "Bagian Penguat Dan Jaringan Umpan Balik." },
  { "en": "Apa Syarat Terjadinya Osilasi?", "id": "Kriteria Osilasi Barkhausen." },
  { "en": "Sebutkan Dua Kriteria Osilasi Barkhausen?", "id": "Penguatan Loop Satu Dan Total Fasa 360°." },
  { "en": "Jenis Sinyal Apa Yang Dihasilkan Osilator?", "id": "Bisa Berupa Gelombang Sinus, Kotak, Segitiga." },
  { "en": "Apa Itu Osilator LC?", "id": "Osilator Yang Menggunakan Jaringan Induktor-Kapasitor." },
  { "en": "Apa Contoh Rangkaian Osilator LC?", "id": "Osilator Colpitts, Hartley, Dan Clapp." },
  { "en": "Apa Perbedaan Osilator Colpitts Dan Hartley?", "id": "Colpitts Menggunakan Pembagi Kapasitif, Hartley Induktif." },
  { "en": "Apa Itu Osilator RC?", "id": "Osilator Yang Menggunakan Jaringan Resistor-Kapasitor." },
  { "en": "Apa Contoh Rangkaian Osilator RC?", "id": "Osilator Geser Fasa (Phase-Shift) Wien Bridge." },
  { "en": "Berapa Pergeseran Fasa Setiap Tingkat Osilator Phase-Shift?", "id": "Enam Puluh Derajat (60°) Per Tingkat RC." },
  { "en": "Apa Itu Osilator Kristal?", "id": "Osilator Yang Menggunakan Kristal Kuarsa." },
  { "en": "Apa Keuntungan Utama Osilator Kristal?", "id": "Stabilitas Frekuensi Yang Sangat Tinggi." },
  { "en": "Apa Itu Efek Piezoelektrik Pada Kristal?", "id": "Kristal Bergetar Ketika Diberi Tegangan Listrik." },
  { "en": "Apa Itu Multivibrator Astabil?", "id": "Osilator Yang Menghasilkan Gelombang Kotak." },
  { "en": "Apakah Multivibrator Astabil Memerlukan Sinyal Pemicu?", "id": "Tidak, Berosilasi Secara Terus Menerus." },
  { "en": "Apa Itu Osilator Relaksasi?", "id": "Osilator Menghasilkan Bentuk Gelombang Non-Sinusoidal." },
  { "en": "Apa Itu Jitter Dalam Sinyal Osilator?", "id": "Variasi Perioda Sinyal Yang Tidak Diinginkan." },
  { "en": "Apa Itu Derau Fasa (Phase Noise)?", "id": "Fluktuasi Acak Fasa Sinyal Osilator." },
  { "en": "Apa Fungsi Utama Power Supply (Catu Daya)?", "id": "Menyediakan Energi Listrik DC Untuk Perangkat Elektronik." },
  { "en": "Dari Mana Sumber Energi Catu Daya?", "id": "Umumnya Dari Jaringan Listrik AC." },
  { "en": "Sebutkan Empat Blok Utama Catu Daya Linear?", "id": "Transformator, Penyearah, Filter, Dan Regulator." },
  { "en": "Apa Fungsi Blok Transformator (Trafo)?", "id": "Menurunkan Tegangan AC Dari Jaringan Listrik." },
  { "en": "Apa Fungsi Blok Penyearah (Rectifier)?", "id": "Mengubah Tegangan AC Menjadi Tegangan DC." },
  { "en": "Apa Itu Penyearah Setengah Gelombang?", "id": "Hanya Melewatkan Setengah Siklus Dari Gelombang AC." },
  { "en": "Berapa Banyak Dioda Untuk Penyearah Setengah Gelombang?", "id": "Hanya Membutuhkan Satu Buah Dioda." },
  { "en": "Apa Itu Penyearah Gelombang Penuh?", "id": "Melewatkan Kedua Setengah Siklus Gelombang AC." },
  { "en": "Berapa Dioda Untuk Penyearah Jembatan (Bridge)?", "id": "Membutuhkan Empat Buah Dioda." },
  { "en": "Apa Fungsi Blok Filter Dalam Catu Daya?", "id": "Meratakan Tegangan DC Yang Masih Beriak." },
  { "en": "Komponen Apa Yang Umumnya Digunakan Sebagai Filter?", "id": "Kapasitor Elektrolit Dengan Nilai Kapasitansi Besar." },
  { "en": "Apa Itu Tegangan Riak (Ripple Voltage)?", "id": "Variasi Tegangan DC Setelah Proses Penyearahan." },
  { "en": "Apa Fungsi Blok Regulator Tegangan?", "id": "Menjaga Tegangan Output Agar Tetap Stabil." },
  { "en": "Komponen Apa Yang Digunakan Sebagai Regulator Sederhana?", "id": "Dioda Zener Atau Integrated Circuit (IC)." },
  { "en": "Apa Itu Regulator Tegangan Linear?", "id": "Regulator Yang Beroperasi Di Daerah Linear." },
  { "en": "Apa Kelemahan Regulator Tegangan Linear?", "id": "Efisiensi Rendah Karena Disipasi Panas." },
  { "en": "Apa Itu Catu Daya Switching?", "id": "Catu Daya Menggunakan Saklar Elektronik Cepat." },
  { "en": "Apa Keuntungan Catu Daya Switching?", "id": "Efisiensi Sangat Tinggi Dan Ukuran Kecil." },
  { "en": "Apa Itu Inrush Current?", "id": "Arus Puncak Saat Catu Daya Dinyalakan." },
  { "en": "Apa Fungsi Sekring (Fuse) Pada Catu Daya?", "id": "Melindungi Rangkaian Dari Arus Berlebih." },
  { "en": "Apa Itu Heatsink (Pendingin)?", "id": "Komponen Untuk Membuang Panas Dari Regulator." },
  { "en": "Apa Itu Tegangan Output Unregulated?", "id": "Tegangan Setelah Filter Sebelum Regulator." },
  { "en": "Apa Itu Tegangan Output Regulated?", "id": "Tegangan Stabil Setelah Blok Regulator." },
  { "en": "Apa Itu Penguat Inverting (Inverting Amplifier)?", "id": "Penguat Op-Amp Yang Membalik Fasa Sinyal." },
  { "en": "Berapa Pergeseran Fasa Penguat Inverting?", "id": "Seratus Delapan Puluh Derajat (180°)." },
  { "en": "Bagaimana Rumus Penguatan Penguat Inverting?", "id": "Minus Resistansi Feedback Dibagi Resistansi Input." },
  { "en": "Apa Itu Penguat Non-Inverting?", "id": "Penguat Op-Amp Yang Fasanya Sama." },
  { "en": "Bagaimana Rumus Penguatan Penguat Non-Inverting?", "id": "Satu Ditambah Rasio Resistansi Feedback Input." },
  { "en": "Apa Itu Penguat Penyangga (Buffer Amplifier)?", "id": "Penguat Dengan Penguatan Tegangan Sama Dengan Satu." },
  { "en": "Apa Fungsi Utama Penguat Penyangga?", "id": "Pencocokan Impedansi (Impedance Matching)." },
  { "en": "Apa Itu Penguat Penjumlah (Summing Amplifier)?", "id": "Menjumlahkan Beberapa Sinyal Input Secara Tertimbang." },
  { "en": "Apa Itu Penguat Integrator?", "id": "Rangkaian Op-Amp Yang Menghasilkan Integral Input." },
  { "en": "Apa Itu Penguat Differentiator?", "id": "Rangkaian Op-Amp Menghasilkan Turunan Sinyal Input." },
  { "en": "Apa Itu Slew Rate Pada Op-Amp?", "id": "Laju Perubahan Tegangan Output Maksimum." },
  { "en": "Apa Satuan Dari Slew Rate?", "id": "Volt Per Mikrodetik (V/µs)." },
  { "en": "Apa Itu Gain-Bandwidth Product (GBW)?", "id": "Hasil Perkalian Penguatan Dengan Bandwidth." },
  { "en": "Apa Sifat Gain-Bandwidth Product?", "id": "Nilainya Konstan Untuk Op-Amp Tertentu." },
  { "en": "Apa Itu Bias Transistor?", "id": "Memberikan Tegangan DC Untuk Operasi Linear." },
  { "en": "Apa Titik Operasi Transistor?", "id": "Titik Q (Quiescent Point)." },
  { "en": "Apa Itu Rangkaian Bias Pembagi Tegangan?", "id": "Metode Pembiasan Transistor Yang Paling Stabil." },
  { "en": "Apa Itu Garis Beban DC (DC Load Line)?", "id": "Grafik Hubungan Arus Kolektor Dan Tegangan." },
  { "en": "Apa Itu Penguat Common Emitter?", "id": "Konfigurasi Transistor Dengan Penguatan Tegangan Tinggi." },
  { "en": "Apa Itu Penguat Common Collector?", "id": "Konfigurasi Transistor Dengan Penguatan Arus Tinggi." },
  { "en": "Penguat Common Collector Dikenal Juga Sebagai Apa?", "id": "Pengikut Emitter (Emitter Follower)." },
  { "en": "Apa Itu Penguat Common Base?", "id": "Konfigurasi Transistor Untuk Aplikasi Frekuensi Tinggi." },
  { "en": "Apa Itu Penguat Push-Pull?", "id": "Konfigurasi Dua Transistor Pada Penguat Kelas B." },
  { "en": "Apa Itu Thermal Runaway Pada Transistor?", "id": "Kenaikan Suhu Yang Menyebabkan Kerusakan." },
  { "en": "Apa Itu Miller Effect?", "id": "Peningkatan Kapasitansi Input Akibat Penguatan." },
  { "en": "Berapa Laju Roll-Off Filter Orde Pertama?", "id": "Minus Dua Puluh Desibel Per Dekade (-20dB/decade)." },
  { "en": "Berapa Laju Roll-Off Filter Orde Kedua?", "id": "Minus Empat Puluh Desibel Per Dekade (-40dB/decade)." },
  { "en": "Apa Itu Topologi Sallen-Key?", "id": "Desain Populer Untuk Filter Aktif Orde Kedua." },
  { "en": "Berapa Pergeseran Fasa Filter Low-Pass Orde Satu?", "id": "Dari Nol Hingga Minus Sembilan Puluh Derajat." },
  { "en": "Berapa Pergeseran Fasa Filter High-Pass Orde Satu?", "id": "Dari Sembilan Puluh Hingga Nol Derajat." },
  { "en": "Apa Itu Filter Notch?", "id": "Jenis Filter Tolak Pita Sangat Sempit." },
  { "en": "Apa Fungsi Utama Filter Notch?", "id": "Menghilangkan Satu Frekuensi Spesifik Yang Mengganggu." },
  { "en": "Apa Contoh Aplikasi Filter Notch?", "id": "Menghilangkan Dengung (Hum) 50/60 Hz." },
  { "en": "Apa Itu Filter All-Pass?", "id": "Filter Yang Hanya Mengubah Fasa Sinyal." },
  { "en": "Apa Fungsi Filter All-Pass?", "id": "Untuk Koreksi Fasa Atau Phase Shifter." },
  { "en": "Apa Itu Equalizer Grafis?", "id": "Kumpulan Filter Untuk Mengatur Respon Frekuensi." },
  { "en": "Apa Itu Filter Switched Capacitor?", "id": "Filter Menggunakan Saklar Dan Kapasitor." },
  { "en": "Apa Keuntungan Filter Switched Capacitor?", "id": "Frekuensi Cutoff Dapat Diatur Secara Elektronik." },
  { "en": "Apa Itu Filter Bessel?", "id": "Filter Dengan Respon Fasa Paling Linear." },
  { "en": "Kapan Filter Bessel Digunakan?", "id": "Ketika Bentuk Gelombang Harus Dipertahankan." },
  { "en": "Apa Itu Aliasing Dalam Konteks Filter?", "id": "Frekuensi Tinggi Menyamar Sebagai Frekuensi Rendah." },
  { "en": "Filter Apa Yang Digunakan Sebagai Anti-Aliasing?", "id": "Filter Low-Pass Sebelum Proses Sampling Digital." },
  { "en": "Apa Itu Filter Rekonstruksi?", "id": "Filter Low-Pass Setelah Proses Konversi Digital-Analog." },
  { "en": "Apa Itu Girator (Gyrator)?", "id": "Rangkaian Aktif Yang Mensimulasikan Sebuah Induktor." },
  { "en": "Mengapa Girator Berguna Dalam Filter Aktif?", "id": "Menghindari Penggunaan Induktor Fisik Yang Besar." },
  { "en": "Apa Itu Rangkaian Resonator?", "id": "Rangkaian Yang Berosilasi Pada Frekuensi Tertentu." },
  { "en": "Apa Itu Koefisien Filter?", "id": "Nilai Konstanta Yang Menentukan Respon Filter." },
  { "en": "Bagaimana Cara Mengubah Filter Low-Pass Menjadi High-Pass?", "id": "Menukar Posisi Komponen Resistor Dan Kapasitor." },
  { "en": "Apa Itu Jaringan T-Bridge?", "id": "Topologi Filter Pasif Untuk Membuat Notch." },
  { "en": "Apa Itu State Variable Filter?", "id": "Filter Aktif Serbaguna Menghasilkan Tiga Output." },
  { "en": "Apa Itu Multivibrator Monostabil?", "id": "Rangkaian Dengan Satu Keadaan Stabil." },
  { "en": "Apa Fungsi Multivibrator Monostabil?", "id": "Menghasilkan Satu Pulsa Dengan Durasi Tertentu." },
  { "en": "Apakah Monostabil Membutuhkan Pemicu (Trigger)?", "id": "Ya Membutuhkan Pemicu Untuk Berpindah Keadaan." },
  { "en": "Apa Itu Multivibrator Bistabil?", "id": "Rangkaian Dengan Dua Keadaan Stabil." },
  { "en": "Apa Nama Lain Multivibrator Bistabil?", "id": "Flip-Flop Atau Latch." },
  { "en": "Apa Fungsi Multivibrator Bistabil?", "id": "Sebagai Elemen Penyimpan Memori Satu Bit." },
  { "en": "Apa Itu Integrated Circuit (IC) Timer 555?", "id": "IC Serbaguna Untuk Aplikasi Pewaktuan Osilator." },
  { "en": "Sebutkan Tiga Mode Operasi IC 555?", "id": "Mode Astabil, Monostabil, Dan Bistabil." },
  { "en": "Mode Apa Yang Digunakan Untuk Membuat Osilator?", "id": "Mode Astabil Yang Berosilasi Bebas." },
  { "en": "Apa Itu Voltage-Controlled Oscillator (VCO)?", "id": "Frekuensi Output Osilator Dikontrol Oleh Tegangan." },
  { "en": "Di Mana VCO Biasa Digunakan?", "id": "Phase-Locked Loop (PLL) Dan Synthesizer Frekuensi." },
  { "en": "Apa Itu Phase-Locked Loop (PLL)?", "id": "Sistem Umpan Balik Mengunci Fasa Osilator." },
  { "en": "Apa Itu Osilator Jembatan Wien (Wien Bridge)?", "id": "Osilator RC Populer Untuk Gelombang Sinus." },
  { "en": "Apa Komponen Penentu Frekuensi Osilator Jembatan Wien?", "id": "Dua Pasang Jaringan Resistor Dan Kapasitor." },
  { "en": "Apa Itu Osilator Clapp?", "id": "Modifikasi Dari Osilator Colpitts." },
  { "en": "Apa Keuntungan Osilator Clapp?", "id": "Stabilitas Frekuensi Yang Lebih Baik." },
  { "en": "Apa Itu Armstrong Oscillator?", "id": "Osilator LC Menggunakan Kopling Trafo." },
  { "en": "Apa Itu Ring Oscillator?", "id": "Osilator Terbuat Dari Gerbang Logika Inverter." },
  { "en": "Berapa Jumlah Inverter Untuk Ring Oscillator?", "id": "Harus Berjumlah Ganjil Untuk Dapat Berosilasi." },
  { "en": "Apa Itu Duty Cycle Sinyal?", "id": "Rasio Waktu 'High' Terhadap Total Perioda." },
  { "en": "Berapa Duty Cycle Gelombang Kotak Ideal?", "id": "Lima Puluh Persen (50%)." },
  { "en": "Bagaimana Cara Mengatur Duty Cycle IC 555?", "id": "Dengan Mengubah Rasio Dua Resistor Eksternal." },
  { "en": "Apa Itu Regulasi Beban (Load Regulation)?", "id": "Kemampuan Catu Daya Menjaga Output Stabil." },
  { "en": "Apa Itu Regulasi Jalur (Line Regulation)?", "id": "Kemampuan Catu Daya Menjaga Output Stabil." },
  { "en": "Apa Itu IC Regulator Seri 78xx?", "id": "Keluarga IC Regulator Tegangan Positif Tetap." },
  { "en": "Apa Tegangan Output Dari IC 7805?", "id": "Tegangan Output Stabil Positif Lima Volt." },
  { "en": "Apa Itu IC Regulator Seri 79xx?", "id": "Keluarga IC Regulator Tegangan Negatif Tetap." },
  { "en": "Apa Tegangan Output Dari IC 7912?", "id": "Tegangan Output Stabil Negatif Dua Belas Volt." },
  { "en": "Apa Itu Tegangan Dropout?", "id": "Selisih Tegangan Input Dan Output Minimum." },
  { "en": "Apa Itu Low-Dropout (LDO) Regulator?", "id": "Regulator Dengan Tegangan Dropout Sangat Rendah." },
  { "en": "Apa Itu Rangkaian Proteksi Crowbar?", "id": "Melindungi Beban Dari Tegangan Berlebih (Overvoltage)." },
  { "en": "Bagaimana Cara Kerja Rangkaian Crowbar?", "id": "Membuat Hubung Singkat Untuk Memutuskan Sekring." },
  { "en": "Apa Itu Proteksi Arus Berlebih (Overcurrent)?", "id": "Membatasi Arus Output Pada Nilai Maksimum." },
  { "en": "Apa Itu Foldback Current Limiting?", "id": "Mengurangi Arus Dan Tegangan Saat Kelebihan Beban." },
  { "en": "Apa Itu Konverter Buck?", "id": "Jenis Catu Daya Switching Penurun Tegangan." },
  { "en": "Apa Itu Konverter Boost?", "id": "Jenis Catu Daya Switching Penaik Tegangan." },
  { "en": "Apa Itu Penyearah Sinkron (Synchronous Rectifier)?", "id": "Mengganti Dioda Dengan MOSFET Untuk Efisiensi." },
  { "en": "Apa Itu Kapasitor Bulk (Bulk Capacitor)?", "id": "Kapasitor Utama Penyimpan Energi Catu Daya." },
  { "en": "Apa Itu Kapasitor Bypass Atau Decoupling?", "id": "Menyaring Derau (Noise) Frekuensi Tinggi Lokal." },
  { "en": "Apa Itu Power Supply Rejection Ratio (PSRR)?", "id": "Kemampuan Menolak Riak Dari Jalur Catu Daya." },
  { "en": "Apa Itu Catu Daya Ganda (Dual Supply)?", "id": "Menyediakan Tegangan Positif, Negatif, Dan Ground." },
  { "en": "Kapan Catu Daya Ganda Dibutuhkan?", "id": "Untuk Mengoperasikan Op-Amp Dan Rangkaian Analog." },
  { "en": "Apa Itu Referensi Tegangan (Voltage Reference)?", "id": "Sirkuit Menghasilkan Tegangan Sangat Stabil." },
  { "en": "Apa Itu Penguat Instrumentasi (Instrumentation Amplifier)?", "id": "Penguat Diferensial Presisi Tinggi." },
  { "en": "Apa Keuntungan Utama Penguat Instrumentasi?", "id": "Common-Mode Rejection Ratio (CMRR) Sangat Tinggi." },
  { "en": "Bagaimana Cara Mengatur Penguatan Penguat Instrumentasi?", "id": "Biasanya Dengan Mengubah Satu Resistor Eksternal." },
  { "en": "Apa Itu Penguat Isolasi (Isolation Amplifier)?", "id": "Penguat Dengan Isolasi Elektrik Antar Sisi." },
  { "en": "Kapan Penguat Isolasi Digunakan?", "id": "Dalam Aplikasi Medis Dan Industri Tegangan Tinggi." },
  { "en": "Apa Itu Derau Johnson-Nyquist (Thermal Noise)?", "id": "Derau Yang Dihasilkan Oleh Agitasi Termal." },
  { "en": "Derau Johnson Bergantung Pada Apa?", "id": "Resistansi, Suhu, Dan Bandwidth." },
  { "en": "Apa Itu Derau Tembak (Shot Noise)?", "id": "Derau Akibat Sifat Diskrit Aliran Muatan." },
  { "en": "Apa Itu Derau Kedip (Flicker Noise / 1/f)?", "id": "Derau Frekuensi Rendah Yang Dominan." },
  { "en": "Apa Itu Angka Derau (Noise Figure)?", "id": "Ukuran Degradasi Sinyal-ke-Derau (SNR)." },
  { "en": "Berapa Angka Derau Ideal Sebuah Penguat?", "id": "Nol Desibel (0 dB)." },
  { "en": "Apa Itu Penguat Bertingkat (Cascaded Amplifier)?", "id": "Menghubungkan Output Satu Penguat Ke Input Lain." },
  { "en": "Bagaimana Menghitung Total Penguatan Penguat Bertingkat?", "id": "Dengan Mengalikan Penguatan Setiap Tingkat." },
  { "en": "Apa Itu Distorsi Intermodulasi (IMD)?", "id": "Munculnya Frekuensi Penjumlahan Dan Pengurangan." },
  { "en": "Apa Itu Titik Potong Orde Ketiga (IP3)?", "id": "Ukuran Kinerja Linearitas Sebuah Penguat." },
  { "en": "Apa Itu Penguat Logaritmik?", "id": "Penguat Yang Outputnya Proposional Logaritma Input." },
  { "en": "Apa Itu Penguat Anti-Logaritmik?", "id": "Penguat Yang Outputnya Proposional Eksponensial Input." },
  { "en": "Apa Itu Penguat Transimpedansi?", "id": "Mengubah Sinyal Input Arus Menjadi Tegangan." },
  { "en": "Di Mana Penguat Transimpedansi Digunakan?", "id": "Penerima Optik Untuk Menguatkan Arus Fotodioda." },
  { "en": "Apa Itu Penguat Terdistribusi?", "id": "Penguat Frekuensi Sangat Tinggi (Microwave)." },
  { "en": "Apa Itu Automatic Gain Control (AGC)?", "id": "Rangkaian Otomatis Menyesuaikan Penguatan Penguat." },
  { "en": "Apa Itu Penguat Kelas D?", "id": "Penguat Switching Sangat Efisien." },
  { "en": "Bagaimana Prinsip Kerja Penguat Kelas D?", "id": "Menggunakan Pulse Width Modulation (PWM)." },
  { "en": "Apa Fungsi Filter Output Penguat Kelas D?", "id": "Menghilangkan Komponen Frekuensi Tinggi Dari Sinyal PWM." },
  { "en": "Apa Itu Slew-Induced Distortion (SID)?", "id": "Distorsi Akibat Sinyal Melebihi Slew Rate." },
  { "en": "Apa Itu Penundaan Grup (Group Delay)?", "id": "Ukuran Penundaan Waktu Amplop Sinyal." },
  { "en": "Apa Itu Penundaan Fasa (Phase Delay)?", "id": "Ukuran Penundaan Waktu Setiap Komponen Frekuensi." },
  { "en": "Filter Apa Yang Memiliki Penundaan Grup Konstan?", "id": "Filter Bessel Yang Menjaga Bentuk Gelombang." },
  { "en": "Apa Itu Topologi Filter Biquad?", "id": "Filter Aktif Orde Kedua Menggunakan Integrator." },
  { "en": "Apa Itu Respon Impuls Filter?", "id": "Output Filter Ketika Diberi Input Impuls." },
  { "en": "Apa Hubungan Respon Impuls Dan Respon Frekuensi?", "id": "Respon Frekuensi Adalah Transformasi Fourier Impuls." },
  { "en": "Bagaimana Lokasi Pole Filter Butterworth?", "id": "Pole Terletak Di Atas Lingkaran." },
  { "en": "Bagaimana Lokasi Pole Filter Chebyshev?", "id": "Pole Terletak Di Atas Elips." },
  { "en": "Apa Itu Filter Finite Impulse Response (FIR)?", "id": "Jenis Filter Digital Tanpa Umpan Balik." },
  { "en": "Apa Itu Filter Infinite Impulse Response (IIR)?", "id": "Jenis Filter Digital Dengan Umpan Balik." },
  { "en": "Filter Analog Lebih Mirip FIR Atau IIR?", "id": "Filter Analog Lebih Mirip Dengan IIR." },
  { "en": "Apa Itu Jaringan Lattice?", "id": "Struktur Filter Simetris Untuk Filter All-Pass." },
  { "en": "Apa Itu Filter Adaptif?", "id": "Filter Yang Koefisiennya Dapat Berubah." },
  { "en": "Di Mana Filter Adaptif Digunakan?", "id": "Peredam Gema (Echo Cancellation) Dan Equalization." },
  { "en": "Apa Itu Filter Comb?", "id": "Filter Menghasilkan Serangkaian Notch Berjarak Reguler." },
  { "en": "Bagaimana Respon Frekuensi Filter Comb?", "id": "Terlihat Seperti Gigi Sisir (Comb)." },
  { "en": "Apa Itu Filter Pasir Kuarsa (Crystal Filter)?", "id": "Filter Sangat Selektif Menggunakan Kristal Kuarsa." },
  { "en": "Di Mana Filter Pasir Kuarsa Digunakan?", "id": "Penerima Radio Intermediate Frequency (IF)." },
  { "en": "Apa Itu Filter Mekanis?", "id": "Filter Menggunakan Resonator Mekanis." },
  { "en": "Apa Itu Filter Surface Acoustic Wave (SAW)?", "id": "Filter Untuk Frekuensi Radio (RF)." },
  { "en": "Apa Itu Osilator Kuadratur?", "id": "Osilator Menghasilkan Dua Sinyal Sinusoidal." },
  { "en": "Berapa Perbedaan Fasa Output Osilator Kuadratur?", "id": "Sembilan Puluh Derajat (90°)." },
  { "en": "Apa Kegunaan Osilator Kuadratur?", "id": "Modulator/Demodulator I/Q Dalam Komunikasi." },
  { "en": "Apa Itu Direct Digital Synthesis (DDS)?", "id": "Metode Menghasilkan Sinyal Analog Secara Digital." },
  { "en": "Apa Komponen Utama DDS?", "id": "Phase Accumulator, Look-Up Table (LUT), DAC." },
  { "en": "Apa Keuntungan Direct Digital Synthesis (DDS)?", "id": "Resolusi Frekuensi Sangat Halus." },
  { "en": "Apa Itu Osilator Dioda Gunn?", "id": "Osilator Solid-State Untuk Frekuensi Microwave." },
  { "en": "Apa Itu Efek Gunn?", "id": "Properti Beberapa Semikonduktor Menghasilkan Resistansi Negatif." },
  { "en": "Apa Itu Osilator Dioda Terobosan (Tunnel Diode)?", "id": "Osilator Lain Berbasis Resistansi Negatif." },
  { "en": "Apa Itu Osilator Unijunction Transistor (UJT)?", "id": "Jenis Osilator Relaksasi Sederhana." },
  { "en": "Apa Itu Synthesizer Frekuensi?", "id": "Rangkaian Menghasilkan Berbagai Frekuensi Dari Referensi." },
  { "en": "Apa Peran Phase-Locked Loop (PLL) Dalam Synthesizer?", "id": "Mengalikan Frekuensi Referensi Stabil." },
  { "en": "Apa Itu Osilator Pierce?", "id": "Jenis Osilator Kristal Populer Untuk Mikrokontroler." },
  { "en": "Apa Itu Osilator Opto-Elektronik?", "id": "Osilator Menggunakan Komponen Optik Dan Elektronik." },
  { "en": "Apa Itu Derau Putih (White Noise)?", "id": "Sinyal Acak Dengan Kerapatan Spektral Datar." },
  { "en": "Apa Itu Derau Merah Jambu (Pink Noise)?", "id": "Derau Dengan Daya Sama Per Oktaf." },
  { "en": "Apa Itu Konverter Flyback?", "id": "Topologi Catu Daya Switching Terisolasi." },
  { "en": "Apa Ciri Khas Konverter Flyback?", "id": "Menggunakan Trafo Yang Juga Bertindak Induktor." },
  { "en": "Apa Itu Konverter Forward?", "id": "Topologi Switching Lainnya Yang Terisolasi." },
  { "en": "Apa Perbedaan Utama Flyback Dan Forward?", "id": "Forward Mentransfer Energi Saat Saklar 'On'." },
  { "en": "Apa Itu Konverter Cuk?", "id": "Topologi Switching Dapat Menaikkan/Menurunkan Tegangan." },
  { "en": "Apa Fungsi Rangkaian Power Factor Correction (PFC)?", "id": "Membuat Beban Catu Daya Terlihat Resitif." },
  { "en": "Mengapa Power Factor Correction (PFC) Penting?", "id": "Mengurangi Distorsi Harmonik Pada Jaringan Listrik." },
  { "en": "Apa Itu Inverter?", "id": "Rangkaian Yang Mengubah Daya DC Menjadi AC." },
  { "en": "Di Mana Inverter Digunakan?", "id": "Uninterruptible Power Supply (UPS) Dan Sel Surya." },
  { "en": "Apa Itu Uninterruptible Power Supply (UPS)?", "id": "Menyediakan Daya Darurat Saat Listrik Padam." },
  { "en": "Apa Jenis Utama UPS?", "id": "Standby (Offline), Line-Interactive, Dan Online." },
  { "en": "Apa Itu Catu Daya Tanpa Transformator?", "id": "Catu Daya Menggunakan Kapasitor Penurun Tegangan." },
  { "en": "Apa Bahaya Catu Daya Tanpa Transformator?", "id": "Tidak Ada Isolasi Dari Jaringan Listrik." },
  { "en": "Apa Itu Soft Start Dalam Catu Daya?", "id": "Fitur Menaikkan Tegangan Output Secara Perlahan." },
  { "en": "Apa Tujuan Soft Start?", "id": "Membatasi Arus Inrush Saat Dinyalakan." },
  { "en": "Apa Itu Snubber Circuit?", "id": "Rangkaian Melindungi Saklar Dari Lonjakan Tegangan." },
  { "en": "Komponen Apa Yang Digunakan Dalam Snubber?", "id": "Biasanya Resistor Dan Kapasitor (RC)." },
  { "en": "Apa Itu Tegangan Mode-Common (Common-Mode Voltage)?", "id": "Komponen Tegangan Yang Sama Pada Dua Jalur." },
  { "en": "Apa Itu Filter Mode-Common (Common-Mode Choke)?", "id": "Induktor Untuk Meredam Derau Mode-Common." },
  { "en": "Apa Itu Standar Efisiensi 80 Plus?", "id": "Sertifikasi Efisiensi Untuk Catu Daya Komputer." },
  { "en": "Apa Itu Power over Ethernet (PoE)?", "id": "Mengirimkan Daya Listrik Melalui Kabel Ethernet." },
  { "en": "Apa Itu Hot Swap?", "id": "Kemampuan Mencabut Memasang Perangkat Tanpa Mematikan." },
  { "en": "Apa Itu Kapasitor X?", "id": "Kapasitor Keamanan Dipasang Antara Jalur AC." },
  { "en": "Apa Itu Kapasitor Y?", "id": "Kapasitor Keamanan Dipasang Dari Jalur AC ke Ground." },
  { "en": "Apa Itu Penguat Frekuensi Radio (RF Amplifier)?", "id": "Penguat Didesain Untuk Bekerja Frekuensi Tinggi." },
  { "en": "Apa Fungsi Low Noise Amplifier (LNA)?", "id": "Menguatkan Sinyal Sangat Lemah Tanpa Derau." },
  { "en": "Di Mana Posisi LNA Dalam Rantai Penerima?", "id": "Biasanya Terletak Paling Depan Setelah Antena." },
  { "en": "Apa Fungsi Power Amplifier (PA)?", "id": "Menguatkan Sinyal Untuk Ditransmisikan Oleh Antena." },
  { "en": "Di Mana Posisi PA Dalam Rantai Pemancar?", "id": "Biasanya Terletak Paling Akhir Sebelum Antena." },
  { "en": "Apa Itu Titik Kompresi 1-dB (P1dB)?", "id": "Ukuran Batas Linearitas Output Sebuah Penguat." },
  { "en": "Apa Itu Jaringan Pencocokan Impedansi?", "id": "Rangkaian Memastikan Transfer Daya Maksimum." },
  { "en": "Sebutkan Dua Jenis Jaringan Pencocokan Sederhana?", "id": "Jaringan L (L-Network) Dan Pi (Pi-Network)." },
  { "en": "Apa Itu Penguat Cascode?", "id": "Kombinasi Konfigurasi Common-Emitter Dan Common-Base." },
  { "en": "Apa Keuntungan Utama Dari Penguat Cascode?", "id": "Bandwidth Sangat Lebar Dan Isolasi Tinggi." },
  { "en": "Apa Itu Parameter-S (Scattering Parameters)?", "id": "Mendeskripsikan Perilaku Jaringan RF." },
  { "en": "Apa Arti S21 Dalam Parameter-S?", "id": "Penguatan Daya Maju (Forward Power Gain)." },
  { "en": "Apa Arti S11 Dalam Parameter-S?", "id": "Koefisien Refleksi Input (Input Return Loss)." },
  { "en": "Apa Itu Kriteria Stabilitas Rollet (K-Factor)?", "id": "Ukuran Stabilitas Tanpa Syarat Penguat RF." },
  { "en": "Kapan Penguat Dikatakan Stabil Tanpa Syarat?", "id": "Ketika Nilai K Lebih Besar Dari Satu." },
  { "en": "Apa Itu Penguat Umpan Balik Arus (Current Feedback)?", "id": "Jenis Op-Amp Dengan Bandwidth Sangat Lebar." },
  { "en": "Apa Itu Rangkaian Bias Aktif?", "id": "Menggunakan Transistor Lain Untuk Menstabilkan Titik Q." },
  { "en": "Apa Itu Penguat Darlington?", "id": "Konfigurasi Dua Transistor Untuk Penguatan Arus Tinggi." },
  { "en": "Apa Kelemahan Pasangan Darlington?", "id": "Tegangan Saturasi Tinggi Dan Kecepatan Rendah." },
  { "en": "Apa Itu Cermin Arus (Current Mirror)?", "id": "Rangkaian Menyalin Arus Dari Satu Jalur." },
  { "en": "Apa Itu Beban Aktif (Active Load)?", "id": "Menggantikan Resistor Beban Dengan Cermin Arus." },
  { "en": "Apa Keuntungan Menggunakan Beban Aktif?", "id": "Mendapatkan Penguatan Tegangan Yang Sangat Tinggi." },
  { "en": "Apa Itu Penguat Operasional Konduktansi (OTA)?", "id": "Penguat Yang Outputnya Berupa Sumber Arus." },
  { "en": "Apa Itu Bootstrapping Dalam Rangkaian Penguat?", "id": "Teknik Umpan Balik Positif Menaikkan Impedansi." },
  { "en": "Apa Itu Distorsi Amplitudo?", "id": "Perubahan Bentuk Gelombang Akibat Kliping." },
  { "en": "Apa Itu Filter Stub Saluran Transmisi?", "id": "Filter Frekuensi Tinggi Menggunakan Potongan Saluran." },
  { "en": "Apa Itu Filter Rongga (Cavity Filter)?", "id": "Filter Sangat Selektif Menggunakan Resonator Rongga Logam." },
  { "en": "Di Mana Filter Rongga Umum Digunakan?", "id": "Base Station Seluler Dan Pemancar Radio." },
  { "en": "Apa Itu Filter Heliks?", "id": "Jenis Filter Rongga Menggunakan Elemen Heliks." },
  { "en": "Apa Itu Filter Interdigital?", "id": "Filter Microwave Dengan Batang-Batang Resonansi Paralel." },
  { "en": "Apa Itu Matched Filter?", "id": "Filter Optimal Untuk Mendeteksi Sinyal Dikenal." },
  { "en": "Apa Kegunaan Matched Filter?", "id": "Memaksimalkan Rasio Sinyal-ke-Derau (SNR)." },
  { "en": "Di Mana Matched Filter Digunakan?", "id": "Sistem Radar, Sonar, Dan Komunikasi Digital." },
  { "en": "Apa Itu Filter Kosinus Naik (Raised-Cosine Filter)?", "id": "Filter Untuk Mengurangi Interferensi Antar Simbol." },
  { "en": "Apa Itu Koefisien Roll-Off?", "id": "Parameter Yang Menentukan Lebar Pita Berlebih." },
  { "en": "Apa Itu Jaringan Penunda (Delay Line)?", "id": "Komponen Yang Menunda Sinyal Sejumlah Waktu." },
  { "en": "Apa Itu Filter transversal?", "id": "Struktur Filter Menggunakan Jaringan Penunda." },
  { "en": "Apa Itu Penyetelan (Tuning) Filter?", "id": "Mengubah Frekuensi Tengah Atau Cutoff Filter." },
  { "en": "Bagaimana Cara Menyetel Filter Secara Elektronik?", "id": "Menggunakan Dioda Varaktor Atau Kapasitor Digital." },
  { "en": "Apa Itu Dioda Varaktor?", "id": "Dioda Yang Kapasitansinya Berubah Dengan Tegangan." },
  { "en": "Apa Itu Filter YIG (Yttrium Iron Garnet)?", "id": "Filter Microwave Yang Dapat Disetel Medan Magnet." },
  { "en": "Apa Itu Phase-Linear Filter?", "id": "Filter Yang Memiliki Penundaan Grup Konstan." },
  { "en": "Apa Keuntungan Dari Phase-Linear Filter?", "id": "Tidak Menyebabkan Distorsi Fasa Pada Sinyal." },
  { "en": "Filter Apa Yang Paling Mendekati Phase-Linear?", "id": "Filter Bessel Adalah Contohnya." },
  { "en": "Apa Itu Distorsi Fasa?", "id": "Perubahan Fasa Tidak Seragam Terhadap Frekuensi." },
  { "en": "Apa Itu Osilator YIG (Yttrium Iron Garnet)?", "id": "Osilator Microwave Yang Dapat Disetel Secara Luas." },
  { "en": "Apa Keuntungan Osilator YIG?", "id": "Rentang Penyetelan Lebar Dan Derau Fasa Rendah." },
  { "en": "Apa Itu Dielectric Resonator Oscillator (DRO)?", "id": "Osilator Microwave Menggunakan Resonator Keramik." },
  { "en": "Apa Keuntungan Dielectric Resonator Oscillator (DRO)?", "id": "Ukuran Kecil Dan Stabilitas Suhu Baik." },
  { "en": "Apa Itu Injection Locking?", "id": "Proses Sinkronisasi Osilator Dengan Sinyal Eksternal." },
  { "en": "Apa Syarat Terjadinya Injection Locking?", "id": "Frekuensi Sinyal Eksternal Harus Dekat." },
  { "en": "Apa Itu Pengali Frekuensi (Frequency Multiplier)?", "id": "Rangkaian Menghasilkan Frekuensi Output Kelipatan Input." },
  { "en": "Bagaimana Cara Kerja Pengali Frekuensi?", "id": "Menggunakan Komponen Non-Linear Untuk Menghasilkan Harmonik." },
  { "en": "Apa Itu Pembagi Frekuensi (Frequency Divider/Prescaler)?", "id": "Rangkaian Menghasilkan Frekuensi Output Bagian Input." },
  { "en": "Rangkaian Digital Apa Yang Digunakan Sebagai Pembagi?", "id": "Rangkaian Counter Atau Flip-Flop." },
  { "en": "Apa Itu Osilator Terkontrol Suhu (TCXO)?", "id": "Osilator Kristal Dengan Kompensasi Suhu." },
  { "en": "Apa Itu Osilator Terkontrol Oven (OCXO)?", "id": "Osilator Kristal Di Dalam Oven Suhu Konstan." },
  { "en": "Osilator Mana Yang Paling Stabil?", "id": "Osilator Terkontrol Oven (OCXO)." },
  { "en": "Apa Itu Jam Atom?", "id": "Standar Frekuensi Berbasis Transisi Atom." },
  { "en": "Apa Itu Barkhausen Noise?", "id": "Derau Yang Dihasilkan Perubahan Domain Magnetik." },
  { "en": "Apa Itu Osilator Pemblokiran (Blocking Oscillator)?", "id": "Osilator Menghasilkan Pulsa Sempit." },
  { "en": "Apa Itu Sirkuit Tangki (Tank Circuit)?", "id": "Jaringan Resonansi LC Paralel." },
  { "en": "Apa Fungsi Sirkuit Tangki Dalam Osilator?", "id": "Menyimpan Energi Dan Menentukan Frekuensi Osilasi." },
  { "en": "Apa Itu Pulling Frekuensi Osilator?", "id": "Pergeseran Frekuensi Akibat Perubahan Beban." },
  { "en": "Apa Itu Pushing Frekuensi Osilator?", "id": "Pergeseran Frekuensi Akibat Perubahan Catu Daya." },
  { "en": "Apa Metode Pengisian Baterai CC-CV?", "id": "Constant Current Lalu Constant Voltage." },
  { "en": "Tahap Apa Yang Mengisi Sebagian Besar Kapasitas?", "id": "Tahap Arus Konstan (Constant Current)." },
  { "en": "Apa Fungsi Tahap Tegangan Konstan (CV)?", "id": "Mengisi Sisa Kapasitas Baterai Secara Perlahan." },
  { "en": "Apa Itu Battery Management System (BMS)?", "id": "Sistem Elektronik Yang Mengelola Baterai." },
  { "en": "Apa Fungsi Utama Dari BMS?", "id": "Melindungi Baterai Dari Kondisi Berbahaya." },
  { "en": "Kondisi Apa Yang Dipantau BMS?", "id": "Tegangan, Arus, Suhu, Dan Keadaan Pengisian." },
  { "en": "Apa Itu Cell Balancing?", "id": "Fitur BMS Menyamakan Tegangan Setiap Sel." },
  { "en": "Apa Itu Pompa Muatan (Charge Pump)?", "id": "Konverter Tegangan DC-DC Menggunakan Kapasitor." },
  { "en": "Apa Keuntungan Pompa Muatan?", "id": "Tidak Membutuhkan Induktor Sehingga Ukurannya Kecil." },
  { "en": "Apa Kelemahan Pompa Muatan?", "id": "Kemampuan Arus Terbatas Dan Riak Tinggi." },
  { "en": "Apa Itu Electromagnetic Interference (EMI)?", "id": "Gangguan Elektromagnetik Yang Tidak Diinginkan." },
  { "en": "Apa Sumber Utama EMI Catu Daya Switching?", "id": "Proses Pensaklaran Cepat Arus Dan Tegangan." },
  { "en": "Apa Itu Filter EMI?", "id": "Filter Untuk Mencegah EMI Masuk Keluar." },
  { "en": "Komponen Apa Yang Ada Dalam Filter EMI?", "id": "Induktor (Choke), Kapasitor X, Dan Kapasitor Y." },
  { "en": "Apa Itu Konduksi EMI?", "id": "EMI Yang Merambat Melalui Kabel." },
  { "en": "Apa Itu Radiasi EMI?", "id": "EMI Yang Merambat Melalui Udara." },
  { "en": "Apa Itu Ferrite Bead?", "id": "Komponen Pasif Untuk Menekan Derau Frekuensi Tinggi." },
  { "en": "Apa Itu Ground Loop?", "id": "Kondisi Di Mana Ada Beberapa Jalur Ground." },
  { "en": "Apa Masalah Yang Disebabkan Ground Loop?", "id": "Dapat Menimbulkan Dengung (Hum) Dan Derau." },
  { "en": "Apa Itu Isolasi Galvanik?", "id": "Memisahkan Dua Bagian Rangkaian Secara Elektrik." },
  { "en": "Bagaimana Cara Mencapai Isolasi Galvanik?", "id": "Menggunakan Transformator Atau Optocoupler." },
  { "en": "Apa Itu Optocoupler?", "id": "Komponen Mengirim Sinyal Menggunakan Cahaya." },
  { "en": "Apa Itu Konverter Resonansi?", "id": "Topologi Switching Mengurangi Rugi-Rugi Pensaklaran." },
  { "en": "Apa Itu Penguat Diferensial Sepenuhnya (Fully Differential)?", "id": "Penguat Dengan Input Dan Output Diferensial." },
  { "en": "Apa Keuntungan Penguat Diferensial Sepenuhnya?", "id": "Penolakan Derau Mode-Common Yang Sangat Baik." },
  { "en": "Apa Itu Tegangan Offset Input Op-Amp?", "id": "Tegangan DC Kecil Untuk Output Nol." },
  { "en": "Bagaimana Cara Mengurangi Efek Tegangan Offset?", "id": "Menggunakan Pin Offset Null Atau Rangkaian Eksternal." },
  { "en": "Apa Itu Arus Bias Input Op-Amp?", "id": "Arus DC Kecil Mengalir Ke Terminal Input." },
  { "en": "Apa Itu Arus Offset Input Op-Amp?", "id": "Selisih Antara Dua Arus Bias Input." },
  { "en": "Op-Amp Tipe Apa Dengan Arus Bias Rendah?", "id": "Op-Amp Dengan Input JFET Atau CMOS." },
  { "en": "Apa Itu Rangkaian Komparator?", "id": "Membandingkan Dua Tegangan Dan Menghasilkan Output Digital." },
  { "en": "Apa Perbedaan Op-Amp Dan Komparator Khusus?", "id": "Komparator Didesain Untuk Beralih Lebih Cepat." },
  { "en": "Apa Itu Komparator Dengan Histeresis?", "id": "Menggunakan Umpan Balik Positif Untuk Stabilitas." },
  { "en": "Apa Nama Lain Komparator Dengan Histeresis?", "id": "Pemicu Schmitt (Schmitt Trigger)." },
  { "en": "Apa Fungsi Pemicu Schmitt?", "id": "Mengubah Sinyal Lambat Menjadi Tepi Tajam." },
  { "en": "Apa Itu Penguat Jembatan (Bridge Amplifier)?", "id": "Menguatkan Sinyal Kecil Dari Sensor Jembatan." },
  { "en": "Apa Contoh Sensor Jembatan?", "id": "Strain Gauge Dalam Jembatan Wheatstone." },
  { "en": "Apa Itu Penguat Audio?", "id": "Penguat Didesain Khusus Untuk Spektrum Frekuensi Audio." },
  { "en": "Apa Itu Penguat Headphone?", "id": "Penguat Daya Rendah Untuk Mendorong Headphone." },
  { "en": "Apa Itu Preamplifier (Preamp)?", "id": "Menguatkan Sinyal Sangat Lemah Sebelum Penguat Utama." },
  { "en": "Apa Itu Kontrol Nada (Tone Control)?", "id": "Filter Mengatur Bass Dan Treble." },
  { "en": "Apa Itu Rangkaian Baxandall Tone Control?", "id": "Desain Populer Untuk Kontrol Bass Treble." },
  { "en": "Apa Itu Loudness Control?", "id": "Meningkatkan Bass Treble Pada Volume Rendah." },
  { "en": "Apa Itu Penguat Doherty?", "id": "Teknik Efisiensi Tinggi Untuk Power Amplifier RF." },
  { "en": "Apa Itu Linearitas Penguat?", "id": "Kemampuan Menguatkan Sinyal Tanpa Menambah Distorsi." },
  { "en": "Apa Itu Band Gap Reference?", "id": "Sirkuit Referensi Tegangan Sangat Stabil Suhu." },
  { "en": "Apa Itu Ground Semu (Virtual Ground)?", "id": "Simpul Dengan Tegangan Stabil Nol Volt." },
  { "en": "Di Mana Titik Ground Semu Penguat Inverting?", "id": "Pada Terminal Input Inverting Op-Amp." },
  { "en": "Apa Itu Filter Eliptik (Cauer)?", "id": "Filter Dengan Riak Di Passband Stopband." },
  { "en": "Apa Keuntungan Utama Filter Eliptik?", "id": "Transisi Paling Tajam Antara Passband Stopband." },
  { "en": "Apa Kekurangan Filter Eliptik?", "id": "Distorsi Fasa Yang Sangat Buruk." },
  { "en": "Apa Itu Jaringan Tunda Semua Lolos (All-Pass Delay)?", "id": "Filter Untuk Menyamakan Penundaan Grup." },
  { "en": "Apa Itu Duplekser (Duplexer)?", "id": "Filter Memungkinkan Antena Dipakai Bersama." },
  { "en": "Di Mana Duplekser Digunakan?", "id": "Radio Dua Arah (Transceiver) Seperti Ponsel." },
  { "en": "Apa Itu Diplexer?", "id": "Filter Menggabungkan Memisahkan Sinyal Frekuensi Berbeda." },
  { "en": "Apa Itu Triplexer?", "id": "Filter Menggabungkan Memisahkan Tiga Pita Frekuensi." },
  { "en": "Apa Itu Respon Fasa Minimum?", "id": "Sistem Stabil Dengan Invers Yang Stabil." },
  { "en": "Apa Itu Respon Fasa Non-Minimum?", "id": "Sistem Dengan Zero Di Bidang Kanan." },
  { "en": "Apa Itu Filter Digital?", "id": "Filter Diimplementasikan Menggunakan Prosesor Sinyal Digital." },
  { "en": "Apa Itu Analog-to-Digital Converter (ADC)?", "id": "Mengubah Sinyal Analog Menjadi Data Digital." },
  { "en": "Apa Itu Digital-to-Analog Converter (DAC)?", "id": "Mengubah Data Digital Menjadi Sinyal Analog." },
  { "en": "Apa Itu Proses Oversampling?", "id": "Sampling Pada Frekuensi Jauh Di Atas Nyquist." },
  { "en": "Apa Manfaat Oversampling Dalam Konverter Data?", "id": "Meningkatkan Resolusi Dan Menyederhanakan Filter Analog." },
  { "en": "Apa Itu Filter Dekimasi (Decimation)?", "id": "Mengurangi Laju Sampling Sinyal Digital." },
  { "en": "Apa Itu Filter Interpolasi?", "id": "Meningkatkan Laju Sampling Sinyal Digital." },
  { "en": "Apa Itu Catu Daya Terprogram (Programmable PSU)?", "id": "Catu Daya Yang Outputnya Dapat Diatur Komputer." },
  { "en": "Apa Itu Electronic Load?", "id": "Perangkat Mensimulasikan Beban Untuk Menguji Catu Daya." },
  { "en": "Apa Itu Osilator Terkunci Fasa (Phase-Locked Oscillator)?", "id": "Osilator Yang Disinkronkan Ke Sinyal Referensi." },
  { "en": "Apa Itu Osilator Backward Wave (BWO)?", "id": "Sumber Gelombang Mikro Berbasis Tabung Vakum." },
  { "en": "Apa Itu Klystron?", "id": "Tabung Vakum Penguat Daya Frekuensi Sangat Tinggi." },
  { "en": "Apa Itu Magnetron?", "id": "Tabung Vakum Osilator Daya Tinggi." },
  { "en": "Di Mana Magnetron Digunakan?", "id": "Oven Microwave Dan Sistem Radar." },
  { "en": "Apa Itu Traveling-Wave Tube (TWT)?", "id": "Tabung Vakum Penguat Broadband Untuk Microwave." },
  { "en": "Di Mana TWT Digunakan?", "id": "Satelit Komunikasi Dan Peperangan Elektronik." },
  { "en": "Apa Itu Jitter Siklus-ke-Siklus?", "id": "Perubahan Periode Antara Dua Siklus Berurutan." },
  { "en": "Apa Itu Akurasi Frekuensi Osilator?", "id": "Seberapa Dekat Frekuensi Dengan Nilai Nominalnya." },
  { "en": "Apa Satuan Akurasi Frekuensi?", "id": "Parts Per Million (PPM)." },
  { "en": "Apa Itu Penuaan (Aging) Kristal Osilator?", "id": "Pergeseran Frekuensi Lambat Seiring Waktu." },
  { "en": "Apa Itu Histeresis Suhu Kristal?", "id": "Frekuensi Tidak Kembali Sama Persis." },
  { "en": "Apa Itu Overtone Crystal Oscillator?", "id": "Osilator Bekerja Pada Harmonik Ganjil Kristal." },
  { "en": "Apa Itu Generator Sinyal Arbitrer (AWG)?", "id": "Instrumen Menghasilkan Bentuk Gelombang Kustom." },
  { "en": "Apa Itu Penganalisis Spektrum (Spectrum Analyzer)?", "id": "Instrumen Mengukur Sinyal Domain Frekuensi." },
  { "en": "Apa Itu Penganalisis Jaringan Vektor (VNA)?", "id": "Instrumen Mengukur Parameter-S Jaringan RF." },
  { "en": "Apa Itu Osiloskop?", "id": "Instrumen Mengukur Sinyal Domain Waktu." },
  { "en": "Apa Itu Pelacakan Catu Daya (Power Supply Tracking)?", "id": "Dua Output Catu Daya Saling Mengikuti." },
  { "en": "Apa Itu Penginderaan Jauh (Remote Sensing) Catu Daya?", "id": "Mengompensasi Jatuh Tegangan Pada Kabel Beban." },
  { "en": "Berapa Terminal Untuk Penginderaan Jauh?", "id": "Empat Terminal (Dua Daya, Dua Indra)." },
  { "en": "Apa Itu Linear Power Supply Unit (PSU)?", "id": "Catu Daya Yang Menggunakan Regulator Linear." },
  { "en": "Apa Itu Switched-Mode Power Supply (SMPS)?", "id": "Catu Daya Yang Menggunakan Regulator Switching." },
  { "en": "Mana Yang Lebih Bising, Linear Atau SMPS?", "id": "SMPS Cenderung Menghasilkan Lebih Banyak Derau." },
  { "en": "Mana Yang Lebih Efisien, Linear Atau SMPS?", "id": "SMPS Jauh Lebih Efisien Dibanding Linear." },
  { "en": "Apa Itu Dioda Schottky?", "id": "Dioda Dengan Jatuh Tegangan Maju Rendah." },
  { "en": "Di Mana Dioda Schottky Digunakan Catu Daya?", "id": "Sebagai Penyearah Dalam SMPS Efisiensi Tinggi." },
  { "en": "Apa Itu Dioda Pemulihan Cepat (Fast Recovery)?", "id": "Dioda Dengan Waktu Pemulihan Balik Singkat." },
  { "en": "Apa Itu Transient Response Catu Daya?", "id": "Respon Output Terhadap Perubahan Beban Tiba-Tiba." },
  { "en": "Apa Itu Power Sequencing?", "id": "Menyalakan Beberapa Jalur Tegangan Urutan Tertentu." },
  { "en": "Mengapa Power Sequencing Penting?", "id": "Untuk Mencegah Kerusakan Pada Sirkuit Kompleks." },
  { "en": "Apa Itu Dioda Freewheeling (Flyback)?", "id": "Dioda Melindungi Saklar Dari Lonjakan Induktif." },
  { "en": "Apa Itu Konverter SEPIC?", "id": "Topologi Switching Dapat Menaikkan/Menurunkan Tegangan." },
  { "en": "Apa Itu Konverter Zeta?", "id": "Topologi Switching Mirip Dengan Konverter SEPIC." },
  { "en": "Apa Itu Zero Crossing Detector?", "id": "Rangkaian Mendeteksi Saat Sinyal AC Melewati Nol." },
  { "en": "Apa Itu Rectifier Terkendali Silikon (SCR)?", "id": "Jenis Thyristor Untuk Kontrol Daya AC." },
  { "en": "Apa Itu TRIAC?", "id": "Saklar Semikonduktor Dua Arah Untuk Daya AC." },
  { "en": "Apa Fungsi Rangkaian Dimmer Lampu?", "id": "Mengontrol Kecerahan Lampu Dengan Memotong Fasa." },
  { "en": "Apa Itu Zero Voltage Switching (ZVS)?", "id": "Teknik Mengurangi Rugi-Rugi Pada SMPS." },
  { "en": "Apa Itu Zero Current Switching (ZCS)?", "id": "Teknik Lain Mengurangi Rugi-Rugi Pada SMPS." },
  { "en": "Apa Itu Varistor Oksida Logam (MOV)?", "id": "Komponen Melindungi Dari Lonjakan Tegangan Transien." },
  { "en": "Apa Itu Tabung Pelepasan Gas (GDT)?", "id": "Komponen Perlindungan Lonjakan Tegangan Tinggi." },
  { "en": "Apa Itu Isolator Digital?", "id": "Mengisolasi Sinyal Digital Menggunakan Kapasitansi Magnet." },
  { "en": "Apa Itu Kelas Keamanan Catu Daya?", "id": "Menunjukkan Tingkat Perlindungan Terhadap Sengatan Listrik." },
  { "en": "Apa Itu Penguat Gain Terprogram (PGA)?", "id": "Penguat Yang Gain-nya Dapat Diatur Digital." },
  { "en": "Apa Itu Penguat Stabil Chopper (Chopper-Stabilized Amplifier)?", "id": "Penguat Dengan Offset Dan Drift Sangat Rendah." },
  { "en": "Bagaimana Cara Kerja Penguat Stabil Chopper?", "id": "Menggunakan Modulasi Demodulasi Untuk Menghilangkan Offset." },
  { "en": "Apa Itu Penguat Video?", "id": "Penguat Broadband Didesain Untuk Sinyal Video." },
  { "en": "Apa Karakteristik Penting Dari Penguat Video?", "id": "Respon Fasa Linear Dan Bandwidth Lebar." },
  { "en": "Apa Itu Penguat Lock-In (Lock-In Amplifier)?", "id": "Mengukur Sinyal AC Sangat Kecil." },
  { "en": "Bagaimana Penguat Lock-In Mengekstrak Sinyal?", "id": "Menggunakan Demodulasi Sensitif Fasa (PSD)." },
  { "en": "Apa Itu Sel Gilbert (Gilbert Cell)?", "id": "Inti Rangkaian Pencampur (Mixer) Analog." },
  { "en": "Apa Fungsi Pencampur (Mixer) Dalam Radio?", "id": "Menggeser Sinyal Dari Satu Frekuensi Lain." },
  { "en": "Apa Itu Frekuensi Gambar (Image Frequency)?", "id": "Input Palsu Yang Tidak Diinginkan." },
  { "en": "Filter Apa Yang Digunakan Menolak Frekuensi Gambar?", "id": "Filter Preselektor RF Sebelum Mixer." },
  { "en": "Apa Itu Beban Fantom (Dummy Load)?", "id": "Beban Resitif Untuk Menguji Pemancar." },
  { "en": "Apa Itu Attenuator?", "id": "Rangkaian Pasif Untuk Melemahkan Sinyal." },
  { "en": "Apa Satuan Atenuasi?", "id": "Desibel (dB)." },
  { "en": "Apa Itu Penguat Hibrida?", "id": "Modul Penguat Menggabungkan Beberapa Teknologi Chip." },
  { "en": "Apa Itu Feedforward?", "id": "Teknik Untuk Meningkatkan Linearitas Penguat." },
  { "en": "Apa Itu Pre-Distorsi?", "id": "Teknik Lain Untuk Meningkatkan Linearitas Penguat." },
  { "en": "Apa Itu Diagram Mata (Eye Diagram)?", "id": "Metode Menganalisis Kualitas Sinyal Komunikasi Digital." },
  { "en": "Apa Itu Jitter Dalam Diagram Mata?", "id": "Penyempitan Mata Secara Horizontal." },
  { "en": "Apa Itu Derau Dalam Diagram Mata?", "id": "Penebalan Garis Sinyal Secara Vertikal." },
  { "en": "Apa Itu Penguat Diferensial Cascade?", "id": "Meningkatkan Impedansi Output Dan Penguatan." },
  { "en": "Apa Itu Wilson Current Mirror?", "id": "Peningkatan Dari Cermin Arus Dasar." },
  { "en": "Apa Keuntungan Wilson Current Mirror?", "id": "Impedansi Output Tinggi Dan Error Arus Rendah." },
  { "en": "Apa Itu Widlar Current Source?", "id": "Memodifikasi Cermin Arus Untuk Arus Kecil." },
  { "en": "Apa Itu Active Antenna?", "id": "Antena Yang Mengintegrasikan Penguat." },
  { "en": "Apa Fungsi Phase-Locked Loop (PLL) Sebagai Filter?", "id": "Sebagai Filter Lolos Pita Pelacak (Tracking)." },
  { "en": "Apa Itu Filter Kalman?", "id": "Algoritma Optimal Untuk Estimasi Keadaan." },
  { "en": "Apa Kegunaan Filter Kalman Di Elektronik?", "id": "Pelacakan Sinyal Dan Fusi Sensor." },
  { "en": "Apa Itu Plot Bode?", "id": "Grafik Respon Frekuensi (Magnitudo Dan Fasa)." },
  { "en": "Apa Itu Gain Margin?", "id": "Ukuran Stabilitas Penguatan Sistem Umpan Balik." },
  { "en": "Apa Itu Phase Margin?", "id": "Ukuran Stabilitas Fasa Sistem Umpan Balik." },
  { "en": "Apa Itu Plot Nyquist?", "id": "Plot Respon Frekuensi Dalam Koordinat Polar." },
  { "en": "Apa Itu Kriteria Stabilitas Nyquist?", "id": "Menentukan Stabilitas Dari Plot Loop Terbuka." },
  { "en": "Apa Itu Respon Transien?", "id": "Perilaku Sistem Saat Merespon Perubahan." },
  { "en": "Apa Itu Overshoot?", "id": "Respon Melebihi Nilai Akhir Sebelum Stabil." },
  { "en": "Apa Itu Waktu Naik (Rise Time)?", "id": "Waktu Respon Naik Dari 10% Hingga 90%." },
  { "en": "Apa Itu Waktu Tunak (Settling Time)?", "id": "Waktu Respon Masuk Dalam Batas Toleransi." },
  { "en": "Apa Itu Akar Ciri (Pole) Sistem?", "id": "Menentukan Perilaku Respon Transien Sistem." },
  { "en": "Apa Itu Zero Sistem?", "id": "Mempengaruhi Amplitudo Respon Sistem." },
  { "en": "Apa Itu Dekade Dalam Plot Bode?", "id": "Perubahan Frekuensi Sepuluh Kali Lipat." },
  { "en": "Apa Itu Oktaf Dalam Plot Bode?", "id": "Perubahan Frekuensi Dua Kali Lipat." },
  { "en": "Apa Itu Integrator Miller?", "id": "Integrator Op-Amp Dengan Kapasitor Umpan Balik." },
  { "en": "Apa Itu Rangkaian Clamper?", "id": "Menggeser Level DC Sinyal AC." },
  { "en": "Apa Itu Rangkaian Clipper (Limiter)?", "id": "Memotong Bagian Sinyal Di Atas Level Tertentu." },
  { "en": "Apa Itu Dioda Varicap?", "id": "Nama Lain Untuk Dioda Varaktor." },
  { "en": "Apa Itu Synthesizer Frekuensi Integer-N?", "id": "Menghasilkan Kelipatan Bulat Frekuensi Referensi." },
  { "en": "Apa Itu Synthesizer Frekuensi Fraksional-N?", "id": "Menghasilkan Kelipatan Pecahan Frekuensi Referensi." },
  { "en": "Apa Keuntungan Synthesizer Fraksional-N?", "id": "Resolusi Frekuensi Lebih Halus Dan Cepat." },
  { "en": "Apa Itu Spurious Signals (Spur)?", "id": "Frekuensi Palsu Yang Tidak Diinginkan." },
  { "en": "Apa Itu Prescaler Dua-Modulus?", "id": "Pembagi Frekuensi Dengan Dua Rasio Pembagian." },
  { "en": "Apa Itu Detektor Fasa-Frekuensi (PFD)?", "id": "Membandingkan Fasa Dan Frekuensi Dua Sinyal." },
  { "en": "Apa Itu Pompa Muatan (Charge Pump) Dalam PLL?", "id": "Mengubah Sinyal Error Digital Menjadi Arus Analog." },
  { "en": "Apa Itu Loop Filter Dalam PLL?", "id": "Filter Low-Pass Menentukan Dinamika Loop." },
  { "en": "Apa Itu Cincin Modulator (Ring Modulator)?", "id": "Jenis Mixer Menghasilkan Frekuensi Jumlah Selisih." },
  { "en": "Apa Itu Osilator Terkopel (Coupled Oscillators)?", "id": "Dua Atau Lebih Osilator Saling Mempengaruhi." },
  { "en": "Apa Itu Mode Sinkronisasi (Mode-Locking)?", "id": "Membuat Osilator Berosilasi Dengan Fasa Sama." },
  { "en": "Apa Itu Generator Fungsi?", "id": "Instrumen Menghasilkan Berbagai Bentuk Gelombang Standar." },
  { "en": "Apa Itu Counter Frekuensi?", "id": "Instrumen Untuk Mengukur Frekuensi Sinyal." },
  { "en": "Apa Itu Allan Variance?", "id": "Ukuran Stabilitas Frekuensi Osilator." },
  { "en": "Apa Itu Micro-Electro-Mechanical System (MEMS) Oscillator?", "id": "Osilator Berbasis Resonator Silikon Miniatur." },
  { "en": "Apa Itu Kontrol Tegangan Modus-Arus (Current-Mode)?", "id": "Metode Kontrol SMPS Menggunakan Umpan Balik Arus." },
  { "en": "Apa Itu Kontrol Tegangan Modus-Tegangan (Voltage-Mode)?", "id": "Metode Kontrol SMPS Menggunakan Umpan Balik Tegangan." },
  { "en": "Apa Keuntungan Kontrol Modus-Arus?", "id": "Respon Transien Cepat Dan Proteksi Arus." },
  { "en": "Apa Itu Penguat Kelas E?", "id": "Penguat Switching RF Efisiensi Sangat Tinggi." },
  { "en": "Apa Kondisi Operasi Penguat Kelas E?", "id": "Zero Voltage Switching (ZVS)." },
  { "en": "Apa Itu Konverter DC-DC Terisolasi?", "id": "Konverter Dengan Isolasi Galvanik Antara Input Output." },
  { "en": "Mengapa Isolasi Dalam Catu Daya Penting?", "id": "Untuk Keselamatan Dan Mengurangi Derau." },
  { "en": "Apa Itu Kapasitor Super (Supercapacitor)?", "id": "Komponen Penyimpan Energi Di Antara Baterai Kapasitor." },
  { "en": "Apa Keuntungan Kapasitor Super?", "id": "Pengisian Sangat Cepat Dan Siklus Hidup Panjang." },
  { "en": "Apa Kekurangan Kapasitor Super?", "id": "Kepadatan Energi Lebih Rendah Dari Baterai." },
  { "en": "Apa Itu Maximum Power Point Tracking (MPPT)?", "id": "Mengekstrak Daya Maksimum Dari Sumber Surya." },
  { "en": "Bagaimana Cara Kerja MPPT?", "id": "Secara Aktif Menyesuaikan Impedansi Beban." },
  { "en": "Apa Itu Konverter Matriks?", "id": "Konverter AC-AC Langsung Tanpa Penyimpanan DC." },
  { "en": "Apa Itu Cycloconverter?", "id": "Konverter AC-AC Langsung Mensintesis Frekuensi Rendah." },
  { "en": "Apa Itu Droop Sharing?", "id": "Metode Untuk Paralel Catu Daya." },
  { "en": "Apa Itu Power Management Integrated Circuit (PMIC)?", "id": "IC Mengintegrasikan Beberapa Fungsi Manajemen Daya." },
  { "en": "Apa Itu Standby Power (Vampire Power)?", "id": "Daya Yang Dikonsumsi Saat Perangkat Mati." },
  { "en": "Apa Itu Energy Star?", "id": "Program Internasional Untuk Efisiensi Energi." },
  { "en": "Apa Itu Topologi Totem-Pole PFC?", "id": "Desain PFC Efisiensi Tinggi Menggunakan Transistor Cepat." },
  { "en": "Apa Itu Dioda Ideal?", "id": "Sirkuit Aktif Mensimulasikan Dioda Tanpa Jatuh Tegangan." },
  { "en": "Apa Itu Efek Termoelektrik?", "id": "Konversi Langsung Perbedaan Suhu Menjadi Tegangan." },
  { "en": "Apa Itu Generator Termoelektrik Radioisotop (RTG)?", "id": "Catu Daya Untuk Wahana Antariksa." },
  { "en": "Bagaimana RTG Menghasilkan Listrik?", "id": "Menggunakan Panas Dari Peluruhan Radioaktif." },
  { "en": "Apa Itu Efisiensi Dinding-Steker (Wall-Plug Efficiency)?", "id": "Efisiensi Total Sistem Dari Jaringan Listrik." },
  { "en": "Apa Itu Sinyal Mode-Diferensial (Differential-Mode)?", "id": "Sinyal Dengan Polaritas Berlawanan Antar Dua Jalur." },
  { "en": "Apa Itu Sinyal Mode-Common (Common-Mode)?", "id": "Sinyal Dengan Polaritas Sama Pada Dua Jalur." },
  { "en": "Penguat Apa Yang Dirancang Menolak Sinyal Mode-Common?", "id": "Penguat Diferensial Atau Penguat Instrumentasi." },
  { "en": "Apa Itu Konfigurasi Bridge-Tied Load (BTL)?", "id": "Dua Penguat Mendorong Satu Beban Secara Diferensial." },
  { "en": "Apa Keuntungan Utama Konfigurasi BTL?", "id": "Mendapatkan Daya Output Empat Kali Lipat." },
  { "en": "Apa Itu Penguat Servo DC?", "id": "Rangkaian Umpan Balik Menghilangkan Offset DC Output." },
  { "en": "Apa Itu Neutralization Dalam Penguat RF?", "id": "Membatalkan Kapasitansi Umpan Balik Internal." },
  { "en": "Apa Tujuan Neutralization?", "id": "Mencegah Osilasi Yang Tidak Diinginkan." },
  { "en": "Apa Itu Konverter Impedansi Negatif (NIC)?", "id": "Rangkaian Aktif Menghasilkan Impedansi Bernilai Negatif." },
  { "en": "Apa Itu Penguat Parametrik?", "id": "Penguat Microwave Dengan Derau Sangat Rendah." },
  { "en": "Bagaimana Penguat Parametrik Bekerja?", "id": "Memvariasikan Parameter Reaktif Seperti Kapasitansi." },
  { "en": "Apa Itu Rangkaian Cascode Terlipat (Folded Cascode)?", "id": "Modifikasi Cascode Untuk Rentang Tegangan Lebih Lebar." },
  { "en": "Apa Itu Penguat Terkopel Langsung (DC Coupled)?", "id": "Penguat Yang Dapat Menguatkan Sinyal DC." },
  { "en": "Apa Itu Penguat Terkopel AC (AC Coupled)?", "id": "Menggunakan Kapasitor Untuk Memblokir Komponen DC." },
  { "en": "Apa Itu Efek Early?", "id": "Variasi Lebar Basis Efektif Transistor." },
  { "en": "Apa Akibat Dari Efek Early?", "id": "Membatasi Penguatan Intrinsik Dan Impedansi Output." },
  { "en": "Apa Itu Model Hybrid-Pi?", "id": "Model Sinyal Kecil Untuk Transistor BJT." },
  { "en": "Apa Itu Frekuensi Transisi (fT)?", "id": "Frekuensi Di Mana Penguatan Arus Menjadi Satu." },
  { "en": "Apa Itu Frekuensi Osilasi Maksimum (fmax)?", "id": "Frekuensi Di Mana Penguatan Daya Menjadi Satu." },
  { "en": "Apa Itu Penguat Audio Kelas G?", "id": "Penguat Kelas AB Dengan Jalur Catu Daya Ganda." },
  { "en": "Apa Itu Penguat Audio Kelas H?", "id": "Penguat Kelas AB Dengan Catu Daya Termodulasi." },
  { "en": "Apa Tujuan Kelas G Dan H?", "id": "Meningkatkan Efisiensi Penguat Audio." },
  { "en": "Apa Itu Slew Rate Limiting?", "id": "Distorsi Saat Sinyal Input Berubah Terlalu Cepat." },
  { "en": "Apa Itu Tegangan Saturasi Kolektor-Emitter (VCE(sat))?", "id": "Jatuh Tegangan Minimum Pada Transistor." },
  { "en": "Apa Itu Faktor Crest?", "id": "Rasio Nilai Puncak Sinyal Terhadap RMS." },
  { "en": "Apa Yang Direpresentasikan Pole Dalam Respon Frekuensi?", "id": "Puncak (Peak) Dalam Respon Frekuensi Filter." },
  { "en": "Apa Yang Direpresentasikan Zero Dalam Respon Frekuensi?", "id": "Lembah (Notch) Dalam Respon Frekuensi Filter." },
  { "en": "Di Mana Lokasi Pole Filter Stabil?", "id": "Di Setengah Bidang Kiri S-Plane." },
  { "en": "Apa Itu Ekualisasi Penundaan Grup (Group Delay Equalization)?", "id": "Mengompensasi Variasi Penundaan Grup Filter." },
  { "en": "Filter Apa Yang Digunakan Untuk Ekualisasi Penundaan?", "id": "Filter All-Pass." },
  { "en": "Apa Itu Filter GIC (Generalized Impedance Converter)?", "id": "Topologi Filter Aktif Mensimulasikan Induktor." },
  { "en": "Apa Itu Transformasi Low-Pass Ke Band-Pass?", "id": "Metode Matematis Mendesain Filter Band-Pass." },
  { "en": "Apa Itu Filter Gaussian?", "id": "Filter Tanpa Overshoot Pada Respon Step." },
  { "en": "Apa Kegunaan Filter Gaussian?", "id": "Sistem Komunikasi Digital Dan Pemrosesan Gambar." },
  { "en": "Apa Itu Respon Sinyal Kecil?", "id": "Perilaku Rangkaian Terhadap Sinyal Amplitudo Kecil." },
  { "en": "Apa Itu Respon Sinyal Besar?", "id": "Perilaku Rangkaian Terhadap Sinyal Amplitudo Besar." },
  { "en": "Apa Itu Filter Pasif Tanpa Rugi (Lossless)?", "id": "Filter Ideal Hanya Terbuat Dari L Dan C." },
  { "en": "Apa Itu Constant Resistance Network?", "id": "Jaringan Yang Impedansinya Tidak Bergantung Frekuensi." },
  { "en": "Apa Itu Jaringan Zobel?", "id": "Jenis Constant Resistance Network Untuk Kompensasi." },
  { "en": "Apa Itu Topologi Cauer Untuk Filter Pasif?", "id": "Implementasi Filter Eliptik Dengan Jaringan Ladder." },
  { "en": "Apa Itu Topologi Chebyshev Untuk Filter Pasif?", "id": "Implementasi Filter Chebyshev Dengan Jaringan Ladder." },
  { "en": "Apa Itu Filter Harmonik?", "id": "Filter Untuk Menekan Frekuensi Harmonik." },
  { "en": "Di Mana Filter Harmonik Digunakan?", "id": "Pada Output Pemancar Radio." },
  { "en": "Apa Itu Rangkaian L-Section Filter?", "id": "Filter Terdiri Dari Satu Induktor Seri Kapasitor." },
  { "en": "Apa Itu Rangkaian Pi-Section Filter?", "id": "Filter Terdiri Dari Satu Induktor Dua Kapasitor." },
  { "en": "Bagaimana Prinsip Osilator Resistansi Negatif?", "id": "Membatalkan Rugi-Rugi Rangkaian Tangki LC." },
  { "en": "Bagaimana Cara Membuat Osilator Relaksasi Schmitt Trigger?", "id": "Menggunakan Jaringan Umpan Balik RC." },
  { "en": "Apa Itu Voltage-to-Frequency Converter (VFC)?", "id": "Rangkaian Menghasilkan Frekuensi Proposional Tegangan." },
  { "en": "Apa Itu Frequency-to-Voltage Converter (FVC)?", "id": "Rangkaian Menghasilkan Tegangan Proposional Frekuensi." },
  { "en": "Apa Itu Osilator Butler?", "id": "Jenis Osilator Kristal Overtone." },
  { "en": "Apa Itu Osilator Franklin?", "id": "Jenis Osilator LC Sangat Stabil." },
  { "en": "Apa Itu Osilator Seiler?", "id": "Modifikasi Dari Osilator Colpitts VFO." },
  { "en": "Apa Itu Variable Frequency Oscillator (VFO)?", "id": "Osilator Yang Frekuensinya Dapat Diubah." },
  { "en": "Apa Masalah Utama VFO?", "id": "Stabilitas Frekuensi Yang Kurang Baik." },
  { "en": "Apa Itu Chirp Dalam Sinyal Osilator?", "id": "Perubahan Frekuensi Cepat Selama Transmisi." },
  { "en": "Apa Itu Pemantulan Frekuensi (Frequency Hopping)?", "id": "Mengubah Frekuensi Transmisi Secara Periodik." },
  { "en": "Apa Tujuan Pemantulan Frekuensi?", "id": "Meningkatkan Keamanan Dan Ketahanan Terhadap Gangguan." },
  { "en": "Apa Itu Spread Spectrum?", "id": "Teknik Komunikasi Menyebarkan Sinyal." },
  { "en": "Apa Itu Osilator Gerbang Logika?", "id": "Osilator Sederhana Menggunakan Gerbang Logika." },
  { "en": "Apa Itu Microphonics?", "id": "Efek Getaran Mekanis Mengubah Frekuensi Osilator." },
  { "en": "Apa Itu Jitter Deterministik?", "id": "Jitter Yang Memiliki Penyebab Yang Dapat Diprediksi." },
  { "en": "Apa Itu Jitter Acak (Random Jitter)?", "id": "Jitter Yang Tidak Memiliki Pola Tertentu." },
  { "en": "Apa Itu Diagram Konstelasi?", "id": "Plot Sinyal Modulasi Digital." },
  { "en": "Apa Efek Derau Fasa Pada Konstelasi?", "id": "Menyebabkan Titik Konstelasi Berotasi." },
  { "en": "Apa Itu Konverter Buck Multi-Fasa?", "id": "Menggunakan Beberapa Konverter Buck Secara Paralel." },
  { "en": "Apa Keuntungan Konverter Buck Multi-Fasa?", "id": "Riak Arus Lebih Kecil Dan Respon Cepat." },
  { "en": "Di Mana Konverter Buck Multi-Fasa Digunakan?", "id": "Catu Daya Untuk Mikroprosesor (CPU)." },
  { "en": "Apa Komponen Utama Regulator LDO?", "id": "Pass Transistor, Error Amplifier, Voltage Reference." },
  { "en": "Apa Itu Pass Transistor?", "id": "Transistor Yang Mengatur Aliran Arus Ke Beban." },
  { "en": "Apa Jenis Pass Transistor Di LDO?", "id": "Biasanya Transistor Jenis PMOS Atau PNP." },
  { "en": "Apa Fungsi Error Amplifier Di LDO?", "id": "Membandingkan Tegangan Output Dengan Referensi." },
  { "en": "Apa Itu Equivalent Series Resistance (ESR)?", "id": "Resistansi Parasitik Internal Sebuah Kapasitor." },
  { "en": "Apa Pengaruh ESR Terhadap Riak Tegangan?", "id": "ESR Yang Tinggi Meningkatkan Tegangan Riak." },
  { "en": "Kapasitor Jenis Apa Dengan ESR Rendah?", "id": "Kapasitor Keramik Dan Polimer." },
  { "en": "Apa Itu Equivalent Series Inductance (ESL)?", "id": "Induktansi Parasitik Internal Sebuah Kapasitor." },
  { "en": "Bagaimana Cara Kerja Rangkaian Proteksi Crowbar?", "id": "Memicu SCR Untuk Menghubung Singkat." },
  { "en": "Apa Itu Polyfuse (PTC Resettable Fuse)?", "id": "Sekring Dapat Pulih Sendiri Setelah dingin." },
  { "en": "Apa Itu Proteksi Tegangan Balik?", "id": "Melindungi Rangkaian Dari Polaritas Terbalik." },
  { "en": "Komponen Apa Untuk Proteksi Tegangan Balik?", "id": "Dioda Seri Atau Dioda Shunt." },
  { "en": "Apa Itu Mode Hening (Burst Mode) SMPS?", "id": "Mode Operasi Efisiensi Tinggi Beban Ringan." },
  { "en": "Apa Itu Current Sensing?", "id": "Mengukur Arus Yang Mengalir Dalam Rangkaian." },
  { "en": "Bagaimana Cara Mengukur Arus?", "id": "Menggunakan Resistor Shunt Atau Sensor Efek Hall." },
  { "en": "Apa Itu Topologi Resonansi LLC?", "id": "Konverter DC-DC Efisiensi Tinggi." },
  { "en": "Apa Itu Dioda Sinkron?", "id": "Nama Lain Untuk Penyearah Sinkron." },
  { "en": "Apa Itu Active OR-ing?", "id": "Menggunakan MOSFET Untuk Menggabungkan Catu Daya." },
  { "en": "Apa Itu Sequencing Catu Daya?", "id": "Mengatur Urutan Waktu Nyala Catu Daya." },
  { "en": "Apa Itu Total Harmonic Distortion + Noise (THD+N)?", "id": "Ukuran Total Distorsi Harmonik Dan Derau." },
  { "en": "Apa Itu Signal-to-Noise and Distortion Ratio (SINAD)?", "id": "Ukuran Kualitas Keseluruhan Sinyal." },
  { "en": "Bagaimana Hubungan Antara SINAD Dan ENOB?", "id": "SINAD Digunakan Menghitung Effective Number of Bits." },
  { "en": "Apa Itu Effective Number of Bits (ENOB)?", "id": "Ukuran Resolusi Aktual Konverter Data." },
  { "en": "Apa Itu Penguat Gain Variabel (VGA)?", "id": "Penguat Yang Gain-nya Dapat Diatur Analog." },
  { "en": "Apa Perbedaan PGA Dan VGA?", "id": "PGA Diatur Digital, VGA Diatur Analog." },
  { "en": "Apa Itu Koreksi Gamma Dalam Video?", "id": "Kompensasi Non-Linearitas Persepsi Kecerahan Mata." },
  { "en": "Apa Itu Pasangan Sziklai (Sziklai Pair)?", "id": "Alternatif Konfigurasi Darlington Menggunakan Transistor Komplementer." },
  { "en": "Apa Keuntungan Pasangan Sziklai?", "id": "Tegangan Saturasi Lebih Rendah Dari Darlington." },
  { "en": "Apa Itu Current Conveyor?", "id": "Blok Rangkaian Elektronik Serbaguna." },
  { "en": "Apa Itu Penguat Terkopel Trafo?", "id": "Menggunakan Trafo Untuk Kopling Antar Tingkat." },
  { "en": "Di Mana Penguat Terkopel Trafo Digunakan?", "id": "Penguat Daya RF Untuk Pencocokan Impedansi." },
  { "en": "Apa Itu Penguat Tuned?", "id": "Penguat Yang Bekerja Pada Pita Frekuensi Sempit." },
  { "en": "Bagaimana Penguat Tuned Dibuat?", "id": "Menggunakan Sirkuit Tangki LC Sebagai Beban." },
  { "en": "Apa Itu Stagger Tuning?", "id": "Menggeser Frekuensi Resonansi Setiap Tingkat Penguat." },
  { "en": "Apa Tujuan Stagger Tuning?", "id": "Mendapatkan Respon Frekuensi Yang Lebih Datar." },
  { "en": "Apa Itu Noise Floor?", "id": "Level Derau Latar Belakang Suatu Sistem." },
  { "en": "Apa Itu Dynamic Range?", "id": "Rasio Antara Sinyal Terbesar Dan Terkecil." },
  { "en": "Apa Itu Desensitisasi?", "id": "Pengurangan Sensitivitas Penerima Akibat Sinyal Kuat." },
  { "en": "Apa Itu Blocking Dalam Penerima?", "id": "Sinyal Pengganggu Kuat Memblokir Sinyal Lemah." },
  { "en": "Apa Itu Cross-Modulation?", "id": "Modulasi Sinyal Diinginkan Oleh Sinyal Pengganggu." },
  { "en": "Apa Itu Penguat Cryogenic?", "id": "Penguat Didinginkan Suhu Sangat Rendah." },
  { "en": "Mengapa Penguat Perlu Didinginkan?", "id": "Untuk Mengurangi Derau Termal Secara Drastis." },
  { "en": "Apa Itu Efek Tembakan (Shot Effect)?", "id": "Nama Lain Untuk Derau Tembak (Shot Noise)." },
  { "en": "Apa Itu Rangkaian bootstrap?", "id": "Teknik Menaikkan Impedansi Input Secara Dinamis." },
  { "en": "Apa Itu Filter Finite Impulse Response (FIR)?", "id": "Filter Digital Tanpa Umpan Balik." },
  { "en": "Apa Sifat Utama Filter FIR?", "id": "Selalu Stabil Dan Dapat Memiliki Fasa Linear." },
  { "en": "Apa Itu Filter Infinite Impulse Response (IIR)?", "id": "Filter Digital Dengan Umpan Balik." },
  { "en": "Apa Sifat Utama Filter IIR?", "id": "Sangat Efisien Tapi Bisa Jadi Tidak Stabil." },
  { "en": "Struktur Apa Yang Digunakan Untuk Filter IIR?", "id": "Direct Form, Cascade Form, Parallel Form." },
  { "en": "Apa Itu Jendela (Windowing) Dalam Desain Filter FIR?", "id": "Metode Untuk Mereduksi Riak Pada Respon." },
  { "en": "Apa Contoh Fungsi Jendela?", "id": "Hamming, Hanning, Blackman, Kaiser." },
  { "en": "Apa Itu Metode Parks-McClellan?", "id": "Algoritma Mendesain Filter FIR Optimal." },
  { "en": "Apa Itu Filter Kuadratur Cermin (QMF)?", "id": "Bank Filter Untuk Memisahkan Sinyal." },
  { "en": "Apa Itu Delay-Locked Loop (DLL)?", "id": "Rangkaian Mirip PLL Untuk Sinkronisasi Fasa." },
  { "en": "Apa Perbedaan Utama PLL Dan DLL?", "id": "DLL Tidak Mengalikan Frekuensi Referensi." },
  { "en": "Di Mana DLL Digunakan?", "id": "Sinkronisasi Clock Dalam Memori DDR SDRAM." },
  { "en": "Apa Itu Spurious-Free Dynamic Range (SFDR)?", "id": "Rasio Sinyal Terhadap Komponen Palsu Terkuat." },
  { "en": "Apa Itu Reciprocal Mixing?", "id": "Proses Derau Fasa Osilator Tercampur Sinyal." },
  { "en": "Apa Efek Reciprocal Mixing?", "id": "Mengurangi Kemampuan Menerima Sinyal Lemah." },
  { "en": "Apa Itu Pushing Figure Osilator?", "id": "Ukuran Perubahan Frekuensi Akibat Tegangan Catu." },
  { "en": "Apa Itu Pulling Figure Osilator?", "id": "Ukuran Perubahan Frekuensi Akibat Beban." },
  { "en": "Apa Itu Osilator Penstabil Diri (Self-Quenching)?", "id": "Osilator Yang Periodik Berhenti Dan Mulai." },
  { "en": "Apa Itu Mode Squegging?", "id": "Osilasi Amplitudo Rendah Pada Osilator." },
  { "en": "Apa Itu Harmonik?", "id": "Frekuensi Yang Merupakan Kelipatan Bulat Fundamental." },
  { "en": "Apa Itu Subharmonik?", "id": "Frekuensi Yang Merupakan Bagian Bulat Fundamental." },
  { "en": "Apa Itu Penyearah Presisi?", "id": "Rangkaian Op-Amp Menyearahkan Sinyal Tanpa Jatuh Tegangan." },
  { "en": "Apa Itu Detektor Amplop (Envelope Detector)?", "id": "Rangkaian Mengekstrak Amplop Sinyal Termodulasi AM." },
  { "en": "Apa Itu Automatic Frequency Control (AFC)?", "id": "Rangkaian Menjaga Osilator Tetap Terkunci Frekuensi." },
  { "en": "Apa Itu Detektor Kuadratur?", "id": "Rangkaian Untuk Mendemodulasi Sinyal FM." },
  { "en": "Apa Itu Discriminator Frekuensi?", "id": "Rangkaian Mengubah Perubahan Frekuensi Menjadi Tegangan." },
  { "en": "Apa Itu Foster-Seeley Discriminator?", "id": "Jenis Discriminator FM Populer." },
  { "en": "Apa Itu Ratio Detector?", "id": "Jenis Lain Discriminator FM." },
  { "en": "Apa Itu Tanda Sertifikasi UL (Underwriters Laboratories)?", "id": "Menunjukkan Produk Telah Diuji Keamanan." },
  { "en": "Apa Itu Tanda CE (Conformité Européenne)?", "id": "Pernyataan Produk Memenuhi Standar Uni Eropa." },
  { "en": "Apa Itu Inrush Current Limiter?", "id": "Rangkaian Untuk Membatasi Arus Puncak." },
  { "en": "Apa Itu Termistor NTC (Negative Temperature Coefficient)?", "id": "Resistansi Menurun Saat Suhu Naik." },
  { "en": "Bagaimana Termistor NTC Membatasi Arus Inrush?", "id": "Resistansi Tinggi Saat Dingin, Rendah Saat Panas." },
  { "en": "Apa Itu Penyearahan Sinkron (Synchronous Rectification)?", "id": "Mengganti Dioda Dengan MOSFET." },
  { "en": "Mengapa Penyearahan Sinkron Lebih Efisien?", "id": "Jatuh Tegangan Pada MOSFET Lebih Rendah." },
  { "en": "Apa Itu Baterai Lithium-Ion (Li-ion)?", "id": "Jenis Baterai Isi Ulang Populer." },
  { "en": "Apa Itu Baterai Lithium Polymer (LiPo)?", "id": "Varian Baterai Li-ion Dengan Elektrolit Polimer." },
  { "en": "Apa Keuntungan Baterai LiPo?", "id": "Bentuk Fleksibel Dan Kepadatan Energi Tinggi." },
  { "en": "Apa Itu Baterai Nickel-Metal Hydride (NiMH)?", "id": "Jenis Baterai Isi Ulang Lainnya." },
  { "en": "Apa Itu Efek Memori Baterai?", "id": "Pengurangan Kapasitas Jika Tidak Dikosongkan Sepenuhnya." },
  { "en": "Baterai Jenis Apa Yang Menderita Efek Memori?", "id": "Baterai NiCd Dan Sedikit Pada NiMH." },
  { "en": "Apa Itu Self-Discharge Rate?", "id": "Laju Baterai Kehilangan Muatan Saat Tidak Dipakai." },
  { "en": "Apa Itu C-Rate Baterai?", "id": "Ukuran Laju Pengisian Atau Pengosongan." },
  { "en": "Apa Arti 1C Rate?", "id": "Mengisi Atau Mengosongkan Baterai Dalam Satu Jam." },
  { "en": "Apa Itu Power Supply Sequencing?", "id": "Menghidupkan Beberapa Rel Tegangan Urutan Tertentu." },
  { "en": "Apa Itu Power Good Signal?", "id": "Sinyal Menunjukkan Semua Rel Tegangan Stabil." },
  { "en": "Apa Itu Ripple Steering?", "id": "Teknik Mengurangi Riak EMI Dalam Konverter Multi-Fasa." },
  { "en": "Apa Itu Tegangan Tembus Balik (Reverse Breakdown)?", "id": "Tegangan Balik Maksimum Yang Dapat Ditahan Dioda." },
  { "en": "Apa Itu Kapasitansi Sambungan (Junction Capacitance)?", "id": "Kapasitansi Parasitik Pada Dioda Semikonduktor." },
  { "en": "Apa Itu Waktu Pemulihan Balik (Reverse Recovery Time)?", "id": "Waktu Dioda Berhenti Menghantar Arus Balik." },
  { "en": "Apa Itu Regulator Shunt?", "id": "Regulator Menjaga Tegangan Dengan Mengalihkan Arus." },
  { "en": "Apa Contoh Sederhana Regulator Shunt?", "id": "Dioda Zener Dengan Resistor Seri." },
  { "en": "Apa Itu Regulator Seri?", "id": "Regulator Menempatkan Elemen Kontrol Seri." },
  { "en": "Apa Contoh Regulator Seri?", "id": "Regulator LDO Dan IC Seri 78xx." },
  { "en": "Apa Itu Konverter Kuasi-Resonan?", "id": "Konverter Switching Mengurangi Rugi Dengan Resonansi." },
  { "en": "Apa Itu Mode Lembah Switching (Valley Switching)?", "id": "Teknik Mengurangi Rugi Pada Konverter Flyback." },
  { "en": "Apa Itu Pengganda Tegangan (Voltage Multiplier)?", "id": "Rangkaian Dioda Kapasitor Menaikkan Tegangan DC." },
  { "en": "Apa Contoh Pengganda Tegangan?", "id": "Rangkaian Cockcroft-Walton." },
  { "en": "Di Mana Pengganda Tegangan Digunakan?", "id": "Tabung Sinar Katoda Dan Akselerator Partikel." },
  { "en": "Apa Itu Konverter Terisolasi Jembatan Penuh (Full-Bridge)?", "id": "Topologi SMPS Daya Tinggi." },
  { "en": "Apa Itu Konverter Terisolasi Jembatan Setengah (Half-Bridge)?", "id": "Varian Jembatan Penuh Dengan Komponen Lebih Sedikit." },
  { "en": "Apa Itu Phase-Shifted Full-Bridge?", "id": "Teknik Kontrol Mencapai Zero Voltage Switching." },
  { "en": "Apa Itu OR-ing Dioda?", "id": "Menggabungkan Dua Catu Daya Menggunakan Dioda." },
  { "en": "Apa Kelemahan OR-ing Dioda?", "id": "Jatuh Tegangan Dan Disipasi Daya Pada Dioda." },
  { "en": "Apa Itu Batas Rentang Mode-Common (Common-Mode Range)?", "id": "Rentang Tegangan Input Yang Diizinkan Op-Amp." },
  { "en": "Apa Yang Terjadi Jika Input Melebihi Rentang Mode-Common?", "id": "Penguat Berhenti Bekerja Secara Linear." },
  { "en": "Apa Itu Batas Ayunan Output (Output Voltage Swing)?", "id": "Rentang Tegangan Output Maksimum Op-Amp." },
  { "en": "Mengapa Ayunan Output Tidak Mencapai Rel Catu Daya?", "id": "Karena Adanya Jatuh Tegangan Internal." },
  { "en": "Apa Itu Op-Amp Rail-to-Rail?", "id": "Op-Amp Dengan Ayunan Output Sangat Dekat Rel." },
  { "en": "Apa Itu Kompensasi Frekuensi Internal?", "id": "Kapasitor Internal Op-Amp Untuk Menjamin Stabilitas." },
  { "en": "Apa Itu Op-Amp Decompensated?", "id": "Op-Amp Tanpa Kompensasi Internal." },
  { "en": "Kapan Op-Amp Decompensated Digunakan?", "id": "Untuk Penguatan Tinggi Dengan Bandwidth Maksimal." },
  { "en": "Apa Itu Umpan Balik Aktif (Active Feedback)?", "id": "Menggunakan Komponen Aktif Dalam Jaringan Umpan Balik." },
  { "en": "Apa Itu Cermin Arus Cascode?", "id": "Modifikasi Cermin Arus Dengan Akurasi Tinggi." },
  { "en": "Apa Keuntungan Cermin Arus Cascode?", "id": "Impedansi Output Sangat Tinggi." },
  { "en": "Apa Itu Penguat Umpan Balik Tegangan (VFA)?", "id": "Jenis Op-Amp Standar Yang Umum." },
  { "en": "Apa Itu Penguat Umpan Balik Arus (CFA)?", "id": "Jenis Op-Amp Dengan Slew Rate Tinggi." },
  { "en": "Apa Keuntungan Utama CFA Dibanding VFA?", "id": "Bandwidth Hampir Tidak Bergantung Pada Penguatan." },
  { "en": "Apa Itu Rangkaian Howland Current Pump?", "id": "Sumber Arus Presisi Menggunakan Op-Amp." },
  { "en": "Apa Itu Gyrator?", "id": "Rangkaian Aktif Yang Mensimulasikan Sebuah Induktor." },
  { "en": "Mengapa Gyrator Berguna Dalam Sirkuit Terpadu?", "id": "Induktor Fisik Sulit Diintegrasikan Dalam Chip." },
  { "en": "Apa Itu Penguat Terkunci (Latch-Up)?", "id": "Kondisi Hubung Singkat Internal Pada CMOS." },
  { "en": "Apa Penyebab Latch-Up?", "id": "Struktur SCR Parasitik Dalam Sirkuit CMOS." },
  { "en": "Bagaimana Cara Mencegah Latch-Up?", "id": "Desain Layout Yang Baik Dan Guard Rings." },
  { "en": "Apa Itu Tegangan Noise Ekuivalen Input?", "id": "Sumber Tegangan Derau Ideal Di Input." },
  { "en": "Apa Itu Arus Noise Ekuivalen Input?", "id": "Sumber Arus Derau Ideal Di Input." },
  { "en": "Kapan Arus Noise Menjadi Penting?", "id": "Ketika Resistansi Sumber Sangat Tinggi." },
  { "en": "Apa Itu Phase Reversal Pada Op-Amp?", "id": "Fasa Output Tiba-Tiba Membalik." },
  { "en": "Kapan Phase Reversal Terjadi?", "id": "Ketika Tegangan Input Melebihi Batas Tertentu." },
  { "en": "Apa Itu Sensitivitas Filter (Filter Sensitivity)?", "id": "Ukuran Perubahan Respon Akibat Variasi Komponen." },
  { "en": "Mengapa Sensitivitas Filter Penting?", "id": "Komponen Nyata Memiliki Toleransi Nilai." },
  { "en": "Topologi Filter Mana Yang Memiliki Sensitivitas Rendah?", "id": "Topologi Berbasis Integrator Seperti Biquad." },
  { "en": "Apa Itu Toleransi Komponen?", "id": "Penyimpangan Nilai Komponen Dari Nilai Nominalnya." },
  { "en": "Apa Itu Koefisien Suhu Komponen?", "id": "Ukuran Perubahan Nilai Komponen Terhadap Suhu." },
  { "en": "Apa Itu Teknik Peninggian Q (Q-Enhancement)?", "id": "Meningkatkan Faktor Q Filter Secara Aktif." },
  { "en": "Apa Itu Filter Ladder Kristal?", "id": "Filter Sangat Tajam Menggunakan Beberapa Kristal." },
  { "en": "Apa Itu Bank Filter (Filter Bank)?", "id": "Kumpulan Filter Untuk Memisahkan Sinyal." },
  { "en": "Di Mana Bank Filter Digunakan?", "id": "Equalizer Audio Dan Penganalisis Spektrum." },
  { "en": "Apa Itu Filter Bessel-Thomson?", "id": "Nama Lain Untuk Filter Bessel." },
  { "en": "Apa Itu Filter Linkwitz-Riley?", "id": "Filter Populer Untuk Crossover Audio." },
  { "en": "Apa Sifat Utama Filter Linkwitz-Riley?", "id": "Respon Datar Pada Frekuensi Crossover." },
  { "en": "Apa Itu Jaringan Crossover?", "id": "Filter Memisahkan Sinyal Audio Untuk Speaker." },
  { "en": "Apa Itu Woofer, Midrange, Dan Tweeter?", "id": "Jenis Speaker Untuk Frekuensi Rendah, Tengah, Tinggi." },
  { "en": "Apa Itu Transformasi Frekuensi?", "id": "Mengubah Prototipe Low-Pass Ke Tipe Filter Lain." },
  { "en": "Apa Itu Prototipe Filter Low-Pass Ternormalisasi?", "id": "Desain Filter Referensi Dengan Cutoff 1 Rad/s." },
  { "en": "Apa Itu Polinomial Chebyshev?", "id": "Dasar Matematis Untuk Desain Filter Chebyshev." },
  { "en": "Apa Itu Polinomial Butterworth?", "id": "Dasar Matematis Untuk Desain Filter Butterworth." },
  { "en": "Apa Itu Girator Kapasitansi Negatif?", "id": "Rangkaian Aktif Mensimulasikan Kapasitor Negatif." },
  { "en": "Apa Itu Filter Non-Linear?", "id": "Filter Yang Perilakunya Bergantung Amplitudo Sinyal." },
  { "en": "Apa Itu Pullability Kristal Osilator?", "id": "Kemampuan Frekuensi Diubah Oleh Beban Kapasitif." },
  { "en": "Bagaimana Cara 'Menarik' Frekuensi Kristal?", "id": "Dengan Menambahkan Kapasitor Seri Atau Paralel." },
  { "en": "Apa Itu Numerically Controlled Oscillator (NCO)?", "id": "Osilator Digital Murni Menghasilkan Gelombang." },
  { "en": "Apa Komponen Utama NCO?", "id": "Phase Accumulator Dan Phase-to-Amplitude Converter." },
  { "en": "Apa Trade-Off Utama Desain VCO?", "id": "Antara Derau Fasa Dan Rentang Penyetelan." },
  { "en": "Mengapa Ada Trade-Off Tersebut?", "id": "Resonator Q Tinggi Sulit Disetel Jauh." },
  { "en": "Bagaimana Kriteria Barkhausen Terlihat Di Plot Nyquist?", "id": "Kurva G(jω)H(jω) Melingkupi Titik (-1, 0)." },
  { "en": "Apa Itu Osilator Kuarsa Mikro-elektromekanis (MEMS)?", "id": "Alternatif Modern Untuk Osilator Kristal Kuarsa." },
  { "en": "Apa Itu Start-Up Time Osilator?", "id": "Waktu Yang Dibutuhkan Osilator Mencapai Amplitudo Stabil." },
  { "en": "Apa Itu Dead Zone Pada Detektor Fasa?", "id": "Daerah Di Mana PFD Tidak Sensitif." },
  { "en": "Apa Itu Cycle Slip Dalam PLL?", "id": "Loncatan Fasa Sebesar Kelipatan 2π." },
  { "en": "Apa Itu Integer Boundary Spurs?", "id": "Spur Khas Pada Synthesizer Fraksional-N." },
  { "en": "Apa Itu Modulator Delta-Sigma?", "id": "Teknik Untuk Mencapai Resolusi Fraksional." },
  { "en": "Di Mana Modulator Delta-Sigma Digunakan?", "id": "Synthesizer Fraksional-N Dan Konverter Data." },
  { "en": "Apa Itu Time-to-Digital Converter (TDC)?", "id": "Mengukur Interval Waktu Dan Mengubahnya Digital." },
  { "en": "Apa Itu Osilator Terdistribusi?", "id": "Osilator Microwave Menggunakan Prinsip Saluran Transmisi." },
  { "en": "Apa Itu Push-Push Oscillator?", "id": "Osilator Menghasilkan Harmonik Kedua Sebagai Output." },
  { "en": "Apa Itu Push-Pull Oscillator?", "id": "Osilator Menekan Harmonik Kedua." },
  { "en": "Apa Itu Penuaan (Aging) Osilator?", "id": "Pergeseran Frekuensi Jangka Panjang." },
  { "en": "Apa Itu Retrace Histeresis Suhu?", "id": "Frekuensi Tidak Kembali Sama Persis." },
  { "en": "Apa Itu Induktansi Parasitik?", "id": "Induktansi Yang Tidak Diinginkan Pada Jalur PCB." },
  { "en": "Apa Itu Kapasitansi Parasitik?", "id": "Kapasitansi Yang Tidak Diinginkan Antar Jalur PCB." },
  { "en": "Bagaimana Parasitik Mempengaruhi Catu Daya Switching?", "id": "Menyebabkan Ringing Dan Lonjakan Tegangan." },
  { "en": "Apa Pentingnya Layout Printed Circuit Board (PCB)?", "id": "Sangat Kritis Untuk Kinerja Catu Daya." },
  { "en": "Apa Itu Loop Arus Kritis?", "id": "Jalur Arus Switching Yang Harus Dibuat Kecil." },
  { "en": "Apa Itu Resistansi Termal (Thermal Resistance)?", "id": "Ukuran Hambatan Aliran Panas Dari Junction." },
  { "en": "Apa Satuan Resistansi Termal?", "id": "Derajat Celcius Per Watt (°C/W)." },
  { "en": "Apa Itu Junction Temperature?", "id": "Suhu Internal Inti Semikonduktor." },
  { "en": "Mengapa Menjaga Junction Temperature Penting?", "id": "Untuk Keandalan Dan Mencegah Kerusakan." },
  { "en": "Apa Itu Heatsink?", "id": "Komponen Logam Untuk Mendinginkan Perangkat Elektronik." },
  { "en": "Apa Itu Thermal Pad/Grease?", "id": "Material Meningkatkan Transfer Panas Antar Permukaan." },
  { "en": "Apa Itu Derating?", "id": "Mengoperasikan Komponen Di Bawah Rating Maksimumnya." },
  { "en": "Apa Itu Rangkaian Soft-Start?", "id": "Menaikkan Tegangan Output Secara Bertahap." },
  { "en": "Apa Itu Interleaving Dalam Konverter?", "id": "Menjalankan Konverter Paralel Dengan Fasa Berbeda." },
  { "en": "Apa Manfaat Interleaving?", "id": "Mereduksi Riak Arus Input Dan Output." },
  { "en": "Apa Itu Kompensasi Droop?", "id": "Sengaja Menurunkan Tegangan Output Saat Beban Naik." },
  { "en": "Apa Tujuan Kompensasi Droop?", "id": "Untuk Membantu Pembagian Beban Secara Merata." },
  { "en": "Apa Itu Point of Load (PoL) Converter?", "id": "Konverter DC-DC Kecil Ditempatkan Dekat Beban." },
  { "en": "Apa Keuntungan Konverter Point of Load (PoL)?", "id": "Mengurangi Rugi-Rugi Distribusi Daya." },
  { "en": "Apa Itu Arsitektur Daya Terdistribusi?", "id": "Sistem Menggunakan Banyak Konverter PoL." },
  { "en": "Apa Itu Digital Power?", "id": "Manajemen Catu Daya Menggunakan Kontrol Digital." },
  { "en": "Apa Itu Tahap Output Kuasi-Komplementer?", "id": "Menggunakan Dua Jenis Transistor Sama (NPN)." },
  { "en": "Apa Itu Distorsi Termal Pada Penguat Audio?", "id": "Distorsi Akibat Perubahan Suhu Transistor." },
  { "en": "Apa Itu Voltage-Controlled Amplifier (VCA)?", "id": "Penguat Yang Gain-nya Dikontrol Oleh Tegangan." },
  { "en": "Di Mana VCA Digunakan?", "id": "Synthesizer Audio Dan Rangkaian Kompresor." },
  { "en": "Apa Itu Koreksi Umpan Maju (Feed-Forward)?", "id": "Teknik Mengurangi Distorsi Dengan Menambah Sinyal Error." },
  { "en": "Apa Fungsi Rangkaian Proteksi Input Op-Amp?", "id": "Melindungi Input Dari Tegangan Berlebih." },
  { "en": "Apa Itu Rangkaian Anti-Jenuh (Anti-Saturation)?", "id": "Mencegah Transistor Masuk Ke Daerah Jenuh." },
  { "en": "Mengapa Kejenuhan Transistor Harus Dihindari?", "id": "Karena Memperlambat Waktu Switching." },
  { "en": "Apa Itu Dioda Baker Clamp?", "id": "Rangkaian Anti-Jenuh Menggunakan Dioda Schottky." },
  { "en": "Apa Itu Penguat Long-Tailed Pair?", "id": "Nama Lain Untuk Tahap Input Diferensial." },
  { "en": "Apa Fungsi Sumber Arus Ekor (Tail Current)?", "id": "Meningkatkan Common-Mode Rejection Ratio (CMRR)." },
  { "en": "Apa Itu Offset Nulling?", "id": "Prosedur Untuk Menghilangkan Tegangan Offset." },
  { "en": "Apa Itu Penguat Terkopel Optik (Opto-Coupled)?", "id": "Nama Lain Untuk Penguat Isolasi." },
  { "en": "Apa Itu Titik Setengah Daya (Half-Power Point)?", "id": "Nama Lain Untuk Frekuensi Cutoff." },
  { "en": "Apa Itu Power Bandwidth?", "id": "Rentang Frekuensi Output Daya Penuh." },
  { "en": "Apa Itu Respon Impuls?", "id": "Output Sistem Terhadap Sinyal Input Impuls." },
  { "en": "Apa Itu Respon Step?", "id": "Output Sistem Terhadap Sinyal Input Step." },
  { "en": "Bagaimana Hubungan Respon Step Dan Impuls?", "id": "Respon Step Adalah Integral Respon Impuls." },
  { "en": "Apa Itu Waktu Pemulihan Overload?", "id": "Waktu Penguat Pulih Dari Kondisi Saturasi." },
  { "en": "Apa Itu Current Sense Amplifier?", "id": "Penguat Khusus Untuk Mengukur Arus." },
  { "en": "Apa Itu High-Side Current Sensing?", "id": "Mengukur Arus Antara Catu Daya Dan Beban." },
  { "en": "Apa Itu Low-Side Current Sensing?", "id": "Mengukur Arus Antara Beban Dan Ground." },
  { "en": "Apa Itu Common Mode Choke?", "id": "Induktor Untuk Menekan Derau Mode Common." },
  { "en": "Apa Itu Kapasitansi Miller?", "id": "Efek Peningkatan Kapasitansi Akibat Penguatan." },
  { "en": "Apa Itu Penguat JFET Source Follower?", "id": "Mirip Dengan Pengikut Emitter BJT." },
  { "en": "Apa Itu Riak Penundaan Grup (Group Delay Ripple)?", "id": "Variasi Penundaan Grup Di Dalam Passband." },
  { "en": "Filter Mana Yang Punya Riak Penundaan Grup?", "id": "Filter Eliptik Dan Filter Chebyshev." },
  { "en": "Apa Dampak Riak Penundaan Grup?", "id": "Menyebabkan Distorsi Pada Sinyal Digital." },
  { "en": "Bagaimana Phase-Locked Loop (PLL) Mendemodulasi FM?", "id": "Tegangan Kontrol VCO Mereplikasi Sinyal Audio." },
  { "en": "Apa Itu Predistorsi Filter LC?", "id": "Menyesuaikan Nilai Komponen Ideal." },
  { "en": "Mengapa Predistorsi Filter LC Diperlukan?", "id": "Untuk Mengompensasi Rugi-Rugi Komponen Nyata." },
  { "en": "Apa Itu Filter Distributed-Element?", "id": "Filter Microwave Menggunakan Saluran Transmisi." },
  { "en": "Apa Itu Hairpin Filter?", "id": "Jenis Filter Distributed-Element Bentuk Jepit Rambut." },
  { "en": "Apa Itu Filter Coaxial?", "id": "Filter Menggunakan Resonator Kabel Koaksial." },
  { "en": "Apa Itu Filter Dielektrik?", "id": "Filter Menggunakan Resonator Keramik Dielektrik." },
  { "en": "Apa Itu Resonator FBAR (Film Bulk Acoustic Resonator)?", "id": "Resonator Kinerja Tinggi Untuk Filter RF." },
  { "en": "Apa Keuntungan Resonator FBAR?", "id": "Ukuran Sangat Kecil Dan Faktor Q Tinggi." },
  { "en": "Apa Itu Bank Filter Terprogram?", "id": "Bank Filter Yang Dapat Diatur Digital." },
  { "en": "Apa Itu Analisis Monte Carlo?", "id": "Simulasi Statistik Memperhitungkan Toleransi Komponen." },
  { "en": "Apa Itu Analisis Skenario Terburuk (Worst-Case)?", "id": "Menganalisis Kinerja Pada Batas Toleransi Ekstrem." },
  { "en": "Apa Itu Respon Amplitudo Ideal Filter?", "id": "Datar Di Passband, Nol Di Stopband." },
  { "en": "Apa Itu Respon Fasa Ideal Filter?", "id": "Fasa Berbanding Lurus Dengan Frekuensi." },
  { "en": "Apa Itu Skirt Selectivity?", "id": "Ukuran Kecuraman Transisi Filter." },
  { "en": "Apa Itu Shape Factor?", "id": "Rasio Bandwidth Stopband Terhadap Passband." },
  { "en": "Berapa Shape Factor Ideal?", "id": "Satu (1)." },
  { "en": "Apa Itu Efek Leeson?", "id": "Model Matematis Untuk Derau Fasa Osilator." },
  { "en": "Apa Penyebab Derau 1/f³ Dalam Osilator?", "id": "Upkonversi Derau Flicker Dari Perangkat Aktif." },
  { "en": "Apa Itu Kristal Overtone?", "id": "Kristal Didesain Berosilasi Pada Harmonik Mekanis." },
  { "en": "Mengapa Kristal Overtone Digunakan?", "id": "Untuk Mencapai Frekuensi Osilasi Lebih Tinggi." },
  { "en": "Apa Itu Osilator Robinson?", "id": "Osilator Minimalis Dengan Stabilitas Baik." },
  { "en": "Apa Itu Digital-to-Time Converter (DTC)?", "id": "Menghasilkan Penundaan Waktu Yang Dapat Diprogram." },
  { "en": "Apa Itu All-Digital Phase-Locked Loop (ADPLL)?", "id": "PLL Diimplementasikan Sebagian Besar Secara Digital." },
  { "en": "Apa Keuntungan ADPLL?", "id": "Portabilitas Desain Dan Fleksibilitas Tinggi." },
  { "en": "Apa Itu Time-to-Digital Converter (TDC) Dalam ADPLL?", "id": "Menggantikan Peran Detektor Fasa Analog." },
  { "en": "Apa Itu Osilator Terkendali Digital (DCO)?", "id": "Menggantikan VCO Analog Dalam ADPLL." },
  { "en": "Apa Itu Derau Kuantisasi?", "id": "Error Akibat Representasi Sinyal Diskrit." },
  { "en": "Apa Itu Noise Shaping?", "id": "Teknik Memindahkan Derau Kuantisasi Keluar Pita." },
  { "en": "Di Mana Noise Shaping Digunakan?", "id": "Konverter Data Delta-Sigma Dan Synthesizer Fraksional-N." },
  { "en": "Apa Itu Isolasi Spektral?", "id": "Ukuran Seberapa Bersih Output Osilator." },
  { "en": "Apa Itu Jitter Perioda?", "id": "Deviasi Maksimum Dari Periode Sinyal Ideal." },
  { "en": "Apa Itu Jitter Fasa?", "id": "Integral Derau Fasa, Diukur Dalam Derajat." },
  { "en": "Apa Itu Mode Spurious?", "id": "Frekuensi Osilasi Yang Tidak Diinginkan." },
  { "en": "Apa Penyebab Mode Spurious Kristal?", "id": "Resonansi Mekanis Lain Dalam Kristal." },
  { "en": "Apa Itu Drive Level Kristal?", "id": "Daya Yang Diberikan Ke Kristal Osilator." },
  { "en": "Mengapa Drive Level Penting?", "id": "Terlalu Tinggi Merusak, Terlalu Rendah Gagal Osilasi." },
  { "en": "Apa Itu Kontroler Hot-Swap?", "id": "IC Mengelola Penyambungan Papan Ke Bus Aktif." },
  { "en": "Bagaimana Teknik Pengukuran Riak Output Yang Benar?", "id": "Menggunakan Probe 1x Dengan Ground Sangat Pendek." },
  { "en": "Mengapa Ground Probe Harus Pendek?", "id": "Untuk Menghindari Induktansi Yang Mengambil Derau." },
  { "en": "Apa Itu Penuaan Kapasitor Elektrolit?", "id": "Kehilangan Kapasitansi Dan Peningkatan ESR." },
  { "en": "Apa Penyebab Penuaan Kapasitor Elektrolit?", "id": "Penguapan Perlahan Cairan Elektrolit." },
  { "en": "Apa Itu Kontrol Droop?", "id": "Metode Untuk Paralel Catu Daya Tanpa Komunikasi." },
  { "en": "Bagaimana Kontrol Droop Bekerja?", "id": "Sedikit Menurunkan Tegangan Saat Arus Meningkat." },
  { "en": "Apa Itu Kelas Isolasi Catu Daya?", "id": "Menentukan Persyaratan Keamanan Isolasi." },
  { "en": "Apa Itu Isolasi Diperkuat (Reinforced Insulation)?", "id": "Sistem Isolasi Tunggal Yang Sangat Andal." },
  { "en": "Apa Itu Jarak Rambat (Creepage)?", "id": "Jarak Terpendek Antara Konduktor Sepanjang Permukaan." },
  { "en": "Apa Itu Jarak Bebas (Clearance)?", "id": "Jarak Terpendek Antara Konduktor Melalui Udara." },
  { "en": "Mengapa Creepage Dan Clearance Penting?", "id": "Untuk Mencegah Kegagalan Isolasi Dan Sengatan Listrik." },
  { "en": "Apa Itu Uji Hi-Pot (High Potential)?", "id": "Menguji Kemampuan Isolasi Menahan Tegangan Tinggi." },
  { "en": "Apa Itu Arus Bocor (Leakage Current)?", "id": "Arus Kecil Mengalir Melintasi Penghalang Isolasi." },
  { "en": "Apa Itu Konverter Isolated Gate Driver?", "id": "Catu Daya Kecil Menggerakkan Gerbang MOSFET." },
  { "en": "Apa Itu Tegangan Tembus Dioda?", "id": "Tegangan Balik Di Mana Dioda Mulai Menghantar." },
  { "en": "Apa Itu Active Clamp Forward Converter?", "id": "Topologi Mengurangi Ringing Dan Meningkatkan Efisiensi." },
  { "en": "Apa Itu Saturasi Magnetik?", "id": "Kondisi Inti Trafo Tidak Dapat Menyimpan Energi." },
  { "en": "Apa Akibat Saturasi Magnetik?", "id": "Arus Sangat Tinggi Dan Kegagalan Komponen." },
  { "en": "Apa Itu Siklus Histeresis?", "id": "Kurva B-H Material Magnetik." },
  { "en": "Apa Itu Remanensi?", "id": "Magnetisasi Sisa Setelah Medan Dihilangkan." },
  { "en": "Apa Itu Gaya Koersif?", "id": "Medan Balik Untuk Menghilangkan Magnetisasi Sisa." },
  { "en": "Apa Itu Elektromigrasi Dalam Sirkuit Terpadu?", "id": "Pergerakan Atom Logam Akibat Aliran Arus." },
  { "en": "Apa Akibat Dari Elektromigrasi?", "id": "Dapat Menyebabkan Kegagalan Sirkuit Terbuka." },
  { "en": "Apa Itu Hot Carrier Injection?", "id": "Elektron Energi Tinggi Merusak Gerbang Transistor." },
  { "en": "Apa Akibat Hot Carrier Injection?", "id": "Degradasi Kinerja Transistor Seiring Waktu." },
  { "en": "Apa Itu Operational Transconductance Amplifier (OTA)?", "id": "Penguat Dengan Output Arus Terkontrol Tegangan." },
  { "en": "Bagaimana Penguatan OTA Diatur?", "id": "Dengan Mengubah Arus Bias Eksternal." },
  { "en": "Bagaimana Cascode Mengurangi Efek Miller?", "id": "Menjaga Tegangan Kolektor-Basis Tetap Konstan." },
  { "en": "Apa Perbedaan Garis Beban AC Dan DC?", "id": "Garis Beban AC Memperhitungkan Impedansi Kopling." },
  { "en": "Apa Itu Penguat Kelas F?", "id": "Penguat Switching RF Efisiensi Tinggi." },
  { "en": "Bagaimana Penguat Kelas F Bekerja?", "id": "Menggunakan Resonator Harmonik Untuk Membentuk Gelombang." },
  { "en": "Apa Itu Penguat Umpan Balik Positif?", "id": "Sebagian Output Dikembalikan Sejajar Fasa." },
  { "en": "Apa Kegunaan Umpan Balik Positif?", "id": "Membuat Osilator Dan Rangkaian Regeneratif." },
  { "en": "Apa Itu Sirkuit Regeneratif?", "id": "Sirkuit Dengan Penguatan Sangat Tinggi." },
  { "en": "Apa Itu Koefisien Kopling Transformator?", "id": "Ukuran Seberapa Erat Dua Kumparan Terkopel." },
  { "en": "Berapa Nilai Koefisien Kopling Ideal?", "id": "Satu (1)." },
  { "en": "Apa Itu Induktansi Bocor (Leakage Inductance)?", "id": "Fluks Magnetik Yang Tidak Terkopel." },
  { "en": "Apa Akibat Induktansi Bocor?", "id": "Menyebabkan Lonjakan Tegangan Pada Rangkaian Switching." },
  { "en": "Apa Itu Kapasitansi Antar Kumparan?", "id": "Kapasitansi Parasitik Antara Kumparan Primer Sekunder." },
  { "en": "Apa Itu Penguat Distribusi?", "id": "Penguat Bandwidth Ultra-Lebar (UWB)." },
  { "en": "Bagaimana Prinsip Kerja Penguat Distribusi?", "id": "Menggabungkan Kapasitansi Transistor Ke Saluran Transmisi." },
  { "en": "Apa Itu Penguat Video Terkoreksi Peaking?", "id": "Menambahkan Induktor Untuk Memperluas Bandwidth." },
  { "en": "Apa Itu Shunt Peaking?", "id": "Menempatkan Induktor Seri Dengan Resistor Beban." },
  { "en": "Apa Itu Series Peaking?", "id": "Menempatkan Induktor Antara Output Dan Beban." },
  { "en": "Apa Itu Penguat Diferensial Empat Kuadran?", "id": "Nama Lain Untuk Sel Gilbert." },
  { "en": "Apa Itu Titik Saturasi?", "id": "Titik Di Mana Penguatan Menurun." },
  { "en": "Apa Itu Filter Resonator Mikro-Elektromekanis (MEMS)?", "id": "Filter Miniatur Berbasis Resonator Silikon." },
  { "en": "Apa Itu Resonator Heliks?", "id": "Elemen Resonansi Berbentuk Heliks Dalam Pelindung." },
  { "en": "Apa Keuntungan Resonator Heliks?", "id": "Faktor Kualitas (Q) Sangat Tinggi." },
  { "en": "Apa Itu Penyetelan (Tuning) Filter Mekanis?", "id": "Mengubah Frekuensi Dengan Menyesuaikan Sekrup Tuning." },
  { "en": "Apa Itu Penyetelan Filter Otomatis?", "id": "Sirkuit Mengkalibrasi Respon Filter Secara Otomatis." },
  { "en": "Apa Itu Insertion Loss Filter?", "id": "Pelemahan Sinyal Di Dalam Daerah Passband." },
  { "en": "Apa Itu Return Loss Filter?", "id": "Ukuran Seberapa Baik Impedansi Filter Cocok." },
  { "en": "Berapa Nilai Return Loss Ideal?", "id": "Tak Terhingga (Infinity)." },
  { "en": "Apa Itu Voltage Standing Wave Ratio (VSWR)?", "id": "Ukuran Lain Untuk Pencocokan Impedansi." },
  { "en": "Apa Itu Filter Air-Stripline?", "id": "Filter Microwave Menggunakan Udara Sebagai Dielektrik." },
  { "en": "Apa Itu Filter Substrate-Integrated Waveguide (SIW)?", "id": "Mensimulasikan Pemandu Gelombang Pada Papan PCB." },
  { "en": "Apa Itu Filter YIG (Yttrium Iron Garnet)?", "id": "Filter Microwave Disetel Dengan Medan Magnet." },
  { "en": "Apa Itu Limiter?", "id": "Rangkaian Melindungi Komponen Dari Daya Berlebih." },
  { "en": "Bagaimana Cara Kerja Limiter Dioda PIN?", "id": "Resistansi Dioda Berubah Dengan Daya RF." },
  { "en": "Apa Itu Bank Kapasitor Terprogram?", "id": "Kumpulan Kapasitor Yang Dapat Diaktifkan Digital." },
  { "en": "Apa Itu Koefisien Kualitas (Q) Tak Berbeban?", "id": "Faktor Q Resonator Tanpa Beban Eksternal." },
  { "en": "Apa Itu Koefisien Kualitas (Q) Berbeban?", "id": "Faktor Q Resonator Dengan Beban Eksternal." },
  { "en": "Apa Itu Kopling Kritis?", "id": "Kopling Yang Memberikan Transfer Daya Maksimum." },
  { "en": "Apa Itu Kopling Kurang (Undercoupling)?", "id": "Kopling Lebih Lemah Dari Kopling Kritis." },
  { "en": "Apa Itu Kopling Lebih (Overcoupling)?", "id": "Kopling Lebih Kuat Dari Kopling Kritis." },
  { "en": "Apa Itu Jam Atom Cesium?", "id": "Standar Frekuensi Utama Dunia." },
  { "en": "Apa Itu Jam Atom Rubidium?", "id": "Jam Atom Komersial Yang Lebih Ringkas." },
  { "en": "Apa Prinsip Dasar Jam Atom?", "id": "Mengunci Frekuensi Osilator Ke Transisi Atom." },
  { "en": "Apa Itu Osilator Spin-Torque (Spintronic Oscillator)?", "id": "Osilator Nano Berbasis Efek Spintronik." },
  { "en": "Bagaimana Cara Mengukur Derau Fasa?", "id": "Menggunakan Penganalisis Spektrum Atau Sistem Khusus." },
  { "en": "Apa Itu Teknik Dua Osilator?", "id": "Metode Pengukuran Derau Fasa." },
  { "en": "Apa Itu Mode Hopping?", "id": "Loncatan Tiba-Tiba Frekuensi Osilator." },
  { "en": "Apa Penyebab Mode Hopping?", "id": "Adanya Beberapa Mode Resonansi Yang Mungkin." },
  { "en": "Apa Itu Karakterisasi AM-ke-PM?", "id": "Perubahan Fasa Akibat Perubahan Amplitudo." },
  { "en": "Apa Itu Osilator Penstabil Rongga Kriogenik (Cryogenic)?", "id": "Osilator Stabilitas Sangat Tinggi Untuk Sains." },
  { "en": "Apa Itu Sisir Frekuensi Optik (Optical Frequency Comb)?", "id": "Sumber Cahaya Dengan Garis Spektrum." },
  { "en": "Apa Itu Global Positioning System (GPS) Disciplined Oscillator?", "id": "Osilator Lokal Disinkronkan Dengan Sinyal GPS." },
  { "en": "Apa Itu Osilator Metronom?", "id": "Osilator Audio Menghasilkan Bunyi Klik Berirama." },
  { "en": "Apa Itu Generator Nada DTMF?", "id": "Menghasilkan Nada Panggilan Telepon." },
  { "en": "Apa Itu Modulasi Sudut?", "id": "Modulasi Frekuensi (FM) Dan Modulasi Fasa (PM)." },
  { "en": "Apa Itu Indeks Modulasi?", "id": "Ukuran Seberapa Jauh Sinyal Termodulasi Bervariasi." },
  { "en": "Apa Itu Bandwidth Carson?", "id": "Aturan Praktis Untuk Bandwidth Sinyal FM." },
  { "en": "Apa Itu Pre-Emphasis Dan De-Emphasis?", "id": "Teknik Meningkatkan Sinyal-ke-Derau Dalam FM." },
  { "en": "Apa Itu Siaran Stereo FM?", "id": "Mengirimkan Sinyal Kiri Dan Kanan." },
  { "en": "Apa Itu Sinyal Pilot Stereo?", "id": "Sinyal 19 kHz Untuk Mendeteksi Siaran Stereo." },
  { "en": "Apa Itu Standar Keamanan IEC 60950?", "id": "Standar Untuk Peralatan Teknologi Informasi." },
  { "en": "Apa Itu Catu Daya Kelas Medis?", "id": "Catu Daya Memenuhi Standar Keamanan Medis." },
  { "en": "Apa Syarat Utama Catu Daya Medis?", "id": "Arus Bocor Sangat Rendah Dan Isolasi Tinggi." },
  { "en": "Apa Itu Proteksi Thermal Shutdown?", "id": "Fitur Mematikan IC Jika Terlalu Panas." },
  { "en": "Apa Itu Power Supply Rejection Ratio (PSRR)?", "id": "Kemampuan Perangkat Menolak Derau Catu Daya." },
  { "en": "Mengapa PSRR Penting Untuk Op-Amp?", "id": "Mencegah Riak Catu Daya Muncul Di Output." },
  { "en": "Apa Itu Active Current Sharing?", "id": "Metode Presisi Untuk Paralel Catu Daya." },
  { "en": "Apa Itu Konverter Bidireksional?", "id": "Konverter DC-DC Yang Arusnya Dapat Mengalir Dua Arah." },
  { "en": "Di Mana Konverter Bidireksional Digunakan?", "id": "Sistem Hibrida Kendaraan Dan Penyimpanan Energi." },
  { "en": "Apa Itu Energy Harvesting?", "id": "Mengumpulkan Energi Dari Sumber Lingkungan." },
  { "en": "Apa Contoh Sumber Energy Harvesting?", "id": "Getaran (Piezoelektrik), Cahaya (Surya), Panas (Termoelektrik)." },
  { "en": "Apa Itu Catu Daya Tak Terinterupsi (UPS) Online?", "id": "Selalu Menyuplai Daya Melalui Baterai." },
  { "en": "Apa Keuntungan UPS Online?", "id": "Perlindungan Maksimal Tanpa Waktu Transfer." },
  { "en": "Apa Itu Catu Daya Tak Terinterupsi (UPS) Offline?", "id": "Beralih Ke Baterai Saat Listrik Padam." },
  { "en": "Apa Itu Automatic Voltage Regulator (AVR)?", "id": "Fitur UPS Menstabilkan Tegangan Tanpa Baterai." },
  { "en": "Apa Itu Kapasitor Keramik Multi-Lapis (MLCC)?", "id": "Jenis Kapasitor Paling Umum Saat Ini." },
  { "en": "Apa Itu Efek Piezoelektrik DC Bias MLCC?", "id": "Kapasitansi Berkurang Dengan Tegangan DC." },
  { "en": "Apa Itu Efek Mikrofonik Kapasitor?", "id": "Getaran Menghasilkan Sinyal Tegangan Kecil." },
  { "en": "Apa Itu Faktor Disipasi (Tan δ)?", "id": "Ukuran Rugi-Rugi Internal Kapasitor." },
  { "en": "Apa Itu Arus Bocor Kapasitor?", "id": "Arus DC Kecil Mengalir Melalui Dielektrik." },
  { "en": "Apa Itu Tegangan Kerja Kapasitor?", "id": "Tegangan Maksimum Yang Dapat Diterapkan." },
  { "en": "Apa Keuntungan Transistor Gallium Arsenide (GaAs) FET?", "id": "Kecepatan Sangat Tinggi Untuk Aplikasi RF." },
  { "en": "Apa Keuntungan Transistor Silikon-Germanium (SiGe)?", "id": "Menggabungkan Kecepatan Tinggi Dengan Biaya Rendah." },
  { "en": "Apa Perbedaan Utama BJT Dan MOSFET?", "id": "BJT Dikontrol Arus, MOSFET Tegangan." },
  { "en": "Apa Itu Penguat Transkonduktansi?", "id": "Mengubah Input Tegangan Menjadi Output Arus." },
  { "en": "Apa Satuan Transkonduktansi?", "id": "Siemens (S) Atau Mhos." },
  { "en": "Apa Itu SPICE (Simulation Program with Integrated Circuit Emphasis)?", "id": "Standar Industri Untuk Simulasi Rangkaian Elektronik." },
  { "en": "Apa Itu Model Gummel-Poon?", "id": "Model SPICE Detail Untuk Transistor BJT." },
  { "en": "Apa Itu Model BSIM?", "id": "Model SPICE Standar Untuk Transistor MOSFET." },
  { "en": "Apa Itu Layout Common-Centroid?", "id": "Teknik Layout Mengurangi Mismatch Pasangan Diferensial." },
  { "en": "Mengapa Mismatch Penting Dalam Pasangan Diferensial?", "id": "Menyebabkan Tegangan Offset Yang Tidak Diinginkan." },
  { "en": "Apa Itu Penguat Chopper?", "id": "Nama Lain Untuk Penguat Stabil Chopper." },
  { "en": "Apa Itu Derau Burst (Popcorn Noise)?", "id": "Transisi Acak Antara Dua Level Tegangan." },
  { "en": "Apa Itu Penguat Terkopel Emitor (Emitter-Coupled Logic)?", "id": "Keluarga Logika Kecepatan Sangat Tinggi." },
  { "en": "Apa Itu Penguat Video Diferensial?", "id": "Menguatkan Sinyal Video Diferensial Seperti LVDS." },
  { "en": "Apa Itu Low-Voltage Differential Signaling (LVDS)?", "id": "Standar Sinyal Diferensial Kecepatan Tinggi." },
  { "en": "Apa Itu Efek Kirk?", "id": "Penurunan Bandwidth Pada Arus Kolektor Tinggi." },
  { "en": "Apa Itu Pembebanan Sumber (Source Loading)?", "id": "Efek Impedansi Sumber Mengurangi Penguatan." },
  { "en": "Apa Itu Pembebanan Output (Output Loading)?", "id": "Efek Impedansi Beban Mengurangi Penguatan." },
  { "en": "Apa Itu Model Ebers-Moll?", "id": "Model Fisik Sederhana Untuk Transistor BJT." },
  { "en": "Apa Itu Daerah Operasi Linear?", "id": "Daerah Di Mana Penguat Berfungsi Normal." },
  { "en": "Apa Itu Daerah Cut-Off?", "id": "Daerah Di Mana Transistor Tidak Menghantar." },
  { "en": "Apa Itu Daerah Saturasi?", "id": "Daerah Di Mana Transistor Menghantar Sepenuhnya." },
  { "en": "Apa Itu Titik Intersep (Intercept Point)?", "id": "Ukuran Kinerja Linearitas Penguat Atau Mixer." },
  { "en": "Apa Itu Penguatan Konversi Mixer?", "id": "Rasio Daya Output IF Terhadap Input RF." },
  { "en": "Apa Itu Angka Derau (Noise Figure) Mixer?", "id": "Ukuran Tambahan Derau Yang Dihasilkan Mixer." },
  { "en": "Apa Itu Sintesis Filter Aktif?", "id": "Proses Mendesain Filter Dari Fungsi Transfer." },
  { "en": "Apa Itu Faktor Kualitas (Q) Kapasitor?", "id": "Ukuran Seberapa Ideal Kapasitor Tersebut." },
  { "en": "Apa Penyebab Faktor Q Kapasitor Terbatas?", "id": "Resistansi Seri Ekuivalen (ESR)." },
  { "en": "Apa Itu Faktor Kualitas (Q) Induktor?", "id": "Ukuran Seberapa Ideal Induktor Tersebut." },
  { "en": "Apa Penyebab Faktor Q Induktor Terbatas?", "id": "Resistansi DC Kawat Dan Rugi Inti." },
  { "en": "Apa Itu Filter Mikrostrip (Microstrip)?", "id": "Filter Dibuat Pada PCB Menggunakan Jalur Transmisi." },
  { "en": "Apa Itu Filter Stripline?", "id": "Filter Mirip Mikrostrip Dengan Lapisan Ground Tambahan." },
  { "en": "Apa Keuntungan Stripline Dibanding Mikrostrip?", "id": "Isolasi Yang Lebih Baik Dan Radiasi Rendah." },
  { "en": "Apa Itu Equalizer Parametrik?", "id": "Equalizer Dengan Frekuensi, Gain, Q Dapat Diatur." },
  { "en": "Apa Itu Equalizer Grafis?", "id": "Equalizer Dengan Frekuensi Tetap Dan Gain Diatur." },
  { "en": "Apa Itu Normalisasi Filter?", "id": "Proses Mendesain Filter Dengan Nilai Referensi." },
  { "en": "Apa Itu Denormalisasi Filter?", "id": "Mengubah Skala Filter Ternormalisasi Ke Nilai Praktis." },
  { "en": "Apa Itu Metode Sintesis Butterworth?", "id": "Prosedur Matematis Untuk Menghitung Nilai Komponen." },
  { "en": "Apa Itu Metode Sintesis Chebyshev?", "id": "Prosedur Matematis Untuk Desain Filter Chebyshev." },
  { "en": "Apa Itu Pole Damping?", "id": "Bagian Riil Dari Lokasi Pole." },
  { "en": "Apa Itu Pole Frekuensi?", "id": "Bagian Imajiner Dari Lokasi Pole." },
  { "en": "Apa Itu Resonansi Spurious?", "id": "Resonansi Tambahan Yang Tidak Diinginkan." },
  { "en": "Apa Penyebab Resonansi Spurious Kapasitor?", "id": "Induktansi Seri Ekuivalen (ESL)." },
  { "en": "Apa Penyebab Resonansi Spurious Induktor?", "id": "Kapasitansi Paralel Ekuivalen (EPC)." },
  { "en": "Apa Itu Frekuensi Resonansi Diri (SRF)?", "id": "Frekuensi Di Mana Komponen Mulai Berperilaku Berbeda." },
  { "en": "Bagaimana Osilator Kuadratur Diimplementasikan?", "id": "Dua Integrator Dihubungkan Dalam Loop." },
  { "en": "Apa Itu Osilator Opto-Elektronik (OEO)?", "id": "Osilator Stabilitas Ultra-Tinggi Menggunakan Serat Optik." },
  { "en": "Apa Keuntungan Utama OEO?", "id": "Derau Fasa Sangat Rendah Pada Frekuensi Microwave." },
  { "en": "Apa Itu Time-to-Digital Converter (TDC)?", "id": "Sirkuit Mengukur Interval Waktu Sangat Pendek." },
  { "en": "Di Mana TDC Digunakan?", "id": "ADPLL, Pencacah Frekuensi, Dan Eksperimen Fisika." },
  { "en": "Apa Sumber Utama Spur Dalam DDS?", "id": "Pemotongan Fasa Dan Error Amplitudo DAC." },
  { "en": "Apa Itu Clock Jitter Cleaner?", "id": "IC PLL Untuk Mengurangi Jitter Sinyal Clock." },
  { "en": "Apa Itu Osilator Butler?", "id": "Jenis Osilator Kristal Dengan Stabilitas Baik." },
  { "en": "Apa Itu Osilator Pierce?", "id": "Jenis Osilator Kristal Paling Umum." },
  { "en": "Apa Itu Pull Range Kristal?", "id": "Rentang Frekuensi Kristal Dapat Ditarik." },
  { "en": "Apa Itu Trimmer Capacitor?", "id": "Kapasitor Variabel Kecil Untuk Penyetelan Halus." },
  { "en": "Apa Itu Gated Oscillator?", "id": "Osilator Yang Dapat Dinyalakan Dan Dimatikan." },
  { "en": "Apa Itu Osilator Royer?", "id": "Konverter DC-AC Resonansi Sederhana." },
  { "en": "Apa Itu Penstabilan Frekuensi?", "id": "Teknik Mengunci Frekuensi Osilator Ke Referensi." },
  { "en": "Apa Itu Standar Frekuensi Primer?", "id": "Standar Berbasis Sifat Fisika Fundamental." },
  { "en": "Apa Itu Standar Frekuensi Sekunder?", "id": "Standar Dikalibrasi Terhadap Standar Primer." },
  { "en": "Apa Itu Diseminasi Waktu Dan Frekuensi?", "id": "Proses Mendistribusikan Sinyal Referensi Akurat." },
  { "en": "Sinyal Apa Yang Digunakan Untuk Diseminasi?", "id": "Sinyal Radio Gelombang Panjang Dan Sinyal GPS." },
  { "en": "Apa Itu Teknik Grounding Bintang (Star Ground)?", "id": "Menghubungkan Semua Ground Ke Satu Titik Pusat." },
  { "en": "Apa Tujuan Grounding Bintang?", "id": "Menghindari Ground Loop Dan Derau." },
  { "en": "Apa Itu Ground Plane?", "id": "Lapisan Tembaga Luas Pada PCB." },
  { "en": "Apa Fungsi Ground Plane?", "id": "Menyediakan Jalur Balik Impedansi Rendah." },
  { "en": "Apa Perbedaan Bypass Dan Decoupling Kapasitor?", "id": "Tujuan Sama, Perspektif Desain Sedikit Berbeda." },
  { "en": "Apa Fungsi Kapasitor Bypass?", "id": "Menyediakan Jalur Pintas Untuk Derau AC." },
  { "en": "Apa Fungsi Kapasitor Decoupling?", "id": "Mengisolasi Sirkuit Dari Derau Catu Daya." },
  { "en": "Apa Itu Safe Operating Area (SOA) MOSFET?", "id": "Grafik Batas Aman Tegangan Dan Arus." },
  { "en": "Apa Saja Batasan Dalam Kurva SOA?", "id": "Arus Maksimum, Tegangan Tembus, Disipasi Daya." },
  { "en": "Apa Itu Secondary Breakdown?", "id": "Kegagalan Transistor Akibat Hot Spot Lokal." },
  { "en": "Apa Itu Rangkaian Gate Driver?", "id": "Rangkaian Menggerakkan Gerbang MOSFET Secara Efisien." },
  { "en": "Mengapa MOSFET Membutuhkan Gate Driver Khusus?", "id": "Karena Kapasitansi Gerbangnya Cukup Besar." },
  { "en": "Apa Itu Shoot-Through Dalam Rangkaian Half-Bridge?", "id": "Kondisi Kedua MOSFET Menyala Bersamaan." },
  { "en": "Apa Akibat Dari Shoot-Through?", "id": "Hubung Singkat Catu Daya Dan Kerusakan." },
  { "en": "Apa Itu Dead Time Control?", "id": "Mencegah Shoot-Through Dengan Waktu Jeda." },
  { "en": "Apa Itu Interleaving Dalam SMPS?", "id": "Nama Lain Untuk Operasi Multi-Fasa." },
  { "en": "Apa Itu Kapasitor Dielektrik Kelas I?", "id": "Kapasitor Keramik Stabil Suhu (Contoh: C0G)." },
  { "en": "Apa Itu Kapasitor Dielektrik Kelas II?", "id": "Kapasitor Keramik Efisiensi Tinggi (Contoh: X7R)." },
  { "en": "Apa Kelemahan Kapasitor Kelas II?", "id": "Kapasitansi Berubah Dengan Suhu Dan Tegangan." },
  { "en": "Apa Itu Induktor Toroidal?", "id": "Induktor Dengan Inti Berbentuk Donat (Torus)." },
  { "en": "Apa Keuntungan Induktor Toroidal?", "id": "Medan Magnet Terkurung Di Dalam Inti." },
  { "en": "Apa Itu Ferrite Bead?", "id": "Komponen Pasif Menekan Derau Frekuensi Tinggi." },
  { "en": "Bagaimana Ferrite Bead Bekerja?", "id": "Menjadi Sangat Lossy Pada Frekuensi Tinggi." },
  { "en": "Apa Itu Waktu Tunak (Settling Time) Op-Amp?", "id": "Waktu Output Masuk Dalam Batas Error Akhir." },
  { "en": "Apa Satuan Yang Digunakan Untuk Settling Time?", "id": "Nanodetik (ns) Atau Mikrodetik (µs)." },
  { "en": "Apa Itu Feedthrough Kapasitif?", "id": "Bocornya Sinyal Input Ke Output." },
  { "en": "Apa Itu Temperatur Derau (Noise Temperature)?", "id": "Ukuran Derau Dinyatakan Dalam Suhu Ekuivalen." },
  { "en": "Bagaimana Hubungan Temperatur Derau Dan Angka Derau?", "id": "Keduanya Adalah Ukuran Kinerja Derau." },
  { "en": "Apa Itu Op-Amp Umpan Balik Arus (CFA)?", "id": "Jenis Op-Amp Dengan Bandwidth Sangat Lebar." },
  { "en": "Apa Perbedaan Internal Utama CFA?", "id": "Memiliki Input Inverting Impedansi Rendah." },
  { "en": "Apakah Bandwidth CFA Bergantung Pada Penguatan?", "id": "Tidak, Bandwidth Hampir Konstan." },
  { "en": "Apa Itu Rangkaian Startup Referensi Bandgap?", "id": "Memastikan Rangkaian Mulai Bekerja Dengan Benar." },
  { "en": "Apa Itu Penguat Komposit?", "id": "Menggabungkan Dua Op-Amp Untuk Kinerja Lebih Baik." },
  { "en": "Apa Contoh Penguat Komposit?", "id": "Menggabungkan Op-Amp Presisi Dengan Kecepatan Tinggi." },
  { "en": "Apa Itu Penguat Jembatan Wheatstone?", "id": "Penguat Diferensial Untuk Sensor Jembatan." },
  { "en": "Apa Itu Linearitas Integral (Integral Linearity)?", "id": "Ukuran Seberapa Lurus Fungsi Transfer Penguat." },
  { "en": "Apa Itu Linearitas Diferensial (Differential Linearity)?", "id": "Ukuran Keseragaman Langkah Kuantisasi." },
  { "en": "Apa Itu Step Recovery Amplifier?", "id": "Digunakan Dalam Pengali Frekuensi." },
  { "en": "Apa Itu Dioda Step Recovery?", "id": "Dioda Dengan Efek Pemulihan Balik Tajam." },
  { "en": "Apa Itu Efek Kedip (Flicker Effect)?", "id": "Nama Lain Untuk Derau 1/f." },
  { "en": "Apa Itu Pojok Derau (Noise Corner)?", "id": "Frekuensi Di Mana Derau 1/f Sama." },
  { "en": "Apa Itu Penguat Parametrik Josephson?", "id": "Penguat Kuantum Dengan Derau Sangat Rendah." },
  { "en": "Apa Itu Amplifier Maser?", "id": "Penguat Microwave Berbasis Emisi Terstimulasi." },
  { "en": "Apa Itu Penguat Optik?", "id": "Menguatkan Sinyal Cahaya Tanpa Konversi Listrik." },
  { "en": "Apa Contoh Penguat Optik?", "id": "Erbium-Doped Fiber Amplifier (EDFA)." },
  { "en": "Apa Itu Penguat Raman?", "id": "Jenis Lain Penguat Serat Optik." },
  { "en": "Apa Itu Rangkaian Translinear?", "id": "Rangkaian Yang Memanfaatkan Hubungan Eksponensial BJT." },
  { "en": "Apa Prinsip Translinear?", "id": "Produk Arus Searah Jarum Jam Sama." },
  { "en": "Apa Itu Topologi Filter Twin-T?", "id": "Filter Notch Pasif Hanya Menggunakan RC." },
  { "en": "Apa Kesulitan Implementasi Filter Twin-T?", "id": "Sangat Sensitif Terhadap Nilai Komponen." },
  { "en": "Bagaimana Cara Membuat Filter Twin-T Aktif?", "id": "Dengan Menambahkan Penyangga Op-Amp." },
  { "en": "Apa Fungsi Kapasitor Feedforward?", "id": "Meningkatkan Stabilitas Fasa Penguat Umpan Balik." },
  { "en": "Apa Itu Sintesis Filter Menggunakan OTA?", "id": "Membuat Filter Aktif Dengan Penguat Transkonduktansi." },
  { "en": "Apa Keuntungan Filter Berbasis OTA?", "id": "Frekuensi Cutoff Dapat Diatur Dengan Arus." },
  { "en": "Apa Itu Jaringan Ladder?", "id": "Topologi Filter Dengan Komponen Seri Shunt." },
  { "en": "Apa Sifat Jaringan Ladder Pasif?", "id": "Memiliki Sensitivitas Yang Relatif Rendah." },
  { "en": "Apa Itu Filter Elips Kuadratur Cermin (QMF)?", "id": "Jenis Bank Filter Untuk Pemrosesan Sinyal." },
  { "en": "Apa Itu Ripple Stopband?", "id": "Variasi Atenuasi Di Dalam Daerah Stopband." },
  { "en": "Filter Mana Yang Punya Ripple Stopband?", "id": "Filter Eliptik (Cauer)." },
  { "en": "Apa Itu Atenuasi Ultimatum (Ultimate Attenuation)?", "id": "Tingkat Atenuasi Maksimum Di Stopband Jauh." },
  { "en": "Apa Itu Pole Riil?", "id": "Pole Yang Terletak Di Sumbu Riil." },
  { "en": "Apa Itu Pasangan Pole Kompleks Konjugat?", "id": "Pole Dengan Bagian Riil Imajiner." },
  { "en": "Apa Yang Terjadi Jika Pole Di Sumbu Imajiner?", "id": "Sistem Akan Berosilasi Tanpa Henti." },
  { "en": "Apa Itu Filter Digital CIC (Cascaded Integrator-Comb)?", "id": "Filter Efisien Untuk Interpolasi Dekimasi." },
  { "en": "Apa Itu Jaringan Penunda Lattice?", "id": "Struktur Filter All-Pass Digital." },
  { "en": "Apa Itu Dithering Dalam Konverter Data?", "id": "Menambahkan Derau Untuk Meningkatkan Kinerja." },
  { "en": "Apa Itu Proses Manufaktur Wafer?", "id": "Proses Membuat Sirkuit Terpadu Di Atas Wafer." },
  { "en": "Apa Itu Pengetesan Wafer (Wafer Probing)?", "id": "Menguji Chip Saat Masih Di Wafer." },
  { "en": "Apa Itu Osilator Harmonik?", "id": "Osilator Yang Menghasilkan Bentuk Gelombang Sinusoidal." },
  { "en": "Apa Itu Osilator Relaksasi?", "id": "Osilator Yang Menghasilkan Bentuk Gelombang Non-Sinusoidal." },
  { "en": "Apa Itu Detektor Fasa-Frekuensi (PFD)?", "id": "Sirkuit Digital Membandingkan Tepi Naik Sinyal." },
  { "en": "Apa Itu Keadaan Tiga (Tri-State) Output PFD?", "id": "Up, Down, Dan Impedansi Tinggi." },
  { "en": "Apa Fungsi Pompa Muatan (Charge Pump) di PLL?", "id": "Mengubah Sinyal PFD Menjadi Arus." },
  { "en": "Apa Itu Detektor Fasa Tipe I?", "id": "Mixer Analog Atau Gerbang XOR." },
  { "en": "Apa Kelemahan Detektor Fasa Tipe I?", "id": "Rentang Tangkap Terbatas Dan Sensitif Duty Cycle." },
  { "en": "Apa Itu Deteksi Kunci (Lock Detection) PLL?", "id": "Sirkuit Menunjukkan Kapan PLL Telah Terkunci." },
  { "en": "Apa Itu Osilator Vackar?", "id": "Varian Osilator Clapp Dengan Amplitudo Stabil." },
  { "en": "Apa Itu Osilator Seiler?", "id": "Varian Lain Osilator Clapp." },
  { "en": "Apa Itu Kalibrasi VCO?", "id": "Proses Memilih Pita Frekuensi VCO." },
  { "en": "Apa Itu Bandwidth Loop PLL?", "id": "Menentukan Seberapa Cepat PLL Merespon Perubahan." },
  { "en": "Bagaimana Bandwidth Loop Mempengaruhi Derau Fasa?", "id": "Loop Sempit Menyaring Derau Referensi." },
  { "en": "Bagaimana Bandwidth Loop Mempengaruhi Waktu Kunci?", "id": "Loop Lebar Mengunci Lebih Cepat." },
  { "en": "Apa Itu Damping Factor PLL?", "id": "Menentukan Respon Transien Loop." },
  { "en": "Apa Itu Overshoot Fasa?", "id": "Loop Melebihi Fasa Akhir Sebelum Stabil." },
  { "en": "Apa Itu Siklus Batas (Limit Cycle) PLL?", "id": "Osilasi Kecil Yang Tidak Diinginkan." },
  { "en": "Apa Itu Getaran (Jitter) Referensi?", "id": "Jitter Yang Berasal Dari Sinyal Referensi." },
  { "en": "Apa Itu Getaran (Jitter) VCO?", "id": "Jitter Yang Dihasilkan Oleh VCO Sendiri." },
  { "en": "Apa Itu Zero Voltage Switching (ZVS)?", "id": "Menyalakan Saklar Saat Tegangan Nol." },
  { "en": "Apa Itu Zero Current Switching (ZCS)?", "id": "Menyalakan Atau Mematikan Saklar Saat Arus Nol." },
  { "en": "Apa Keuntungan Konverter Resonansi?", "id": "Rugi-Rugi Pensaklaran Sangat Rendah." },
  { "en": "Apa Itu Rangkaian Snubber Aktif?", "id": "Snubber Menggunakan Komponen Aktif Untuk Efisiensi." },
  { "en": "Apa Itu Pelindung (Shielding) EMI?", "id": "Menggunakan Kandang Konduktif Untuk Memblokir EMI." },
  { "en": "Bagaimana Layout PCB Mengurangi EMI?", "id": "Meminimalkan Area Loop Arus Kecepatan Tinggi." },
  { "en": "Apa Itu Ground Stitching Vias?", "id": "Via Menghubungkan Lapisan Ground Untuk Mengurangi Induktansi." },
  { "en": "Apa Itu Konverter Buck Sinkron?", "id": "Konverter Buck Menggunakan MOSFET." },
  { "en": "Apa Fungsi Dioda Badan (Body Diode) MOSFET?", "id": "Menghantar Selama Waktu Mati (Dead Time)." },
  { "en": "Apa Masalah Dengan Dioda Badan?", "id": "Pemulihan Balik Lambat Menyebabkan Rugi-Rugi." },
  { "en": "Apa Itu Efek Miller Pada MOSFET?", "id": "Peningkatan Kapasitansi Gerbang Selama Switching." },
  { "en": "Apa Itu Plateau Miller?", "id": "Daerah Datar Pada Kurva Tegangan Gerbang." },
  { "en": "Apa Itu Arus Avalanche?", "id": "Arus Yang Mengalir Saat Tegangan Tembus." },
  { "en": "Apa Itu Kemampuan Energi Avalanche?", "id": "Kemampuan MOSFET Menahan Energi Saat Tembus." },
  { "en": "Apa Itu SOA Terbatas Termal?", "id": "Batas Operasi Akibat Pemanasan Perangkat." },
  { "en": "Apa Itu SOA Terbatas Arus?", "id": "Batas Operasi Akibat Arus Maksimum." },
  { "en": "Apa Itu Efek JFET Parasitik?", "id": "Terbentuknya Kanal JFET Dalam MOSFET Lateral." },
  { "en": "Apa Itu Boot-Strap Power Supply?", "id": "Catu Daya Mengambang Untuk Menggerakkan High-Side." },
  { "en": "Apa Itu Under-Voltage Lockout (UVLO)?", "id": "Mematikan Sirkuit Jika Catu Daya Terlalu Rendah." },
  { "en": "Mengapa UVLO Penting Untuk MOSFET Driver?", "id": "Mencegah Operasi Di Daerah Resistansi Tinggi." },
  { "en": "Apa Itu Konverter Interleaved?", "id": "Nama Lain Untuk Konverter Multi-Fasa." },
  { "en": "Apa Itu Ripple Steering?", "id": "Pembatalan Riak Arus Dalam Konverter Interleaved." },
  { "en": "Apa Itu Arus Sirkulasi?", "id": "Arus Yang Mengalir Antar Konverter Paralel." },
  { "en": "Apa Itu Arsitektur Penerima Superheterodyne?", "id": "Mengubah Frekuensi RF Menjadi Frekuensi IF Tetap." },
  { "en": "Apa Itu Penguat Frekuensi Menengah (IF Amplifier)?", "id": "Penguat Gain Tinggi Pada Frekuensi IF Tetap." },
  { "en": "Apa Keuntungan Arsitektur Superheterodyne?", "id": "Gain Dan Selektivitas Tinggi Mudah Dicapai." },
  { "en": "Apa Itu Pencampur (Mixer)?", "id": "Rangkaian Non-Linear Yang Menghasilkan Frekuensi Baru." },
  { "en": "Apa Dua Output Utama Dari Mixer?", "id": "Frekuensi Jumlah Dan Frekuensi Selisih." },
  { "en": "Frekuensi Mana Yang Dipilih Sebagai IF?", "id": "Biasanya Frekuensi Selisih Antara RF Dan LO." },
  { "en": "Apa Itu Penolakan Frekuensi Bayangan (Image Rejection)?", "id": "Kemampuan Menerima Untuk Menolak Frekuensi Bayangan." },
  { "en": "Apa Itu Automatic Level Control (ALC)?", "id": "Menjaga Daya Output Pemancar Tetap Konstan." },
  { "en": "Apa Perbedaan Pencocokan Derau Dan Daya?", "id": "Pencocokan Derau Meminimalkan Noise Figure." },
  { "en": "Apa Itu Penguat Kelas AB?", "id": "Kompromi Antara Linearitas Kelas A Efisiensi B." },
  { "en": "Apa Itu Sudut Konduksi?", "id": "Bagian Dari Siklus Sinyal Transistor Menghantar." },
  { "en": "Berapa Sudut Konduksi Penguat Kelas A?", "id": "Tiga Ratus Enam Puluh Derajat (360°)." },
  { "en": "Berapa Sudut Konduksi Penguat Kelas B?", "id": "Seratus Delapan Puluh Derajat (180°)." },
  { "en": "Berapa Sudut Konduksi Penguat Kelas C?", "id": "Kurang Dari Seratus Delapan Puluh Derajat." },
  { "en": "Apa Itu Penguat Jembatan Penuh (Full-Bridge)?", "id": "Nama Lain Untuk Konfigurasi BTL." },
  { "en": "Apa Itu Penguat Terkopel Transformer?", "id": "Menggunakan Transformer Untuk Kopling Antar Tingkat." },
  { "en": "Apa Itu Penguat Video?", "id": "Penguat Dengan Bandwidth Lebar." },
  { "en": "Apa Itu Penguat Audio?", "id": "Penguat Untuk Sinyal Dalam Rentang Pendengaran." },
  { "en": "Apa Itu Penguat Operasional Presisi?", "id": "Op-Amp Dengan Tegangan Offset Sangat Rendah." },
  { "en": "Apa Itu Proses Trimming Laser?", "id": "Menyesuaikan Resistor Internal Chip Untuk Akurasi." },
  { "en": "Apa Itu Dioda PIN?", "id": "Dioda Dengan Lapisan Intrinsik Untuk Aplikasi RF." },
  { "en": "Apa Kegunaan Dioda PIN?", "id": "Sebagai Saklar RF Dan Attenuator Variabel." },
  { "en": "Apa Itu Penguat Logaritmik Deteksi?", "id": "Mengukur Kekuatan Sinyal RF Dalam Skala Logaritmik." },
  { "en": "Apa Satuan Output Penguat Logaritmik?", "id": "Desibel (dB) Atau Desibel-milliwatt (dBm)." },
  { "en": "Apa Itu Penguat Terbatas (Limiting Amplifier)?", "id": "Menghasilkan Amplitudo Output Konstan." },
  { "en": "Apa Itu Filter Surface Acoustic Wave (SAW)?", "id": "Filter Kompak Untuk Aplikasi Frekuensi Radio." },
  { "en": "Bagaimana Filter SAW Bekerja?", "id": "Mengubah Sinyal Listrik Menjadi Gelombang Akustik." },
  { "en": "Apa Itu Filter Bulk Acoustic Wave (BAW)?", "id": "Filter Kinerja Lebih Tinggi Dari SAW." },
  { "en": "Apa Keuntungan Filter BAW?", "id": "Ukuran Kecil, Rugi Rendah, Dan Penanganan Daya." },
  { "en": "Di Mana Filter BAW Dan SAW Digunakan?", "id": "Ponsel Pintar Dan Perangkat Nirkabel Lainnya." },
  { "en": "Apa Itu Aliasing?", "id": "Efek Sinyal Frekuensi Tinggi Muncul Rendah." },
  { "en": "Bagaimana Cara Mencegah Aliasing?", "id": "Menggunakan Filter Anti-Aliasing Sebelum Sampling." },
  { "en": "Apa Itu Teorema Sampling Nyquist?", "id": "Frekuensi Sampling Harus Dua Kali Bandwidth." },
  { "en": "Apa Itu Filter Notch Twin-T?", "id": "Filter Pasif Untuk Menolak Satu Frekuensi." },
  { "en": "Apa Itu Filter Aktif Universal?", "id": "IC Filter Yang Responnya Dapat Diprogram." },
  { "en": "Apa Itu Transformasi Butterworth?", "id": "Metode Matematis Untuk Desain Filter Butterworth." },
  { "en": "Apa Itu Transformasi Chebyshev?", "id": "Metode Matematis Untuk Desain Filter Chebyshev." },
  { "en": "Apa Itu Spektrum Sinyal?", "id": "Representasi Sinyal Dalam Domain Frekuensi." },
  { "en": "Apa Itu Analisis Fourier?", "id": "Menguraikan Sinyal Menjadi Komponen Sinusoidal." },
  { "en": "Apa Itu Konvolusi?", "id": "Operasi Matematis Terkait Respon Filter." },
  { "en": "Apa Teorema Konvolusi?", "id": "Konvolusi Domain Waktu Sama Dengan Perkalian Frekuensi." },
  { "en": "Apa Itu Fungsi Transfer?", "id": "Rasio Output Terhadap Input Domain Frekuensi." },
  { "en": "Apa Itu Diagram Kutub-Nol (Pole-Zero Plot)?", "id": "Visualisasi Fungsi Transfer Di Bidang Kompleks." },
  { "en": "Bagaimana Kutub Dan Nol Mempengaruhi Respon?", "id": "Kutub Menciptakan Puncak, Nol Menciptakan Lembah." },
  { "en": "Apa Itu Filter Resonansi Keramik?", "id": "Filter Menggunakan Resonator Keramik Piezoelektrik." },
  { "en": "Apa Itu Peran Local Oscillator (LO) Di SDR?", "id": "Menentukan Frekuensi Penerimaan Atau Transmisi." },
  { "en": "Apa Itu Software-Defined Radio (SDR)?", "id": "Sistem Radio Di Mana Pemrosesan Dilakukan Digital." },
  { "en": "Apa Itu Arsitektur Zero-IF (Direct Conversion)?", "id": "Langsung Mengubah Sinyal RF Ke Baseband." },
  { "en": "Apa Masalah Utama Arsitektur Zero-IF?", "id": "Kebocoran LO Dan Offset DC." },
  { "en": "Apa Itu Arsitektur Low-IF?", "id": "Kompromi Antara Superheterodyne Dan Zero-IF." },
  { "en": "Apa Dampak Derau Fasa LO?", "id": "Menyebabkan Reciprocal Mixing Dan Degradasi SNR." },
  { "en": "Apa Itu Frekuensi Clock?", "id": "Sinyal Periodik Untuk Sinkronisasi Sirkuit Digital." },
  { "en": "Apa Itu Clock Tree Synthesis?", "id": "Proses Mendesain Jaringan Distribusi Clock." },
  { "en": "Apa Itu Clock Skew?", "id": "Perbedaan Waktu Tiba Sinyal Clock." },
  { "en": "Apa Itu Clock Jitter?", "id": "Variasi Waktu Tepi Sinyal Clock." },
  { "en": "Apa Itu Osilator Terkontrol Tegangan Digital (DCO)?", "id": "Osilator Dengan Frekuensi Diatur Secara Digital." },
  { "en": "Apa Itu Flying Capacitor?", "id": "Kapasitor Digunakan Untuk Transfer Muatan." },
  { "en": "Di Mana Flying Capacitor Digunakan?", "id": "Pompa Muatan Dan Konverter Kapasitif Switched." },
  { "en": "Apa Itu Osilator Relaksasi Thyristor?", "id": "Osilator Menggunakan SCR Atau UJT." },
  { "en": "Apa Itu Generator Gelombang Gigi Gergaji (Sawtooth)?", "id": "Osilator Menghasilkan Sinyal Ramp Linear." },
  { "en": "Di Mana Gelombang Gigi Gergaji Digunakan?", "id": "Sirkuit Sapuan (Sweep) Osiloskop Dan Synthesizer." },
  { "en": "Apa Itu Generator Gelombang Segitiga?", "id": "Osilator Menghasilkan Sinyal Naik Turun Linear." },
  { "en": "Bagaimana Cara Membuat Gelombang Segitiga?", "id": "Mengintegrasikan Sinyal Gelombang Kotak." },
  { "en": "Apa Itu Kontrol Loop Digital?", "id": "Mengimplementasikan Loop Kontrol Catu Daya Digital." },
  { "en": "Apa Keuntungan Kontrol Loop Digital?", "id": "Fleksibilitas, Dapat Diprogram, Dan Adaptif." },
  { "en": "Apa Itu Respon Transien Beban?", "id": "Bagaimana Output Catu Daya Merespon Perubahan Beban." },
  { "en": "Apa Itu Droop Tegangan?", "id": "Penurunan Tegangan Sesaat Saat Beban Naik." },
  { "en": "Apa Itu Overshoot Tegangan?", "id": "Kenaikan Tegangan Sesaat Saat Beban Turun." },
  { "en": "Apa Itu Perlindungan Electrostatic Discharge (ESD)?", "id": "Melindungi Sirkuit Dari Listrik Statis." },
  { "en": "Apa Model Tubuh Manusia (Human Body Model - HBM)?", "id": "Standar Pengujian Ketahanan ESD." },
  { "en": "Apa Itu Laju Kegagalan (Failure In Time - FIT)?", "id": "Jumlah Kegagalan Per Miliar Jam Operasi." },
  { "en": "Apa Itu Mean Time Between Failures (MTBF)?", "id": "Waktu Rata-Rata Antara Kegagalan." },
  { "en": "Apa Hubungan MTBF Dan FIT?", "id": "Keduanya Adalah Ukuran Keandalan (Reliability)." },
  { "en": "Apa Itu Analisis Pohon Kegagalan (Fault Tree Analysis)?", "id": "Metode Analisis Keandalan Sistem." },
  { "en": "Apa Itu Mode Kegagalan Dan Analisis Efek (FMEA)?", "id": "Metode Lain Untuk Menganalisis Keandalan." },
  { "en": "Apa Itu Tegangan Tembus Sekunder (Secondary Breakdown)?", "id": "Kegagalan Transistor BJT Akibat Hot Spot." },
  { "en": "Apa Itu Efek Kirk (Kirk Effect)?", "id": "Perluasan Basis Yang Membatasi Kinerja BJT." },
  { "en": "Apa Itu Proses Manufaktur CMOS?", "id": "Proses Membuat Transistor PMOS Dan NMOS." },
  { "en": "Apa Itu Proses Manufaktur Bipolar?", "id": "Proses Membuat Transistor BJT." },
  { "en": "Apa Itu Proses BiCMOS?", "id": "Menggabungkan Teknologi Bipolar Dan CMOS." },
  { "en": "Apa Keuntungan BiCMOS?", "id": "Menggabungkan Kecepatan Bipolar Dengan Kepadatan CMOS." },
  { "en": "Apa Itu Kapasitor Parit (Trench Capacitor)?", "id": "Kapasitor Dibuat Dalam Parit Di Silikon." },
  { "en": "Apa Keuntungan Kapasitor Parit?", "id": "Kepadatan Kapasitansi Sangat Tinggi." },
  { "en": "Apa Itu Kapasitor Polisilikon?", "id": "Kapasitor Dibuat Menggunakan Lapisan Polisilikon." },
  { "en": "Apa Itu Resistor Film Tipis?", "id": "Resistor Presisi Dibuat Dalam Sirkuit Terpadu." },
  { "en": "Apa Itu Efek Hall?", "id": "Tegangan Timbul Melintasi Konduktor." },
  { "en": "Apa Aplikasi Sensor Efek Hall?", "id": "Penginderaan Arus Dan Deteksi Posisi Magnetik." },
  { "en": "Apa Itu Sensor Magnetoresistif?", "id": "Sensor Yang Resistansinya Berubah Dengan Medan Magnet." },
  { "en": "Apa Itu Dioda Terobosan (Tunnel Diode)?", "id": "Dioda Dengan Efek Terobosan Kuantum." },
  { "en": "Apa Sifat Unik Dioda Terobosan?", "id": "Memiliki Daerah Resistansi Diferensial Negatif." },
  { "en": "Apa Itu Analog-to-Digital Converter (ADC)?", "id": "Mengubah Sinyal Analog Menjadi Nilai Digital." },
  { "en": "Apa Itu Digital-to-Analog Converter (DAC)?", "id": "Mengubah Nilai Digital Menjadi Sinyal Analog." },
  { "en": "Apa Itu Resolusi Konverter Data?", "id": "Jumlah Bit Yang Digunakan Merepresentasikan Sinyal." },
  { "en": "Apa Itu Laju Sampling (Sample Rate)?", "id": "Seberapa Sering Sinyal Analog Diukur." },
  { "en": "Apa Itu Arsitektur Flash ADC?", "id": "ADC Paralel Kecepatan Sangat Tinggi." },
  { "en": "Apa Komponen Utama Flash ADC?", "id": "Jaringan Resistor Dan Tumpukan Komparator." },
  { "en": "Apa Kekurangan Utama Flash ADC?", "id": "Membutuhkan Banyak Komparator Dan Konsumsi Daya Tinggi." },
  { "en": "Apa Itu Arsitektur Successive Approximation Register (SAR) ADC?", "id": "ADC Populer Dengan Keseimbangan Baik." },
  { "en": "Bagaimana Cara Kerja SAR ADC?", "id": "Menggunakan Pencarian Biner Untuk Menemukan Nilai Digital." },
  { "en": "Apa Itu Arsitektur Delta-Sigma ADC?", "id": "ADC Resolusi Sangat Tinggi Untuk Audio." },
  { "en": "Bagaimana Prinsip Kerja Delta-Sigma ADC?", "id": "Menggunakan Oversampling Dan Noise Shaping." },
  { "en": "Apa Itu Rangkaian Sample-and-Hold (S/H)?", "id": "Menangkap Dan Menahan Tegangan Sinyal Analog." },
  { "en": "Mengapa ADC Membutuhkan Rangkaian Sample-and-Hold?", "id": "Menjaga Sinyal Stabil Selama Proses Konversi." },
  { "en": "Apa Itu Arsitektur R-2R Ladder DAC?", "id": "DAC Presisi Hanya Menggunakan Dua Nilai Resistor." },
  { "en": "Apa Itu Arsitektur String DAC?", "id": "DAC Menggunakan Jaringan Pembagi Tegangan Resistor." },
  { "en": "Apa Itu Glitch Pada Output DAC?", "id": "Lonjakan Energi Sesaat Saat Kode Berubah." },
  { "en": "Kapan Glitch DAC Paling Buruk Terjadi?", "id": "Saat Transisi Bit Paling Signifikan (MSB)." },
  { "en": "Apa Itu Kode Termometer?", "id": "Representasi Digital Untuk Mengurangi Glitch DAC." },
  { "en": "Apa Itu Monotonisitas DAC?", "id": "Output Selalu Naik Dengan Kenaikan Kode." },
  { "en": "Apa Itu Penguat Drive ADC?", "id": "Op-Amp Didesain Khusus Menggerakkan Input ADC." },
  { "en": "Apa Karakteristik Penting Penguat Drive ADC?", "id": "Waktu Tunak Cepat Dan Distorsi Rendah." },
  { "en": "Apa Itu Aperture Jitter?", "id": "Ketidakpastian Waktu Pada Momen Sampling." },
  { "en": "Apa Dampak Aperture Jitter?", "id": "Membatasi Sinyal-ke-Derau Rasio (SNR) Frekuensi Tinggi." },
  { "en": "Apa Itu Kuantisasi?", "id": "Proses Memetakan Nilai Kontinu Ke Diskrit." },
  { "en": "Apa Itu Derau Kuantisasi?", "id": "Error Yang Diperkenalkan Oleh Proses Kuantisasi." },
  { "en": "Apa Peran Filter Anti-Aliasing?", "id": "Membatasi Bandwidth Sinyal Sebelum Sampling." },
  { "en": "Filter Jenis Apa Yang Digunakan Untuk Anti-Aliasing?", "id": "Filter Low-Pass Dengan Roll-Off Curam." },
  { "en": "Apa Peran Filter Rekonstruksi?", "id": "Menghaluskan Output Tangga Dari DAC." },
  { "en": "Apa Nama Lain Filter Rekonstruksi?", "id": "Filter Anti-Imaging." },
  { "en": "Apa Itu Filter Kapasitor Switched (Switched-Capacitor)?", "id": "Filter Aktif Menggunakan Kapasitor Dan Saklar." },
  { "en": "Bagaimana Kapasitor Switched Mensimulasikan Resistor?", "id": "Dengan Mengisi Dan Mengosongkan Kapasitor." },
  { "en": "Apa Keuntungan Filter Kapasitor Switched?", "id": "Frekuensi Cutoff Tergantung Frekuensi Clock." },
  { "en": "Apa Itu Filter Digital?", "id": "Filter Diimplementasikan Dalam Perangkat Lunak." },
  { "en": "Apa Itu Prosesor Sinyal Digital (DSP)?", "id": "Mikroprosesor Khusus Untuk Pemrosesan Sinyal." },
  { "en": "Apa Itu Jaringan Crossover Aktif?", "id": "Filter Crossover Diimplementasikan Sebelum Penguat Daya." },
  { "en": "Apa Keuntungan Crossover Aktif?", "id": "Fleksibilitas Dan Interaksi Antar Speaker Rendah." },
  { "en": "Apa Itu Filter Fasa Linear?", "id": "Filter Dengan Penundaan Grup Konstan." },
  { "en": "Apa Itu Transformasi Hilbert?", "id": "Filter All-Pass Menggeser Fasa -90 Derajat." },
  { "en": "Apa Itu Single-Sideband (SSB) Modulation?", "id": "Modulasi Amplitudo Yang Menekan Satu Sisi Pita." },
  { "en": "Filter Apa Yang Digunakan Untuk SSB?", "id": "Filter Kristal Atau Mekanis Sangat Tajam." },
  { "en": "Apa Itu Vestigial Sideband (VSB)?", "id": "Modulasi Digunakan Dalam Siaran Televisi Analog." },
  { "en": "Apa Itu Filter Equalizer?", "id": "Filter Untuk Mengompensasi Distorsi Kanal." },
  { "en": "Apa Itu Decoupling Jaringan Catu Daya?", "id": "Menggunakan Kapasitor Untuk Mengisolasi Derau." },
  { "en": "Apa Aturan Praktis Penempatan Kapasitor Decoupling?", "id": "Tempatkan Sedekat Mungkin Dengan Pin IC." },
  { "en": "Apa Itu Osilator Cincin (Ring Oscillator)?", "id": "Osilator Terbuat Dari Rantai Inverter Ganjil." },
  { "en": "Di Mana Osilator Cincin Digunakan?", "id": "Generator Clock Internal Chip Dan Sensor Proses." },
  { "en": "Apa Itu Phase Interpolator?", "id": "Sirkuit Menghasilkan Fasa Di Antara Dua Sinyal." },
  { "en": "Apa Itu Osilator Kuadratur?", "id": "Osilator Menghasilkan Sinyal Sinus Dan Cosinus." },
  { "en": "Apa Kegunaan Osilator Kuadratur?", "id": "Modulator Dan Demodulator I/Q." },
  { "en": "Apa Itu Modulasi I/Q?", "id": "Modulasi Menggunakan Komponen In-Phase Dan Quadrature." },
  { "en": "Apa Itu Demodulator Sinkron?", "id": "Demodulator Menggunakan Local Oscillator." },
  { "en": "Apa Itu Detektor Koheren?", "id": "Nama Lain Untuk Demodulator Sinkron." },
  { "en": "Apa Itu Noise Figure Cascade?", "id": "Rumus Menghitung Total Noise Figure." },
  { "en": "Tingkat Mana Yang Paling Mempengaruhi Noise Figure?", "id": "Tingkat Penguat Paling Pertama (LNA)." },
  { "en": "Apa Itu IP3 Cascade?", "id": "Rumus Menghitung Total Linearitas." },
  { "en": "Apa Itu Konversi Harmonik?", "id": "Pencampuran Menggunakan Harmonik Dari Local Oscillator." },
  { "en": "Apa Itu Sub-Harmonic Mixer?", "id": "Mixer Yang Menggunakan Sub-Harmonik LO." },
  { "en": "Apa Itu Isolasi LO-ke-RF?", "id": "Ukuran Kebocoran Sinyal LO Ke Port RF." },
  { "en": "Apa Itu Isolasi RF-ke-IF?", "id": "Ukuran Kebocoran Sinyal RF Ke Port IF." },
  { "en": "Apa Itu Dynamic Voltage and Frequency Scaling (DVFS)?", "id": "Menyesuaikan Tegangan Frekuensi Sesuai Beban." },
  { "en": "Apa Tujuan DVFS?", "id": "Mengurangi Konsumsi Daya Prosesor." },
  { "en": "Apa Itu Adaptive Voltage Scaling (AVS)?", "id": "Menyesuaikan Tegangan Catu Daya Secara Adaptif." },
  { "en": "Apa Itu Power Islands (Voltage Islands)?", "id": "Area Chip Dengan Catu Daya Independen." },
  { "en": "Apa Itu Clock Gating?", "id": "Mematikan Sinyal Clock Ke Blok Logika." },
  { "en": "Apa Tujuan Clock Gating?", "id": "Mengurangi Konsumsi Daya Dinamis." },
  { "en": "Apa Itu Power Gating?", "id": "Mematikan Catu Daya Ke Blok Logika." },
  { "en": "Apa Tujuan Power Gating?", "id": "Mengurangi Konsumsi Daya Bocor (Leakage)." },
  { "en": "Apa Itu Inti Induktor Ferit?", "id": "Material Keramik Untuk Aplikasi Frekuensi Tinggi." },
  { "en": "Apa Itu Inti Induktor Serbuk Besi (Powdered Iron)?", "id": "Material Untuk Aplikasi Filter Arus DC." },
  { "en": "Apa Itu Rugi Histeresis Inti?", "id": "Energi Hilang Akibat Siklus Magnetisasi." },
  { "en": "Apa Itu Rugi Arus Eddy Inti?", "id": "Energi Hilang Akibat Arus Sirkulasi." },
  { "en": "Bagaimana Rugi Inti Bergantung Frekuensi?", "id": "Rugi Inti Meningkat Cepat Dengan Frekuensi." },
  { "en": "Apa Itu Efek Kulit (Skin Effect)?", "id": "Arus AC Mengalir Di Permukaan Konduktor." },
  { "en": "Apa Itu Efek Proksimitas (Proximity Effect)?", "id": "Arus Mengumpul Akibat Konduktor Berdekatan." },
  { "en": "Kawat Apa Yang Digunakan Mengurangi Efek Kulit?", "id": "Kawat Litz (Litz Wire)." },
  { "en": "Apa Itu Kawat Litz?", "id": "Kabel Terdiri Dari Banyak Untaian Terisolasi." },
  { "en": "Apa Itu Standar Keamanan IEC 62368?", "id": "Standar Keamanan Baru Untuk Peralatan AV IT." },
  { "en": "Apa Itu Hazard-Based Safety Engineering (HBSE)?", "id": "Pendekatan Keamanan Berbasis Identifikasi Bahaya." },
  { "en": "Apa Itu Isolasi Fungsional?", "id": "Isolasi Yang Hanya Diperlukan Untuk Fungsi." },
  { "en": "Apa Itu Isolasi Dasar?", "id": "Isolasi Tunggal Untuk Perlindungan Dari Sengatan." },
  { "en": "Apa Itu Isolasi Ganda?", "id": "Dua Sistem Isolasi Independen." },
  { "en": "Apa Itu Sistem Proteksi Termal?", "id": "Melindungi Perangkat Dari Suhu Berlebih." },
  { "en": "Apa Itu Termistor PTC (Positive Temperature Coefficient)?", "id": "Resistansi Meningkat Tajam Saat Suhu Naik." },
  { "en": "Di Mana Termistor PTC Digunakan?", "id": "Sebagai Sekring Yang Dapat Dipulihkan (Resettable Fuse)." },
  { "en": "Apa Itu Kelvin Connection?", "id": "Teknik Pengukuran Empat Titik." },
  { "en": "Mengapa Kelvin Connection Digunakan?", "id": "Menghilangkan Error Akibat Resistansi Kabel." },
  { "en": "Apa Itu Dielectric Absorption?", "id": "Efek Kapasitor Mengingat Sebagian Muatan." },
  { "en": "Dielektrik Apa Dengan Penyerapan Rendah?", "id": "Polipropilena, Polistirena, Dan Teflon." },
  { "en": "Apa Itu S-Parameter?", "id": "Nama Lain Untuk Scattering Parameters." },
  { "en": "Apa Itu Lingkaran Stabilitas (Stability Circles) Penguat?", "id": "Representasi Grafis Stabilitas Penguat RF." },
  { "en": "Apa Yang Direpresentasikan Oleh Lingkaran Stabilitas?", "id": "Daerah Impedansi Beban Sumber." },
  { "en": "Apa Itu Penguat Low-Voltage Differential Signaling (LVDS)?", "id": "Driver Receiver Untuk Sinyal Diferensial." },
  { "en": "Berapa Amplitudo Sinyal LVDS?", "id": "Sangat Kecil, Sekitar 350 Milivolt." },
  { "en": "Apa Keuntungan LVDS?", "id": "Kecepatan Tinggi, Derau Rendah, Konsumsi Daya Rendah." },
  { "en": "Apa Itu Resistor Terminasi LVDS?", "id": "Resistor 100 Ohm Di Antara Jalur." },
  { "en": "Apa Fungsi Common-Mode Choke?", "id": "Menekan Derau Mode-Common Pada Jalur Diferensial." },
  { "en": "Apa Itu Penguat Logaritmik?", "id": "Penguat Dengan Output Sebanding Logaritma Input." },
  { "en": "Di Mana Penguat Logaritmik Digunakan?", "id": "Pengukuran Daya RF Dan Kompresi Sinyal." },
  { "en": "Apa Itu Penguat Terkunci Fasa (Phase-Locked Amplifier)?", "id": "Penguat Yang Outputnya Terkunci Fasa." },
  { "en": "Apa Itu Penguat Optik Semikonduktor (SOA)?", "id": "Penguat Cahaya Berbasis Chip Semikonduktor." },
  { "en": "Apa Itu Penguat Serat Optik (Fiber Amplifier)?", "id": "Penguat Cahaya Menggunakan Serat Optik." },
  { "en": "Apa Itu Sinyal Baseband?", "id": "Sinyal Informasi Sebelum Dimodulasi." },
  { "en": "Apa Itu Sinyal Passband?", "id": "Sinyal Informasi Setelah Dimodulasi." },
  { "en": "Apa Itu Penguatan Tersedia (Available Gain)?", "id": "Penguatan Maksimum Yang Mungkin Dari Penguat." },
  { "en": "Apa Itu Penguatan Transduser (Transducer Gain)?", "id": "Penguatan Aktual Dengan Sumber Beban Nyata." },
  { "en": "Apa Itu Penguatan Operasi (Operating Gain)?", "id": "Penguatan Daya Dalam Kondisi Operasi Tertentu." },
  { "en": "Apa Itu Dioda Varactor?", "id": "Dioda Berfungsi Sebagai Kapasitor Terkontrol Tegangan." },
  { "en": "Apa Itu Faktor Penyesuaian (Tuning Ratio) Varactor?", "id": "Rasio Kapasitansi Maksimum Terhadap Minimum." },
  { "en": "Apa Itu Simbol Skematik Dioda Varactor?", "id": "Simbol Dioda Dengan Simbol Kapasitor." },
  { "en": "Apa Itu Penguat Kelas S?", "id": "Jenis Penguat Switching Untuk Modulasi AM." },
  { "en": "Apa Itu Penguat Linierisasi Umpan Maju (Feedforward)?", "id": "Teknik Mengurangi Distorsi Pada Penguat Daya." },
  { "en": "Apa Itu Penguat Linierisasi Predistorsi?", "id": "Sengaja Mendistorsi Sinyal Untuk Membatalkan Distorsi." },
  { "en": "Apa Itu Penguat Biparasit?", "id": "Osilasi Frekuensi Sangat Tinggi." },
  { "en": "Bagaimana Mencegah Osilasi Biparasit?", "id": "Menambahkan Ferrite Bead Atau Resistor." },
  { "en": "Apa Perbedaan Fungsional Diplexer Dan Duplexer?", "id": "Diplexer Frekuensi, Duplexer Arah Transmisi." },
  { "en": "Apa Itu Tepi Filter (Filter Skirt)?", "id": "Daerah Transisi Antara Passband Dan Stopband." },
  { "en": "Apa Itu Faktor Bentuk (Shape Factor) Filter?", "id": "Rasio Bandwidth Stopband Terhadap Passband." },
  { "en": "Apa Itu Peninggian Q (Q-Enhancement)?", "id": "Menggunakan Umpan Balik Positif." },
  { "en": "Apa Itu Sirkulator RF?", "id": "Perangkat Tiga Port Mengarahkan Sinyal." },
  { "en": "Bagaimana Cara Kerja Sirkulator?", "id": "Menggunakan Material Ferit Dan Medan Magnet." },
  { "en": "Apa Itu Isolator RF?", "id": "Perangkat Dua Port Melewatkan Sinyal Satu Arah." },
  { "en": "Bagaimana Cara Membuat Isolator?", "id": "Menggunakan Sirkulator Dengan Satu Port Diterminasi." },
  { "en": "Apa Itu Kopler Arah (Directional Coupler)?", "id": "Mencuplik Sebagian Kecil Sinyal." },
  { "en": "Apa Itu Faktor Kopling?", "id": "Ukuran Berapa Banyak Sinyal Dicuplik." },
  { "en": "Apa Itu Direktifitas Kopler?", "id": "Ukuran Kemampuan Memisahkan Sinyal Maju Mundur." },
  { "en": "Apa Itu Hibrida Kuadratur 90 Derajat?", "id": "Kopler Yang Membagi Sinyal Beda Fasa 90°." },
  { "en": "Apa Itu Magic Tee?", "id": "Komponen Pemandu Gelombang Hibrida 180 Derajat." },
  { "en": "Apa Itu Penggeser Fasa (Phase Shifter)?", "id": "Rangkaian Untuk Mengubah Fasa Sinyal." },
  { "en": "Apa Itu Attenuator Variabel?", "id": "Attenuator Yang Pelemahan Dapat Diatur." },
  { "en": "Apa Itu Filter Notch Adaptif?", "id": "Filter Yang Dapat Melacak Frekuensi." },
  { "en": "Apa Itu Jaringan Pembatalan Sisi Pita (Sideband Cancelling)?", "id": "Teknik Menghasilkan Sinyal SSB." },
  { "en": "Apa Itu Filter Butterworth?", "id": "Filter Dengan Respon Passband Maksimal Datar." },
  { "en": "Apa Itu Filter Chebyshev?", "id": "Filter Dengan Transisi Tajam." },
  { "en": "Apa Itu Kebocoran Local Oscillator (LO Leakage)?", "id": "Sinyal LO Bocor Ke Antena Atau IF." },
  { "en": "Apa Akibat Kebocoran LO?", "id": "Menyebabkan Offset DC Pada Penerima Zero-IF." },
  { "en": "Apa Itu Penerima Homodyne?", "id": "Nama Lain Untuk Penerima Zero-IF." },
  { "en": "Apa Perbedaan PLL Integer-N Dan Fraksional-N?", "id": "Fraksional-N Dapat Menghasilkan Frekuensi Pecahan." },
  { "en": "Apa Itu Spur Pecahan (Fractional Spur)?", "id": "Spur Khas Pada PLL Fraksional-N." },
  { "en": "Apa Itu Dithering Pada PLL Fraksional-N?", "id": "Mengacak Pola Pembagian Untuk Mengurangi Spur." },
  { "en": "Apa Itu Penuaan (Aging) Kristal?", "id": "Perubahan Frekuensi Lambat Seiring Waktu." },
  { "en": "Apa Itu Loncatan Aktivitas (Activity Dip)?", "id": "Anomali Resonansi Kristal Pada Suhu Tertentu." },
  { "en": "Apa Itu Osilator Terkontrol Oven (OCXO)?", "id": "Osilator Kristal Dalam Oven Suhu Konstan." },
  { "en": "Apa Itu Osilator Terkompensasi Suhu (TCXO)?", "id": "Osilator Kristal Dengan Sirkuit Kompensasi Suhu." },
  { "en": "Apa Itu Mikrofonik (Microphonics)?", "id": "Efek Getaran Mekanis Pada Frekuensi Osilator." },
  { "en": "Apa Itu Derau Fasa Aditif (Additive Phase Noise)?", "id": "Derau Fasa Ditambahkan Oleh Komponen." },
  { "en": "Apa Itu Derau Flicker?", "id": "Derau Frekuensi Rendah Dengan Spektrum 1/f." },
  { "en": "Apa Itu Lantai Derau Fasa (Phase Noise Floor)?", "id": "Level Derau Fasa Jauh Dari Frekuensi Carrier." },
  { "en": "Apa Itu Osilator YIG?", "id": "Osilator Disetel Menggunakan Bola Yttrium Iron Garnet." },
  { "en": "Apa Itu Osilator Dielektrik Resonator (DRO)?", "id": "Osilator Menggunakan Keping Keramik Resonansi." },
  { "en": "Apa Itu Derau Popcorn?", "id": "Ledakan Derau Acak." },
  { "en": "Apa Itu Osilator Kunci Injeksi (Injection Locked)?", "id": "Osilator Disinkronkan Dengan Sinyal Eksternal." },
  { "en": "Apa Itu Pengujian Emisi Terkonduksi (Conducted Emissions)?", "id": "Mengukur Derau Yang Dirambatkan Melalui Kabel." },
  { "en": "Apa Itu Pengujian Emisi Terdiasi (Radiated Emissions)?", "id": "Mengukur Derau Yang Dipancarkan Melalui Udara." },
  { "en": "Apa Itu Line Impedance Stabilization Network (LISN)?", "id": "Jaringan Untuk Pengujian Emisi Terkonduksi." },
  { "en": "Apa Itu Ruang Anechoic?", "id": "Ruangan Kedap Gema Elektromagnetik." },
  { "en": "Apa Itu Sel TEM (Transverse ElectroMagnetic)?", "id": "Perangkat Untuk Pengujian Imunitas Radiasi." },
  { "en": "Apa Itu Rangkaian Snubber RC?", "id": "Melindungi Saklar Dari Laju Perubahan Tegangan." },
  { "en": "Apa Itu Rangkaian Snubber RCD?", "id": "Snubber Yang Mengembalikan Energi." },
  { "en": "Apa Itu Urutan Catu Daya (Power Supply Sequencing)?", "id": "Menghidupkan Rel Tegangan Urutan Tertentu." },
  { "en": "Apa Itu Ultracapacitor?", "id": "Nama Lain Untuk Superkapasitor." },
  { "en": "Bagaimana Ultracapacitor Menyimpan Energi?", "id": "Menggunakan Lapisan Ganda Elektrostatis." },
  { "en": "Apa Itu Kepadatan Daya (Power Density)?", "id": "Daya Per Satuan Volume Atau Massa." },
  { "en": "Apa Itu Kepadatan Energi (Energy Density)?", "id": "Energi Per Satuan Volume Atau Massa." },
  { "en": "Mana Yang Lebih Tinggi Kepadatan Energinya?", "id": "Baterai Memiliki Kepadatan Energi Lebih Tinggi." },
  { "en": "Mana Yang Lebih Tinggi Kepadatan Dayanya?", "id": "Kapasitor Memiliki Kepadatan Daya Lebih Tinggi." },
  { "en": "Apa Itu Mode Henti (Hiccup Mode)?", "id": "Mode Proteksi Catu Daya." },
  { "en": "Bagaimana Mode Henti Bekerja?", "id": "Catu Daya Berulang Kali Mencoba Restart." },
  { "en": "Apa Itu Kontrol Sisi Primer (Primary-Side Control)?", "id": "Kontrol SMPS Tanpa Umpan Balik Optocoupler." },
  { "en": "Apa Keuntungan Kontrol Sisi Primer?", "id": "Biaya Lebih Rendah Dan Ukuran Kecil." },
  { "en": "Apa Itu Regulator Terisolasi?", "id": "Regulator Dengan Isolasi Galvanik." },
  { "en": "Apa Itu Modulator Lebar Pulsa (PWM)?", "id": "Sinyal Dengan Lebar Pulsa Yang Bervariasi." },
  { "en": "Apa Itu Modulasi Frekuensi Pulsa (PFM)?", "id": "Sinyal Dengan Frekuensi Pulsa Yang Bervariasi." },
  { "en": "Kapan PFM Lebih Efisien Dari PWM?", "id": "Pada Kondisi Beban Sangat Ringan." },
  { "en": "Apa Itu Dithering Frekuensi Switching?", "id": "Menyebarkan Energi EMI Ke Pita Lebar." },
  { "en": "Apa Itu Penginderaan Arus Tanpa Rugi (Lossless)?", "id": "Mengukur Arus Tanpa Resistor Shunt." },
  { "en": "Bagaimana Cara Mengukur Arus Tanpa Rugi?", "id": "Menggunakan Resistansi MOSFET Atau Induktor." },
  { "en": "Apa Itu Penguat Umpan Balik Transimpedansi (TIA)?", "id": "Jenis Penguat Umpan Balik Arus." },
  { "en": "Di Mana TIA Umum Digunakan?", "id": "Penerima Serat Optik Dan Sensor Cahaya." },
  { "en": "Apa Itu Kompensasi Dominant-Pole?", "id": "Teknik Menstabilkan Penguat Dengan Satu Kutub." },
  { "en": "Apa Itu Kompensasi Lead-Lag?", "id": "Teknik Kompensasi Frekuensi Lainnya." },
  { "en": "Apa Itu Penguat Audio Kelas T?", "id": "Desain Penguat Kelas D Berpemilik." },
  { "en": "Apa Itu Penguat Jembatan Terikat Beban (BTL)?", "id": "Nama Lain Untuk Konfigurasi Bridge-Tied Load." },
  { "en": "Apa Itu Sinyal Diferensial Tegangan Rendah (LVDS)?", "id": "Standar Sinyal Diferensial Kecepatan Tinggi." },
  { "en": "Apa Itu Penguat Sense Arus?", "id": "Penguat Untuk Mengukur Aliran Arus." },
  { "en": "Apa Itu Penguat RF Terdistribusi?", "id": "Menggabungkan Beberapa Transistor Untuk Bandwidth Lebar." },
  { "en": "Apa Itu Sel Unit Gilbert?", "id": "Inti Dari Banyak Mixer RF." },
  { "en": "Apa Itu Faktor Kebisingan (Noise Factor)?", "id": "Rasio SNR Input Terhadap SNR Output." },
  { "en": "Apa Hubungan Noise Factor Dan Noise Figure?", "id": "Noise Figure Adalah Noise Factor Dalam dB." },
  { "en": "Apa Itu Penguat Parametrik?", "id": "Penguat Kebisingan Sangat Rendah Untuk Sinyal Microwave." },
  { "en": "Apa Itu Penguat Maser?", "id": "Penguat Microwave Kebisingan Sangat Rendah Lainnya." },
  { "en": "Apa Itu Penguat Optik Raman?", "id": "Penguat Menggunakan Efek Raman Terstimulasi." },
  { "en": "Apa Itu Penguat Serat Doped Erbium (EDFA)?", "id": "Jenis Penguat Serat Optik Paling Umum." },
  { "en": "Apa Itu Isolator Optik?", "id": "Memungkinkan Cahaya Lewat Hanya Satu Arah." },
  { "en": "Apa Itu Sirkulator Optik?", "id": "Perangkat Tiga Port Mengarahkan Cahaya." },
  { "en": "Apa Itu Titik Bias?", "id": "Titik Operasi DC Suatu Perangkat." },
  { "en": "Apa Itu Penguat Kaskade?", "id": "Nama Lain Untuk Penguat Bertingkat." },
  { "en": "Apa Itu Resistor Film Tebal?", "id": "Resistor Dibuat Dengan Menempelkan Pasta Resistif." },
  { "en": "Apa Itu Resistor Film Logam?", "id": "Resistor Dengan Toleransi Rendah Dan Kebisingan Rendah." },
  { "en": "Apa Itu Resistor Komposisi Karbon?", "id": "Jenis Resistor Lama Dengan Toleransi Tinggi." },
  { "en": "Apa Itu Resistor Wirewound?", "id": "Resistor Dibuat Dengan Melilitkan Kawat Resistif." },
  { "en": "Apa Itu Potensiometer?", "id": "Resistor Variabel Dengan Tiga Terminal." },
  { "en": "Apa Itu Filter Air Dielektrik?", "id": "Filter Menggunakan Udara Sebagai Dielektrik." },
  { "en": "Apa Itu Filter Waveguide Terintegrasi Substrat (SIW)?", "id": "Pemandu Gelombang Dibuat Di Dalam Papan Sirkuit." },
  { "en": "Apa Itu Filter Resonator Cincin?", "id": "Filter Microwave Menggunakan Loop Saluran Transmisi." },
  { "en": "Apa Itu Filter Phase-Locked Loop (PLL)?", "id": "PLL Digunakan Sebagai Filter Pelacak." },
  { "en": "Apa Itu Filter Bank?", "id": "Kumpulan Filter Untuk Memisahkan Sinyal." },
  { "en": "Apa Itu Filter Kuadratur Cermin (QMF)?", "id": "Pasangan Filter Untuk Memisahkan Sinyal." },
  { "en": "Apa Itu Waktu Tunda Grup (Group Delay)?", "id": "Penundaan Amplop Sinyal Melalui Filter." },
  { "en": "Apa Itu Riak Waktu Tunda Grup?", "id": "Variasi Waktu Tunda Grup Di Passband." },
  { "en": "Apa Itu Ekualiser Waktu Tunda Grup?", "id": "Filter All-Pass Untuk Mengompensasi Riak." },
  { "en": "Apa Itu Transformasi Impedansi?", "id": "Mengubah Prototipe Filter Untuk Impedansi Berbeda." },
  { "en": "Apa Itu Filter Normalisasi Frekuensi?", "id": "Filter Didesain Untuk Frekuensi Cutoff 1 Hz." },
  { "en": "Apa Itu Impedansi Gambar?", "id": "Impedansi Karakteristik Suatu Jaringan Filter." },
  { "en": "Apa Itu Metode Penyisipan Rugi (Insertion Loss)?", "id": "Metode Modern Untuk Desain Filter." },
  { "en": "Apa Itu Polinomial Legendre?", "id": "Polinomial Ortogonal Digunakan Dalam Desain Filter." },
  { "en": "Apa Itu Filter Gaussian?", "id": "Filter Dengan Respon Impuls Berbentuk Lonceng." },
  { "en": "Apa Itu Filter Cosine-Raised?", "id": "Filter Untuk Mengurangi Interferensi Antar Simbol." },
  { "en": "Apa Itu Kopling Antar Resonator?", "id": "Interaksi Antara Elemen Resonansi Dalam Filter." },
  { "en": "Apa Itu Matriks Kopling?", "id": "Mendeskripsikan Kopling Dalam Filter Microwave." },
  { "en": "Apa Itu Sintesis Jaringan?", "id": "Proses Mendesain Sirkuit Dari Fungsi Matematis." },
  { "en": "Apa Itu Fungsi Positif-Nyata?", "id": "Fungsi Matematis Yang Dapat Diwujudkan." },
  { "en": "Apa Itu Osilator Kunci Injeksi Regeneratif?", "id": "Osilator Dengan Umpan Balik Untuk Mengunci." },
  { "en": "Apa Itu Osilator Kunci Injeksi Langsung?", "id": "Sinyal Injeksi Diterapkan Langsung Ke Osilator." },
  { "en": "Apa Itu Pembagi Frekuensi Kunci Injeksi?", "id": "Osilator Yang Mengunci Ke Subharmonik." },
  { "en": "Apa Itu Osilator Opto-Elektronik (OEO)?", "id": "Osilator Kebisingan Sangat Rendah Menggunakan Serat Optik." },
  { "en": "Apa Itu Osilator Spin Torsi Nano?", "id": "Osilator Miniatur Berbasis Efek Spintronik." },
  { "en": "Apa Itu Osilator Relaksasi Avalanche?", "id": "Osilator Menghasilkan Pulsa Sangat Cepat." },
  { "en": "Apa Itu Osilator Pearson-Anson?", "id": "Osilator Relaksasi Menggunakan Lampu Neon." },
  { "en": "Apa Itu Osilator Armstrong?", "id": "Osilator LC Pertama Menggunakan Tabung Vakum." },
  { "en": "Apa Itu Osilator Meissner?", "id": "Osilator LC Terkopel Induktif." },
  { "en": "Apa Itu Osilator Jembatan Wien?", "id": "Osilator RC Menghasilkan Gelombang Sinusoidal Murni." },
  { "en": "Apa Itu Osilator Pergeseran Fasa?", "id": "Osilator RC Menggunakan Tiga Jaringan Pergeseran Fasa." },
  { "en": "Apa Itu Osilator Jembatan Ganda-T?", "id": "Osilator Menggunakan Filter Notch Twin-T." },
  { "en": "Apa Itu Osilator Kuadratur Polifase?", "id": "Menghasilkan Banyak Fasa Dengan Jarak Sama." },
  { "en": "Apa Itu Osilator Diferensial?", "id": "Osilator Yang Menghasilkan Output Diferensial." },
  { "en": "Apa Keuntungan Osilator Diferensial?", "id": "Penolakan Kebisingan Mode-Common Yang Lebih Baik." },
  { "en": "Apa Itu Osilator Terdistribusi?", "id": "Osilator Microwave Menggunakan Saluran Transmisi." },
  { "en": "Apa Itu Osilator Gelombang Mundur (BWO)?", "id": "Osilator Tabung Vakum Untuk Frekuensi Milimeter." },
  { "en": "Apa Itu Magnetron?", "id": "Osilator Tabung Vakum Berdaya Tinggi." },
  { "en": "Apa Itu Klystron Refleks?", "id": "Jenis Klystron Yang Digunakan Sebagai Osilator." },
  { "en": "Apa Itu Pengganda Frekuensi Varactor?", "id": "Menggunakan Kapasitansi Non-Linear Varactor." },
  { "en": "Apa Itu Pengganda Frekuensi Dioda Step-Recovery?", "id": "Menghasilkan Spektrum Harmonik Yang Kaya." },
  { "en": "Apa Itu Konverter Turun (Downconverter)?", "id": "Mixer Yang Mengubah Frekuensi Lebih Tinggi Ke Rendah." },
  { "en": "Apa Itu Konverter Naik (Upconverter)?", "id": "Mixer Yang Mengubah Frekuensi Lebih Rendah Ke Tinggi." },
  { "en": "Apa Itu Catu Daya Laboratorium?", "id": "Catu Daya Serbaguna Untuk Pengujian." },
  { "en": "Apa Itu Beban Elektronik (Electronic Load)?", "id": "Perangkat Yang Mensimulasikan Beban Listrik." },
  { "en": "Apa Saja Mode Operasi Beban Elektronik?", "id": "Arus Konstan, Tegangan Konstan, Resistansi Konstan." },
  { "en": "Apa Itu Bank Baterai?", "id": "Beberapa Baterai Dihubungkan Bersama." },
  { "en": "Apa Itu Pengisian Tetes (Trickle Charging)?", "id": "Mengisi Baterai Dengan Arus Sangat Kecil." },
  { "en": "Apa Itu Pengisian Apung (Float Charging)?", "id": "Menjaga Baterai Terisi Penuh." },
  { "en": "Apa Itu Pemutus Sirkuit (Circuit Breaker)?", "id": "Saklar Otomatis Untuk Proteksi Arus Berlebih." },
  { "en": "Apa Itu Pemutus Sirkuit Gangguan Tanah (GFCI)?", "id": "Melindungi Dari Sengatan Listrik." },
  { "en": "Apa Itu Pemutus Sirkuit Gangguan Busur Api (AFCI)?", "id": "Melindungi Dari Kebakaran Akibat Busur Api." },
  { "en": "Apa Itu Sistem Tenaga Tak Terputus (UPS)?", "id": "Nama Lain Untuk Uninterruptible Power Supply." },
  { "en": "Apa Itu Inverter Gelombang Sinus Murni?", "id": "Inverter Dengan Output AC Kualitas Tinggi." },
  { "en": "Apa Itu Inverter Gelombang Sinus Modifikasi?", "id": "Inverter Dengan Output AC Kualitas Rendah." },
  { "en": "Apa Itu Inverter Terikat Jaringan (Grid-Tied)?", "id": "Inverter Yang Dapat Mengirim Daya Ke Jaringan." },
  { "en": "Apa Itu Anti-Islanding?", "id": "Fitur Keamanan Untuk Inverter Terikat Jaringan." },
  { "en": "Apa Itu Konverter Terisolasi Jembatan Maju (Forward)?", "id": "Topologi SMPS Umum Untuk Daya Sedang." },
  { "en": "Apa Itu Konverter Terisolasi Flyback?", "id": "Topologi SMPS Umum Untuk Daya Rendah." },
  { "en": "Apa Itu Konverter Buck-Boost?", "id": "Topologi SMPS Yang Dapat Menaikkan Turunkan Tegangan." },
  { "en": "Apa Itu Konverter SEPIC?", "id": "Konverter Buck-Boost Dengan Output Non-Inverting." },
  { "en": "Apa Itu Konverter Cuk?", "id": "Konverter Buck-Boost Lainnya." },
  { "en": "Apa Itu Konverter Zeta?", "id": "Varian Dari Konverter SEPIC." },
  { "en": "Apa Itu Konverter Royer?", "id": "Osilator Resonansi Yang Mendorong Transformator." },
  { "en": "Apa Itu Catu Daya Terdistribusi?", "id": "Arsitektur Daya Menggunakan Banyak Konverter Lokal." },
  { "en": "Apa Itu Konverter Titik Beban (PoL)?", "id": "Konverter DC-DC Kecil Dekat Beban." },
  { "en": "Apa Itu Penguat Magnetik (Magnetic Amplifier)?", "id": "Menggunakan Saturasi Inti Magnetik Untuk Penguatan." },
  { "en": "Apa Keuntungan Penguat Magnetik?", "id": "Sangat Kuat Dan Tahan Radiasi Nuklir." },
  { "en": "Apa Itu Lingkaran Kebisingan (Noise Figure Circles)?", "id": "Plot Kontur Noise Figure Di Smith Chart." },
  { "en": "Apa Kegunaan Lingkaran Kebisingan?", "id": "Membantu Mendesain Jaringan Pencocokan Kebisingan Rendah." },
  { "en": "Apa Itu Gunnplexer?", "id": "Transceiver Microwave Sederhana Menggunakan Dioda Gunn." },
  { "en": "Apa Itu Penguat Terowongan Dioda (Tunnel Diode Amplifier)?", "id": "Penguat Microwave Menggunakan Resistansi Negatif." },
  { "en": "Apa Itu Penguat Kriogenik?", "id": "Penguat Didinginkan Hingga Suhu Sangat Rendah." },
  { "en": "Mengapa Penguat Kriogenik Digunakan?", "id": "Untuk Mencapai Angka Kebisingan Sangat Rendah." },
  { "en": "Di Mana Penguat Kriogenik Digunakan?", "id": "Teleskop Radio Dan Eksperimen Fisika." },
  { "en": "Apa Itu Penguat Keseimbangan (Balanced Amplifier)?", "id": "Menggunakan Dua Penguat Paralel." },
  { "en": "Apa Keuntungan Penguat Keseimbangan?", "id": "Pencocokan Impedansi Dan Stabilitas Yang Baik." },
  { "en": "Apa Komponen Kunci Penguat Keseimbangan?", "id": "Hibrida Kuadratur Di Input Dan Output." },
  { "en": "Apa Itu Penguat Kelas F Terbalik (Inverse)?", "id": "Varian Penguat Kelas F." },
  { "en": "Apa Itu Penguat Batas (Limiting Amplifier)?", "id": "Menghasilkan Amplitudo Output Konstan." },
  { "en": "Apa Itu Detektor Log Video (Log Video Amplifier)?", "id": "Mengompresi Rentang Dinamis Sinyal Video." },
  { "en": "Apa Itu Penguat Aktif Kaskade?", "id": "Meningkatkan Penguatan Dan Bandwidth." },
  { "en": "Apa Itu Cermin Arus Widlar?", "id": "Cermin Arus Untuk Menghasilkan Arus Kecil." },
  { "en": "Apa Itu Beban Aktif?", "id": "Menggunakan Cermin Arus Sebagai Beban." },
  { "en": "Apa Itu Penguat Diferensial Teleskopik?", "id": "Penguat Dengan Penguatan Sangat Tinggi." },
  { "en": "Apa Itu Penguat Operasional Konduktansi (OTA)?", "id": "Penguat Dengan Output Arus." },
  { "en": "Apa Itu Kompensasi Kutub-Nol (Pole-Zero)?", "id": "Teknik Menstabilkan Penguat Umpan Balik." },
  { "en": "Apa Itu Penguat Umpan Balik Arus (CFA)?", "id": "Op-Amp Dengan Bandwidth Lebar." },
  { "en": "Apa Itu Penguat Umpan Balik Tegangan (VFA)?", "id": "Jenis Op-Amp Standar." },
  { "en": "Apa Itu Dioda Varactor?", "id": "Dioda Yang Berfungsi Sebagai Kapasitor Variabel." },
  { "en": "Apa Itu Penguat Video Terdistribusi?", "id": "Penguat Dengan Bandwidth Sangat Lebar." },
  { "en": "Apa Itu Resonator Rongga (Cavity Resonator)?", "id": "Struktur Logam Berongga Untuk Resonansi Gelombang Mikro." },
  { "en": "Apa Faktor Q Dari Resonator Rongga?", "id": "Sangat Tinggi, Menghasilkan Selektivitas Tajam." },
  { "en": "Apa Itu Filter Heliks?", "id": "Filter Menggunakan Elemen Heliks Di Dalam Rongga." },
  { "en": "Apa Keuntungan Filter Heliks?", "id": "Ukuran Kompak Dan Faktor Q Tinggi." },
  { "en": "Apa Perbedaan Praktis Stripline Dan Mikrostrip?", "id": "Stripline Memiliki Radiasi Lebih Rendah." },
  { "en": "Apa Itu Mode Evanescent?", "id": "Gelombang Yang Meluruh Secara Eksponensial." },
  { "en": "Apa Itu Filter Mode Evanescent?", "id": "Filter Waveguide Yang Beroperasi Di Bawah Cutoff." },
  { "en": "Apa Itu Jendela (Iris) Waveguide?", "id": "Penyempitan Di Dalam Waveguide." },
  { "en": "Apa Fungsi Jendela Waveguide?", "id": "Menciptakan Reaktansi Untuk Membuat Filter." },
  { "en": "Apa Itu Filter Waffle-Iron?", "id": "Filter Frekuensi Tinggi Untuk Menekan Harmonik." },
  { "en": "Apa Itu Filter Suspended Substrate?", "id": "Filter Dengan Substrat Digantung Di Udara." },
  { "en": "Apa Itu Filter Combline?", "id": "Filter Microwave Menggunakan Batang Resonansi." },
  { "en": "Apa Itu Filter Rambut Jepit (Hairpin)?", "id": "Filter Mikrostrip Dengan Resonator Berbentuk U." },
  { "en": "Apa Itu Filter Cincin Terpisah (Split-Ring)?", "id": "Elemen Kunci Dalam Desain Metamaterial." },
  { "en": "Apa Itu Metamaterial?", "id": "Material Buatan Dengan Sifat Elektromagnetik Unik." },
  { "en": "Apa Itu Filter Frekuensi Audio?", "id": "Filter Untuk Memanipulasi Sinyal Suara." },
  { "en": "Apa Itu Filter Crossover Pasif?", "id": "Jaringan LC Untuk Memisahkan Frekuensi Speaker." },
  { "en": "Apa Itu Filter Crossover Aktif?", "id": "Filter Elektronik Sebelum Tahap Penguatan." },
  { "en": "Apa Itu Jaringan Zobel?", "id": "Membantu Menstabilkan Impedansi Speaker." },
  { "en": "Apa Itu Jaringan Konjugat?", "id": "Mengompensasi Induktansi Voice Coil Speaker." },
  { "en": "Apa Itu Osilator Klystron Refleks?", "id": "Tabung Vakum Yang Dapat Berfungsi Osilator." },
  { "en": "Apa Itu Osilator Barkhausen-Kurz?", "id": "Osilator Tabung Vakum Frekuensi Sangat Tinggi." },
  { "en": "Apa Itu Emisi Spurious?", "id": "Emisi Yang Tidak Diinginkan Dari Pemancar." },
  { "en": "Badan Apa Yang Mengatur Emisi Spurious?", "id": "Federal Communications Commission (FCC) Dan Lainnya." },
  { "en": "Apa Itu Topeng Spektral (Spectral Mask)?", "id": "Batas Level Daya Emisi." },
  { "en": "Apa Itu Kebocoran Pembawa (Carrier Leakage)?", "id": "Sinyal Pembawa Yang Tidak Sepenuhnya Ditekan." },
  { "en": "Apa Itu Penekanan Sisi Pita (Sideband Suppression)?", "id": "Ukuran Seberapa Baik Sisi Pita Dihilangkan." },
  { "en": "Apa Itu Penekanan Harmonik?", "id": "Ukuran Seberapa Baik Harmonik Ditekan." },
  { "en": "Apa Itu Osilator Terkunci Optik (Optically Locked)?", "id": "Osilator Disinkronkan Dengan Sinyal Optik." },
  { "en": "Apa Itu Sisir Frekuensi (Frequency Comb)?", "id": "Spektrum Terdiri Dari Garis-Garis Teratur." },
  { "en": "Apa Aplikasi Sisir Frekuensi?", "id": "Spektroskopi Presisi, Jam Optik, Dan Lainnya." },
  { "en": "Apa Itu Osilator Atom Chip-Scale (CSAC)?", "id": "Jam Atom Miniatur." },
  { "en": "Apa Itu Osilator Putaran (Spin Oscillator)?", "id": "Osilator Berbasis Presesi Spin Nuklir." },
  { "en": "Apa Itu Efek Faraday?", "id": "Rotasi Polarisasi Cahaya Oleh Medan Magnet." },
  { "en": "Apa Itu Efek Kerr?", "id": "Perubahan Indeks Bias Oleh Medan Listrik." },
  { "en": "Apa Itu Efek Pockels?", "id": "Varian Dari Efek Kerr." },
  { "en": "Apa Itu Modulator Elektro-Optik?", "id": "Menggunakan Efek Pockels Untuk Memodulasi Cahaya." },
  { "en": "Apa Itu Modulator Akusto-Optik?", "id": "Menggunakan Gelombang Suara Untuk Memodulasi Cahaya." },
  { "en": "Apa Itu Generator Marx?", "id": "Menghasilkan Pulsa Tegangan Sangat Tinggi." },
  { "en": "Bagaimana Generator Marx Bekerja?", "id": "Mengisi Kapasitor Paralel, Melepasnya Seri." },
  { "en": "Apa Itu Generator Cockcroft-Walton?", "id": "Pengganda Tegangan Untuk Menghasilkan DC Tinggi." },
  { "en": "Di Mana Generator Cockcroft-Walton Digunakan?", "id": "Akselerator Partikel Dan Mesin Sinar-X." },
  { "en": "Apa Itu Saturasi Transformator?", "id": "Inti Tidak Dapat Menyimpan Lebih Banyak Fluks." },
  { "en": "Apa Akibat Saturasi Transformator?", "id": "Arus Primer Melonjak Dan Distorsi." },
  { "en": "Apa Itu Pipa Panas (Heat Pipe)?", "id": "Perangkat Transfer Panas Pasif." },
  { "en": "Bagaimana Pipa Panas Bekerja?", "id": "Menggunakan Penguapan Dan Kondensasi Fluida." },
  { "en": "Apa Itu Pendinginan Termoelektrik (TEC)?", "id": "Menggunakan Efek Peltier Untuk Mendinginkan." },
  { "en": "Apa Keuntungan TEC?", "id": "Solid-State, Tanpa Bagian Bergerak." },
  { "en": "Apa Itu Sirkuit Crowbar?", "id": "Sirkuit Proteksi Tegangan Berlebih." },
  { "en": "Bagaimana Sirkuit Crowbar Bekerja?", "id": "Membuat Hubung Singkat Untuk Membakar Sekring." },
  { "en": "Apa Itu Thyristor (SCR)?", "id": "Saklar Semikonduktor Dapat Dikunci." },
  { "en": "Apa Itu TRIAC?", "id": "Thyristor Dua Arah Untuk Kontrol AC." },
  { "en": "Apa Itu DIAC?", "id": "Dioda Pemicu Untuk Thyristor." },
  { "en": "Apa Itu Kontrol Sudut Fasa?", "id": "Teknik Mengontrol Daya AC." },
  { "en": "Di Mana Kontrol Sudut Fasa Digunakan?", "id": "Peredup Lampu Dan Kontrol Kecepatan Motor." },
  { "en": "Apa Itu Konverter Matriks?", "id": "Konverter AC-ke-AC Langsung." },
  { "en": "Apa Itu Kontrol Histeresis?", "id": "Metode Kontrol Sederhana Dengan Umpan Balik." },
  { "en": "Apa Itu Catu Daya Tak Terputus (UPS) Siaga?", "id": "Beralih Ke Baterai Saat Listrik Gagal." },
  { "en": "Apa Itu UPS Interaktif Jalur?", "id": "UPS Siaga Dengan Stabilisasi Tegangan." },
  { "en": "Apa Itu UPS Konversi Ganda (Online)?", "id": "Selalu Menjalankan Beban Dari Inverter." },
  { "en": "Apa Keuntungan UPS Online?", "id": "Isolasi Total Dari Gangguan Jaringan." },
  { "en": "Apa Itu Bypass Statis?", "id": "Saklar Cadangan Dalam Sistem UPS Online." },
  { "en": "Apa Itu Bank Kapasitor?", "id": "Kumpulan Kapasitor Untuk Koreksi Faktor Daya." }



        ];

        let questions = [];

        rawVocabularyList.sort((a, b) => {
            const enA = a.en.toLowerCase();
            const enB = b.en.toLowerCase();
            if (enA < enB) return -1;
            if (enA > enB) return 1;
            return 0;
        });

        function generateQuestions() {
            const allIndonesianTranslations = rawVocabularyList.map(item => item.id);
            questions = [];
            rawVocabularyList.forEach(vocabItem => {
                const correctAnswer = vocabItem.id;
                const distractors = [];
                let attempts = 0;
                while (distractors.length < 3 && attempts < allIndonesianTranslations.length * 2) {
                    const randomIndex = Math.floor(Math.random() * allIndonesianTranslations.length);
                    const potentialDistractor = allIndonesianTranslations[randomIndex];
                    if (potentialDistractor !== correctAnswer && !distractors.includes(potentialDistractor)) {
                        distractors.push(potentialDistractor);
                    }
                    attempts++;
                }
                while (distractors.length < 3) {
                    const fallbackOptions = ["opsi lain A", "opsi lain B", "opsi lain C", "opsi lain D", "opsi lain E", "opsi lain F"];
                    let fallbackIndex = 0;
                    let safetyNet = 0;
                    while(distractors.length < 3 && safetyNet < fallbackOptions.length * 3) {
                        const fbOption = fallbackOptions[fallbackIndex % fallbackOptions.length] + `_${distractors.length}${Math.floor(Math.random()*100)}`;
                        if (fbOption !== correctAnswer && !distractors.includes(fbOption)) {
                             distractors.push(fbOption);
                        }
                        fallbackIndex++;
                        safetyNet++;
                    }
                     if(distractors.length < 3) {
                        for(let i=0; i < (3-distractors.length); i++){
                            distractors.push("pilihan default " + (i+1+distractors.length) + Math.random().toString(36).substring(7));
                        }
                     }
                }
                const answerOptions = [
                    { text: correctAnswer, correct: true },
                    { text: distractors[0], correct: false },
                    { text: distractors[1], correct: false },
                    { text: distractors[2], correct: false }
                ];
                questions.push({
                    question: vocabItem.en,
                    answers: answerOptions
                });
            });
        }

        generateQuestions();

        function saveProgress() {
            if (!questionContainerElement.classList.contains('hide') && orderedQuestions && currentQuestionIndex < orderedQuestions.length) {
                 const progress = {
                    currentQuestionIndex: currentQuestionIndex,
                    score: score,
                    orderedQuestions: orderedQuestions
                };
                localStorage.setItem('quizProgress', JSON.stringify(progress));
            }
        }

        function loadProgress() {
            const savedProgress = localStorage.getItem('quizProgress');
            if (savedProgress) {
                try {
                    const progressData = JSON.parse(savedProgress);
                    if (progressData && typeof progressData.currentQuestionIndex === 'number' &&
                        typeof progressData.score === 'number' && Array.isArray(progressData.orderedQuestions) &&
                        progressData.orderedQuestions.length > 0 &&
                        progressData.currentQuestionIndex < progressData.orderedQuestions.length &&
                        progressData.orderedQuestions.length === questions.length) { // Validasi tambahan: jumlah soal harus sama
                        return progressData;
                    } else {
                        clearProgress();
                        return null;
                    }
                } catch (e) {
                    console.error("Error parsing saved progress:", e);
                    clearProgress();
                    return null;
                }
            }
            return null;
        }

        function clearProgress() {
            localStorage.removeItem('quizProgress');
        }

        prev50Button.addEventListener('click', () => navigateQuestions(-JUMP_AMOUNT));
        prevQuestionButton.addEventListener('click', () => navigateQuestions(-1)); // Event listener untuk tombol baru
        next50Button.addEventListener('click', () => navigateQuestions(JUMP_AMOUNT));

        function navigateQuestions(amount) {
            clearTimeout(questionTimeout);
            if (!orderedQuestions || orderedQuestions.length === 0) return;

            let newIndex = currentQuestionIndex + amount;
            if (newIndex < 0) newIndex = 0;
            else if (newIndex >= orderedQuestions.length) newIndex = orderedQuestions.length - 1;

            if (newIndex !== currentQuestionIndex) {
                currentQuestionIndex = newIndex;
                setNextQuestion();
            } else {
                updateSkipButtonStates();
            }
        }

        function updateSkipButtonStates() {
            if (!orderedQuestions || orderedQuestions.length === 0 || questionContainerElement.classList.contains('hide')) {
                skipNavigationControls.classList.add('hide');
                if(prev50Button) prev50Button.disabled = true;
                if(prevQuestionButton) prevQuestionButton.disabled = true; // Nonaktifkan tombol baru
                if(next50Button) next50Button.disabled = true;
                return;
            }
            skipNavigationControls.classList.remove('hide');
            const isFirstQuestion = currentQuestionIndex === 0;
            const isLastQuestion = currentQuestionIndex === (orderedQuestions.length - 1);

            if(prev50Button) prev50Button.disabled = isFirstQuestion;
            if(prevQuestionButton) prevQuestionButton.disabled = isFirstQuestion; // Atur status disabled tombol baru
            if(next50Button) next50Button.disabled = isLastQuestion;

            if (orderedQuestions.length <= 1) {
                if(prev50Button) prev50Button.disabled = true;
                if(prevQuestionButton) prevQuestionButton.disabled = true; // Atur status disabled tombol baru
                if(next50Button) next50Button.disabled = true;
            }
        }


        window.addEventListener('load', () => {
            const savedData = loadProgress();
            startButton.innerText = 'Mulai';
            completionMessageElement.classList.add('hide');
            if (savedData) {
                continueButton.classList.remove('hide');
            } else {
                continueButton.classList.add('hide');
            }
            if (questionContainerElement.classList.contains('hide')) {
                initialControls.classList.remove('hide');
                skipNavigationControls.classList.add('hide');
            } else {
                 initialControls.classList.add('hide');
                 // Mungkin juga perlu updateSkipButtonStates() di sini jika kuis dilanjutkan
                 // dan langsung menampilkan soal.
            }
        });

        startButton.addEventListener('click', () => startGame(false));
        continueButton.addEventListener('click', () => startGame(true));

        function startGame(isContinuing = false) {
            clearTimeout(questionTimeout);
            completionMessageElement.classList.add('hide');
            if (!isContinuing) {
                startButton.innerText = 'Mulai';
            }
            initialControls.classList.add('hide');
            questionContainerElement.classList.remove('hide');
            questionCounterElement.classList.remove('hide');

            const savedData = loadProgress();
            if (isContinuing && savedData && savedData.orderedQuestions && savedData.orderedQuestions.length === questions.length) {
                orderedQuestions = savedData.orderedQuestions;
                currentQuestionIndex = savedData.currentQuestionIndex;
                score = savedData.score;
            } else {
                clearProgress();
                orderedQuestions = [...questions];
                currentQuestionIndex = 0;
                score = 0;
            }

            if (!orderedQuestions || orderedQuestions.length === 0) {
                showResults();
                completionMessageElement.innerText = "Tidak ada soal untuk ditampilkan.";
                completionMessageElement.style.color = "#dc3545";
                completionMessageElement.classList.remove('hide');
                startButton.innerText = 'Mulai';
                return;
            }
            setNextQuestion();
        }

        function setNextQuestion() {
            resetState();
            if (orderedQuestions && currentQuestionIndex < orderedQuestions.length) {
                questionCounterElement.innerText = `${currentQuestionIndex + 1} / ${orderedQuestions.length}`;
                showQuestion(orderedQuestions[currentQuestionIndex]);
                saveProgress();
                if (document.activeElement && typeof document.activeElement.blur === 'function') {
                    document.activeElement.blur();
                }
            } else {
                showResults();
            }
            updateSkipButtonStates(); // Panggil di sini untuk memastikan state tombol selalu update
        }

        function showQuestion(questionData) {
            questionElement.innerText = questionData.question;
            answerButtonsElement.innerHTML = '';
            const shuffledAnswers = [...questionData.answers].sort(() => Math.random() - 0.5);
            shuffledAnswers.forEach(answer => {
                const button = document.createElement('button');
                button.innerText = answer.text;
                button.classList.add('btn');
                if (answer.correct) {
                    button.dataset.correct = answer.correct;
                }
                button.addEventListener('click', selectAnswer);
                answerButtonsElement.appendChild(button);
            });
        }

        function resetState() {
            clearTimeout(questionTimeout);
            while (answerButtonsElement.firstChild) {
                answerButtonsElement.removeChild(answerButtonsElement.firstChild);
            }
        }

        function selectAnswer(e) {
            const selectedButton = e.target;
            const correct = selectedButton.dataset.correct === 'true';
            if (correct) { score++; }
            Array.from(answerButtonsElement.children).forEach(button => {
                setStatusClass(button, button.dataset.correct === 'true');
                button.disabled = true;
            });
            saveProgress();
            questionTimeout = setTimeout(() => {
                if (orderedQuestions && currentQuestionIndex < orderedQuestions.length -1) {
                    currentQuestionIndex++;
                    setNextQuestion();
                } else if (orderedQuestions && currentQuestionIndex === orderedQuestions.length -1) {
                    showResults();
                }
            }, 7000);
        }

        function setStatusClass(element, correct) {
            clearStatusClass(element);
            if (correct) { element.classList.add('correct'); }
            else { element.classList.add('wrong'); }
        }

        function clearStatusClass(element) {
            element.classList.remove('correct');
            element.classList.remove('wrong');
        }

        function showResults() {
            clearTimeout(questionTimeout);
            questionContainerElement.classList.add('hide');
            questionCounterElement.classList.add('hide');
            skipNavigationControls.classList.add('hide');
            clearProgress();
            completionMessageElement.innerText = "Selamat Kuis Sudah Selesai 🎉";
            completionMessageElement.style.color = "#28a745";
            completionMessageElement.classList.remove('hide');
            startButton.innerText = 'Ulangi Kuis';
            initialControls.classList.remove('hide');
            continueButton.classList.add('hide');
        }
    </script>
</body>
</html>
