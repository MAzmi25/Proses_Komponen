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


  { "en": "Apa Bahan Dasar Chip Modern?", "id": "Kristal Silikon (Si) Sangat Murni." },
  { "en": "Dari Mana Sumber 'Silikon (Si)'?", "id": "Pasir Kuarsa (Silikon Dioksida - SiO₂)." },
  { "en": "Apa Itu 'Ingot'?", "id": "Batangan Kristal Tunggal Silikon (Si)." },
  { "en": "Apa Proses Pembuatan 'Ingot'?", "id": "Proses Czochralski." },
  { "en": "Apa Itu 'Wafer'?", "id": "Irisan Tipis Dari Ingot Silikon (Si)." },
  { "en": "Mengapa Produksi Chip Butuh 'Ruang Bersih'?", "id": "Untuk Menghindari Kontaminasi Partikel Debu." },
  { "en": "Apa Proses Utama Pembuatan Pola Sirkuit?", "id": "Fotolitografi." },
  { "en": "Apa Sumber Cahaya Dalam 'Fotolitografi'?", "id": "Cahaya Ultraviolet (UV)." },
  { "en": "Apa Itu 'Photoresist'?", "id": "Cairan Polimer Peka Cahaya." },
  { "en": "Apa Fungsi 'Photoresist'?", "id": "Membentuk Pola Masking Sementara." },
  { "en": "Apa Itu 'Mask' Atau 'Reticle'?", "id": "Template Kaca Pola Sirkuit." },
  { "en": "Apa Proses 'Deposisi'?", "id": "Proses Menambahkan Lapisan Material Tipis." },
  { "en": "Apa Kepanjangan 'CVD (Chemical Vapor Deposition)'?", "id": "Deposisi Uap Kimia." },
  { "en": "Apa Kepanjangan 'PVD (Physical Vapor Deposition)'?", "id": "Deposisi Uap Fisik." },
  { "en": "Apa Itu 'Oksidasi Termal'?", "id": "Menumbuhkan Lapisan SiO₂ (Silikon Dioksida)." },
  { "en": "Apa Fungsi Lapisan 'Oksida'?", "id": "Sebagai Isolator Atau Lapisan Gerbang." },
  { "en": "Apa Itu 'Etching'?", "id": "Proses Menghilangkan Material." },
  { "en": "Apa Itu 'Wet Etching'?", "id": "Etching Menggunakan Bahan Kimia Cair." },
  { "en": "Apa Itu 'Dry Etching'?", "id": "Etching Menggunakan Gas Plasma." },
  { "en": "Apa Tujuan 'Doping'?", "id": "Mengubah Sifat Listrik Semikonduktor." },
  { "en": "Apa Dua Metode Utama 'Doping'?", "id": "Difusi Dan Implantasi Ion." },
  { "en": "Apa Itu 'Implantasi Ion'?", "id": "Menembakkan Ion Dopan Ke Wafer." },
  { "en": "Apa Itu 'Difusi'?", "id": "Menyebarkan Dopan Menggunakan Suhu Tinggi." },
  { "en": "Apa Itu 'Annealing'?", "id": "Pemanasan Untuk Mengaktifkan Dopan." },
  { "en": "Apa Itu 'Planarisasi'?", "id": "Proses Meratakan Permukaan Wafer." },
  { "en": "Apa Kepanjangan 'CMP (Chemical Mechanical Planarization)'?", "id": "Planarisasi Kimia Mekanis." },
  { "en": "Bagaimana Cara Membuat 'Resistor'?", "id": "Dengan Mendoping Sebuah Wilayah Silikon (Si)." },
  { "en": "Bagaimana Cara Membuat 'Kapasitor'?", "id": "Dua Konduktor Dipisahkan Oleh Dielektrik." },
  { "en": "Bagaimana Cara Membuat 'Dioda'?", "id": "Dengan Membuat Sambungan P-N." },
  { "en": "Bagaimana Cara Membuat 'Transistor MOSFET'?", "id": "Kombinasi Deposisi, Doping, Dan Etsa." },
  { "en": "Apa Itu 'Metalisasi'?", "id": "Proses Pembentukan Jalur Logam." },
  { "en": "Logam Apa Yang Umum Untuk 'Interkoneksi'?", "id": "Tembaga (Cu) Atau Aluminium (Al)." },
  { "en": "Apa Itu 'Interkoneksi'?", "id": "Jalur Logam Penghubung Antar Transistor." },
  { "en": "Apa Itu 'Dielektrik'?", "id": "Bahan Isolator Listrik." },
  { "en": "Apa Itu 'Dielektrik Low-k'?", "id": "Isolator Untuk Mengurangi Kapasitansi Parasit." },
  { "en": "Apa Itu 'Wafer Dicing'?", "id": "Proses Memotong Wafer Menjadi Die." },
  { "en": "Apa Itu 'Die'?", "id": "Satu Keping Chip Individual." },
  { "en": "Apa Itu 'Pengemasan IC (Integrated Circuit)'?", "id": "Proses Melindungi Dan Menghubungkan Die." },
  { "en": "Apa Itu 'Wire Bonding'?", "id": "Menyambung Die Ke Kaki Paket." },
  { "en": "Apa Itu 'Flip-Chip'?", "id": "Pemasangan Die Terbalik Ke Substrat." },
  { "en": "Apa Itu 'Substrat Paket'?", "id": "Papan Sirkuit Kecil Tempat Die." },
  { "en": "Apa Itu 'Enkapsulasi'?", "id": "Melapisi Die Dengan Bahan Pelindung." },
  { "en": "Apa Bahan Untuk 'Enkapsulasi'?", "id": "Plastik Epoksi Atau Keramik." },
  { "en": "Apa Itu 'Leadframe'?", "id": "Kerangka Logam Kaki-kaki Paket." },
  { "en": "Apa Itu 'Pengujian Wafer'?", "id": "Pengujian Die Saat Masih Di Wafer." },
  { "en": "Apa Itu 'Probe Card'?", "id": "Kartu Jarum Untuk Pengujian Wafer." },
  { "en": "Apa Itu 'Pengujian Akhir (Final Test)'?", "id": "Pengujian Chip Setelah Dikemas." },
  { "en": "Apa Itu 'Yield'?", "id": "Persentase Chip Baik Dari Satu Wafer." },
  { "en": "Apa Itu 'Fab' Atau 'Foundry'?", "id": "Pabrik Pembuatan Semikonduktor." },
  { "en": "Apa Itu 'Epitaksi'?", "id": "Pertumbuhan Lapisan Kristal Di Atas Substrat." },
  { "en": "Apa Kepanjangan 'MBE (Molecular Beam Epitaxy)'?", "id": "Epitaksi Berkas Molekuler." },
  { "en": "Apa Itu 'Polisilicon'?", "id": "Silikon (Si) Polikristalin." },
  { "en": "Apa Fungsi 'Polisilicon' Dalam MOSFET?", "id": "Sebagai Material Gerbang (Gate)." },
  { "en": "Apa Itu 'Oksida Gerbang (Gate Oxide)'?", "id": "Lapisan Isolator Tipis Di Bawah Gerbang." },
  { "en": "Apa Itu 'Kontak Ohmik'?", "id": "Sambungan Resistansi Rendah Logam-Semikonduktor." },
  { "en": "Apa Itu 'Silisida'?", "id": "Senyawa Silikon (Si) Dan Logam." },
  { "en": "Apa Fungsi 'Silisida'?", "id": "Mengurangi Resistansi Kontak." },
  { "en": "Apa Itu 'Passivasi'?", "id": "Pelapisan Lapisan Pelindung Terakhir." },
  { "en": "Lapisan Apa Untuk 'Passivasi'?", "id": "Silikon Nitrida (Si₃N₄)." },
  { "en": "Apa Itu 'Trench Isolation'?", "id": "Isolasi Antar Transistor Dengan Parit." },
  { "en": "Apa Kepanjangan 'STI (Shallow Trench Isolation)'?", "id": "Isolasi Parit Dangkal." },
  { "en": "Apa Itu 'Layout'?", "id": "Gambar Geometris Desain Sirkuit." },
  { "en": "Apa Kepanjangan 'EDA (Electronic Design Automation)'?", "id": "Otomatisasi Desain Elektronik." },
  { "en": "Apa Itu 'PDK (Process Design Kit)'?", "id": "Informasi Proses Fabrikasi Untuk Desainer." },
  { "en": "Bagaimana 'LED (Light Emitting Diode)' Dibuat?", "id": "Menggunakan Semikonduktor Celah Pita Langsung." },
  { "en": "Bahan Apa Untuk 'LED (Light Emitting Diode)' Biru?", "id": "Galium Nitrida (GaN)." },
  { "en": "Apa Itu 'Sputtering'?", "id": "Teknik PVD (Physical Vapor Deposition) Untuk Deposisi." },
  { "en": "Bagaimana Cara Membuat 'Koil Induktor'?", "id": "Dengan Membentuk Pola Logam Spiral." },
  { "en": "Apa Itu 'Resistor Film Tipis'?", "id": "Resistor Dibuat Dari Lapisan Tipis." },
  { "en": "Apa Itu 'Resistor Film Tebal'?", "id": "Resistor Dibuat Dari Pasta Konduktif." },
  { "en": "Apa Itu 'Through-Hole Technology'?", "id": "Komponen Dengan Kaki Menembus PCB." },
  { "en": "Apa Kepanjangan 'SMT (Surface-Mount Technology)'?", "id": "Teknologi Pemasangan Permukaan." },
  { "en": "Apa Itu 'Solder Paste'?", "id": "Campuran Timah Solder Dan Flux." },
  { "en": "Apa Itu 'Reflow Oven'?", "id": "Oven Untuk Melelehkan Solder Paste." },
  { "en": "Apa Itu 'Pick-and-Place Machine'?", "id": "Mesin Pemasang Komponen SMT (Surface-Mount Technology)." },
  { "en": "Apa Itu 'PCB (Printed Circuit Board)'?", "id": "Papan Sirkuit Tercetak." },
  { "en": "Bahan Apa Untuk 'PCB (Printed Circuit Board)'?", "id": "FR-4 (Fiberglass-Epoxy)." },
  { "en": "Apa Itu 'Jalur (Trace)' PCB?", "id": "Jalur Tembaga Penghubung Komponen." },
  { "en": "Apa Itu 'Via'?", "id": "Lubang Konduktif Antar Lapisan PCB." },
  { "en": "Apa Itu 'Solder Mask'?", "id": "Lapisan Pelindung Jalur Tembaga." },
  { "en": "Apa Itu 'Silkscreen'?", "id": "Label Teks Dan Simbol Di PCB." },
  { "en": "Apa Itu 'Prototipe PCB'?", "id": "Pembuatan PCB Dalam Jumlah Kecil." },
  { "en": "Apa Itu 'Manufaktur Aditif'?", "id": "Proses Membangun Objek Lapis Demi Lapis." },
  { "en": "Apa Itu '3D Printing'?", "id": "Contoh Manufaktur Aditif." },
  { "en": "Apa Itu 'Manufaktur Subtraktif'?", "id": "Proses Membuang Material Dari Blok." },
  { "en": "Contoh 'Manufaktur Subtraktif'?", "id": "Mesin CNC (Computer Numerical Control)." },
  { "en": "Apa Itu 'Layar Sentuh Kapasitif'?", "id": "Layar Berbasis Lapisan Konduktif." },
  { "en": "Bahan Apa Untuk 'Layar Kapasitif'?", "id": "Indium Tin Oxide (ITO)." },
  { "en": "Apa Itu 'Indium Tin Oxide (ITO)'?", "id": "Oksida Konduktif Yang Transparan." },
  { "en": "Bagaimana 'Sel Surya' Dibuat?", "id": "Dengan Membuat Sambungan P-N Area Besar." },
  { "en": "Apa Itu 'Lapisan Anti-Refleksi'?", "id": "Lapisan Untuk Mengurangi Pantulan Cahaya." },
  { "en": "Bahan Apa Untuk 'Lapisan Anti-Refleksi'?", "id": "Silikon Nitrida (Si₃N₄)." },
  { "en": "Apa Itu 'Kontak Depan' Sel Surya?", "id": "Jaringan Logam Tipis Di Permukaan." },
  { "en": "Apa Itu 'Kontak Belakang' Sel Surya?", "id": "Lapisan Logam Penuh Di Belakang." },
  { "en": "Bagaimana 'Kabel' Dibuat?", "id": "Dengan Menarik Logam Melalui Cetakan." },
  { "en": "Apa Itu 'Isolasi Kabel'?", "id": "Bahan Non-Konduktif Pembungkus Kabel." },
  { "en": "Bahan Apa Untuk 'Isolasi Kabel'?", "id": "PVC (Polyvinyl Chloride) Atau Karet." },
  { "en": "Apa Itu 'Fotolitografi Ekstrem UV (EUV)'?", "id": "Litografi Menggunakan Cahaya EUV (Extreme Ultraviolet)." },
  { "en": "Mengapa 'Litografi EUV (Extreme Ultraviolet)' Penting?", "id": "Untuk Membuat Transistor Sangat Kecil." },
  { "en": "Apa Itu 'FinFET'?", "id": "Transistor Dengan Kanal Berbentuk Sirip." },
  { "en": "Bagaimana Cara Membuat 'Fin' Pada FinFET?", "id": "Dengan Proses Etsa Anisotropik." },
  { "en": "Apa Itu 'Gate-All-Around (GAA) FET'?", "id": "Gerbang Mengelilingi Kanal Dari Semua Sisi." },
  { "en": "Bagaimana 'Kawat Nano' Dibuat?", "id": "Melalui Proses Etsa Atau Pertumbuhan." },
  { "en": "Apa Itu 'Quantum Dot'?", "id": "Kristal Nano Semikonduktor." },
  { "en": "Bagaimana Cara Membuat 'Quantum Dot'?", "id": "Melalui Epitaksi Atau Sintesis Kimia." },
  { "en": "Apa Itu 'Manufaktur MEMS (Microelectromechanical Systems)'?", "id": "Pembuatan Perangkat Mekanis Mikro." },
  { "en": "Apa Itu 'Micromachining'?", "id": "Teknik Etsa Untuk Membentuk Struktur MEMS." },
  { "en": "Apa Itu 'Bulk Micromachining'?", "id": "Mengetsa Sebagian Besar Substrat Wafer." },
  { "en": "Apa Itu 'Surface Micromachining'?", "id": "Membangun Struktur Lapis Demi Lapis." },
  { "en": "Apa Itu 'LIGA'?", "id": "Proses Untuk Membuat Struktur Tinggi." },
  { "en": "Bagaimana 'Akselerometer MEMS' Dibuat?", "id": "Dengan Membuat Struktur Massa-Pegas Mikro." },
  { "en": "Apa Itu 'Atomic Layer Deposition (ALD)'?", "id": "Deposisi Material Satu Lapisan Atom." },
  { "en": "Apa Keuntungan 'ALD (Atomic Layer Deposition)'?", "id": "Kontrol Ketebalan Yang Sangat Presisi." },
  { "en": "Apa Itu 'Dielectric High-k'?", "id": "Material Isolator Dengan Konstanta Tinggi." },
  { "en": "Bahan Apa Untuk 'Dielectric High-k'?", "id": "Hafnium Oksida (HfO₂)." },
  { "en": "Apa Itu 'Gerbang Logam (Metal Gate)'?", "id": "Penggunaan Logam Sebagai Elektroda Gerbang." },
  { "en": "Mengapa Beralih Ke 'Gerbang Logam'?", "id": "Untuk Mengatasi Masalah Deplesi Polisilicon." },
  { "en": "Apa Itu 'Strained Silicon'?", "id": "Silikon (Si) Dengan Kisi Diregangkan." },
  { "en": "Apa Tujuan 'Strained Silicon'?", "id": "Meningkatkan Mobilitas Elektron Dan Lubang." },
  { "en": "Apa Itu 'Epitaksi Selektif'?", "id": "Pertumbuhan Epitaksi Hanya Di Area Tertentu." },
  { "en": "Apa Itu 'Wafer Bonding'?", "id": "Proses Menggabungkan Dua Wafer." },
  { "en": "Apa Kepanjangan 'SOI (Silicon-On-Insulator)'?", "id": "Silikon (Si) Di Atas Isolator." },
  { "en": "Bagaimana Cara Membuat Wafer 'SOI'?", "id": "Dengan Wafer Bonding Atau Implantasi Oksigen." },
  { "en": "Apa Itu 'Wafer Reclaim'?", "id": "Proses Mendaur Ulang Wafer Uji." },
  { "en": "Apa Itu 'Gettering'?", "id": "Proses Menghilangkan Kontaminasi Dari Wafer." },
  { "en": "Apa Itu 'Sputter Deposition'?", "id": "Teknik PVD (Physical Vapor Deposition) Menembakkan Argon." },
  { "en": "Apa Itu 'Evaporasi Termal'?", "id": "Deposisi Dengan Menguapkan Material." },
  { "en": "Apa Itu 'Plasma Etching'?", "id": "Etching Menggunakan Gas Reaktif Terionisasi." },
  { "en": "Apa Kepanjangan 'RIE (Reactive Ion Etching)'?", "id": "Etsa Ion Reaktif." },
  { "en": "Apa Keuntungan 'RIE (Reactive Ion Etching)'?", "id": "Hasil Etsa Vertikal (Anisotropik)." },
  { "en": "Apa Itu 'Chemical Downstream Etching'?", "id": "Etching Menggunakan Spesies Reaktif Jarak Jauh." },
  { "en": "Apa Itu 'Ion Milling'?", "id": "Etching Fisik Menggunakan Berkas Ion." },
  { "en": "Apa Itu 'Lift-off'?", "id": "Teknik Patterning Dengan Melarutkan Resist." },
  { "en": "Apa Itu 'Damascene Process'?", "id": "Proses Untuk Membuat Interkoneksi Tembaga." },
  { "en": "Apa Itu 'Dual Damascene'?", "id": "Membuat Parit Dan Via Sekaligus." },
  { "en": "Apa Itu 'Batas Butir (Grain Boundary)'?", "id": "Antarmuka Antara Butiran Kristal." },
  { "en": "Apa Itu 'Polikristalin'?", "id": "Material Yang Terdiri Dari Banyak Butir." },
  { "en": "Apa Itu 'Amorf'?", "id": "Material Tanpa Struktur Kristal Teratur." },
  { "en": "Apa Itu 'Rekristalisasi'?", "id": "Proses Pembentukan Kembali Butiran Kristal." },
  { "en": "Bagaimana 'Resistor Film Tebal' Dibuat?", "id": "Dengan Sablon Pasta Resistif." },
  { "en": "Bagaimana 'Kapasitor Keramik' Dibuat?", "id": "Dengan Menumpuk Lapisan Keramik-Logam." },
  { "en": "Apa Kepanjangan 'MLCC (Multi-layer Ceramic Capacitor)'?", "id": "Kapasitor Keramik Multi Lapis." },
  { "en": "Bagaimana 'Induktor Inti Ferit' Dibuat?", "id": "Melilitkan Kawat Pada Inti Ferit." },
  { "en": "Apa Itu 'Sintering'?", "id": "Proses Memadatkan Material Dengan Panas." },
  { "en": "Bagaimana Cara Membuat 'Magnet Permanen'?", "id": "Dengan Sintering Dan Proses Medan Magnet." },
  { "en": "Apa Itu 'Die Casting'?", "id": "Proses Mencetak Logam Cair." },
  { "en": "Bagaimana 'Heatsink Aluminium' Dibuat?", "id": "Melalui Proses Ekstrusi Atau Casting." },
  { "en": "Apa Itu 'Ekstrusi'?", "id": "Mendorong Material Melalui Cetakan." },
  { "en": "Bagaimana 'Kabel Tembaga' Dibuat?", "id": "Melalui Proses Penarikan (Drawing)." },
  { "en": "Apa Itu 'Electroplating'?", "id": "Pelapisan Logam Menggunakan Arus Listrik." },
  { "en": "Bagaimana 'Konektor Berlapis Emas' Dibuat?", "id": "Dengan Proses Electroplating Emas." },
  { "en": "Apa Itu 'Injection Molding'?", "id": "Mencetak Plastik Cair Ke Dalam Cetakan." },
  { "en": "Bagaimana 'Badan IC Plastik' Dibuat?", "id": "Dengan Proses Injection Molding." },
  { "en": "Apa Itu 'Printed Electronics'?", "id": "Mencetak Sirkuit Menggunakan Tinta Konduktif." },
  { "en": "Apa Itu 'Tinta Konduktif'?", "id": "Tinta Mengandung Partikel Perak Atau Karbon." },
  { "en": "Bagaimana 'Sensor Fleksibel' Dibuat?", "id": "Dengan Mencetak Di Atas Substrat Fleksibel." },
  { "en": "Apa Itu 'Manufaktur Roll-to-Roll'?", "id": "Proses Produksi Kontinu Berbasis Gulungan." },
  { "en": "Bagaimana 'Layar OLED (Organic LED)' Dibuat?", "id": "Deposisi Uap Material Organik." },
  { "en": "Apa Itu 'Fine Metal Mask (FMM)'?", "id": "Masker Logam Untuk Deposisi OLED." },
  { "en": "Apa Itu 'Backplane' Layar?", "id": "Sirkuit Transistor Pengontrol Piksel." },
  { "en": "Apa Kepanjangan 'TFT (Thin-Film Transistor)'?", "id": "Transistor Film Tipis." },
  { "en": "Bagaimana 'Panel Sentuh' Dibuat?", "id": "Melapisi Kaca Dengan Material Transparan." },
  { "en": "Bahan Apa Untuk 'Panel Sentuh'?", "id": "ITO (Indium Tin Oxide)." },
  { "en": "Apa Itu 'Laser Scriber'?", "id": "Laser Untuk Memotong Atau Membuat Pola." },
  { "en": "Bagaimana 'Serat Optik' Dibuat?", "id": "Menarik Kaca Panas Dari Preform." },
  { "en": "Apa Itu 'Preform' Serat Optik?", "id": "Batangan Kaca Besar Sumber Serat." },
  { "en": "Apa Itu 'Doping' Pada Serat Optik?", "id": "Menambah Germanium Untuk Ubah Indeks Bias." },
  { "en": "Bagaimana 'Kamera Lensa' Dibuat?", "id": "Dengan Menggiling Dan Memoles Kaca." },
  { "en": "Apa Itu 'Coating' Lensa?", "id": "Lapisan Tipis Untuk Mengurangi Refleksi." },
  { "en": "Bagaimana 'Sensor Gambar CCD' Dibuat?", "id": "Proses Fabrikasi Mirip IC (Integrated Circuit)." },
  { "en": "Apa Itu 'Microlens Array'?", "id": "Lensa Mikro Di Atas Tiap Piksel." },
  { "en": "Apa Itu 'Filter Warna' Sensor Gambar?", "id": "Filter Merah, Hijau, Biru (RGB)." },
  { "en": "Pola Apa Yang Digunakan 'Filter Warna'?", "id": "Pola Bayer." },
  { "en": "Bagaimana 'Mikrofon MEMS' Dibuat?", "id": "Dengan Surface Micromachining." },
  { "en": "Apa Itu 'Diafragma' Mikrofon?", "id": "Membran Tipis Yang Bergetar." },
  { "en": "Bagaimana 'Speaker' Dibuat?", "id": "Merakit Magnet, Koil, Dan Kerucut." },
  { "en": "Apa Itu 'Voice Coil'?", "id": "Koil Kawat Yang Bergerak." },
  { "en": "Bagaimana 'Baterai Lithium-Ion' Dibuat?", "id": "Melapisi Elektroda Dan Merakit Sel." },
  { "en": "Apa Itu 'Elektroda' Baterai?", "id": "Anoda Dan Katoda." },
  { "en": "Apa Itu 'Elektrolit' Baterai?", "id": "Medium Penghantar Ion Antar Elektroda." },
  { "en": "Apa Itu 'Separator'?", "id": "Membran Pemisah Anoda Dan Katoda." },
  { "en": "Bagaimana 'Panel Surya' Dirakit?", "id": "Menyambung Sel Surya Secara Seri-Paralel." },
  { "en": "Apa Itu 'Laminasi' Panel Surya?", "id": "Menyatukan Lapisan Panel Dengan Panas." },
  { "en": "Apa Itu 'Junction Box'?", "id": "Kotak Koneksi Kabel Di Panel Surya." },
  { "en": "Bagaimana 'Heat Pipe' Dibuat?", "id": "Mengisi Pipa Dengan Cairan Kerja." },
  { "en": "Apa Itu 'Sumbu (Wick)' Heat Pipe?", "id": "Struktur Untuk Transportasi Cairan." },
  { "en": "Bagaimana 'Kristal Kuarsa' Dibuat?", "id": "Tumbuh Secara Hidrotermal." },
  { "en": "Bagaimana 'Osilator Kristal' Dibuat?", "id": "Memotong Kristal Pada Sudut Tepat." },
  { "en": "Apa Itu 'Lapping'?", "id": "Proses Penghalusan Permukaan Presisi." },
  { "en": "Apa Itu 'Polishing'?", "id": "Proses Membuat Permukaan Sangat Halus." },
  { "en": "Apa Itu 'Metrologi'?", "id": "Ilmu Pengukuran." },
  { "en": "Apa Itu Desain 'RTL (Register-Transfer Level)'?", "id": "Mendeskripsikan Aliran Data Antar Register." },
  { "en": "Apa Itu 'Sintesis Logika'?", "id": "Mengubah Kode RTL Menjadi Gerbang Logika." },
  { "en": "Apa Itu 'Place and Route'?", "id": "Menempatkan Dan Menghubungkan Gerbang Logika." },
  { "en": "Apa Tujuan 'Analisis Waktu Statis (STA)'?", "id": "Memverifikasi Waktu Tunda Sirkuit." },
  { "en": "Apa Itu 'Verifikasi' Dalam Desain Chip?", "id": "Memastikan Desain Berfungsi Dengan Benar." },
  { "en": "Apa Kepanjangan 'SiP (System-in-Package)'?", "id": "Sistem Dalam Sebuah Paket." },
  { "en": "Bagaimana Komponen Disusun Dalam 'SiP'?", "id": "Beberapa Die Berbeda Dalam Satu Paket." },
  { "en": "Apa Kepanjangan 'PoP (Package-on-Package)'?", "id": "Paket Yang Ditumpuk Di Atas Paket." },
  { "en": "Apa Itu 'Interposer'?", "id": "Substrat Perantara Untuk Menghubungkan Chip." },
  { "en": "Apa Itu 'Analisis Kegagalan'?", "id": "Mencari Penyebab Kerusakan Komponen." },
  { "en": "Apa Itu 'Kurva Bak Mandi (Bathtub Curve)'?", "id": "Grafik Laju Kegagalan Seiring Waktu." },
  { "en": "Apa Itu 'Infant Mortality'?", "id": "Tingkat Kegagalan Tinggi Di Awal." },
  { "en": "Apa Itu 'Pengujian Dipercepat'?", "id": "Simulasi Penuaan Komponen Secara Cepat." },
  { "en": "Bagaimana Cara Membuat 'Resistor Film Tipis'?", "id": "Deposisi Lapisan Resistif Sangat Tipis." },
  { "en": "Apa Beda Proses 'Czochralski' Dan 'Float-Zone'?", "id": "Float-Zone Hasilkan Silikon (Si) Lebih Murni." },
  { "en": "Apa Kepanjangan 'SiGe (Silicon-Germanium)'?", "id": "Paduan Silikon (Si) Dan Germanium (Ge)." },
  { "en": "Apa Fungsi Substrat 'Safir'?", "id": "Untuk Membuat LED (Light Emitting Diode) Biru." },
  { "en": "Apa Itu 'Photoresist Positif'?", "id": "Area Terekspos Cahaya Menjadi Larut." },
  { "en": "Apa Itu 'Photoresist Negatif'?", "id": "Area Terekspos Cahaya Menjadi Keras." },
  { "en": "Apa Itu 'Pengujian Parametrik'?", "id": "Mengukur Karakteristik Listrik Transistor." },
  { "en": "Apa Itu 'Wafer Sort' Atau 'Die Sort'?", "id": "Memilah Die Baik Dan Buruk." },
  { "en": "Apa Itu 'Pengujian Burn-in'?", "id": "Mengoperasikan Chip Pada Suhu Tinggi." },
  { "en": "Apa Tujuan 'Pengujian Burn-in'?", "id": "Menyaring Kegagalan Awal (Infant Mortality)." },
  { "en": "Apa Kepanjangan 'SPI (Solder Paste Inspection)'?", "id": "Inspeksi Pasta Solder." },
  { "en": "Apa Kepanjangan 'AOI (Automated Optical Inspection)'?", "id": "Inspeksi Optik Otomatis." },
  { "en": "Apa Fungsi 'AOI (Automated Optical Inspection)'?", "id": "Memeriksa Kesalahan Pemasangan Komponen." },
  { "en": "Apa Itu 'Flying Probe Test'?", "id": "Pengujian PCB (Printed Circuit Board) Tanpa Fixture." },
  { "en": "Apa Itu 'In-Circuit Test (ICT)'?", "id": "Pengujian Tiap Komponen Di Papan Sirkuit." },
  { "en": "Apa Itu 'Functional Test (FCT)'?", "id": "Pengujian Fungsi Papan Sirkuit Lengkap." },
  { "en": "Apa Itu 'Conformal Coating'?", "id": "Lapisan Pelindung Tipis Untuk PCB." },
  { "en": "Apa Tujuan 'Conformal Coating'?", "id": "Melindungi Dari Kelembaban Dan Korosi." },
  { "en": "Apa Itu 'Potting'?", "id": "Mengisi Selungkup Dengan Resin Pelindung." },
  { "en": "Bagaimana 'Sekring (Fuse)' Dibuat?", "id": "Dengan Kawat Titik Lebur Rendah." },
  { "en": "Apa Itu 'Elemen Sekring'?", "id": "Kawat Yang Meleleh Saat Arus Berlebih." },
  { "en": "Bagaimana 'Varistor' Dibuat?", "id": "Dengan Sintering Keramik Oksida Logam." },
  { "en": "Bahan Utama 'MOV (Metal-Oxide Varistor)'?", "id": "Seng Oksida (ZnO)." },
  { "en": "Bagaimana 'Termistor' Dibuat?", "id": "Dari Bahan Oksida Logam Semikonduktor." },
  { "en": "Apa Beda Pembuatan 'NTC' Dan 'PTC'?", "id": "Beda Dalam Komposisi Material Oksida." },
  { "en": "Bagaimana 'Sensor Efek Hall' Dibuat?", "id": "Dengan Fabrikasi Lapisan Semikonduktor Tipis." },
  { "en": "Apa Itu 'Cleanroom Class 10'?", "id": "Maksimal 10 Partikel Per Kaki Kubik." },
  { "en": "Apa Itu 'Bunny Suit'?", "id": "Pakaian Pelindung Untuk Masuk Cleanroom." },
  { "en": "Mengapa Lampu Di Cleanroom Kuning?", "id": "Agar Tidak Mengekspos Photoresist." },
  { "en": "Apa Itu 'Yield Enhancement'?", "id": "Upaya Peningkatan Persentase Produk Bagus." },
  { "en": "Apa Itu 'Process Control'?", "id": "Memantau Dan Mengontrol Parameter Fabrikasi." },
  { "en": "Apa Itu 'Statistical Process Control (SPC)'?", "id": "Kontrol Proses Menggunakan Metode Statistik." },
  { "en": "Apa Itu 'Ionizer' Di Cleanroom?", "id": "Menghilangkan Muatan Statis Dari Udara." },
  { "en": "Apa Itu 'HEPA (High-Efficiency Particulate Air) Filter'?", "id": "Filter Udara Dengan Efisiensi Sangat Tinggi." },
  { "en": "Apa Itu 'Lithography Stepper'?", "id": "Mesin Proyeksi Pola Ke Wafer." },
  { "en": "Apa Itu 'Wafer Chuck'?", "id": "Meja Vakum Yang Memegang Wafer." },
  { "en": "Apa Itu 'Spin Coating'?", "id": "Melapisi Wafer Dengan Memutarnya Cepat." },
  { "en": "Material Apa Yang Dilapisi 'Spin Coating'?", "id": "Photoresist." },
  { "en": "Apa Itu 'Soft Bake'?", "id": "Pemanasan Awal Untuk Menguapkan Pelarut." },
  { "en": "Apa Itu 'Exposure'?", "id": "Proses Penyinaran Wafer Dengan Cahaya UV." },
  { "en": "Apa Itu 'Post-Exposure Bake (PEB)'?", "id": "Pemanasan Setelah Penyinaran." },
  { "en": "Apa Itu 'Development'?", "id": "Melarutkan Photoresist Untuk Membentuk Pola." },
  { "en": "Apa Itu 'Hard Bake'?", "id": "Pemanasan Akhir Untuk Mengeraskan Resist." },
  { "en": "Apa Itu 'Stripping' Atau 'Ashing'?", "id": "Proses Menghilangkan Photoresist." },
  { "en": "Apa Itu 'Dopant Activation'?", "id": "Mengaktifkan Atom Dopan Setelah Implantasi." },
  { "en": "Apa Itu 'Silikon Amorf Terhidrogenasi'?", "id": "Silikon Amorf Dengan Ikatan Hidrogen." },
  { "en": "Bagaimana 'Kaca' Dibuat?", "id": "Melelehkan Pasir Silika Pada Suhu Tinggi." },
  { "en": "Bagaimana 'Layar LCD (Liquid Crystal Display)' Dirakit?", "id": "Menumpuk Filter Polarisasi, Kaca, Kristal." },
  { "en": "Apa Itu 'Kristal Cair'?", "id": "Fasa Materi Antara Cair Dan Padat." },
  { "en": "Bagaimana 'Filter Polarisasi' Dibuat?", "id": "Dengan Merenggangkan Film Polimer." },
  { "en": "Bagaimana 'LED Organik (OLED)' Dibuat?", "id": "Deposisi Uap Lapisan Organik Tipis." },
  { "en": "Apa Itu 'Substrat Fleksibel'?", "id": "Bahan Dasar Tipis Yang Bisa Ditekuk." },
  { "en": "Contoh 'Substrat Fleksibel'?", "id": "Poliimida (Polyimide)." },
  { "en": "Bagaimana 'Panel E-Ink' Dibuat?", "id": "Mengisi Kapsul Mikro Dengan Partikel." },
  { "en": "Apa Isi Kapsul 'E-Ink'?", "id": "Partikel Putih Dan Hitam Bermuatan." },
  { "en": "Bagaimana 'Serat Karbon' Dibuat?", "id": "Memanaskan Polimer Hingga Menjadi Karbon." },
  { "en": "Bagaimana 'Magnet Neodymium' Dibuat?", "id": "Metalurgi Serbuk (Sintering)." },
  { "en": "Apa Itu 'Anodizing'?", "id": "Proses Oksidasi Elektrolitik Pada Logam." },
  { "en": "Apa Tujuan 'Anodizing'?", "id": "Meningkatkan Ketahanan Korosi Dan Tampilan." },
  { "en": "Bagaimana 'Konektor USB' Dibuat?", "id": "Stamping Logam Dan Injection Molding." },
  { "en": "Apa Itu 'Stamping' Logam?", "id": "Mencetak Bentuk Dari Lembaran Logam." },
  { "en": "Bagaimana 'Motor Brushless' Dibuat?", "id": "Melilitkan Kumparan Dan Memasang Magnet." },
  { "en": "Apa Itu 'Laminasi' Inti Motor?", "id": "Menumpuk Pelat Logam Tipis Terisolasi." },
  { "en": "Apa Tujuan 'Laminasi' Inti?", "id": "Mengurangi Kerugian Arus Eddy." },
  { "en": "Apa Itu 'Arus Eddy'?", "id": "Arus Sirkulasi Yang Timbul Di Konduktor." },
  { "en": "Bagaimana 'Saklar Mekanis' Dibuat?", "id": "Merakit Kontak Logam Dan Pegas." },
  { "en": "Bagaimana 'Potensiometer' Dibuat?", "id": "Melapisi Substrat Dengan Elemen Resistif." },
  { "en": "Bahan Apa Untuk 'Elemen Resistif'?", "id": "Karbon Atau Cermet." },
  { "en": "Bagaimana 'Quartz Crystal' Dipanen?", "id": "Dibuat Sintetis Atau Ditambang." },
  { "en": "Mengapa Sudut Potongan 'Kristal' Penting?", "id": "Menentukan Frekuensi Dan Stabilitas Suhu." },
  { "en": "Bagaimana 'Superkapasitor' Dibuat?", "id": "Menggunakan Elektroda Karbon Aktif Area Luas." },
  { "en": "Apa Itu 'Karbon Aktif'?", "id": "Karbon Dengan Porositas Sangat Tinggi." },
  { "en": "Bagaimana 'Sel Bahan Bakar' Dirakit?", "id": "Menumpuk Elektroda Dan Membran Elektrolit." },
  { "en": "Apa Itu 'Catalyst Layer'?", "id": "Lapisan Katalis Pemicu Reaksi Kimia." },
  { "en": "Bahan Katalis Apa Yang Umum?", "id": "Platinum (Pt)." },
  { "en": "Bagaimana 'Sensor Gas' Dibuat?", "id": "Deposisi Material Peka Gas." },
  { "en": "Apa Prinsip Kerja 'Sensor Gas'?", "id": "Perubahan Konduktivitas Saat Gas Terdeteksi." },
  { "en": "Bagaimana 'Tabung Vakum' Dibuat?", "id": "Merakit Elektroda Dalam Selungkup Kaca." },
  { "en": "Apa Itu 'Getter' Dalam Tabung Vakum?", "id": "Bahan Yang Menyerap Sisa Gas." },
  { "en": "Bagaimana 'Filamen Pijar' Dibuat?", "id": "Menarik Kawat Tungsten Sangat Tipis." },
  { "en": "Mengapa Lampu Pijar Diisi Gas Inert?", "id": "Untuk Mencegah Filamen Terbakar." },
  { "en": "Bagaimana 'Tabung Neon' Dibuat?", "id": "Mengisi Tabung Kaca Dengan Gas Neon." },
  { "en": "Bagaimana 'Lampu Fluorescent' Bekerja?", "id": "Gas Melepaskan UV, Fosfor Pancarkan Cahaya." },
  { "en": "Apa Itu 'Fosfor'?", "id": "Bahan Yang Berpendar Saat Terkena UV." },
  { "en": "Apa Fungsi 'Lapisan Penghalang (Barrier Layer)'?", "id": "Mencegah Difusi Tembaga (Cu)." },
  { "en": "Mengapa 'Litografi Imersi' Digunakan?", "id": "Untuk Meningkatkan Resolusi Pola." },
  { "en": "Cairan Apa Yang Digunakan 'Litografi Imersi'?", "id": "Air Deionisasi Sangat Murni." },
  { "en": "Bagaimana 'Kapasitor Trench DRAM' Dibuat?", "id": "Dengan Menggali Parit Dalam Silikon (Si)." },
  { "en": "Apa Itu 'Binning'?", "id": "Mengelompokkan Chip Berdasarkan Kinerja." },
  { "en": "Bagaimana 'HEMT (High-Electron-Mobility Transistor)' Dibuat?", "id": "Dengan Struktur Heterojunction." },
  { "en": "Bagaimana 'IGBT (Insulated-Gate Bipolar Transistor)' Dibuat?", "id": "Gabungan Struktur BJT Dan MOSFET." },
  { "en": "Apa Itu 'Manajemen Termal'?", "id": "Mengontrol Panas Yang Dihasilkan Komponen." },
  { "en": "Bagaimana 'Sensor Tekanan Kapasitif' Bekerja?", "id": "Mengukur Perubahan Jarak Antar Pelat." },
  { "en": "Apa Itu 'Orientasi Kristal' Wafer?", "id": "Arah Bidang Atom Kristal." },
  { "en": "Apa Arti Orientasi '<100>'?", "id": "Orientasi Umum Untuk Fabrikasi CMOS." },
  { "en": "Apa Fungsi 'Wafer Flat' Atau 'Notch'?", "id": "Menandai Orientasi Kristal Wafer." },
  { "en": "Apa Itu 'Wafer Dicing Tape'?", "id": "Pita Perekat Untuk Memegang Die." },
  { "en": "Apa Itu 'Die Attach Adhesive'?", "id": "Perekat Untuk Melekatkan Die." },
  { "en": "Apa Fungsi 'Underfill'?", "id": "Memperkuat Koneksi Bola Solder." },
  { "en": "Apa Itu 'Handler' Pengujian Akhir?", "id": "Mesin Otomatis Penanganan Chip Uji." },
  { "en": "Bagaimana 'Kapasitor Elektrolit' Dibuat?", "id": "Menggulung Foil Anoda, Katoda, Separator." },
  { "en": "Apa Itu 'Anodizing' Foil Aluminium?", "id": "Menumbuhkan Lapisan Oksida Dielektrik." },
  { "en": "Bagaimana 'Resistor Karbon' Dibuat?", "id": "Melapisi Batang Keramik Dengan Karbon." },
  { "en": "Bagaimana 'Resistor Metal Film' Dibuat?", "id": "Deposisi Vakum Lapisan Logam Tipis." },
  { "en": "Apa Itu 'Laser Trimming'?", "id": "Menyesuaikan Nilai Resistor Dengan Laser." },
  { "en": "Bagaimana 'Kapasitor Film' Dibuat?", "id": "Menggulung Lapisan Film Plastik Metalized." },
  { "en": "Bagaimana 'Kabel Koaksial' Dibuat?", "id": "Menyatukan Konduktor, Insulator, Pelindung." },
  { "en": "Apa Itu 'Proses Drawing' Kawat?", "id": "Menarik Logam Melalui Cetakan Kecil." },
  { "en": "Apa Itu 'Annealing' Kawat?", "id": "Melembutkan Kawat Setelah Proses Drawing." },
  { "en": "Bagaimana 'Termokopel' Dibuat?", "id": "Menyambungkan Dua Jenis Kawat Logam." },
  { "en": "Apa Itu 'Sambungan Panas' Termokopel?", "id": "Ujung Yang Mengukur Suhu." },
  { "en": "Apa Itu 'Sambungan Dingin' Termokopel?", "id": "Ujung Referensi Yang Diketahui Suhunya." },
  { "en": "Bagaimana 'Strain Gauge' Dibuat?", "id": "Membentuk Pola Foil Logam Tipis." },
  { "en": "Bagaimana 'Load Cell' Dirakit?", "id": "Menempelkan Strain Gauge Ke Struktur Logam." },
  { "en": "Apa Itu 'Wafer Bumping'?", "id": "Proses Menambahkan Bola Solder Ke Wafer." },
  { "en": "Apa Itu 'Solder Bump'?", "id": "Bola Solder Kecil Untuk Koneksi Flip-Chip." },
  { "en": "Apa Itu 'Ball Grid Array (BGA)'?", "id": "Jenis Paket IC (Integrated Circuit) Bola Solder." },
  { "en": "Bagaimana 'Paket BGA' Dibuat?", "id": "Melekatkan Die Ke Substrat Dengan Bola." },
  { "en": "Apa Itu 'Rework' Dalam Perakitan?", "id": "Proses Memperbaiki Papan Sirkuit." },
  { "en": "Apa Itu 'X-ray Inspection'?", "id": "Inspeksi Sambungan Solder Yang Tersembunyi." },
  { "en": "Mengapa 'X-ray' Digunakan Untuk BGA?", "id": "Untuk Melihat Koneksi Di Bawah Chip." },
  { "en": "Apa Itu 'Pasta Solder Bebas Timbal'?", "id": "Solder Tanpa Kandungan Timbal (Pb)." },
  { "en": "Mengapa Beralih Ke 'Solder Bebas Timbal'?", "id": "Karena Alasan Lingkungan Dan Kesehatan." },
  { "en": "Apa Kepanjangan 'RoHS'?", "id": "Restriction of Hazardous Substances." },
  { "en": "Apa Itu 'Standar IPC'?", "id": "Standar Industri Untuk Perakitan Elektronik." },
  { "en": "Apa Itu 'Proses Manufaktur Aditif'?", "id": "Membangun Komponen Lapis Demi Lapis." },
  { "en": "Apa Itu 'Antena PCB (Printed Circuit Board)'?", "id": "Antena Dibuat Dari Jalur Tembaga." },
  { "en": "Bagaimana 'Osilator MEMS' Dibuat?", "id": "Membuat Struktur Resonator Di Atas Silikon." },
  { "en": "Apa Itu 'Stiction' Dalam MEMS?", "id": "Masalah Pelekatan Antar Struktur Mikro." },
  { "en": "Apa Itu 'Getaran Kisi'?", "id": "Getaran Termal Atom Dalam Kristal." },
  { "en": "Apa Itu 'Fonon'?", "id": "Kuantum Energi Dari Getaran Kisi." },
  { "en": "Bagaimana 'Kristal Sintetis' Dibuat?", "id": "Ditumbuhkan Dalam Kondisi Laboratorium Terkontrol." },
  { "en": "Apa Itu 'Proses Hidrotermal'?", "id": "Metode Menumbuhkan Kristal Kuarsa." },
  { "en": "Apa Itu 'Autoclave'?", "id": "Bejana Tekanan Tinggi Untuk Sintesis." },
  { "en": "Apa Itu 'Seed Crystal'?", "id": "Kristal Kecil Sebagai Titik Awal." },
  { "en": "Bagaimana 'LED (Light Emitting Diode) Putih' Dibuat?", "id": "Melapisi Chip LED Biru Dengan Fosfor." },
  { "en": "Apa Itu 'Fosfor'?", "id": "Bahan Yang Mengubah Panjang Gelombang Cahaya." },
  { "en": "Apa Itu 'Chip-on-Board (COB) LED'?", "id": "Banyak Chip LED Dipasang Langsung." },
  { "en": "Apa Keuntungan 'COB (Chip-on-Board) LED'?", "id": "Cahaya Lebih Merata Dan Padat." },
  { "en": "Apa Itu 'Quantum Dot Enhancement Film (QDEF)'?", "id": "Film Untuk Meningkatkan Warna Layar LCD." },
  { "en": "Apa Itu 'Fabrikasi Semikonduktor Senyawa'?", "id": "Pembuatan Chip Dari Bahan Seperti GaAs." },
  { "en": "Apa Tantangan 'Fabrikasi Semikonduktor Senyawa'?", "id": "Wafer Lebih Rapuh Dan Mahal." },
  { "en": "Apa Itu 'Passivation Layer'?", "id": "Lapisan Pelindung Terakhir Pada Die." },
  { "en": "Apa Fungsi 'Passivation'?", "id": "Melindungi Dari Goresan Dan Kontaminasi." },
  { "en": "Apa Itu 'Wire Sawing'?", "id": "Proses Memotong Ingot Menjadi Wafer." },
  { "en": "Apa Itu 'Slurry'?", "id": "Campuran Partikel Abrasif Dan Cairan." },
  { "en": "Apa Itu 'Lapping' Wafer?", "id": "Proses Menghaluskan Permukaan Wafer." },
  { "en": "Apa Itu 'Edge Grinding'?", "id": "Membulatkan Tepi Wafer." },
  { "en": "Apa Itu 'Wafer Cleaning'?", "id": "Proses Membersihkan Wafer Dari Kontaminan." },
  { "en": "Mengapa 'Wafer Cleaning' Penting?", "id": "Kontaminan Dapat Menyebabkan Cacat Sirkuit." },
  { "en": "Apa Itu 'RCA Clean'?", "id": "Prosedur Standar Pembersihan Wafer Silikon." },
  { "en": "Apa Itu 'Mask Aligner'?", "id": "Mesin Litografi Untuk Penjajaran Mask." },
  { "en": "Apa Itu 'Proximity Lithography'?", "id": "Mask Berada Sangat Dekat Dengan Wafer." },
  { "en": "Apa Itu 'Contact Lithography'?", "id": "Mask Menyentuh Langsung Wafer." },
  { "en": "Apa Itu 'Projection Lithography'?", "id": "Pola Masker Diproyesikan Ke Wafer." },
  { "en": "Apa Itu 'Reticle'?", "id": "Nama Lain Untuk Photomask." },
  { "en": "Apa Itu 'Pellicle'?", "id": "Membran Pelindung Yang Dipasang Di Mask." },
  { "en": "Apa Fungsi 'Pellicle'?", "id": "Menjaga Debu Agar Tidak Terfokus." },
  { "en": "Apa Itu 'Critical Dimension (CD)'?", "id": "Ukuran Fitur Terkecil Dalam Desain." },
  { "en": "Apa Kepanjangan 'CD-SEM (Critical Dimension SEM)'?", "id": "Mikroskop Elektron Pengukur Dimensi Kritis." },
  { "en": "Apa Itu 'Overlay'?", "id": "Akurasi Penjajaran Antar Lapisan." },
  { "en": "Apa Itu 'Hard Mask'?", "id": "Lapisan Keras (Bukan Resist) Untuk Etsa." },
  { "en": "Mengapa 'Hard Mask' Digunakan?", "id": "Untuk Mengetsa Fitur Yang Sangat Dalam." },
  { "en": "Apa Itu 'Plasma Ashing'?", "id": "Proses Menghilangkan Photoresist Dengan Plasma." },
  { "en": "Apa Itu 'Wafer Backside Coating'?", "id": "Pelapisan Bagian Belakang Wafer." },
  { "en": "Apa Itu 'Thermal Budget'?", "id": "Total Paparan Suhu Tinggi Selama Proses." },
  { "en": "Mengapa 'Thermal Budget' Penting?", "id": "Suhu Berlebih Dapat Merusak Dopan." },
  { "en": "Apa Itu 'Rapid Thermal Anneal (RTA)'?", "id": "Proses Annealing Dengan Waktu Sangat Singkat." },
  { "en": "Apa Itu 'Furnace Annealing'?", "id": "Annealing Dalam Tungku Untuk Waktu Lama." },
  { "en": "Apa Itu 'Spreading Resistance'?", "id": "Metode Mengukur Profil Resistivitas." },
  { "en": "Apa Kepanjangan 'SIMS (Secondary Ion Mass Spectrometry)'?", "id": "Spektrometri Massa Ion Sekunder." },
  { "en": "Apa Fungsi 'SIMS'?", "id": "Mengukur Profil Konsentrasi Dopan." },
  { "en": "Apa Itu 'Process Integration'?", "id": "Menggabungkan Semua Langkah Proses Fabrikasi." },
  { "en": "Apa Itu 'Process Flow'?", "id": "Urutan Langkah-langkah Dalam Fabrikasi." },
  { "en": "Apa Itu 'Scribe Line'?", "id": "Garis Antar Die Tempat Wafer Dipotong." },
  { "en": "Apa Itu 'Test Structure'?", "id": "Struktur Tes Di Scribe Line." },
  { "en": "Apa Fungsi 'Test Structure'?", "id": "Memantau Kualitas Dan Parameter Proses." },
  { "en": "Apa Itu 'Binning' Chip?", "id": "Mengelompokkan Chip Berdasarkan Hasil Tes." },
  { "en": "Apa Itu 'Tape and Reel'?", "id": "Metode Pengemasan Komponen Untuk Perakitan." },
  { "en": "Apa Penyebab 'Elektromigrasi'?", "id": "Momentum Elektron Mendorong Atom Logam." },
  { "en": "Apa Itu 'Injeksi Pembawa Panas (HCI)'?", "id": "Elektron Energi Tinggi Merusak Oksida." },
  { "en": "Apa Itu 'Latch-up'?", "id": "Hubung Singkat Parasit Dalam CMOS." },
  { "en": "Apa Fungsi 'Through-Silicon Via (TSV)'?", "id": "Koneksi Vertikal Langsung Antar Chip." },
  { "en": "Apa Itu 'Gettering'?", "id": "Proses Menjebak Kontaminan Jauh Dari Perangkat." },
  { "en": "Apa Beda 'Polisilicon' Dan 'Silikon Kristal Tunggal'?", "id": "Polisilicon Terdiri Dari Banyak Butir Kristal." },
  { "en": "Apa Keunggulan 'Galium Arsenida (GaAs)'?", "id": "Mobilitas Elektron Sangat Tinggi." },
  { "en": "Bagaimana 'Filter SAW (Surface Acoustic Wave)' Dibuat?", "id": "Dengan Pola Elektroda Di Bahan Piezoelektrik." },
  { "en": "Apa Perbedaan Utama 'SEM' Dan 'TEM'?", "id": "SEM (Scanning Electron Microscope) Lihat Permukaan, TEM Tembus." },
  { "en": "Apa Itu 'Stencil' Dalam Perakitan PCB?", "id": "Lembaran Logam Untuk Aplikasi Pasta Solder." },
  { "en": "Apa Itu 'Profil Reflow'?", "id": "Kurva Suhu-Waktu Untuk Proses Solder." },
  { "en": "Bagaimana 'Resistor Wirewound' Dibuat?", "id": "Melilitkan Kawat Resistif Di Inti Keramik." },
  { "en": "Bagaimana 'Kapasitor Tantalum' Dibuat?", "id": "Menggunakan Butiran Tantalum Dan Oksida." },
  { "en": "Apa Itu 'Cacat Kristal'?", "id": "Ketidaksempurnaan Dalam Susunan Atom Kristal." },
  { "en": "Apa Itu 'Cacat Titik (Point Defect)'?", "id": "Cacat Yang Melibatkan Posisi Atom Tunggal." },
  { "en": "Apa Itu 'Kekosongan (Vacancy)'?", "id": "Posisi Atom Kosong Dalam Kisi." },
  { "en": "Apa Itu ' interstisial'?", "id": "Atom Tambahan Di Antara Posisi Kisi." },
  { "en": "Apa Itu 'Dislokasi'?", "id": "Cacat Garis Dalam Struktur Kristal." },
  { "en": "Apa Itu 'Wafer Doping Gradien'?", "id": "Konsentrasi Dopan Bervariasi Dengan Kedalaman." },
  { "en": "Bagaimana 'Sensor Optik' Bekerja?", "id": "Mengubah Cahaya Menjadi Sinyal Listrik." },
  { "en": "Bagaimana 'Fototransistor' Dibuat?", "id": "Struktur BJT (Bipolar Junction Transistor) Peka Cahaya." },
  { "en": "Apa Itu 'Quantum Well Infrared Photodetector (QWIP)'?", "id": "Detektor Inframerah Berbasis Struktur Kuantum." },
  { "en": "Apa Itu 'Cryogenic Cooling'?", "id": "Pendinginan Sensor Ke Suhu Sangat Rendah." },
  { "en": "Mengapa Beberapa Sensor Perlu 'Cryogenic Cooling'?", "id": "Untuk Mengurangi Derau Termal." },
  { "en": "Bagaimana 'Termokopel' Dibuat?", "id": "Mengelas Ujung Dua Jenis Logam." },
  { "en": "Apa Itu 'Manufaktur Aditif Elektronik'?", "id": "Mencetak 3D Komponen Dan Sirkuit." },
  { "en": "Apa Itu 'Tinta Perak'?", "id": "Tinta Mengandung Partikel Perak Untuk Konduksi." },
  { "en": "Apa Itu 'Substrat Keramik'?", "id": "Papan Sirkuit Berbasis Keramik Tahan Panas." },
  { "en": "Mengapa Menggunakan 'Substrat Keramik'?", "id": "Untuk Aplikasi Daya Tinggi Dan Frekuensi Tinggi." },
  { "en": "Apa Itu 'Direct Bonded Copper (DBC)'?", "id": "Tembaga (Cu) Dilekatkan Langsung Ke Keramik." },
  { "en": "Apa Itu 'Modul Daya IGBT'?", "id": "Beberapa Chip IGBT Dalam Satu Modul." },
  { "en": "Apa Itu 'Wirebond-less Packaging'?", "id": "Teknik Pengemasan Tanpa Menggunakan Kawat." },
  { "en": "Apa Itu 'Laser Annealing'?", "id": "Proses Annealing Menggunakan Energi Laser." },
  { "en": "Apa Keuntungan 'Laser Annealing'?", "id": "Pemanasan Sangat Cepat Dan Terlokalisasi." },
  { "en": "Apa Itu 'Etching Selektivitas'?", "id": "Rasio Laju Etsa Antara Dua Material." },
  { "en": "Apa Itu 'Plasma'?", "id": "Gas Terionisasi Yang Terdiri Dari Ion." },
  { "en": "Bagaimana 'Plasma' Diciptakan Dalam Fabrikasi?", "id": "Dengan Menerapkan Medan RF (Radio Frequency) Ke Gas." },
  { "en": "Apa Itu ' sputtering'?", "id": "Deposisi Fisik Dengan Menembakkan Ion." },
  { "en": "Apa Itu 'Target' Dalam Sputtering?", "id": "Material Sumber Yang Akan Dideposisikan." },
  { "en": "Apa Itu 'Wafer Boat'?", "id": "Rak Untuk Memegang Wafer Dalam Tungku." },
  { "en": "Bahan Apa Untuk 'Wafer Boat'?", "id": "Kuarsa Atau Silikon Karbida (SiC)." },
  { "en": "Apa Itu 'Fotodetektor Avalanche'?", "id": "Fotodioda Yang Beroperasi Dalam Mode Avalanche." },
  { "en": "Apa Itu 'Gain' Fotodetektor?", "id": "Peningkatan Arus Akibat Efek Internal." },
  { "en": "Bagaimana 'Dioda Varactor' Dibuat?", "id": "Dengan Profil Doping Khusus." },
  { "en": "Bagaimana 'Dioda Tunnel' Dibuat?", "id": "Dengan Tingkat Doping Sangat Tinggi." },
  { "en": "Apa Itu 'Resistansi Diferensial Negatif'?", "id": "Arus Turun Saat Tegangan Naik." },
  { "en": "Bagaimana 'Thyristor' Diaktifkan?", "id": "Dengan Memberi Pulsa Ke Terminal Gerbang." },
  { "en": "Bagaimana 'Thyristor' Dimatikan?", "id": "Dengan Membuat Arus Di Bawah Arus Tahan." },
  { "en": "Apa Itu 'Arus Latching'?", "id": "Arus Minimum Agar Thyristor Tetap On." },
  { "en": "Apa Itu 'Arus Holding'?", "id": "Arus Minimum Menjaga Thyristor On." },
  { "en": "Apa Itu 'dv/dt Rating'?", "id": "Laju Kenaikan Tegangan Maksimum." },
  { "en": "Apa Itu 'di/dt Rating'?", "id": "Laju Kenaikan Arus Maksimum." },
  { "en": "Apa Itu 'Sirkuit Snubber'?", "id": "Sirkuit Pelindung Dari dv/dt Tinggi." },
  { "en": "Bagaimana 'Sirkuit Snubber' Dibuat?", "id": "Dengan Resistor Dan Kapasitor (RC)." },
  { "en": "Bagaimana 'Relay Solid State' Dibuat?", "id": "Menggunakan Optocoupler Dan Thyristor Atau MOSFET." },
  { "en": "Apa Itu 'Zero Crossing Detection'?", "id": "Mendeteksi Saat Sinyal AC Melewati Nol." },
  { "en": "Apa Fungsi 'Zero Crossing'?", "id": "Menyalakan Beban AC Tanpa Lonjakan Arus." },
  { "en": "Apa Itu 'Soldermask Dam'?", "id": "Pemisah Soldermask Antara Kaki IC." },
  { "en": "Apa Itu 'Tenting Vias'?", "id": "Menutup Lubang Via Dengan Soldermask." },
  { "en": "Apa Itu 'Plugging Vias'?", "id": "Mengisi Lubang Via Dengan Material." },
  { "en": "Apa Itu 'Finishing Permukaan PCB'?", "id": "Pelapisan Akhir Pada Pad Tembaga." },
  { "en": "Contoh 'Finishing Permukaan'?", "id": "HASL, ENIG, OSP." },
  { "en": "Apa Kepanjangan 'HASL'?", "id": "Hot Air Solder Leveling." },
  { "en": "Apa Kepanjangan 'ENIG'?", "id": "Electroless Nickel Immersion Gold." },
  { "en": "Apa Itu 'Panelisasi'?", "id": "Menggabungkan Banyak Desain PCB Dalam Panel." },
  { "en": "Apa Itu 'V-Scoring'?", "id": "Membuat Alur Untuk Memisahkan PCB." },
  { "en": "Apa Itu 'Tab Routing'?", "id": "Memisahkan PCB Dengan Tab Kecil." },
  { "en": "Bagaimana 'Konektor' Dibuat?", "id": "Stamping Logam Dan Cetakan Plastik." },
  { "en": "Bagaimana 'Kabel Pita (Ribbon Cable)' Dibuat?", "id": "Menjajarkan Banyak Kawat Dan Melaminasi." },
  { "en": "Bagaimana 'Baterai Koin' Dibuat?", "id": "Menumpuk Elektroda Dan Separator." },
  { "en": "Bagaimana 'Antena Chip' Dibuat?", "id": "Dengan Fabrikasi Keramik Frekuensi Tinggi." },
  { "en": "Apa Itu 'LTCC (Low-Temperature Co-fired Ceramic)'?", "id": "Teknologi Keramik Untuk Modul RF." },
  { "en": "Bagaimana 'Sekring PTC (Positive Temperature Coefficient)' Bekerja?", "id": "Resistansi Naik Drastis Saat Panas." },
  { "en": "Bagaimana 'Sekring PTC' Dibuat?", "id": "Dari Polimer Konduktif." },
  { "en": "Apa Itu 'Superkonduktor Suhu Tinggi'?", "id": "Superkonduktor Di Atas Suhu Nitrogen Cair." },
  { "en": "Bagaimana 'Kawat Superkonduktor' Dibuat?", "id": "Menanam Material Dalam Matriks Logam." },
  { "en": "Apa Itu 'Manufaktur Elektronik Organik'?", "id": "Pembuatan Perangkat Elektronik Berbasis Karbon." },
  { "en": "Apa Keuntungan 'Elektronik Organik'?", "id": "Fleksibel, Transparan, Dan Murah." },
  { "en": "Apa Itu 'Spin Coating'?", "id": "Teknik Melapisi Substrat Dengan Cairan." },
  { "en": "Bagaimana 'Layar Sentuh Resistif' Dibuat?", "id": "Dua Lapisan Konduktif Dengan Pemisah." },
  { "en": "Apa Prinsip 'Layar Resistif'?", "id": "Kontak Antara Dua Lapisan Akibat Tekanan." },
  { "en": "Bagaimana 'Panel Akustik Permukaan (SAW)' Dibuat?", "id": "Dengan Transduser Di Tepi Kaca." },
  { "en": "Bagaimana 'Casing Logam' Dibuat?", "id": "Melalui Proses CNC Atau Stamping." },
  { "en": "Apa Itu 'PVD (Physical Vapor Deposition)' Untuk Coating?", "id": "Memberi Warna Dan Ketahanan Gores." },
  { "en": "Bagaimana 'Kaca Gorilla' Dibuat?", "id": "Melalui Proses Pertukaran Ion Kimia." },
  { "en": "Apa Itu 'Pertukaran Ion'?", "id": "Mengganti Ion Kecil Dengan Ion Besar." },
  { "en": "Apa Efek 'Pertukaran Ion'?", "id": "Menciptakan Tegangan Tekan Di Permukaan." },
  { "en": "Bagaimana 'Serat Zirkonia' Dibuat?", "id": "Proses Elektrospinning Polimer." },
  { "en": "Apa Itu 'Elektrospinning'?", "id": "Membuat Serat Nano Dengan Medan Listrik." },
  { "en": "Bagaimana 'Aerogel' Dibuat?", "id": "Mengeringkan Gel Sambil Jaga Strukturnya." },
  { "en": "Apa Itu 'Pengeringan Superkritis'?", "id": "Metode Kunci Dalam Pembuatan Aerogel." },
  { "en": "Apa Itu 'Fabrikasi Aditif'?", "id": "Nama Lain Untuk Manufaktur Aditif." },
  { "en": "Apa Itu 'Fabrikasi Subtraktif'?", "id": "Nama Lain Untuk Manufaktur Subtraktif." },
  { "en": "Apa Itu 'Wafer Backgrinding'?", "id": "Proses Menipiskan Wafer Setelah Fabrikasi." },
  { "en": "Mengapa Wafer Perlu Ditipiskan?", "id": "Untuk Paket Tipis Dan Manajemen Termal." },
  { "en": "Apa Itu 'Tape Grinding'?", "id": "Pita Pelindung Selama Proses Grinding." },
  { "en": "Apa Itu 'Manufaktur Sensor Gambar'?", "id": "Proses Fabrikasi Chip CCD Atau CMOS." },
  { "en": "Apa Itu 'Lapisan Mikrolensa (Microlens)'?", "id": "Lensa Kecil Di Atas Setiap Piksel." },
  { "en": "Apa Fungsi 'Lapisan Mikrolensa'?", "id": "Memfokuskan Cahaya Ke Area Fotosensitif." },
  { "en": "Apa Itu 'Filter Warna Bayer'?", "id": "Pola Mosaik Filter Merah, Hijau, Biru." },
  { "en": "Bagaimana 'Filter Warna' Dibuat?", "id": "Deposisi Dan Pola Material Pigmen." },
  { "en": "Apa Itu 'Assembly' Modul Kamera?", "id": "Merakit Sensor, Lensa, Dan Elektronik." },
  { "en": "Apa Itu 'Kalibrasi Fokus Aktif'?", "id": "Menyesuaikan Posisi Lensa Secara Otomatis." },
  { "en": "Apa Itu 'Manufaktur Layar Fleksibel'?", "id": "Membuat Layar Di Atas Substrat Plastik." },
  { "en": "Apa Tantangan 'Layar Fleksibel'?", "id": "Proses Suhu Rendah Dan Enkapsulasi." },
  { "en": "Apa Itu 'Enkapsulasi Film Tipis (TFE)'?", "id": "Lapisan Pelindung Tipis Untuk OLED." },
  { "en": "Bagaimana Cara Membuat 'PCB (Printed Circuit Board) Fleksibel'?", "id": "Menggunakan Substrat Poliimida." },
  { "en": "Apa Itu 'Rigid-Flex PCB'?", "id": "Gabungan Papan Sirkuit Kaku Dan Fleksibel." },
  { "en": "Apa Itu 'Electroless Plating'?", "id": "Pelapisan Logam Tanpa Arus Listrik Eksternal." },
  { "en": "Apa Beda 'Electroless' Dan 'Electroplating'?", "id": "Electroless Reaksi Kimia, Electroplating Listrik." },
  { "en": "Apa Itu 'Etsa Plasma'?", "id": "Nama Lain Untuk Dry Etching." },
  { "en": "Apa Itu 'Radikal Bebas'?", "id": "Spesies Reaktif Dalam Proses Plasma." },
  { "en": "Apa Itu 'Deep Reactive-Ion Etching (DRIE)'?", "id": "Proses Etsa Sangat Dalam Untuk MEMS." },
  { "en": "Apa Itu 'Proses Bosch'?", "id": "Metode Populer Untuk DRIE." },
  { "en": "Bagaimana 'Proses Bosch' Bekerja?", "id": "Bergantian Antara Etsa Dan Passivasi." },
  { "en": "Apa Itu 'Aspect Ratio' Dalam Etsa?", "id": "Rasio Kedalaman Terhadap Lebar." },
  { "en": "Apa Itu 'Anisotropi'?", "id": "Sifat Yang Bergantung Pada Arah." },
  { "en": "Apa Itu 'Etsa Anisotropik'?", "id": "Mengetsa Lebih Cepat Arah Vertikal." },
  { "en": "Apa Itu 'Etsa Isotropik'?", "id": "Mengetsa Sama Cepat Ke Segala Arah." },
  { "en": "Apa Itu 'Undercut'?", "id": "Etsa Ke Samping Di Bawah Mask." },
  { "en": "Apa Itu 'Selektivitas' Etsa?", "id": "Perbandingan Laju Etsa Dua Material." },
  { "en": "Apa Itu 'Crystal Growth'?", "id": "Proses Pembuatan Kristal Tunggal." },
  { "en": "Apa Itu 'Seed Crystal'?", "id": "Kristal Kecil Sebagai Awal Pertumbuhan." },
  { "en": "Apa Itu 'Float-Zone Silicon'?", "id": "Metode Pemurnian Silikon (Si) Alternatif." },
  { "en": "Apa Keuntungan 'Float-Zone'?", "id": "Tingkat Kemurnian Yang Sangat Tinggi." },
  { "en": "Apa Itu 'Wafer Slicing'?", "id": "Proses Memotong Ingot Menjadi Wafer." },
  { "en": "Apa Itu 'Wire Saw'?", "id": "Gergaji Kawat Untuk Memotong Wafer." },
  { "en": "Apa Itu 'Kerf Loss'?", "id": "Material Yang Hilang Saat Pemotongan." },
  { "en": "Apa Itu 'Lapping'?", "id": "Proses Menghaluskan Permukaan Wafer." },
  { "en": "Apa Itu 'Polishing'?", "id": "Membuat Permukaan Wafer Sangat Halus." },
  { "en": "Apa Itu 'Wafer Edge Grinding'?", "id": "Proses Membulatkan Tepi Wafer." },
  { "en": "Mengapa Tepi Wafer Dibulatkan?", "id": "Untuk Mencegah Keretakan Dan Pecah." },
  { "en": "Apa Itu 'Wafer Cleaning'?", "id": "Membersihkan Kontaminan Dari Permukaan Wafer." },
  { "en": "Apa Kepanjangan 'UPW (Ultrapure Water)'?", "id": "Air Ultra Murni." },
  { "en": "Mengapa Menggunakan 'UPW (Ultrapure Water)'?", "id": "Karena Tidak Mengandung Ion Pengganggu." },
  { "en": "Apa Itu 'Megasonic Cleaning'?", "id": "Pembersihan Menggunakan Gelombang Suara Tinggi." },
  { "en": "Apa Itu 'Spin-Rinse-Dry'?", "id": "Metode Membersihkan Dan Mengeringkan Wafer." },
  { "en": "Apa Itu 'Metrologi Wafer'?", "id": "Pengukuran Parameter Kritis Pada Wafer." },
  { "en": "Apa Itu 'Defect Inspection'?", "id": "Pemeriksaan Untuk Menemukan Cacat." },
  { "en": "Apa Itu 'KLA Tencor'?", "id": "Perusahaan Terkenal Pembuat Alat Inspeksi." },
  { "en": "Apa Itu 'Yield Modeling'?", "id": "Memprediksi Yield Berdasarkan Data Cacat." },
  { "en": "Apa Itu 'Process Window'?", "id": "Rentang Parameter Proses Yang Diterima." },
  { "en": "Apa Itu 'Lot' Wafer?", "id": "Sekelompok Wafer Yang Diproses Bersama." },
  { "en": "Apa Itu 'Split Lot'?", "id": "Membagi Lot Untuk Membandingkan Proses." },
  { "en": "Apa Itu 'Statistical Process Control (SPC)'?", "id": "Menggunakan Statistik Untuk Mengontrol Proses." },
  { "en": "Apa Itu 'Control Chart'?", "id": "Grafik Untuk Memantau Variasi Proses." },
  { "en": "Apa Itu 'Design of Experiments (DoE)'?", "id": "Metode Statistik Untuk Optimisasi Proses." },
  { "en": "Bagaimana 'Tabung Hampa' Dibuat?", "id": "Merakit Elektroda Dalam Selungkup Kaca." },
  { "en": "Apa Itu 'Getter'?", "id": "Bahan Penyerap Sisa Gas Vakum." },
  { "en": "Apa Itu 'Glass-to-Metal Seal'?", "id": "Teknik Menyambung Kaca Dan Logam." },
  { "en": "Bagaimana 'Filamen Tungsten' Dibuat?", "id": "Melalui Proses Penarikan Kawat." },
  { "en": "Apa Itu 'Sintering' Logam?", "id": "Memadatkan Serbuk Logam Dengan Panas." },
  { "en": "Bagaimana 'Keramik Alumina' Dibuat?", "id": "Sintering Serbuk Aluminium Oksida." },
  { "en": "Apa Itu 'Tape Casting'?", "id": "Metode Membuat Lembaran Keramik Tipis." },
  { "en": "Apa Itu 'Screen Printing'?", "id": "Teknik Mencetak Pasta Melalui Layar." },
  { "en": "Aplikasi 'Screen Printing'?", "id": "Membuat Resistor Film Tebal." },
  { "en": "Apa Itu 'Co-firing'?", "id": "Sintering Beberapa Lapisan Material Bersamaan." },
  { "en": "Apa Kepanjangan 'LTCC (Low-Temperature Co-fired Ceramic)'?", "id": "Keramik Co-fired Suhu Rendah." },
  { "en": "Bagaimana 'Saklar Tombol (Pushbutton)' Dibuat?", "id": "Merakit Kontak Logam, Pegas, Casing." },
  { "en": "Bagaimana 'Konektor' Dibuat?", "id": "Proses Stamping Logam Dan Cetak Plastik." },
  { "en": "Apa Itu 'Overmolding'?", "id": "Mencetak Plastik Di Sekitar Komponen." },
  { "en": "Bagaimana 'Motor Stepper' Dibuat?", "id": "Merakit Stator Berkelok Dan Rotor Magnet." },
  { "en": "Apa Itu 'Winding' Motor?", "id": "Proses Melilitkan Kawat Kumparan." },
  { "en": "Apa Itu 'Impregnasi Vakum'?", "id": "Mengisi Celah Kumparan Dengan Pernis." },
  { "en": "Bagaimana 'Transduser Ultrasonik' Dibuat?", "id": "Menggunakan Material Keramik Piezoelektrik." },
  { "en": "Apa Itu 'Poling' Material Piezoelektrik?", "id": "Menyelaraskan Domain Dipol Dengan Medan Listrik." },
  { "en": "Bagaimana 'Kabel Pita Fleksibel (FFC)' Dibuat?", "id": "Melaminasi Konduktor Datar Antara Plastik." },
  { "en": "Apa Kepanjangan 'FPC (Flexible Printed Circuit)'?", "id": "Sirkuit Tercetak Fleksibel." },
  { "en": "Apa Beda 'FFC' Dan 'FPC'?", "id": "FPC Punya Jalur Etsa, FFC Konduktor Lurus." },
  { "en": "Bagaimana 'Baterai Alkalin' Dibuat?", "id": "Merakit Anoda Seng, Katoda Mangan Dioksida." },
  { "en": "Bagaimana 'Baterai Lithium' Dibuat?", "id": "Merakit Elektroda Lithium Dan Bahan Lainnya." },
  { "en": "Apa Itu 'Calendering' Elektroda?", "id": "Proses Memadatkan Material Elektroda." },
  { "en": "Apa Itu 'Slitting'?", "id": "Memotong Gulungan Elektroda Menjadi Strip." },
  { "en": "Apa Itu 'Tab Welding'?", "id": "Mengelas Terminal Ke Elektroda Baterai." },
  { "en": "Apa Itu 'Electrolyte Filling'?", "id": "Proses Mengisi Sel Baterai Dengan Elektrolit." },
  { "en": "Apa Itu 'Formation Cycling'?", "id": "Siklus Pengisian-Pengosongan Awal Baterai." },
  { "en": "Apa Itu 'Aging' Baterai?", "id": "Proses Penyimpanan Untuk Menstabilkan Baterai." },
  { "en": "Bagaimana 'Osilator Kristal' Disegel?", "id": "Dalam Kemasan Logam Hermetik." },
  { "en": "Mengapa 'Osilator Kristal' Perlu Disegel?", "id": "Untuk Melindungi Dari Lingkungan." },
  { "en": "Apa Itu 'Hermetic Seal'?", "id": "Segel Kedap Udara." },
  { "en": "Apa Itu 'Glass Frit Bonding'?", "id": "Menyambung Menggunakan Bubuk Kaca." },
  { "en": "Apa Itu 'Anodic Bonding'?", "id": "Menyambung Kaca Ke Silikon (Si) Dengan Listrik." },
  { "en": "Apa Itu 'Fusion Bonding'?", "id": "Menyambung Wafer Halus Tanpa Perekat." },
  { "en": "Bagaimana 'Fiber Bragg Grating (FBG)' Dibuat?", "id": "Mengekspos Serat Optik Ke Sinar UV." },
  { "en": "Apa Itu 'Phase Mask'?", "id": "Masker Difraksi Untuk Membuat FBG." },
  { "en": "Bagaimana 'Lensa Cetakan (Molded Lens)' Dibuat?", "id": "Mencetak Polimer Optik Dalam Cetakan." },
  { "en": "Apa Itu 'Diamond Turning'?", "id": "Proses Pemesinan Presisi Untuk Optik." },
  { "en": "Bagaimana 'Cermin Presisi' Dibuat?", "id": "Melapisi Kaca Dengan Lapisan Reflektif." },
  { "en": "Apa Itu 'Pelapisan Dielektrik'?", "id": "Menumpuk Lapisan Untuk Cermin Khusus." },
  { "en": "Apa Itu 'Cermin Dichroic'?", "id": "Cermin Yang Memantulkan Warna Tertentu." },
  { "en": "Apa Itu 'Delayering'?", "id": "Proses Mengupas Lapisan Chip Untuk Analisis." },
  { "en": "Mengapa 'Gettering' Penting?", "id": "Menyingkirkan Kontaminan Logam Perusak." },
  { "en": "Apa Beda Inspeksi 'Brightfield' Dan 'Darkfield'?", "id": "Brightfield Deteksi Kontras, Darkfield Deteksi Hamburan." },
  { "en": "Apa Fungsi 'FIB (Focused Ion Beam)'?", "id": "Memotong Dan Pencitraan Skala Nano." },
  { "en": "Bagaimana Cara Membuat 'Filter BAW (Bulk Acoustic Wave)'?", "id": "Membuat Resonator Akustik Di Dalam Chip." },
  { "en": "Apa Itu 'Gowning Procedure'?", "id": "Prosedur Mengenakan Pakaian Ruang Bersih." },
  { "en": "Apa Fungsi 'Air Shower'?", "id": "Menghilangkan Partikel Debu Dari Pakaian." },
  { "en": "Apa Itu 'Rheologi' Pasta Solder?", "id": "Studi Sifat Aliran Pasta Solder." },
  { "en": "Apa Itu 'Vapor Phase Soldering'?", "id": "Solder Menggunakan Uap Cairan Inert Panas." },
  { "en": "Apa Jenis 'Conformal Coating'?", "id": "Akrilik, Silikon, Uretan." },
  { "en": "Bagaimana 'Potensiometer' Dibuat?", "id": "Dengan Jalur Resistif Dan Kontak Geser." },
  { "en": "Apa Itu 'Substrate Engineering'?", "id": "Memodifikasi Substrat Untuk Kinerja Lebih Baik." },
  { "en": "Apa Itu 'Wafer Dicing' Menggunakan Laser?", "id": "Memotong Wafer Dengan Sinar Laser Presisi." },
  { "en": "Apa Itu 'Thermal Compression Bonding'?", "id": "Menyambung Chip Menggunakan Panas Dan Tekanan." },
  { "en": "Bagaimana 'Cermin MEMS' Dibuat?", "id": "Dengan Surface Micromachining." },
  { "en": "Apa Fungsi 'Cermin MEMS'?", "id": "Mengarahkan Sinar Laser Dengan Cepat." },
  { "en": "Apa Itu 'Dielectric Constant'?", "id": "Kemampuan Bahan Menyimpan Energi Listrik." },
  { "en": "Mengapa 'Dielektrik Low-k' Digunakan?", "id": "Mengurangi Tundaan Sinyal Antar Jalur." },
  { "en": "Apa Itu 'Copper Pillar Bump'?", "id": "Tonjolan Tembaga (Cu) Untuk Koneksi Flip-Chip." },
  { "en": "Apa Keuntungan 'Copper Pillar'?", "id": "Kinerja Listrik Dan Termal Lebih Baik." },
  { "en": "Apa Itu 'Reliability Testing'?", "id": "Pengujian Ketahanan Komponen Seiring Waktu." },
  { "en": "Apa Kepanjangan 'HTOL (High Temperature Operating Life)'?", "id": "Uji Umur Operasi Suhu Tinggi." },
  { "en": "Apa Itu 'Thermal Shock Test'?", "id": "Pengujian Dengan Perubahan Suhu Ekstrem." },
  { "en": "Apa Itu 'Vibration Test'?", "id": "Pengujian Ketahanan Terhadap Getaran Mekanis." },
  { "en": "Apa Itu 'Proses Manufaktur CMOS'?", "id": "Langkah-langkah Membuat Chip CMOS." },
  { "en": "Apa Itu 'Litho-Etch-Litho-Etch (LELE)'?", "id": "Teknik Double Patterning Untuk Skala Kecil." },
  { "en": "Apa Itu 'Self-Aligned Double Patterning (SADP)'?", "id": "Teknik Double Patterning Lainnya." },
  { "en": "Mengapa 'Double Patterning' Diperlukan?", "id": "Untuk Membuat Fitur Lebih Kecil." },
  { "en": "Apa Itu 'Optical Lithography'?", "id": "Nama Lain Untuk Fotolitografi." },
  { "en": "Apa Itu 'E-beam Lithography'?", "id": "Litografi Tanpa Masker Menggunakan Berkas Elektron." },
  { "en": "Apa Kelemahan 'E-beam Lithography'?", "id": "Sangat Lambat, Tidak Untuk Produksi Massal." },
  { "en": "Apa Itu 'Nanoimprint Lithography (NIL)'?", "id": "Mencetak Pola Nano Seperti Stempel." },
  { "en": "Apa Itu 'Molecular Imprinting'?", "id": "Membuat Cetakan Molekuler Di Polimer." },
  { "en": "Apa Itu 'Sol-Gel Process'?", "id": "Metode Kimia Membuat Oksida Logam." },
  { "en": "Bagaimana 'Kaca' Dibuat?", "id": "Mendinginkan Cairan Silika Dengan Cepat." },
  { "en": "Apa Itu 'Tempering' Kaca?", "id": "Proses Pemanasan-Pendinginan Untuk Perkuat Kaca." },
  { "en": "Bagaimana 'LED (Light Emitting Diode) Inframerah' Dibuat?", "id": "Menggunakan Bahan Seperti Galium Arsenida (GaAs)." },
  { "en": "Bagaimana 'Sensor UV (UltraViolet)' Dibuat?", "id": "Menggunakan Semikonduktor Celah Pita Lebar." },
  { "en": "Bahan Apa Untuk 'Sensor UV'?", "id": "Aluminium Galium Nitrida (AlGaN)." },
  { "en": "Apa Itu 'Photomultiplier Tube (PMT)'?", "id": "Detektor Cahaya Sangat Sensitif." },
  { "en": "Bagaimana 'PMT' Dibuat?", "id": "Merakit Fotokatoda Dan Dynode Dalam Tabung Vakum." },
  { "en": "Apa Itu 'Fotokatoda'?", "id": "Bahan Yang Melepas Elektron Saat Kena Cahaya." },
  { "en": "Apa Itu 'Dynode'?", "id": "Elektroda Yang Melipatgandakan Elektron." },
  { "en": "Apa Itu 'Scintillator'?", "id": "Bahan Yang Berpendar Saat Kena Radiasi." },
  { "en": "Bagaimana 'Detektor Scintillation' Dibuat?", "id": "Menggabungkan Scintillator Dengan Photodetector." },
  { "en": "Bagaimana 'Tabung Geiger-Müller' Dibuat?", "id": "Tabung Diisi Gas Dengan Elektroda." },
  { "en": "Apa Itu 'Semikonduktor Organik'?", "id": "Semikonduktor Berbasis Molekul Organik." },
  { "en": "Bagaimana 'Transistor Organik (OFET)' Dibuat?", "id": "Deposisi Lapisan Organik Pada Substrat." },
  { "en": "Apa Itu 'Deposisi Vakum Termal'?", "id": "Menguapkan Material Dalam Vakum Tinggi." },
  { "en": "Apa Itu 'Doctor Blading'?", "id": "Teknik Melapisi Cairan Dengan Ketebalan Tepat." },
  { "en": "Bagaimana 'Sel Surya Organik' Dibuat?", "id": "Dengan Melapisi Polimer Donor-Akseptor." },
  { "en": "Apa Itu 'Fullerene'?", "id": "Molekul Karbon Seperti Bola (C60)." },
  { "en": "Apa Peran 'Fullerene' Dalam Sel Surya?", "id": "Sebagai Material Akseptor Elektron." },
  { "en": "Apa Itu 'Perovskite'?", "id": "Material Kristal Dengan Struktur Tertentu." },
  { "en": "Bagaimana 'Sel Surya Perovskite' Dibuat?", "id": "Melalui Pelapisan Larutan (Solution Coating)." },
  { "en": "Apa Itu 'Slot-Die Coating'?", "id": "Teknik Pelapisan Presisi Skala Besar." },
  { "en": "Apa Itu 'Manufaktur Elektronik Cetak'?", "id": "Mencetak Sirkuit Menggunakan Printer Khusus." },
  { "en": "Apa Itu 'Gravure Printing'?", "id": "Teknik Cetak Untuk Volume Sangat Tinggi." },
  { "en": "Apa Itu 'Flexographic Printing'?", "id": "Teknik Cetak Fleksibel Seperti Stempel." },
  { "en": "Apa Itu 'Aerosol Jet Printing'?", "id": "Mencetak Garis Sangat Halus Dengan Aerosol." },
  { "en": "Apa Itu 'Sintering Fotonik'?", "id": "Sintering Tinta Logam Dengan Kilatan Cahaya." },
  { "en": "Bagaimana 'Transduser Piezoelektrik' Dibuat?", "id": "Sintering Keramik Dan Proses Poling." },
  { "en": "Apa Itu 'Poling'?", "id": "Menyelaraskan Domain Dipol Dengan Medan Listrik." },
  { "en": "Bagaimana 'Kapasitor Variabel (Varco)' Dibuat?", "id": "Merakit Set Pelat Stator Dan Rotor." },
  { "en": "Bagaimana 'Relay Elektromekanis' Dibuat?", "id": "Merakit Koil, Inti Besi, Dan Kontak." },
  { "en": "Apa Itu 'Armature' Relay?", "id": "Bagian Bergerak Yang Mengoperasikan Kontak." },
  { "en": "Bagaimana 'Motor DC' Dibuat?", "id": "Merakit Stator, Rotor, Komutator, Sikat." },
  { "en": "Apa Itu 'Komutator'?", "id": "Saklar Putar Pembalik Arah Arus." },
  { "en": "Apa Itu 'Sikat (Brush)' Motor?", "id": "Kontak Karbon Yang Menyuplai Arus." },
  { "en": "Bagaimana 'Transformer' Dibuat?", "id": "Melilitkan Kumparan Kawat Pada Inti Besi." },
  { "en": "Apa Itu 'Inti Laminasi'?", "id": "Inti Besi Dari Tumpukan Pelat Tipis." },
  { "en": "Mengapa Inti Trafo Perlu Dilaminasi?", "id": "Untuk Mengurangi Kerugian Arus Eddy." },
  { "en": "Bagaimana 'Kabel Listrik' Dibuat?", "id": "Menarik Konduktor Dan Melapisinya Isolasi." },
  { "en": "Apa Itu 'Stranding' Kabel?", "id": "Menggabungkan Banyak Kawat Kecil." },
  { "en": "Mengapa Kabel Dibuat 'Stranded'?", "id": "Agar Lebih Fleksibel." },
  { "en": "Apa Itu 'Die Drawing'?", "id": "Proses Mengecilkan Diameter Kawat." },
  { "en": "Apa Itu 'Annealing'?", "id": "Proses Pemanasan Untuk Melunakkan Logam." },
  { "en": "Apa Itu 'Extrusion' Isolasi?", "id": "Melapisi Kawat Dengan Plastik Meleleh." },
  { "en": "Bagaimana 'Steker Listrik' Dibuat?", "id": "Stamping Pin Logam Dan Cetak Plastik." },
  { "en": "Bagaimana 'Papan Breadboard' Dibuat?", "id": "Merakit Klip Logam Dalam Blok Plastik." },
  { "en": "Bagaimana 'Multimeter Digital' Dirakit?", "id": "Merakit PCB, Layar, Saklar, Probe." },
  { "en": "Bagaimana 'Osiloskop' Dirakit?", "id": "Merakit Sirkuit Kompleks Dan Tabung CRT/Layar." },
  { "en": "Apa Kepanjangan 'CRT (Cathode Ray Tube)'?", "id": "Tabung Sinar Katoda." },
  { "en": "Bagaimana 'Tabung CRT' Dibuat?", "id": "Merakit Senapan Elektron Dalam Tabung Kaca." },
  { "en": "Apa Itu 'Layar Fosfor'?", "id": "Lapisan Yang Berpendar Saat Ditembak Elektron." },
  { "en": "Bagaimana 'Magnetron' Dibuat?", "id": "Membuat Rongga Resonan Dan Katoda." },
  { "en": "Apa Fungsi 'Magnetron'?", "id": "Menghasilkan Gelombang Mikro Daya Tinggi." },
  { "en": "Bagaimana 'Klystron' Bekerja?", "id": "Menguatkan Sinyal RF Dengan Modulasi Kecepatan." },
  { "en": "Bagaimana 'Tabung Gelombang Berjalan (TWT)' Bekerja?", "id": "Interaksi Berkelanjutan Antara Berkas-Gelombang." },
  { "en": "Apa Kepanjangan 'TWT (Traveling-Wave Tube)'?", "id": "Tabung Gelombang Berjalan." },
  { "en": "Bagaimana 'Kabel Serat Optik' Dibuat?", "id": "Melapisi Serat Kaca Dengan Pelindung." },
  { "en": "Apa Itu 'Buffer' Kabel Optik?", "id": "Lapisan Plastik Pelindung Serat." },
  { "en": "Apa Itu 'Kevlar' Dalam Kabel?", "id": "Benang Kuat Untuk Kekuatan Tarik." },
  { "en": "Apa Itu 'Jaket' Kabel?", "id": "Lapisan Pelindung Terluar Kabel." },
  { "en": "Bagaimana 'Konektor Serat Optik' Dipasang?", "id": "Dengan Polishing Presisi Dan Penyambungan." },
  { "en": "Apa Itu 'Fusion Splicing'?", "id": "Menyambung Serat Optik Dengan Meleburnya." },
  { "en": "Apa Itu 'Mechanical Splicing'?", "id": "Menyambung Serat Optik Secara Mekanis." },
  { "en": "Apa Itu 'Phon'?", "id": "Satuan Kerasnya Suara Subjektif." },
  { "en": "Apa Itu 'Impedansi Akustik'?", "id": "Hambatan Medium Terhadap Gelombang Suara." },
  { "en": "Apa Yang Menentukan Frekuensi Dasar Senar?", "id": "Panjang, Tegangan, Dan Massa." },
  { "en": "Apa Itu 'Waktu Koherensi'?", "id": "Rentang Waktu Gelombang Tetap Koheren." },
  { "en": "Apa Itu 'Garis Fraunhofer'?", "id": "Garis Serapan Dalam Spektrum Matahari." },
  { "en": "Apa Itu 'Lebar Garis (Linewidth)' Laser?", "id": "Rentang Frekuensi Sinar Laser." },
  { "en": "Apa Itu 'Birefringence'?", "id": "Indeks Bias Bergantung Polarisasi." },
  { "en": "Apa Itu 'Faktor Kualitas (Q)'?", "id": "Ukuran Kualitas Sebuah Resonator." },
  { "en": "Apa Itu 'Phase Margin'?", "id": "Ukuran Stabilitas Sistem Umpan Balik." },
  { "en": "Apa Itu 'Gain Margin'?", "id": "Ukuran Lain Stabilitas Sistem." },
  { "en": "Apa Itu 'Derajat Kebebasan'?", "id": "Jumlah Cara Sistem Dapat Bergerak." },
  { "en": "Apa Itu 'Analisis Modal'?", "id": "Studi Tentang Mode Getaran Alami." },
  { "en": "Apa Penyebab 'Pergeseran Merah Kosmologis'?", "id": "Ekspansi Alam Semesta." },
  { "en": "Berapa Frekuensi Puncak 'Radiasi Latar Kosmik'?", "id": "Sekitar 160.2 GHz." },
  { "en": "Apa Rumus 'Relasi Planck-Einstein'?", "id": "E = hf (Energi = Konstanta x Frekuensi)." },
  { "en": "Apa Fungsi 'Jendela (Window Function)'?", "id": "Mengurangi Kebocoran Spektral." },
  { "en": "Apa Itu 'Zero-Padding'?", "id": "Menambah Nol Untuk Resolusi Frekuensi." },
  { "en": "Apa Itu 'Spektrum'?", "id": "Komponen Frekuensi Dari Suatu Sinyal." },
  { "en": "Apa Itu 'Frekuensi Normalisasi'?", "id": "Frekuensi Dinyatakan Relatif Terhadap Sampling." },
  { "en": "Apa Itu 'Frekuensi Kompleks'?", "id": "Generalisasi Frekuensi Dengan Komponen Peluruhan." },
  { "en": "Apa Itu 'Pita Konduksi'?", "id": "Rentang Energi Elektron Bisa Bergerak." },
  { "en": "Apa Itu 'Pita Valensi'?", "id": "Rentang Energi Elektron Terikat." },
  { "en": "Apa Itu 'Celah Pita (Band Gap)'?", "id": "Rentang Energi Terlarang Bagi Elektron." },
  { "en": "Apa Itu 'Frekuensi cutoff' Transistor?", "id": "Frekuensi Saat Penguatan Turun Drastis." },
  { "en": "Apa Itu 'Pita Samping (Sideband)'?", "id": "Hasil Modulasi Di Sekitar Carrier." },
  { "en": "Apa Itu 'Modulasi Amplitudo Kuadratur (QAM)'?", "id": "Mengubah Amplitudo Dan Fasa." },
  { "en": "Apa Itu 'Frekuensi Resonansi Seri RLC'?", "id": "Frekuensi Saat Impedansi RLC Minimum." },
  { "en": "Apa Itu 'Frekuensi Resonansi Paralel RLC'?", "id": "Frekuensi Saat Impedansi RLC Maksimum." },
  { "en": "Apa Itu 'Frekuensi Harmonisa'?", "id": "Kelipatan Bilangan Bulat Dari Fundamental." },
  { "en": "Apa Itu 'Frekuensi Subharmonik'?", "id": "Pecahan Bilangan Bulat Dari Fundamental." },
  { "en": "Apa Itu 'Gelombang Gergaji (Sawtooth)'?", "id": "Gelombang Dengan Bentuk Gigi Gergaji." },
  { "en": "Apa Spektrum 'Gelombang Gergaji'?", "id": "Mengandung Semua Harmonisa (Genap Ganjil)." },
  { "en": "Apa Spektrum 'Gelombang Kotak'?", "id": "Hanya Mengandung Harmonisa Ganjil." },
  { "en": "Apa Itu 'Jitter Frekuensi'?", "id": "Deviasi Frekuensi Dari Posisi Ideal." },
  { "en": "Apa Itu 'Spektroskopi'?", "id": "Studi Interaksi Cahaya Dan Materi." },
  { "en": "Apa Itu 'Frekuensi Rotasi Molekul'?", "id": "Frekuensi Rotasi Kuantum Molekul." },
  { "en": "Apa Itu 'Frekuensi Getaran Molekul'?", "id": "Frekuensi Getaran Kuantum Ikatan." },
  { "en": "Apa Itu 'Pita Penyerapan'?", "id": "Rentang Frekuensi Yang Diserap." },
  { "en": "Apa Itu 'Pita Transmisi'?", "id": "Rentang Frekuensi Yang Dilewatkan." },
  { "en": "Apa Itu 'Frekuensi Pusat' Filter?", "id": "Frekuensi Target Dari Filter." },
  { "en": "Apa Itu 'Selektivitas'?", "id": "Kemampuan Filter Memilih Frekuensi." },
  { "en": "Apa Itu 'Respon Fasa'?", "id": "Bagaimana Sistem Mengubah Fasa Sinyal." },
  { "en": "Apa Itu 'Fasa Linier'?", "id": "Pergeseran Fasa Sebanding Frekuensi." },
  { "en": "Apa Keuntungan 'Fasa Linier'?", "id": "Tidak Merusak Bentuk Sinyal." },
  { "en": "Apa Itu 'Pita Radio Publik'?", "id": "Frekuensi Untuk Komunikasi Publik (CB)." },
  { "en": "Apa Itu 'Frekuensi Gelombang Pendek (Shortwave)'?", "id": "Frekuensi Radio Jangkauan Jauh." },
  { "en": "Apa Itu 'Frekuensi Gelombang Menengah (Medium Wave)'?", "id": "Frekuensi Untuk Siaran Radio AM." },
  { "en": "Apa Itu 'Frekuensi Gelombang Panjang (Longwave)'?", "id": "Frekuensi Radio Dengan Jangkauan Sangat Jauh." },
  { "en": "Apa Itu 'Siklus Matahari'?", "id": "Siklus Aktivitas Matahari Sekitar 11 Tahun." },
  { "en": "Bagaimana 'Siklus Matahari' Mempengaruhi Frekuensi Radio?", "id": "Mempengaruhi Propagasi Gelombang Langit." },
  { "en": "Apa Itu 'Peredaman Hujan (Rain Fade)'?", "id": "Pelemahan Sinyal Frekuensi Tinggi Oleh Hujan." },
  { "en": "Frekuensi Apa Yang Terkena 'Peredaman Hujan'?", "id": "Frekuensi Gelombang Mikro (Ku, Ka)." },
  { "en": "Apa Itu 'Osilasi Kuasi-Periodik'?", "id": "Osilasi Yang Hampir Periodik." },
  { "en": "Apa Itu 'Frekuensi Spasial' Citra?", "id": "Ukuran Detail Atau Tekstur Gambar." },
  { "en": "Frekuensi Spasial Rendah Berarti Gambar...?", "id": "Area Halus Dan Kurang Detail." },
  { "en": "Frekuensi Spasial Tinggi Berarti Gambar...?", "id": "Area Tepi Dan Sangat Detail." },
  { "en": "Apa Itu 'Filter High-pass' Dalam Gambar?", "id": "Menajamkan Tepi Dan Detail." },
  { "en": "Apa Itu 'Filter Low-pass' Dalam Gambar?", "id": "Menghaluskan Dan Mengaburkan Gambar." },
  { "en": "Apa Itu 'Frekuensi Seketika (Instantaneous)'?", "id": "Frekuensi Sinyal Pada Momen Tertentu." },
  { "en": "Apa Itu 'Ketidakstabilan Frekuensi'?", "id": "Deviasi Frekuensi Dari Nilai Ideal." },
  { "en": "Apa Itu 'Penuaan Frekuensi (Aging)'?", "id": "Pergeseran Frekuensi Osilator Seiring Waktu." },
  { "en": "Apa Itu 'Frekuensi Ketukan (Beat)'?", "id": "Nama Lain Untuk Frekuensi Denyut." },
  { "en": "Apa Itu 'Nada Isochronous'?", "id": "Nada Denyut Untuk Meditasi." },
  { "en": "Apa Itu 'Frekuensi Resonansi Stoikastik'?", "id": "Fenomena Di Mana Derau Meningkatkan Sinyal." },
  { "en": "Apa Itu 'Gelombang Gravitasi'?", "id": "Riak Dalam Ruang-Waktu." },
  { "en": "Apa Rentang 'Frekuensi Gelombang Gravitasi'?", "id": "Sangat Rendah (Sub-Hertz)." },
  { "en": "Apa Itu 'Chirp Massa'?", "id": "Sinyal Khas Penggabungan Lubang Hitam." },
  { "en": "Apa Itu 'Frekuensi Sirkuit Otak'?", "id": "Osilasi Sinkron Antara Neuron." },
  { "en": "Apa Itu 'Frekuensi Gamma' Otak?", "id": "Terkait Dengan Kesadaran Dan Persepsi." },
  { "en": "Apa Itu 'Medan Frekuensi Radio (RF Field)'?", "id": "Medan Elektromagnetik Dalam Rentang RF." },
  { "en": "Apa Itu 'Paparan Frekuensi Radio'?", "id": "Jumlah Energi RF Yang Diserap Tubuh." },
  { "en": "Apa Kepanjangan 'SAR (Specific Absorption Rate)'?", "id": "Tingkat Penyerapan Spesifik." },
  { "en": "Apa Itu 'Hukum Rayleigh-Jeans'?", "id": "Deskripsi Klasik Radiasi Benda Hitam." },
  { "en": "Apa Itu 'Bencana Ultraviolet'?", "id": "Kegagalan Hukum Rayleigh-Jeans." },
  { "en": "Siapa Yang Memecahkan 'Bencana Ultraviolet'?", "id": "Max Planck." },
  { "en": "Apa Itu 'Kuantisasi Energi'?", "id": "Energi Hanya Ada Dalam Paket Diskrit." },
  { "en": "Apa Itu 'Frekuensi Compton'?", "id": "Pergeseran Frekuensi Foton Setelah Tumbukan." },
  { "en": "Apa Itu 'Frekuensi Sirkuit RLC'?", "id": "Frekuensi Resonansi Sirkuit RLC." },
  { "en": "Apa Itu 'Peredam Frekuensi'?", "id": "Perangkat Yang Menyerap Getaran." },
  { "en": "Apa Itu 'Frekuensi Sampling Audio CD'?", "id": "44.1 Kilohertz (kHz)." },
  { "en": "Apa Itu 'Frekuensi Audio Definisi Tinggi'?", "id": "Frekuensi Sampling Di Atas 44.1 kHz." },
  { "en": "Apa Itu 'Respons Impuls'?", "id": "Output Sistem Terhadap Input Impuls." },
  { "en": "Apa Spektrum 'Fungsi Impuls'?", "id": "Datar, Mengandung Semua Frekuensi." },
  { "en": "Apa Itu 'Kebocoran Spektral'?", "id": "Penyebaran Energi Ke Frekuensi Tetangga." },
  { "en": "Apa Penyebab 'Kebocoran Spektral'?", "id": "Pengamatan Sinyal Dalam Jendela Waktu." },
  { "en": "Apa Itu 'Sinyal Waktu-Terbatas'?", "id": "Sinyal Yang Hanya Ada Sesaat." },
  { "en": "Apa Spektrum 'Sinyal Waktu-Terbatas'?", "id": "Bandwidth Tak Terbatas." },
  { "en": "Apa Itu 'Sinyal Pita-Terbatas'?", "id": "Sinyal Dengan Frekuensi Maksimal." },
  { "en": "Apa Durasi 'Sinyal Pita-Terbatas'?", "id": "Durasi Waktu Tak Terbatas." },
  { "en": "Apa Itu 'Prinsip Ketidakpastian'?", "id": "Tidak Bisa Tahu Waktu-Frekuensi Akurat." },
  { "en": "Apa Itu 'Frekuensi Spasial Nyquist'?", "id": "Frekuensi Spasial Maksimal Tanpa Aliasing." },
  { "en": "Apa Itu 'Diagram Air Terjun (Waterfall)'?", "id": "Visualisasi Spektrum Frekuensi Seiring Waktu." },
  { "en": "Apa Itu 'Frekuensi Resonansi Mekanis'?", "id": "Frekuensi Saat Benda Bergetar Hebat." },
  { "en": "Apa Itu 'Penganalisis Sinyal Vektor (VSA)'?", "id": "Menganalisis Amplitudo Dan Fasa Sinyal." },
  { "en": "Apa Itu 'Downsampling'?", "id": "Mengurangi Laju Sampling Sinyal." },
  { "en": "Apa Itu 'Upsampling'?", "id": "Meningkatkan Laju Sampling Sinyal." },
  { "en": "Apa Itu 'Interpolasi'?", "id": "Membuat Sampel Baru Di Antara Sampel." },
  { "en": "Apa Itu 'Desimasi'?", "id": "Filter Low-pass Diikuti Downsampling." },
  { "en": "Apa Beda 'Frekuensi' Dan 'Amplitudo'?", "id": "Frekuensi Kekerapan, Amplitudo Kekuatan." },
  { "en": "Apa Beda 'Periode' Dan 'Panjang Gelombang'?", "id": "Periode Waktu, Panjang Gelombang Jarak." },
  { "en": "Apa Efek 'Redaman' Pada Frekuensi Alami?", "id": "Sedikit Menurunkan Frekuensi Alami." },
  { "en": "Bagaimana 'Kecepatan Medium' Mempengaruhi Panjang Gelombang?", "id": "Kecepatan Tinggi, Panjang Gelombang Besar." },
  { "en": "Frekuensi Apa Untuk 'Remote TV'?", "id": "Sinar Inframerah (IR)." },
  { "en": "Frekuensi Apa Yang Didengar 'Anjing'?", "id": "Hingga Sekitar 45 kHz (Ultrasonik)." },
  { "en": "Apa Definisi 'Satu Hertz'?", "id": "Satu Siklus Terjadi Per Detik." },
  { "en": "Apa Arti Awalan 'Mega' Dalam Frekuensi?", "id": "Satu Juta (10^6)." },
  { "en": "Apa Terjadi Pada 'Frekuensi' Ambulans Mendekat?", "id": "Terdengar Lebih Tinggi." },
  { "en": "Apa Terjadi Pada 'Frekuensi' Ambulans Menjauh?", "id": "Terdengar Lebih Rendah." },
  { "en": "Apa Itu 'Jitter Periode'?", "id": "Perubahan Periode Antar Siklus Clock." },
  { "en": "Apa Itu 'Jitter Siklus-Ke-Siklus'?", "id": "Perbedaan Antara Dua Siklus Berurutan." },
  { "en": "Apa Itu 'Bandwidth Absolut'?", "id": "Lebar Spektrum Yang Mengandung Semua Daya." },
  { "en": "Apa Itu 'Bandwidth Fraksional'?", "id": "Rasio Bandwidth Terhadap Frekuensi Pusat." },
  { "en": "Apa Itu 'Frekuensi Foton'?", "id": "Frekuensi Gelombang Elektromagnetik Foton." },
  { "en": "Apa Itu 'Getaran Teredam'?", "id": "Getaran Yang Amplitudonya Mengecil." },
  { "en": "Apa Itu 'Aliasing Temporal'?", "id": "Nama Lain Untuk Aliasing Sinyal." },
  { "en": "Apa Itu 'Frekuensi Resonansi Seri RLC'?", "id": "Frekuensi Saat Impedansi Minimum." },
  { "en": "Apa Itu 'Frekuensi Resonansi Paralel RLC'?", "id": "Frekuensi Saat Impedansi Maksimum." },
  { "en": "Apa Itu 'Faktor Disipasi'?", "id": "Kebalikan Dari Faktor Kualitas (Q)." },
  { "en": "Apa Itu 'Frekuensi Sudut Natural Teredam'?", "id": "Frekuensi Osilasi Sistem Teredam." },
  { "en": "Apa Itu 'Frekuensi Spasial Fundamen'?", "id": "Frekuensi Spasial Terendah Suatu Pola." },
  { "en": "Apa Itu 'Frekuensi Toleransi Kristal'?", "id": "Penyimpangan Frekuensi Kristal Yang Diizinkan." },
  { "en": "Apa Satuan 'Toleransi Kristal'?", "id": "Bagian Per Juta (PPM)." },
  { "en": "Apa Itu 'Stabilitas Suhu' Osilator?", "id": "Perubahan Frekuensi Terhadap Perubahan Suhu." },
  { "en": "Apa Itu 'Kompensasi Suhu'?", "id": "Teknik Menjaga Frekuensi Tetap Stabil." },
  { "en": "Apa Kepanjangan 'TCXO (Temperature-Compensated Crystal Oscillator)'?", "id": "Osilator Kristal Terkompensasi Suhu." },
  { "en": "Apa Kepanjangan 'OCXO (Oven-Controlled Crystal Oscillator)'?", "id": "Osilator Kristal Terkontrol Oven." },
  { "en": "Mana Lebih Stabil, 'TCXO' Atau 'OCXO'?", "id": "OCXO Jauh Lebih Stabil." },
  { "en": "Apa Itu 'Kebisingan Termal'?", "id": "Derau Akibat Gerakan Termal Elektron." },
  { "en": "Apakah 'Kebisingan Termal' Tergantung Frekuensi?", "id": "Tidak, Spektrumnya Datar (Putih)." },
  { "en": "Apa Itu 'Respons Amplitudo'?", "id": "Bagian Gain Dari Respons Frekuensi." },
  { "en": "Apa Itu 'Respons Fasa'?", "id": "Bagian Fasa Dari Respons Frekuensi." },
  { "en": "Apa Itu 'Sistem Fasa Minimum'?", "id": "Sistem Stabil Dan Invers Stabil." },
  { "en": "Apa Itu 'Sistem Fasa Non-Minimum'?", "id": "Punya Zero Di Setengah Bidang Kanan." },
  { "en": "Apa Itu 'All-Pass Filter'?", "id": "Filter Yang Hanya Mengubah Fasa." },
  { "en": "Apa Itu 'Tundaan Grup (Group Delay)'?", "id": "Ukuran Distorsi Fasa Rata-rata." },
  { "en": "Apa Itu 'Konstanta Waktu'?", "id": "Ukuran Kecepatan Respons Sistem." },
  { "en": "Bagaimana 'Konstanta Waktu' Terkait Frekuensi Cutoff?", "id": "Berbanding Terbalik." },
  { "en": "Apa Itu 'Frekuensi Gelombang Mikro ISM'?", "id": "2.45 Gigahertz (GHz)." },
  { "en": "Apa Itu 'Pergeseran Frekuensi Akustik'?", "id": "Perubahan Frekuensi Suara Akibat Doppler." },
  { "en": "Apa Itu 'Frekuensi Resonansi Ruangan'?", "id": "Frekuensi Suara Yang Menggema Kuat." },
  { "en": "Apa Itu 'Analisis Getaran'?", "id": "Mengukur Dan Menganalisis Frekuensi Getaran." },
  { "en": "Apa Kegunaan 'Analisis Getaran'?", "id": "Memprediksi Kegagalan Mesin." },
  { "en": "Apa Itu 'Frekuensi Rangkaian Terbuka'?", "id": "Frekuensi Osilasi Sistem Tanpa Beban." },
  { "en": "Apa Itu 'Frekuensi Rangkaian Tertutup'?", "id": "Frekuensi Osilasi Dengan Umpan Balik." },
  { "en": "Apa Itu 'Bandwidth Loop'?", "id": "Ukuran Respons Frekuensi Loop Kontrol." },
  { "en": "Apa Itu 'Frekuensi Tidal'?", "id": "Frekuensi Pasang Surut Laut." },
  { "en": "Apa Itu 'Spektrum Sinyal EKG'?", "id": "Komponen Frekuensi Sinyal Jantung." },
  { "en": "Apa Kepanjangan 'EKG (Elektrokardiogram)'?", "id": "Rekaman Aktivitas Listrik Jantung." },
  { "en": "Apa Frekuensi 'Gelombang P' EKG?", "id": "Terkait Dengan Aktivitas Atrium." },
  { "en": "Apa Frekuensi 'Kompleks QRS' EKG?", "id": "Terkait Dengan Aktivitas Ventrikel." },
  { "en": "Apa Itu 'Frekuensi Fibrilasi'?", "id": "Frekuensi Sangat Cepat Dan Tidak Teratur." },
  { "en": "Apa Itu 'Frekuensi Radar'?", "id": "Frekuensi Gelombang Radio Yang Digunakan Radar." },
  { "en": "Apa Itu 'Radar Doppler'?", "id": "Radar Yang Mengukur Kecepatan." },
  { "en": "Apa Itu 'Sapu Frekuensi (Chirp)' Radar?", "id": "Sinyal Radar Dengan Frekuensi Berubah." },
  { "en": "Apa Itu 'Pita X' Radar?", "id": "Band Frekuensi Sekitar 8-12 GHz." },
  { "en": "Apa Itu 'Pita S' Radar?", "id": "Band Frekuensi Sekitar 2-4 GHz." },
  { "en": "Apa Itu 'Resolusi Frekuensi'?", "id": "Kemampuan Membedakan Dua Frekuensi Dekat." },
  { "en": "Apa Itu 'Jendela (Windowing)'?", "id": "Fungsi Untuk Mengurangi Kebocoran Spektral." },
  { "en": "Contoh 'Fungsi Jendela'?", "id": "Hamming, Hanning, Blackman." },
  { "en": "Apa Itu 'Scalloping Loss'?", "id": "Kerugian Akibat Frekuensi Sinyal." },
  { "en": "Apa Itu 'Periodogram'?", "id": "Estimasi Kepadatan Spektral Daya." },
  { "en": "Apa Itu 'Korelasi Silang'?", "id": "Ukuran Kemiripan Dua Sinyal." },
  { "en": "Apa Itu 'Autokorelasi'?", "id": "Korelasi Silang Sinyal Dengan Dirinya." },
  { "en": "Apa Itu 'Sinyal Stasioner'?", "id": "Sinyal Dengan Statistik Konstan." },
  { "en": "Apa Itu 'Sinyal Ergodik'?", "id": "Rata-rata Waktu Sama Dengan Ensemble." },
  { "en": "Apa Itu 'Spektrometer Massa'?", "id": "Alat Ukur Rasio Massa-Muatan." },
  { "en": "Apa Itu 'Frekuensi Siklotron Ion'?", "id": "Frekuensi Putaran Ion Dalam Medan Magnet." },
  { "en": "Apa Itu 'Frekuensi Radio Astronomi'?", "id": "Studi Benda Langit Pada Frekuensi Radio." },
  { "en": "Apa Itu 'Garis Hidrogen 21 cm'?", "id": "Garis Emisi Penting Dalam Astronomi." },
  { "en": "Apa Itu 'Spektroskopi Resonansi Magnetik'?", "id": "Teknik Analisis Berbasis NMR." },
  { "en": "Apa Itu 'Pergeseran Kimia'?", "id": "Pergeseran Frekuensi Resonansi Akibat Lingkungan." },
  { "en": "Apa Itu 'Frekuensi Getaran Ikatan'?", "id": "Frekuensi Spesifik Getaran Ikatan Kimia." },
  { "en": "Apa Itu 'Spektroskopi FTIR (Fourier Transform Infrared)'?", "id": "Teknik Spektroskopi Inframerah." },
  { "en": "Apa Itu 'Interferometer'?", "id": "Alat Yang Gunakan Interferensi Gelombang." },
  { "en": "Apa Itu 'Interferometer Michelson'?", "id": "Jenis Interferometer Optik Dasar." },
  { "en": "Apa Itu 'Pola Interferensi'?", "id": "Pola Terang-Gelap Akibat Superposisi." },
  { "en": "Apa Itu 'Frekuensi Gelombang De Broglie'?", "id": "Frekuensi Yang Terkait Dengan Partikel." },
  { "en": "Apa Itu 'Frekuensi Zarah'?", "id": "Frekuensi Terkait Dengan Energi Diam." },
  { "en": "Apa Itu 'Frekuensi Cutoff' Serat Optik?", "id": "Frekuensi Di Bawahnya Mode Tunggal." },
  { "en": "Apa Itu 'Mode Orde Tinggi'?", "id": "Pola Propagasi Selain Mode Fundamental." },
  { "en": "Apa Itu 'Penyaringan Mode (Mode Filtering)'?", "id": "Menghilangkan Mode Orde Tinggi." },
  { "en": "Apa Itu 'Frekuensi Terkopel'?", "id": "Frekuensi Osilasi Sistem Yang Terhubung." },
  { "en": "Apa Itu 'Mode Simetris'?", "id": "Mode Getaran Dengan Pola Simetris." },
  { "en": "Apa Itu 'Mode Asimetris'?", "id": "Mode Getaran Dengan Pola Asimetris." },
  { "en": "Apa Itu 'Frekuensi Resonansi Akustik'?", "id": "Frekuensi Suara Yang Diperkuat Ruangan." },
  { "en": "Apa Itu 'Gema (Reverberation)'?", "id": "Pantulan Suara Yang Bertahan." },
  { "en": "Apa Itu 'Waktu Gema'?", "id": "Waktu Suara Meluruh 60 Desibel." },
  { "en": "Apa Itu 'Frekuensi Sirip Pendingin'?", "id": "Frekuensi Getaran Alami Sirip Pendingin." },
  { "en": "Apa Itu 'Flutter'?", "id": "Getaran Tak Stabil Akibat Aerodinamika." },
  { "en": "Apa Itu 'Frekuensi Resonansi Jembatan'?", "id": "Frekuensi Getaran Alami Jembatan." },
  { "en": "Apa Itu 'Frekuensi Langkah Kaki'?", "id": "Frekuensi Langkah Orang Berjalan." },
  { "en": "Apa Itu 'Umpan Balik Akustik'?", "id": "Loop Suara Dari Speaker Ke Mikrofon." },
  { "en": "Bagaimana 'Umpan Balik Akustik' Terjadi?", "id": "Sinyal Diperkuat Berulang Kali." },
  { "en": "Apa Itu 'Pembatalan Umpan Balik'?", "id": "Teknik Untuk Menekan Umpan Balik." },
  { "en": "Apa Itu 'Filter Notch'?", "id": "Filter Yang Menolak Frekuensi Sangat Sempit." },
  { "en": "Apa Beda 'Periode' Dan 'Siklus'?", "id": "Siklus Kejadian, Periode Waktunya." },
  { "en": "Apa Beda 'Frekuensi' Dan 'Laju'?", "id": "Frekuensi Kekerapan, Laju Kecepatan." },
  { "en": "Apa Beda 'Getaran' Dan 'Gelombang'?", "id": "Gelombang Adalah Getaran Yang Merambat." },
  { "en": "Apa Penyebab Utama 'Efek Doppler'?", "id": "Gerak Relatif Antara Sumber-Pengamat." },
  { "en": "Apa Yang Menentukan 'Timbre' Suara?", "id": "Campuran Frekuensi Harmonisa." },
  { "en": "Bagaimana 'Kepadatan Medium' Mempengaruhi Kecepatan Suara?", "id": "Medium Padat, Suara Lebih Cepat." },
  { "en": "Frekuensi Apa Yang Dipakai 'GPS (Global Positioning System)'?", "id": "Sekitar 1.2 Dan 1.5 GHz." },
  { "en": "Frekuensi Apa Untuk 'Remote Kunci Mobil'?", "id": "Biasanya Sekitar 315 Atau 433 MHz." },
  { "en": "Berapa 'Refresh Rate' Umum Bioskop?", "id": "24 Hertz (Hz) Atau 24 FPS." },
  { "en": "Apa Arti 'Modulasi'?", "id": "Menumpangkan Informasi Pada Gelombang Pembawa." },
  { "en": "Apa Arti 'Demodulasi'?", "id": "Memisahkan Informasi Dari Gelombang Pembawa." },
  { "en": "Apa Arti 'Sinkronisasi'?", "id": "Menyelaraskan Waktu Atau Frekuensi." },
  { "en": "Apa Itu 'Frekuensi Alami' Sebuah Struktur?", "id": "Frekuensi Getaran Cenderungannya." },
  { "en": "Apa Itu 'Frekuensi Detak Jantung'?", "id": "Jumlah Detak Jantung Per Waktu." },
  { "en": "Apa Itu 'Frekuensi Getaran Ikatan Kimia'?", "id": "Frekuensi Alami Getaran Antar Atom." },
  { "en": "Apa Itu 'Pita Frekuensi Radio FM'?", "id": "88 Hingga 108 Megahertz (MHz)." },
  { "en": "Apa Itu 'Pita Frekuensi Radio AM'?", "id": "535 Hingga 1705 Kilohertz (kHz)." },
  { "en": "Apa Itu 'Aliasing Spasial'?", "id": "Pola Aneh Akibat Sampling Gambar." },
  { "en": "Apa Itu 'Pola Moiré'?", "id": "Contoh Efek Aliasing Spasial." },
  { "en": "Apa Itu 'Frekuensi Sudut Kritis'?", "id": "Frekuensi Sudut Yang Terkait Redaman." },
  { "en": "Apa Itu 'Frekuensi Resonansi Volume'?", "id": "Resonansi Udara Dalam Sebuah Rongga." },
  { "en": "Apa Itu 'Anharmonicity'?", "id": "Overtone Yang Bukan Kelipatan Bulat." },
  { "en": "Apa Itu 'Spektrum Bintang'?", "id": "Distribusi Frekuensi Cahaya Bintang." },
  { "en": "Apa Yang Bisa Dipelajari Dari 'Spektrum Bintang'?", "id": "Komposisi Kimia Dan Suhu Bintang." },
  { "en": "Apa Itu 'Frekuensi Osilasi Plasma'?", "id": "Frekuensi Osilasi Kolektif Elektron." },
  { "en": "Apa Itu 'Frekuensi Mode TE'?", "id": "Frekuensi Mode Transversal Elektrik." },
  { "en": "Apa Itu 'Frekuensi Mode TM'?", "id": "Frekuensi Mode Transversal Magnetik." },
  { "en": "Apa Itu 'Frekuensi Gelombang Milimeter'?", "id": "Frekuensi Antara 30 Hingga 300 GHz." },
  { "en": "Apa Itu 'Pita Terahertz (THz)'?", "id": "Frekuensi Antara Inframerah Dan Mikro." },
  { "en": "Apa Itu 'Radiasi Terahertz'?", "id": "Gelombang Elektromagnetik Di Pita THz." },
  { "en": "Apa Itu 'Frekuensi Resonansi Orbital'?", "id": "Hubungan Periodik Antara Benda Mengorbit." },
  { "en": "Apa Itu 'Frekuensi angular'?", "id": "Nama Lain Untuk Frekuensi Sudut." },
  { "en": "Apa Itu 'Sinyal In-phase (I)'?", "id": "Komponen Sinyal Searah Dengan Carrier." },
  { "en": "Apa Itu 'Sinyal Kuadratur (Q)'?", "id": "Komponen Sinyal Beda Fasa 90 Derajat." },
  { "en": "Apa Itu 'Diagram I/Q'?", "id": "Representasi Sinyal Pada Bidang Kompleks." },
  { "en": "Apa Itu 'Kanal Komunikasi'?", "id": "Medium Untuk Transmisi Sinyal." },
  { "en": "Apa Itu 'Derau Kanal'?", "id": "Gangguan Acak Dalam Kanal Komunikasi." },
  { "en": "Apa Itu 'Kapasitas Shannon'?", "id": "Laju Data Maksimal Teoritis Kanal." },
  { "en": "Apa Itu 'SNR (Signal-to-Noise Ratio)'?", "id": "Rasio Kekuatan Sinyal Terhadap Derau." },
  { "en": "Bagaimana 'SNR' Mempengaruhi Kapasitas?", "id": "SNR Tinggi, Kapasitas Tinggi." },
  { "en": "Bagaimana 'Bandwidth' Mempengaruhi Kapasitas?", "id": "Bandwidth Lebar, Kapasitas Tinggi." },
  { "en": "Apa Itu 'Kode Koreksi Kesalahan'?", "id": "Menambah Redundansi Untuk Deteksi Kesalahan." },
  { "en": "Apa Itu 'Bit Paritas'?", "id": "Bit Tambahan Untuk Deteksi Kesalahan." },
  { "en": "Apa Itu 'Teori Informasi'?", "id": "Studi Kuantifikasi Informasi." },
  { "en": "Apa Itu 'Entropi' Dalam Informasi?", "id": "Ukuran Ketidakpastian Atau Informasi." },
  { "en": "Apa Itu 'Frekuensi Simbol'?", "id": "Jumlah Simbol Yang Dikirim Per Detik." },
  { "en": "Apa Itu 'Baud Rate'?", "id": "Nama Lain Untuk Frekuensi Simbol." },
  { "en": "Apa Beda 'Bit Rate' Dan 'Baud Rate'?", "id": "Bit Rate Bisa Lebih Tinggi." },
  { "en": "Apa Itu 'Modulasi Digital'?", "id": "Representasi Data Digital Dengan Sinyal Analog." },
  { "en": "Apa Itu 'Diagram Mata (Eye Diagram)'?", "id": "Visualisasi Kualitas Sinyal Digital." },
  { "en": "Apa Itu 'Bukaan Mata'?", "id": "Area Terbuka Dalam Diagram Mata." },
  { "en": "Apa Arti 'Bukaan Mata' Lebar?", "id": "Sinyal Berkualitas Baik, Derau Rendah." },
  { "en": "Apa Itu 'Generator Sinyal Arbitrer'?", "id": "Pembangkit Sinyal Dengan Bentuk Kustom." },
  { "en": "Apa Itu 'Penganalisis Jaringan Vektor (VNA)'?", "id": "Mengukur Respons Frekuensi Kompleks." },
  { "en": "Apa Itu 'Parameter-S'?", "id": "Parameter Yang Diukur Oleh VNA." },
  { "en": "Apa Itu 'Return Loss'?", "id": "Ukuran Sinyal Yang Dipantulkan." },
  { "en": "Apa Itu 'Insertion Loss'?", "id": "Ukuran Sinyal Yang Hilang." },
  { "en": "Apa Itu 'VSWR (Voltage Standing Wave Ratio)'?", "id": "Rasio Gelombang Tegangan Berdiri." },
  { "en": "Apa Arti 'VSWR' Rendah?", "id": "Pencocokan Impedansi Yang Baik." },
  { "en": "Apa Itu 'Pencocokan Impedansi'?", "id": "Menyesuaikan Impedansi Untuk Transfer Daya." },
  { "en": "Apa Itu 'Smith Chart'?", "id": "Grafik Untuk Analisis Impedansi." },
  { "en": "Apa Itu 'Transformasi Fourier Cepat (FFT)'?", "id": "Algoritma Efisien Untuk Menghitung DFT." },
  { "en": "Apa Kepanjangan 'DFT (Discrete Fourier Transform)'?", "id": "Transformasi Fourier Diskrit." },
  { "en": "Apa Itu 'Resolusi Frekuensi' FFT?", "id": "Jarak Antar Titik Frekuensi." },
  { "en": "Apa Itu 'Bin' FFT?", "id": "Satu Titik Frekuensi Hasil FFT." },
  { "en": "Apa Itu 'Frekuensi Resonansi Gyromagnetic'?", "id": "Frekuensi Presesi Momen Magnetik Elektron." },
  { "en": "Apa Itu 'Osilasi Plasma'?", "id": "Osilasi Kolektif Elektron Dalam Plasma." },
  { "en": "Apa Itu 'Panjang Debye'?", "id": "Jarak Perisai Muatan Dalam Plasma." },
  { "en": "Apa Itu 'Frekuensi Resonansi Akustik Ion'?", "id": "Gelombang Suara Dalam Plasma." },
  { "en": "Apa Itu 'Frekuensi Gelombang Permukaan'?", "id": "Gelombang Yang Merambat Di Antarmuka." },
  { "en": "Apa Itu 'Gelombang Kapiler'?", "id": "Riak Kecil Di Permukaan Cairan." },
  { "en": "Apa Itu 'Frekuensi Pasang Surut'?", "id": "Frekuensi Naik Turunnya Permukaan Laut." },
  { "en": "Apa Itu 'Frekuensi Chandler Wobble'?", "id": "Gerakan Sumbu Rotasi Bumi." },
  { "en": "Apa Itu 'Frekuensi Milankovitch'?", "id": "Siklus Perubahan Orbit Bumi." },
  { "en": "Apa Itu 'Frekuensi Presesi Ekuinoks'?", "id": "Gerak Lambat Sumbu Rotasi Bumi." },
  { "en": "Apa Itu 'Sistem Acuan Inersia'?", "id": "Kerangka Acuan Tanpa Percepatan." },
  { "en": "Apa Itu 'Relativitas Khusus'?", "id": "Teori Ruang Dan Waktu Einstein." },
  { "en": "Apa Itu 'Dilatasi Waktu'?", "id": "Waktu Berjalan Lebih Lambat." },
  { "en": "Apa Itu 'Kontraksi Panjang'?", "id": "Objek Tampak Lebih Pendek." },
  { "en": "Bagaimana 'Frekuensi' Dilihat Dalam Relativitas?", "id": "Dipengaruhi Oleh Dilatasi Waktu." },
  { "en": "Apa Itu 'Prinsip Ekuivalensi'?", "id": "Gravitasi Dan Percepatan Ekuivalen." },
  { "en": "Apa Itu 'Relativitas Umum'?", "id": "Teori Gravitasi Einstein." },
  { "en": "Apa Itu 'Lengkungan Ruang-Waktu'?", "id": "Penyebab Gravitasi Menurut Relativitas Umum." },
  { "en": "Apa Itu 'Mekanika Kuantum'?", "id": "Teori Fisika Skala Sangat Kecil." },
  { "en": "Apa Itu 'Kuantisasi'?", "id": "Besaran Fisik Hanya Ada Dalam Nilai Diskrit." },
  { "en": "Apa Itu 'Fungsi Gelombang'?", "id": "Deskripsi Matematis Keadaan Kuantum." },
  { "en": "Apa Itu 'Persamaan Schrödinger'?", "id": "Persamaan Dasar Mekanika Kuantum." },
  { "en": "Apa Itu 'Prinsip Ketidakpastian Heisenberg'?", "id": "Batas Akurasi Pengukuran Pasangan Variabel." },
  { "en": "Apa Pasangan Variabel Dalam 'Prinsip Ketidakpastian'?", "id": "Posisi-Momentum, Energi-Waktu." },
  { "en": "Bagaimana 'Ketidakpastian' Terkait Frekuensi?", "id": "Ketidakpastian Energi Terkait Frekuensi." },
  { "en": "Apa Itu 'Penerowongan Kuantum (Quantum Tunneling)'?", "id": "Partikel Menembus Penghalang Potensial." },
  { "en": "Apa Itu 'Keadaan Dasar (Ground State)'?", "id": "Tingkat Energi Terendah Yang Mungkin." },
  { "en": "Apa Itu 'Keadaan Tereksitasi (Excited State)'?", "id": "Tingkat Energi Di Atas Keadaan Dasar." },
  { "en": "Apa Itu 'Mekanika Statistik'?", "id": "Menerapkan Statistik Pada Sistem Fisik." },
  { "en": "Apa Itu 'Ansambel (Ensemble)'?", "id": "Kumpulan Besar Sistem Serupa." },
  { "en": "Apa Beda 'Frekuensi' Dan 'Fase'?", "id": "Frekuensi Laju, Fase Posisi." },
  { "en": "Apa Beda 'Siklus' Dan 'Hertz'?", "id": "Siklus Kejadian, Hertz Satuan." },
  { "en": "Apa Beda 'Osilasi' Dan 'Rotasi'?", "id": "Osilasi Bolak-balik, Rotasi Berputar." },
  { "en": "Apa Hubungan 'Energi' Dan 'Frekuensi' Foton?", "id": "Energi Sebanding Langsung Dengan Frekuensi." },
  { "en": "Apa Itu 'Frekuensi Angular'?", "id": "Nama Lain Untuk Frekuensi Sudut." },
  { "en": "Frekuensi Radio Apa Yang Dipantulkan 'Bulan'?", "id": "VHF, UHF, Dan Gelombang Mikro." },
  { "en": "Apa Definisi 'Satu Siklus'?", "id": "Satu Gerakan Lengkap Yang Berulang." },
  { "en": "Apa Arti Awalan 'Giga' Dalam Frekuensi?", "id": "Satu Miliar (10^9)." },
  { "en": "Apa Terjadi Pada 'Panjang Gelombang' Ambulans Mendekat?", "id": "Terlihat Lebih Pendek." },
  { "en": "Apa Terjadi Pada 'Panjang Gelombang' Ambulans Menjauh?", "id": "Terlihat Lebih Panjang." },
  { "en": "Apa Itu 'Pita Frekuensi Penyiaran'?", "id": "Alokasi Frekuensi Untuk Radio Dan TV." },
  { "en": "Apa Itu 'Frekuensi Referensi Cesium'?", "id": "Dasar Waktu Atom Internasional." },
  { "en": "Apa Itu 'Periodisitas Spasial'?", "id": "Pengulangan Pola Dalam Ruang." },
  { "en": "Apa Itu 'Bandwidth Sinyal Audio'?", "id": "Rentang Frekuensi Suara Dalam Sinyal." },
  { "en": "Apa Itu 'Frekuensi Denyut Nadi'?", "id": "Frekuensi Detak Arteri Per Menit." },
  { "en": "Apa Itu 'Frekuensi Planck'?", "id": "Batas Atas Frekuensi Yang Mungkin." },
  { "en": "Apa Itu 'Bandwidth Koherensi'?", "id": "Rentang Frekuensi Dengan Fading Serupa." },
  { "en": "Apa Itu 'Waktu Koherensi'?", "id": "Durasi Waktu Kanal Tetap Konstan." },
  { "en": "Apa Beda 'Waktu' Dan 'Koherensi'?", "id": "Berbanding Terbalik Satu Sama Lain." },
  { "en": "Apa Itu 'Frekuensi Resonansi Paramagnetik'?", "id": "Frekuensi Resonansi Spin Elektron." },
  { "en": "Apa Itu 'Filter Sisir (Comb Filter)'?", "id": "Filter Dengan Respon Mirip Sisir." },
  { "en": "Apa Efek 'Filter Sisir'?", "id": "Menciptakan Serangkaian Puncak Dan Lembah." },
  { "en": "Apa Itu 'Frekuensi Alami Tak Teredam'?", "id": "Frekuensi Sistem Tanpa Redaman." },
  { "en": "Apa Itu 'Aliasing Temporal'?", "id": "Aliasing Dalam Domain Waktu." },
  { "en": "Apa Itu 'Frekuensi Pivotal'?", "id": "Frekuensi Referensi Dalam Desain Filter." },
  { "en": "Apa Itu 'Frekuensi Geometris Rata-rata'?", "id": "Pusat Geometris Antara Dua Frekuensi." },
  { "en": "Apa Itu 'Jarak Frekuensi'?", "id": "Pemisahan Antara Dua Frekuensi." },
  { "en": "Apa Itu 'Offset Frekuensi Pembawa'?", "id": "Perbedaan Frekuensi Carrier Aktual-Nominal." },
  { "en": "Apa Itu 'Kunci Frekuensi'?", "id": "Sistem Menyelaraskan Frekuensi Osilator." },
  { "en": "Apa Itu 'Spektrum Sinyal Suara'?", "id": "Distribusi Frekuensi Dalam Ucapan." },
  { "en": "Apa Itu 'Analisis Spektral'?", "id": "Proses Mempelajari Spektrum Frekuensi." },
  { "en": "Apa Itu 'Pita Vokal'?", "id": "Frekuensi Dasar Suara Manusia." },
  { "en": "Apa Itu 'Sibilans'?", "id": "Suara Desis Frekuensi Tinggi (S, F)." },
  { "en": "Apa Itu 'Pita Frekuensi Batuk'?", "id": "Karakteristik Frekuensi Suara Batuk." },
  { "en": "Apa Itu 'Ultrasonografi Medis'?", "id": "Pencitraan Menggunakan Suara Frekuensi Tinggi." },
  { "en": "Frekuensi Berapa 'Ultrasonografi' Digunakan?", "id": "2 Hingga 18 Megahertz (MHz)." },
  { "en": "Apa Itu 'Litotripsi'?", "id": "Menghancurkan Batu Ginjal Dengan Frekuensi." },
  { "en": "Apa Itu 'Frekuensi Resonansi Inti'?", "id": "Frekuensi Resonansi Untuk NMR/MRI." },
  { "en": "Apa Itu 'Spektroskopi Ultraviolet-Tampak (UV-Vis)'?", "id": "Mengukur Penyerapan Cahaya UV-Tampak." },
  { "en": "Apa Itu 'Fluoresensi'?", "id": "Emisi Cahaya Pada Frekuensi Rendah." },
  { "en": "Apa Itu 'Fosforesensi'?", "id": "Emisi Cahaya Yang Tertunda." },
  { "en": "Apa Itu 'Pergeseran Stokes'?", "id": "Perbedaan Frekuensi Absorpsi Dan Emisi." },
  { "en": "Apa Itu 'Pelebaran Garis Spektral Alami'?", "id": "Pelebaran Akibat Prinsip Ketidakpastian." },
  { "en": "Apa Itu 'Frekuensi Pusat Molekul'?", "id": "Frekuensi Transisi Molekuler." },
  { "en": "Apa Itu 'Frekuensi Rotasi Bumi'?", "id": "Satu Putaran Per Hari." },
  { "en": "Apa Itu 'Frekuensi Revolusi Bumi'?", "id": "Satu Putaran Per Tahun." },
  { "en": "Apa Itu 'Efek Pasang Surut'?", "id": "Gaya Gravitasi Periodik Bulan-Matahari." },
  { "en": "Apa Itu 'Frekuensi Osilasi Chandler'?", "id": "Gerakan Sumbu Rotasi Bumi." },
  { "en": "Apa Itu 'Siklus Aktivitas Matahari'?", "id": "Siklus Bintik Matahari Sekitar 11 Tahun." },
  { "en": "Apa Itu 'Getaran Jembatan Akibat Angin'?", "id": "Resonansi Struktur Jembatan Karena Angin." },
  { "en": "Apa Itu 'Frekuensi Kibasan Sayap'?", "id": "Frekuensi Gerakan Sayap Serangga/Burung." },
  { "en": "Apa Itu 'Frekuensi Dengkuran Kucing'?", "id": "Getaran Frekuensi Rendah (20-150 Hz)." },
  { "en": "Apa Itu 'Frekuensi Kerdipan Lampu Neon'?", "id": "Dua Kali Frekuensi Jaringan Listrik." },
  { "en": "Mengapa Lampu Neon Berkedip?", "id": "Arus AC Melewati Nol Dua Kali." },
  { "en": "Apa Itu 'Frekuensi Bingkai Video'?", "id": "Jumlah Bingkai Gambar Per Detik." },
  { "en": "Apa Itu 'Frekuensi Medan Video'?", "id": "Setengah Frekuensi Bingkai (Interlaced)." },
  { "en": "Apa Itu 'Nada Panggil Telepon'?", "id": "Kombinasi Dua Frekuensi Spesifik." },
  { "en": "Apa Itu 'Frekuensi Not Musik'?", "id": "Frekuensi Dasar Yang Dihasilkan Alat Musik." },
  { "en": "Bagaimana 'Frekuensi' Mempengaruhi Persepsi Warna?", "id": "Frekuensi Tinggi Biru, Rendah Merah." },
  { "en": "Bagaimana 'Frekuensi' Mempengaruhi Persepsi Suara?", "id": "Frekuensi Tinggi Nada Tinggi." },
  { "en": "Apa Itu 'Kanal Ion'?", "id": "Protein Yang Melewatkan Ion." },
  { "en": "Apa Itu 'Frekuensi Aktivasi Saluran Ion'?", "id": "Frekuensi Buka-Tutup Kanal Ion." },
  { "en": "Apa Itu 'Potensial Aksi'?", "id": "Pulsa Listrik Singkat Di Neuron." },
  { "en": "Apa Itu 'Laju Tembak Neuron'?", "id": "Frekuensi Potensial Aksi Yang Dihasilkan." },
  { "en": "Apa Itu 'Osilasi Neural'?", "id": "Aktivitas Otak Ritmis Dan Periodik." },
  { "en": "Apa Itu 'Sinkronisasi Fasa' Dalam Otak?", "id": "Penyelarasan Waktu Antara Osilasi Neural." },
  { "en": "Apa Itu 'Jendela Temporal'?", "id": "Durasi Waktu Untuk Pemrosesan Sinyal." },
  { "en": "Apa Itu 'Resolusi Waktu'?", "id": "Kemampuan Membedakan Kejadian Dekat." },
  { "en": "Apa Beda 'Resolusi Waktu' Dan 'Frekuensi'?", "id": "Berbanding Terbalik (Prinsip Ketidakpastian)." },
  { "en": "Apa Itu 'Transformasi Laplace'?", "id": "Generalisasi Transformasi Fourier." },
  { "en": "Apa Itu 'Frekuensi Kompleks' Dalam Laplace?", "id": "Variabel 's' Dengan Bagian Riil-Imajiner." },
  { "en": "Apa Arti Bagian 'Riil' Frekuensi Kompleks?", "id": "Peluruhan Atau Pertumbuhan Eksponensial." },
  { "en": "Apa Arti Bagian 'Imajiner' Frekuensi Kompleks?", "id": "Osilasi Sinusoidal." },
  { "en": "Apa Itu 'Sistem Stabil'?", "id": "Respons Impuls Meluruh Seiring Waktu." },
  { "en": "Apa Itu 'Frekuensi Sudut Tak Teredam'?", "id": "Frekuensi Osilasi Tanpa Redaman." },
  { "en": "Apa Itu 'Karakteristik Frekuensi'?", "id": "Nama Lain Untuk Respons Frekuensi." },
  { "en": "Apa Itu 'Plot Nyquist'?", "id": "Plot Respons Frekuensi Pada Bidang Kompleks." },
  { "en": "Apa Itu 'Kriteria Stabilitas Nyquist'?", "id": "Metode Menentukan Stabilitas Dari Plot." },
  { "en": "Apa Itu 'Frekuensi Sampling Spasial'?", "id": "Jumlah Sampel Per Satuan Jarak." },
  { "en": "Apa Itu 'Piksel'?", "id": "Elemen Terkecil Dari Gambar Digital." },
  { "en": "Bagaimana 'Piksel' Terkait Frekuensi Spasial?", "id": "Kerapatan Piksel Menentukan Frekuensi Maksimal." },
  { "en": "Apa Itu 'Teorema Sampling Whittaker-Shannon'?", "id": "Nama Formal Untuk Teorema Nyquist." },
  { "en": "Apa Itu 'Rekonstruksi Sinyal'?", "id": "Membuat Sinyal Analog Dari Sampel." },
  { "en": "Apa Itu 'Filter Rekonstruksi'?", "id": "Filter Low-pass Untuk Menghaluskan Sinyal." },
  { "en": "Apa Itu 'Zero-Order Hold'?", "id": "Metode Rekonstruksi Sinyal Paling Sederhana." },
  { "en": "Apa Itu 'Sinyal Waktu-Diskrit'?", "id": "Sinyal Yang Didefinisikan Pada Waktu Diskrit." },
  { "en": "Apa Beda 'Diskrit' Dan 'Digital'?", "id": "Digital Adalah Diskrit Yang Dikuantisasi." },
  { "en": "Apa Itu 'Bank Filter'?", "id": "Kumpulan Filter Untuk Memecah Sinyal." },
  { "en": "Apa Itu 'Transformasi Wavelet'?", "id": "Analisis Waktu-Frekuensi Dengan Resolusi Adaptif." },
  { "en": "Apa Beda 'Wavelet' Dan 'Fourier'?", "id": "Wavelet Terlokalisasi Dalam Waktu." },
  { "en": "Apa Itu 'Skalogram'?", "id": "Visualisasi Hasil Transformasi Wavelet." },
  { "en": "Apa Itu 'Sinyal Chirp'?", "id": "Sinyal Dengan Frekuensi Meningkat/Menurun." },
  { "en": "Apa Itu 'Frekuensi Sudut Gibbs'?", "id": "Frekuensi Terkait Fenomena Gibbs." },
  { "en": "Apa Itu 'Fenomena Gibbs'?", "id": "Overshoot Dekat Diskontinuitas." },
  { "en": "Apa Itu 'Teori Perturbasi'?", "id": "Metode Matematis Untuk Solusi Aproksimasi." },
  { "en": "Apa Itu 'Frekuensi Mode Bocor'?", "id": "Mode Yang Kehilangan Energi Saat Merambat." },
  { "en": "Apa Itu 'Frekuensi Normalisasi' Dalam Optik?", "id": "Parameter V-number Serat Optik." }



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
