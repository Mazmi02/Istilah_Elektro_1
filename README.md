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


    { "en": "Apa Itu 'Resistor'?", "id": "Komponen Pasif Penghambat Arus Listrik." },
    { "en": "Apa Itu 'Kapasitor'?", "id": "Menyimpan Energi Dalam Medan Listrik." },
    { "en": "Apa Itu 'Induktor'?", "id": "Menyimpan Energi Dalam Medan Magnet." },
    { "en": "Apa Itu 'Transistor'?", "id": "Komponen Semikonduktor Untuk Saklar, Penguat." },
    { "en": "Apa Itu 'Dioda'?", "id": "Menyearahkan Arus Listrik Satu Arah." },
    { "en": "Apa Itu 'IC' (Integrated Circuit)?", "id": "Sirkuit Elektronik Terintegrasi Dalam Chip." },
    { "en": "Apa Itu 'Tegangan' (Voltage)?", "id": "Beda Potensial Antara Dua Titik." },
    { "en": "Apa Itu 'Arus' (Current)?", "id": "Aliran Muatan Listrik Per Waktu." },
    { "en": "Apa Itu 'Hambatan' (Resistance)?", "id": "Ukuran Penolakan Terhadap Arus Listrik." },
    { "en": "Apa Itu 'Daya' (Power)?", "id": "Laju Energi Listrik Yang Ditransfer." },
    { "en": "Apa Itu 'Frekuensi'?", "id": "Jumlah Getaran Gelombang Per Detik." },
    { "en": "Apa Itu 'Volt'?", "id": "Satuan Ukuran Untuk Tegangan Listrik." },
    { "en": "Apa Itu 'Ampere'?", "id": "Satuan Ukuran Aliran Arus Listrik." },
    { "en": "Apa Itu 'Ohm'?", "id": "Satuan Ukuran Hambatan Listrik Dasar." },
    { "en": "Apa Itu 'Watt'?", "id": "Satuan Ukuran Konsumsi Daya Listrik." },
    { "en": "Apa Itu 'AC' (Alternating Current)?", "id": "Arus Listrik Bolak-Balik Secara Periodik." },
    { "en": "Apa Itu 'DC' (Direct Current)?", "id": "Arus Listrik Searah Jalur Tetap." },
    { "en": "Apa Itu 'LED' (Light Emitting Diode)?", "id": "Dioda Yang Memancarkan Cahaya Elektronik." },
    { "en": "Apa Itu 'Transformer'?", "id": "Alat Pengubah Level Tegangan Listrik." },
    { "en": "Apa Itu 'PCB' (Printed Circuit Board)?", "id": "Papan Jalur Penghubung Komponen Elektronika." },
    { "en": "Apa Itu 'Ground'?", "id": "Titik Referensi Nol Tegangan Listrik." },
    { "en": "Apa Itu 'Farad'?", "id": "Satuan Ukuran Kapasitansi Sebuah Kapasitor." },
    { "en": "Apa Itu 'Henry'?", "id": "Satuan Ukuran Induktansi Sebuah Induktor." },
    { "en": "Apa Itu 'Hertz'?", "id": "Satuan Ukuran Frekuensi Gelombang Listrik." },
    { "en": "Apa Itu 'Joule'?", "id": "Satuan Ukuran Energi Listrik Dasar." },
    { "en": "Apa Itu 'Coulomb'?", "id": "Satuan Ukuran Muatan Listrik Dasar." },
    { "en": "Apa Itu 'Siemens'?", "id": "Satuan Ukuran Konduktansi Aliran Listrik." },
    { "en": "Apa Itu 'Tesla'?", "id": "Satuan Ukuran Kerapatan Fluks Magnetik." },
    { "en": "Apa Itu 'Weber'?", "id": "Satuan Ukuran Fluks Magnetik Dasar." },
    { "en": "Apa Itu 'SCR' (Silicon Controlled Rectifier)?", "id": "Penyearah Terkendali Berbahan Dasar Silikon." },
    { "en": "Apa Itu 'TRIAC' (Triode For Alternating Current)?", "id": "Saklar Elektronik Arus Bolak-Balik, Dua Arah." },
    { "en": "Apa Itu 'DIAC' (Diode For Alternating Current)?", "id": "Dioda Pemicu Arus Bolak-Balik, Dua Arah." },
    { "en": "Apa Itu 'Op-Amp' (Operational Amplifier)?", "id": "Penguat Operasional Sinyal Analog Terintegrasi." },
    { "en": "Apa Itu 'MOSFET' (Metal Oxide Semiconductor Field Effect Transistor)?", "id": "Transistor Efek Medan Kanal Terisolasi." },
    { "en": "Apa Itu 'JFET' (Junction Field Effect Transistor)?", "id": "Transistor Efek Medan Sambungan Tunggal." },
    { "en": "Apa Itu 'IGBT' (Insulated Gate Bipolar Transistor)?", "id": "Transistor Gerbang Terisolasi Daya Tinggi." },
    { "en": "Apa Itu 'Relay'?", "id": "Saklar Elektromagnetik Pengendali Arus Besar." },
    { "en": "Apa Itu 'Saklar' (Switch)?", "id": "Pemutus Dan Penghubung Aliran Listrik." },
    { "en": "Apa Itu 'Sekring' (Fuse)?", "id": "Pengaman Rangkaian Dari Arus Berlebih." },
    { "en": "Apa Itu 'Baterai' (Battery)?", "id": "Sumber Energi Kimia Menjadi Listrik." },
    { "en": "Apa Itu 'Panel Surya' (Solar Panel)?", "id": "Mengubah Cahaya Matahari Menjadi Listrik." },
    { "en": "Apa Itu 'Inverter'?", "id": "Pengubah Arus Searah Menjadi Bolak-Balik." },
    { "en": "Apa Itu 'Konverter' (Converter)?", "id": "Alat Pengubah Level Tegangan Searah." },
    { "en": "Apa Itu 'Rectifier'?", "id": "Rangkaian Penyearah Arus Bolak-Balik." },
    { "en": "Apa Itu 'LDR' (Light Dependent Resistor)?", "id": "Hambatan Yang Berubah Karena Cahaya." },
    { "en": "Apa Itu 'Thermistor'?", "id": "Hambatan Yang Berubah Karena Suhu." },
    { "en": "Apa Itu 'Potensiometer'?", "id": "Hambatan Variabel Yang Dapat Diatur." },
    { "en": "Apa Itu 'Multimeter'?", "id": "Alat Ukur Tegangan, Arus, Hambatan." },
    { "en": "Apa Itu 'Oscilloskop' (Oscilloscope)?", "id": "Alat Visualisasi Bentuk Gelombang Sinyal." },
    { "en": "Apa Itu 'Sinyal' (Signal)?", "id": "Besaran Elektrik Pembawa Informasi Data." },
    { "en": "Apa Itu 'Gelombang Sinus' (Sine Wave)?", "id": "Bentuk Gelombang Harmonik Dasar Listrik." },
    { "en": "Apa Itu 'Impedansi' (Impedance)?", "id": "Hambatan Total Dalam Rangkaian Bolak-Balik." },
    { "en": "Apa Itu 'Admitansi' (Admittance)?", "id": "Kemudahan Aliran Arus Rangkaian Bolak-Balik." },
    { "en": "Apa Itu 'Reaktansi' (Reactance)?", "id": "Hambatan Akibat Induktor Atau Kapasitor." },
    { "en": "Apa Itu 'Konduktor'?", "id": "Bahan Yang Mudah Mengalirkan Listrik." },
    { "en": "Apa Itu 'Isolator'?", "id": "Bahan Yang Sulit Mengalirkan Listrik." },
    { "en": "Apa Itu 'Semikonduktor'?", "id": "Bahan Dengan Konduktivitas Antara Keduanya." },
    { "en": "Apa Itu 'Anoda'?", "id": "Elektroda Bermuatan Positif Komponen Elektronika." },
    { "en": "Apa Itu 'Katoda'?", "id": "Elektroda Bermuatan Negatif Komponen Elektronika." },
    { "en": "Apa Itu 'Gate'?", "id": "Terminal Pengendali Pada Transistor Efek Medan." },
    { "en": "Apa Itu 'Drain'?", "id": "Terminal Saluran Keluar Pada Transistor MOSFET." },
    { "en": "Apa Itu 'Source'?", "id": "Terminal Sumber Pembawa Muatan MOSFET." },
    { "en": "Apa Itu 'Basis' (Base)?", "id": "Terminal Pengendali Arus Transistor Bipolar." },
    { "en": "Apa Itu 'Kolektor' (Collector)?", "id": "Terminal Penerima Muatan Transistor Bipolar." },
    { "en": "Apa Itu 'Emitor' (Emitter)?", "id": "Terminal Pemancar Muatan Transistor Bipolar." },
    { "en": "Apa Itu 'Hukum Ohm'?", "id": "Hubungan Tegangan, Arus, Dan Hambatan." },
    { "en": "Apa Itu 'Fase' (Phase)?", "id": "Posisi Relatif Gelombang Listrik Periodik." },
    { "en": "Apa Itu 'Netral' (Neutral)?", "id": "Kabel Balik Arus Sistem Listrik." },
    { "en": "Apa Itu 'Arus Bocor' (Leakage Current)?", "id": "Aliran Listrik Tidak Diinginkan Melalui Isolator." },
    { "en": "Apa Itu 'Hubung Singkat' (Short Circuit)?", "id": "Kontak Langsung Jalur Positif Dan Negatif." },
    { "en": "Apa Itu 'Beban' (Load)?", "id": "Komponen Yang Mengonsumsi Daya Listrik." },
    { "en": "Apa Itu 'Rangkaian Seri'?", "id": "Komponen Terhubung Secara Berurutan Sejajar." },
    { "en": "Apa Itu 'Rangkaian Paralel'?", "id": "Komponen Terhubung Secara Bercabang Berdampingan." },
    { "en": "Apa Itu 'LPF' (Low Pass Filter)?", "id": "Penyaring Sinyal Frekuensi Rendah Saja." },
    { "en": "Apa Itu 'HPF' (High Pass Filter)?", "id": "Penyaring Sinyal Frekuensi Tinggi Saja." },
    { "en": "Apa Itu 'Kapasitansi'?", "id": "Kemampuan Menyimpan Muatan Listrik Kapasitor." },
    { "en": "Apa Itu 'Induktansi'?", "id": "Kemampuan Menghasilkan Medan Magnet Induktor." },
    { "en": "Apa Itu 'Motor Listrik'?", "id": "Mengubah Energi Listrik Menjadi Gerak." },
    { "en": "Apa Itu 'Generator'?", "id": "Mengubah Energi Gerak Menjadi Listrik." },
    { "en": "Apa Itu 'Rotor'?", "id": "Bagian Berputar Pada Mesin Listrik." },
    { "en": "Apa Itu 'Stator'?", "id": "Bagian Diam Pada Mesin Listrik." },
    { "en": "Apa Itu 'Komutator'?", "id": "Penyearah Mekanis Pada Motor Searah." },
    { "en": "Apa Itu 'Slip Ring'?", "id": "Penghubung Listrik Bagian Berputar Mesin." },
    { "en": "Apa Itu 'Medan Magnet'?", "id": "Daerah Pengaruh Gaya Magnetik Sekitar." },
    { "en": "Apa Itu 'Medan Listrik'?", "id": "Daerah Pengaruh Gaya Listrik Sekitar." },
    { "en": "Apa Itu 'Fluks Magnetik'?", "id": "Total Garis Gaya Magnetik Area." },
    { "en": "Apa Itu 'EMF' (Electromotive Force)?", "id": "Energi Listrik Per Satuan Muatan." },
    { "en": "Apa Itu 'Mikrokontroler' (Microcontroller)?", "id": "Komputer Kecil Dalam Satu Chip." },
    { "en": "Apa Itu 'Mikroprosesor' (Microprocessor)?", "id": "Otak Pemroses Data Pusat Komputer." },
    { "en": "Apa Itu 'RAM' (Random Access Memory)?", "id": "Penyimpanan Data Sementara Kecepatan Tinggi." },
    { "en": "Apa Itu 'ROM' (Read Only Memory)?", "id": "Penyimpanan Data Permanen Hanya Baca." },
    { "en": "Apa Itu 'EEPROM' (Electrically Erasable Programmable Read Only Memory)?", "id": "Memori Permanen Dapat Dihapus Listrik." },
    { "en": "Apa Itu 'Gerbang AND' (AND Gate)?", "id": "Logika Output Tinggi Jika Semua Tinggi." },
    { "en": "Apa Itu 'Gerbang OR' (OR Gate)?", "id": "Logika Output Tinggi Jika Salah Satu Tinggi." },
    { "en": "Apa Itu 'Gerbang NOT' (NOT Gate)?", "id": "Logika Pembalik Nilai Masukan Sinyal." },
    { "en": "Apa Itu 'Flip-Flop'?", "id": "Sirkuit Logika Penyimpan Satu Bit." },
    { "en": "Apa Itu 'ADC' (Analog To Digital Converter)?", "id": "Pengubah Sinyal Analog Menjadi Digital." },
    { "en": "Apa Itu 'DAC' (Digital To Analog Converter)?", "id": "Pengubah Sinyal Digital Menjadi Analog." },
    { "en": "Apa Itu 'Breadboard'?", "id": "Papan Percobaan Rangkaian Elektronika Sementara." },
    { "en": "Apa Itu 'Solder'?", "id": "Alat Penyambung Komponen Dengan Timah." },
    { "en": "Apa Itu 'PWM' (Pulse Width Modulation)?", "id": "Teknik Modulasi Lebar Pulsa Listrik." },
    { "en": "Apa Itu 'Duty Cycle'?", "id": "Persentase Waktu Aktif Sinyal Periodik." },
    { "en": "Apa Itu 'SMPS' (Switched Mode Power Supply)?", "id": "Catu Daya Sistem Pensakelaran Efisien." },
    { "en": "Apa Itu 'UPS' (Uninterruptible Power Supply)?", "id": "Penyedia Daya Cadangan Saat Padam." },
    { "en": "Apa Itu 'AVR' (Automatic Voltage Regulator)?", "id": "Alat Penstabil Tegangan Listrik Otomatis." },
    { "en": "Apa Itu 'Step-Up Transformer'?", "id": "Transformator Peningkat Tegangan Arus Bolak-Balik." },
    { "en": "Apa Itu 'Step-Down Transformer'?", "id": "Transformator Penurun Tegangan Arus Bolak-Balik." },
    { "en": "Apa Itu 'Rectifier Bridge'?", "id": "Penyearah Gelombang Penuh, Empat Dioda." },
    { "en": "Apa Itu 'Ripple Voltage'?", "id": "Sisa Tegangan Bolak-Balik Pada Penyearah." },
    { "en": "Apa Itu 'Zener Diode'?", "id": "Dioda Penstabil Tegangan Referensi Khusus." },
    { "en": "Apa Itu 'Schottky Diode'?", "id": "Dioda Tegangan Rendah, Kecepatan Tinggi." },
    { "en": "Apa Itu 'Photodiode'?", "id": "Dioda Pengubah Cahaya Menjadi Arus." },
    { "en": "Apa Itu 'Varactor Diode'?", "id": "Dioda Dengan Kapasitansi Variabel Terkendali." },
    { "en": "Apa Itu 'Tunnel Diode'?", "id": "Dioda Dengan Karakteristik Resistansi Negatif." },
    { "en": "Apa Itu 'Laser Diode'?", "id": "Dioda Pemancar Cahaya Koheren Intens." },
    { "en": "Apa Itu 'Seven Segment Display'?", "id": "Display Angka Berbasis Tujuh Bagian." },
    { "en": "Apa Itu 'LCD' (Liquid Crystal Display)?", "id": "Layar Penampil Berbasis Kristal Cair." },
    { "en": "Apa Itu 'OLED' (Organic Light Emitting Diode)?", "id": "Panel Layar Organik Pemancar Cahaya." },
    { "en": "Apa Itu 'CRT' (Cathode Ray Tube)?", "id": "Tabung Sinar Katoda Untuk Penampil." },
    { "en": "Apa Itu 'Plasma Display'?", "id": "Layar Menggunakan Sel Gas Bermuatan." },
    { "en": "Apa Itu 'Sensor Ultrasonic'?", "id": "Pengukur Jarak Menggunakan Gelombang Suara." },
    { "en": "Apa Itu 'Sensor PIR' (Passive Infrared)?", "id": "Sensor Pendeteksi Gerakan Melalui Inframerah." },
    { "en": "Apa Itu 'Sensor Accelerometer'?", "id": "Sensor Pengukur Percepatan Linear Fisik." },
    { "en": "Apa Itu 'Gyroscope'?", "id": "Sensor Pengukur Kecepatan Sudut Rotasi." },
    { "en": "Apa Itu 'Magnetometer'?", "id": "Sensor Pengukur Kekuatan Medan Magnet." },
    { "en": "Apa Itu 'Barometer'?", "id": "Alat Pengukur Tekanan Udara Atmosfer." },
    { "en": "Apa Itu 'Humidity Sensor'?", "id": "Sensor Pengukur Kelembapan Udara Sekitar." },
    { "en": "Apa Itu 'Load Cell'?", "id": "Sensor Pengukur Berat Atau Gaya." },
    { "en": "Apa Itu 'Strain Gauge'?", "id": "Sensor Pengukur Regangan Deformasi Benda." },
    { "en": "Apa Itu 'Thermocouple'?", "id": "Sensor Suhu Berdasarkan Perbedaan Tegangan." },
    { "en": "Apa Itu 'RTD' (Resistance Temperature Detector)?", "id": "Sensor Suhu Berdasarkan Perubahan Hambatan." },
    { "en": "Apa Itu 'Hall Effect Sensor'?", "id": "Sensor Pendeteksi Keberadaan Medan Magnet." },
    { "en": "Apa Itu 'Proximity Sensor'?", "id": "Sensor Pendeteksi Keberadaan Objek Terdekat." },
    { "en": "Apa Itu 'Encoder'?", "id": "Sensor Pengubah Gerak Menjadi Digital." },
    { "en": "Apa Itu 'Servo Motor'?", "id": "Motor Dengan Kendali Posisi Presisi." },
    { "en": "Apa Itu 'Stepper Motor'?", "id": "Motor Bergerak Dalam Langkah Bertahap." },
    { "en": "Apa Itu 'BLDC' (Brushless Direct Current) Motor?", "id": "Motor Searah Tanpa Sikat Karbon." },
    { "en": "Apa Itu 'ESC' (Electronic Speed Controller)?", "id": "Pengatur Kecepatan Motor Listrik Elektronik." },
    { "en": "Apa Itu 'Solenoid'?", "id": "Kumparan Pembangkit Gaya Gerak Linear." },
    { "en": "Apa Itu 'Piezoelectric'?", "id": "Bahan Penghasil Listrik Dari Tekanan." },
    { "en": "Apa Itu 'Buzzer'?", "id": "Komponen Penghasil Bunyi Peringatan Elektrik." },
    { "en": "Apa Itu 'Speaker'?", "id": "Pengubah Sinyal Listrik Menjadi Suara." },
    { "en": "Apa Itu 'Microphone'?", "id": "Pengubah Getaran Suara Menjadi Listrik." },
    { "en": "Apa Itu 'Antenna'?", "id": "Perangkat Pengirim Dan Penerima Gelombang." },
    { "en": "Apa Itu 'RF' (Radio Frequency)?", "id": "Frekuensi Gelombang Radio Komunikasi Nirkabel." },
    { "en": "Apa Itu 'Bluetooth'?", "id": "Protokol Komunikasi Data Nirkabel Jarak-Pendek." },
    { "en": "Apa Itu 'Wi-Fi' (Wireless Fidelity)?", "id": "Teknologi Jaringan Lokal Tanpa Kabel." },
    { "en": "Apa Itu 'Zigbee'?", "id": "Protokol Komunikasi Nirkabel Hemat Energi." },
    { "en": "Apa Itu 'LoRa' (Long Range)?", "id": "Teknologi Komunikasi Radio Jarak Jauh." },
    { "en": "Apa Itu 'GSM' (Global System For Mobile Communications)?", "id": "Standar Global Untuk Komunikasi Seluler." },
    { "en": "Apa Itu 'GPS' (Global Positioning System)?", "id": "Sistem Navigasi Berbasis Satelit Global." },
    { "en": "Apa Itu 'UART' (Universal Asynchronous Receiver Transmitter)?", "id": "Protokol Komunikasi Serial Data Asinkron." },
    { "en": "Apa Itu 'SPI' (Serial Peripheral Interface)?", "id": "Protokol Komunikasi Serial Sinkron Cepat." },
    { "en": "Apa Itu 'I2C' (Inter-Integrated Circuit)?", "id": "Protokol Komunikasi Serial Antar Chip." },
    { "en": "Apa Itu 'USB' (Universal Serial Bus)?", "id": "Standar Antarmuka Koneksi Perangkat Universal." },
    { "en": "Apa Itu 'HDMI' (High-Definition Multimedia Interface)?", "id": "Antarmuka Transmisi Video Audio Digital." },
    { "en": "Apa Itu 'Ethernet'?", "id": "Standar Jaringan Lokal Kabel Komputer." },
    { "en": "Apa Itu 'Fiber Optic'?", "id": "Kabel Transmisi Data Menggunakan Cahaya." },
    { "en": "Apa Itu 'Coaxial Cable'?", "id": "Kabel Penghantar Sinyal Frekuensi Tinggi." },
    { "en": "Apa Itu 'Twisted Pair'?", "id": "Kabel Pasangan Berpilin Pengurang Interferensi." },
    { "en": "Apa Itu 'Crosstalk'?", "id": "Gangguan Sinyal Antar Jalur Berdekatan." },
    { "en": "Apa Itu 'EMI' (Electromagnetic Interference)?", "id": "Gangguan Akibat Medan Elektromagnetik Luar." },
    { "en": "Apa Itu 'ESD' (Electrostatic Discharge)?", "id": "Pelepasan Muatan Listrik Statis Tiba-Tiba." },
    { "en": "Apa Itu 'Surge Protector'?", "id": "Alat Pelindung Dari Lonjakan Tegangan." },
    { "en": "Apa Itu 'Circuit Breaker'?", "id": "Saklar Otomatis Pemutus Arus Berlebih." },
    { "en": "Apa Itu 'MCB' (Miniature Circuit Breaker)?", "id": "Pemutus Aliran Listrik Skala Kecil." },
    { "en": "Apa Itu 'MCCB' (Molded Case Circuit Breaker)?", "id": "Pemutus Arus Untuk Kapasitas Menengah." },
    { "en": "Apa Itu 'ELCB' (Earth Leakage Circuit Breaker)?", "id": "Pengaman Listrik Dari Kebocoran Arus." },
    { "en": "Apa Itu 'RCD' (Residual Current Device)?", "id": "Pendeteksi Ketidakseimbangan Arus Jalur Listrik." },
    { "en": "Apa Itu 'Isolation Transformer'?", "id": "Transformator Pemisah Rangkaian Dari Sumber." },
    { "en": "Apa Itu 'Autotransformer'?", "id": "Transformator Dengan Satu Lilitan Tunggal." },
    { "en": "Apa Itu 'Toroidal Transformer'?", "id": "Transformator Berbentuk Donat Efisiensi Tinggi." },
    { "en": "Apa Itu 'Core' (Inti Magnetik)?", "id": "Bahan Jalur Fluks Magnetik Transformator." },
    { "en": "Apa Itu 'Winding' (Lilitan)?", "id": "Gulungan Kawat Penghantar Pada Induktor." },
    { "en": "Apa Itu 'Turn Ratio'?", "id": "Perbandingan Jumlah Lilitan Primer Sekunder." },
    { "en": "Apa Itu 'Efficiency' (Efisiensi)?", "id": "Rasio Daya Keluar Terhadap Masuk." },
    { "en": "Apa Itu 'Power Factor' (Faktor Daya)?", "id": "Rasio Daya Nyata Terhadap Semu." },
    { "en": "Apa Itu 'Apparent Power' (Daya Semu)?", "id": "Hasil Kali Tegangan Dan Arus." },
    { "en": "Apa Itu 'Reactive Power' (Daya Reaktif)?", "id": "Daya Akibat Beban Induktif Kapasitif." },
    { "en": "Apa Itu 'Active Power' (Daya Aktif)?", "id": "Daya Nyata Yang Menjadi Kerja." },
    { "en": "Apa Itu 'Harmonic' (Harmonisa)?", "id": "Gangguan Gelombang Kelipatan Frekuensi Dasar." },
    { "en": "Apa Itu 'THD' (Total Harmonic Distortion)?", "id": "Ukuran Persentase Distorsi Harmonisa Sinyal." },
    { "en": "Apa Itu 'Sine Wave Inverter'?", "id": "Inverter Penghasil Gelombang Sinus Murni." },
    { "en": "Apa Itu 'Modified Sine Wave'?", "id": "Inverter Penghasil Gelombang Sinus Kotak." },
    { "en": "Apa Itu 'Buck Converter'?", "id": "Rangkaian Penurun Tegangan Searah Efisien." },
    { "en": "Apa Itu 'Boost Converter'?", "id": "Rangkaian Peningkat Tegangan Searah Efisien." },
    { "en": "Apa Itu 'Buck-Boost Converter'?", "id": "Rangkaian Penaik Penurun Tegangan Searah." },
    { "en": "Apa Itu 'Flyback Converter'?", "id": "Konverter Daya Dengan Isolasi Transformator." },
    { "en": "Apa Itu 'Linear Regulator'?", "id": "Penstabil Tegangan Dengan Metoda Disipasi." },
    { "en": "Apa Itu 'Voltage Divider'?", "id": "Rangkaian Pembagi Tegangan Menggunakan Hambatan." },
    { "en": "Apa Itu 'Current Divider'?", "id": "Rangkaian Pembagi Arus Jalur Paralel." },
    { "en": "Apa Itu 'Wheatstone Bridge'?", "id": "Jembatan Pengukur Hambatan Sangat Presisi." },
    { "en": "Apa Itu 'Superposition Theorem'?", "id": "Analisis Rangkaian Dengan Banyak Sumber." },
    { "en": "Apa Itu 'Thevenin Theorem'?", "id": "Penyederhanaan Rangkaian Menjadi Sumber Tunggal." },
    { "en": "Apa Itu 'Norton Theorem'?", "id": "Penyederhanaan Rangkaian Menjadi Arus Paralel." },
    { "en": "Apa Itu 'Kirchhoff Current Law'?", "id": "Arus Masuk Sama Dengan Keluar." },
    { "en": "Apa Itu 'Kirchhoff Voltage Law'?", "id": "Total Tegangan Loop Tertutup Nol." },
    { "en": "Apa Itu 'Node' (Simpul)?", "id": "Titik Sambungan Antar Komponen Rangkaian." },
    { "en": "Apa Itu 'Loop' (Lintasan Tertutup)?", "id": "Jalur Arus Listrik Berputar Tertutup." },
    { "en": "Apa Itu 'Mesh' (Mata Jala)?", "id": "Loop Terkecil Dalam Suatu Rangkaian." },
    { "en": "Apa Itu 'BJT' (Bipolar Junction Transistor)?", "id": "Transistor Bipolar Dengan Dua Persambungan." },
    { "en": "Apa Itu 'NPN' (Negative Positive Negative) Transistor?", "id": "Arus Mengalir Jika Basis Positif." },
    { "en": "Apa Itu 'PNP' (Positive Negative Positive) Transistor?", "id": "Arus Mengalir Jika Basis Negatif." },
    { "en": "Apa Itu 'TTL' (Transistor Transistor Logic)?", "id": "Keluarga Logika Digital Berbasis Transistor." },
    { "en": "Apa Itu 'CMOS' (Complementary Metal Oxide Semiconductor)?", "id": "Teknologi IC Konsumsi Daya Rendah." },
    { "en": "Apa Itu 'LDO' (Low Dropout Regulator)?", "id": "Regulator Tegangan Dengan Selisih Kecil." },
    { "en": "Apa Itu 'CMRR' (Common Mode Rejection Ratio)?", "id": "Kemampuan Penolakan Sinyal Mode Bersama." },
    { "en": "Apa Itu 'Slew Rate'?", "id": "Kecepatan Perubahan Maksimum Tegangan Output." },
    { "en": "Apa Itu 'Gain' (Penguatan)?", "id": "Rasio Output Terhadap Input Rangkaian." },
    { "en": "Apa Itu 'Bandwidth' (Lebar Pita)?", "id": "Rentang Frekuensi Kerja Suatu Sistem." },
    { "en": "Apa Itu 'Feedback' (Umpan Balik)?", "id": "Pengembalian Sebagian Sinyal Ke Input." },
    { "en": "Apa Itu 'Oscillator' (Osilator)?", "id": "Pembangkit Sinyal Gelombang Periodik Mandiri." },
    { "en": "Apa Itu 'PLL' (Phase Locked Loop)?", "id": "Sistem Kendali Umpan Balik Fase." },
    { "en": "Apa Itu 'VCO' (Voltage Controlled Oscillator)?", "id": "Osilator Terkendali Melalui Tegangan Masukan." },
    { "en": "Apa Itu 'Crystal Oscillator' (Osilator Kristal)?", "id": "Osilator Dengan Referensi Kristal Kuarsa." },
    { "en": "Apa Itu 'Tank Circuit' (Rangkaian Tanki)?", "id": "Rangkaian Resonansi Paralel Induktor, Kapasitor." },
    { "en": "Apa Itu 'Resonance' (Resonansi)?", "id": "Kondisi Frekuensi Alami Rangkaian Sama." },
    { "en": "Apa Itu 'Q Factor' (Faktor Kualitas)?", "id": "Ukuran Efisiensi Rangkaian Resonansi Listrik." },
    { "en": "Apa Itu 'Skin Effect' (Efek Kulit)?", "id": "Arus Mengalir Di Permukaan Konduktor." },
    { "en": "Apa Itu 'Eddy Current' (Arus Eddy)?", "id": "Arus Induksi Dalam Inti Besi." },
    { "en": "Apa Itu 'Hysteresis' (Histeresis)?", "id": "Keterlambatan Respon Terhadap Gaya Luar." },
    { "en": "Apa Itu 'Saturation' (Saturasi)?", "id": "Kondisi Arus Mencapai Titik Maksimal." },
    { "en": "Apa Itu 'Cut-off' (Titik Mati)?", "id": "Kondisi Arus Berhenti Mengalir Total." },
    { "en": "Apa Itu 'Bias' (Prategangan)?", "id": "Pemberian Tegangan Awal Titik Kerja." },
    { "en": "Apa Itu 'Quiescent Current' (Arus Diam)?", "id": "Arus Saat Rangkaian Tidak Bekerja." },
    { "en": "Apa Itu 'Heat Sink' (Pendingin)?", "id": "Alat Pembuang Panas Komponen Elektronika." },
    { "en": "Apa Itu 'Thermal Paste' (Pasta Termal)?", "id": "Cairan Penghantar Panas Ke Pendingin." },
    { "en": "Apa Itu 'Fan' (Kipas Pendingin)?", "id": "Alat Sirkulasi Udara Pendingin Elektronik." },
    { "en": "Apa Itu 'Thermostat' (Termostat)?", "id": "Saklar Otomatis Berdasarkan Perubahan Suhu." },
    { "en": "Apa Itu 'PTC' (Positive Temperature Coefficient)?", "id": "Hambatan Naik Jika Suhu Naik." },
    { "en": "Apa Itu 'NTC' (Negative Temperature Coefficient)?", "id": "Hambatan Turun Jika Suhu Naik." },
    { "en": "Apa Itu 'Optocoupler' (Isolator Optik)?", "id": "Penghubung Sinyal Menggunakan Media Cahaya." },
    { "en": "Apa Itu 'Phototransistor' (Fototransistor)?", "id": "Transistor Yang Aktif Karena Cahaya." },
    { "en": "Apa Itu 'Solar Cell' (Sel Surya)?", "id": "Komponen Pengubah Cahaya Menjadi Listrik." },
    { "en": "Apa Itu 'Grid Tie Inverter' (Inverter Terhubung Jala)?", "id": "Inverter Sinkron Dengan Jala Listrik." },
    { "en": "Apa Itu 'MPPT' (Maximum Power Point Tracking)?", "id": "Pengoptimal Daya Keluaran Panel Surya." },
    { "en": "Apa Itu 'Deep Cycle Battery' (Baterai Siklus Dalam)?", "id": "Baterai Untuk Pengosongan Daya Besar." },
    { "en": "Apa Itu 'Lead Acid Battery' (Baterai Asam Timbal)?", "id": "Aki Menggunakan Timbal Dan Asam." },
    { "en": "Apa Itu 'Li-Po' (Lithium Polymer) Battery?", "id": "Baterai Ringan, Kerapatan Energi Tinggi." },
    { "en": "Apa Itu 'Ni-Cd' (Nickel Cadmium) Battery?", "id": "Baterai Isi Ulang Berbasis Nikel." },
    { "en": "Apa Itu 'Cell' (Sel Listrik)?", "id": "Unit Dasar Penghasil Energi Listrik." },
    { "en": "Apa Itu 'Parallel Connection' (Sambungan Paralel)?", "id": "Penyambungan Baterai Penambah Kapasitas Arus." },
    { "en": "Apa Itu 'Series Connection' (Sambungan Seri)?", "id": "Penyambungan Baterai Penambah Tegangan Output." },
    { "en": "Apa Itu 'Voltage Drop' (Penurunan Tegangan)?", "id": "Berkurangnya Tegangan Akibat Hambatan Kabel." },
    { "en": "Apa Itu 'Electric Shock' (Sengatan Listrik)?", "id": "Aliran Listrik Melalui Tubuh Manusia." },
    { "en": "Apa Itu 'Insulation' (Isolasi)?", "id": "Bahan Penghambat Aliran Arus Listrik." },
    { "en": "Apa Itu 'Galvanometer'?", "id": "Alat Ukur Arus Listrik Kecil." },
    { "en": "Apa Itu 'Superconductor' (Superkonduktor)?", "id": "Bahan Tanpa Hambatan Suhu Rendah." },
    { "en": "Apa Itu 'Dielectric' (Dielektrik)?", "id": "Bahan Isolasi Di Antara Kapasitor." },
    { "en": "Apa Itu 'Permittivity' (Permitivitas)?", "id": "Ukuran Penyimpanan Medan Listrik Bahan." },
    { "en": "Apa Itu 'Permeability' (Permeabilitas)?", "id": "Ukuran Kemudahan Pembentukan Medan Magnet." },
    { "en": "Apa Itu 'Magnetic Flux Density' (Kerapatan Fluks Magnetik)?", "id": "Jumlah Fluks Per Satuan Luas." },
    { "en": "Apa Itu 'Solenoid Valve' (Katup Solenoid)?", "id": "Katup Kontrol Aliran Berbasis Magnet." },
    { "en": "Apa Itu 'Contactor' (Kontaktor)?", "id": "Saklar Daya Tinggi Sistem Elektromagnetik." },
    { "en": "Apa Itu 'Limit Switch' (Saklar Pembatas)?", "id": "Saklar Pendeteksi Batas Gerak Mekanis." },
    { "en": "Apa Itu 'Push Button' (Tombol Tekan)?", "id": "Saklar Sesaat Dengan Cara Ditekan." },
    { "en": "Apa Itu 'Emergency Stop' (Tombol Berhenti Darurat)?", "id": "Tombol Pemutus Daya Keadaan Darurat." },
    { "en": "Apa Itu 'PLC' (Programmable Logic Controller)?", "id": "Pengendali Logika Digital Industri Terprogram." },
    { "en": "Apa Itu 'HMI' (Human Machine Interface)?", "id": "Antarmuka Grafis Manusia Dan Mesin." },
    { "en": "Apa Itu 'SCADA' (Supervisory Control And Data Acquisition)?", "id": "Sistem Pengawasan Dan Akuisisi Data." },
    { "en": "Apa Itu 'VFD' (Variable Frequency Drive)?", "id": "Pengatur Kecepatan Motor Melalui Frekuensi." },
    { "en": "Apa Itu 'Soft Starter' (Pemula Lembut)?", "id": "Alat Pengurang Arus Start Motor." },
    { "en": "Apa Itu 'Overload Relay' (Relay Beban Lebih)?", "id": "Pengaman Motor Dari Panas Berlebih." },
    { "en": "Apa Itu 'Phase Failure' (Kegagalan Fase)?", "id": "Kondisi Hilangnya Salah Satu Fase." },
    { "en": "Apa Itu 'Under Voltage' (Tegangan Kurang)?", "id": "Tegangan Jatuh Di Bawah Standar." },
    { "en": "Apa Itu 'Over Voltage' (Tegangan Lebih)?", "id": "Tegangan Naik Di Atas Standar." },
    { "en": "Apa Itu 'Busbar' (Batang Penghantar)?", "id": "Batang Logam Penghantar Arus Besar." },
    { "en": "Apa Itu 'Panel Box' (Kotak Panel)?", "id": "Wadah Pelindung Komponen Listrik Kontrol." },
    { "en": "Apa Itu 'DIN Rail' (Rel Din)?", "id": "Logam Standar Dudukan Komponen Panel." },
    { "en": "Apa Itu 'Cable Gland' (Kelen Kabel)?", "id": "Pengikat Dan Pelindung Masukan Kabel." },
    { "en": "Apa Itu 'Cable Lug' (Sepatu Kabel)?", "id": "Terminal Penghubung Ujung Kabel Tembaga." },
    { "en": "Apa Itu 'Ferrite Bead' (Manik-Manik Ferit)?", "id": "Komponen Penekan Gangguan Frekuensi Tinggi." },
    { "en": "Apa Itu 'EMI Shielding' (Pelindung Gangguan Elektromagnetik)?", "id": "Pelapis Penahan Radiasi Gelombang Magnetik." },
    { "en": "Apa Itu 'Antenna Gain' (Penguatan Antena)?", "id": "Ukuran Efektivitas Pengarahan Sinyal Antena." },
    { "en": "Apa Itu 'VSWR' (Voltage Standing Wave Ratio)?", "id": "Rasio Gelombang Pantul Jalur Transmisi." },
    { "en": "Apa Itu 'Decibel' (Desibel)?", "id": "Satuan Perbandingan Logaritmik Kekuatan Sinyal." },
    { "en": "Apa Itu 'Modulation' (Modulasi)?", "id": "Proses Penumpangan Informasi Ke Gelombang." },
    { "en": "Apa Itu 'AM' (Amplitude Modulation)?", "id": "Modulasi Berdasarkan Perubahan Amplitudo Sinyal." },
    { "en": "Apa Itu 'FM' (Frequency Modulation)?", "id": "Modulasi Berdasarkan Perubahan Frekuensi Sinyal." },
    { "en": "Apa Itu 'PM' (Phase Modulation)?", "id": "Modulasi Berdasarkan Perubahan Fase Sinyal." },
    { "en": "Apa Itu 'ASK' (Amplitude Shift Keying)?", "id": "Modulasi Digital Berdasarkan Perubahan Amplitudo." },
    { "en": "Apa Itu 'FSK' (Frequency Shift Keying)?", "id": "Modulasi Digital Berdasarkan Perubahan Frekuensi." },
    { "en": "Apa Itu 'PSK' (Phase Shift Keying)?", "id": "Modulasi Digital Berdasarkan Perubahan Fase." },
    { "en": "Apa Itu 'Multiplexer' (Pelepas)?", "id": "Penggabung Banyak Jalur Menjadi Satu." },
    { "en": "Apa Itu 'Demultiplexer' (Pemisah)?", "id": "Pemisah Satu Jalur Menjadi Banyak." },
    { "en": "Apa Itu 'Priority Encoder' (Penyandi Prioritas)?", "id": "Penyandi Berdasarkan Prioritas Input Tertinggi." },
    { "en": "Apa Itu 'Decoder' (Pendekode)?", "id": "Pengubah Kode Digital Menjadi Output." },
    { "en": "Apa Itu 'Latch' (Grendel)?", "id": "Sirkuit Penyimpan Status Logika Sesaat." },
    { "en": "Apa Itu 'Shift Register' (Register Geser)?", "id": "Rangkaian Pemindah Data Bit Berurutan." },
    { "en": "Apa Itu 'Counter' (Pencacah)?", "id": "Rangkaian Penghitung Jumlah Pulsa Masuk." },
    { "en": "Apa Itu 'Clock' (Pulsa Detak)?", "id": "Sinyal Referensi Waktu Sistem Digital." },
    { "en": "Apa Itu 'Crystal' (Kristal)?", "id": "Komponen Penghasil Getaran Frekuensi Stabil." },
    { "en": "Apa Itu 'Ceramic Resonator' (Resonator Keramik)?", "id": "Pembangkit Getaran Berbahan Keramik Piezoelektrik." },
    { "en": "Apa Itu 'Watchdog Timer' (Pewaktu Penjaga)?", "id": "Sistem Pendeteksi Kegagalan Operasi Mikroprosesor." },
    { "en": "Apa Itu 'Interrupt' (Interupsi)?", "id": "Penghentian Sementara Alur Program Utama." },
    { "en": "Apa Itu 'Memory Stack' (Tumpukan Memori)?", "id": "Tempat Penyimpanan Data Program Sementara." },
    { "en": "Apa Itu 'Data Pointer' (Penunjuk Data)?", "id": "Register Penunjuk Alamat Memori Data." },
    { "en": "Apa Itu 'CPU Register' (Register Unit Pemroses Pusat)?", "id": "Tempat Penyimpanan Internal Prosesor Cepat." },
    { "en": "Apa Itu 'ALU' (Arithmetic Logic Unit)?", "id": "Unit Pelaksana Operasi Aritmatika, Logika." },
    { "en": "Apa Itu 'Control Unit' (Unit Kendali)?", "id": "Unit Pengatur Operasi Kerja Prosesor." },
    { "en": "Apa Itu 'Power Grid' (Jaringan Daya)?", "id": "Jaringan Besar Penghasil Distribusi Listrik." },
    { "en": "Apa Itu 'Synchronous Motor' (Motor Sinkron)?", "id": "Motor Dengan Kecepatan Putaran Tetap." },
    { "en": "Apa Itu 'Induction Motor' (Motor Induksi)?", "id": "Motor Berdasarkan Induksi Medan Magnet." },
    { "en": "Apa Itu 'Slip'?", "id": "Selisih Kecepatan Medan Dan Rotor." },
    { "en": "Apa Itu 'Squirrel Cage' (Sangkar Tupai)?", "id": "Tipe Rotor Motor Induksi Umum." },
    { "en": "Apa Itu 'Carbon Brush' (Sikat Karbon)?", "id": "Penghantar Arus Ke Bagian Berputar." },
    { "en": "Apa Itu 'Armature' (Angker)?", "id": "Bagian Motor Penghasil Gaya Gerak." },
    { "en": "Apa Itu 'Excitation' (Eksitasi)?", "id": "Pemberian Arus Magnet Pada Mesin." },
    { "en": "Apa Itu 'Field Winding' (Lilitan Medan)?", "id": "Kumparan Pembangkit Medan Magnet Utama." },
    { "en": "Apa Itu 'Capacitor Bank' (Bank Kapasitor)?", "id": "Kumpulan Kapasitor Perbaiki Faktor Daya." },
    { "en": "Apa Itu 'SVC' (Static Var Compensator)?", "id": "Alat Kompensasi Daya Reaktif Cepat." },
    { "en": "Apa Itu 'STATCOM' (Static Synchronous Compensator)?", "id": "Kompensator Daya Reaktif Berbasis Inverter." },
    { "en": "Apa Itu 'Voltage Sag' (Tegangan Turun Sesaat)?", "id": "Penurunan Tegangan Listrik Dalam Waktu Singkat." },
    { "en": "Apa Itu 'Voltage Swell' (Tegangan Naik Sesaat)?", "id": "Kenaikan Tegangan Listrik Dalam Waktu Singkat." },
    { "en": "Apa Itu 'Voltage Flicker' (Kedipan Tegangan)?", "id": "Variasi Tegangan Berulang Mengganggu Cahaya." },
    { "en": "Apa Itu 'Transient' (Transien)?", "id": "Lonjakan Energi Singkat Tak Terduga." },
    { "en": "Apa Itu 'Ground Loop' (Loop Tanah)?", "id": "Aliran Arus Akibat Perbedaan Potensial Tanah." },
    { "en": "Apa Itu 'Floating Ground' (Tanah Mengambang)?", "id": "Titik Referensi Tanpa Koneksi Bumi." },
    { "en": "Apa Itu 'Skin Depth' (Kedalaman Kulit)?", "id": "Ukuran Penetrasi Arus Dalam Konduktor." },
    { "en": "Apa Itu 'Proximity Effect' (Efek Kedekatan)?", "id": "Distribusi Arus Terganggu Konduktor Lain." },
    { "en": "Apa Itu 'Corona Effect' (Efek Korona)?", "id": "Ionisasi Udara Di Sekitar Konduktor Tinggi." },
    { "en": "Apa Itu 'Ferranti Effect' (Efek Ferranti)?", "id": "Tegangan Ujung Terima Lebih Tinggi." },
    { "en": "Apa Itu 'Surge Impedance' (Impedansi Surja)?", "id": "Impedansi Karakteristik Jalur Transmisi Listrik." },
    { "en": "Apa Itu 'Reflection Coefficient' (Koefisien Refleksi)?", "id": "Rasio Gelombang Pantul Terhadap Datang." },
    { "en": "Apa Itu 'Standing Wave' (Gelombang Berdiri)?", "id": "Superposisi Gelombang Datang Dan Pantul." },
    { "en": "Apa Itu 'Impedance Matching' (Penyesuaian Impedansi)?", "id": "Maksimalisasi Transfer Daya Antar Perangkat." },
    { "en": "Apa Itu 'Balun' (Balance-Unbalance)?", "id": "Transformator Penghubung Jalur Seimbang, Tidak." },
    { "en": "Apa Itu 'Waveguide' (Pandu Gelombang)?", "id": "Struktur Pemandu Gelombang Elektromagnetik Tinggi." },
    { "en": "Apa Itu 'Stripline'?", "id": "Jalur Transmisi Data Dalam PCB." },
    { "en": "Apa Itu 'Microstrip'?", "id": "Jalur Transmisi Di Permukaan PCB." },
    { "en": "Apa Itu 'Vias' (Lubang Koneksi)?", "id": "Lubang Penghubung Antar Lapisan PCB." },
    { "en": "Apa Itu 'Solder Mask' (Masker Solder)?", "id": "Lapisan Pelindung Jalur Tembaga PCB." },
    { "en": "Apa Itu 'Silk Screen' (Layar Sutra)?", "id": "Label Informasi Komponen Di Atas PCB." },
    { "en": "Apa Itu 'Gerber File' (Berkas Gerber)?", "id": "Format Standar Industri Desain PCB." },
    { "en": "Apa Itu 'BOM' (Bill Of Materials)?", "id": "Daftar Lengkap Komponen Proyek Elektronika." },
    { "en": "Apa Itu 'SMD' (Surface Mount Device)?", "id": "Komponen Elektronika Menempel Di Permukaan." },
    { "en": "Apa Itu 'SMT' (Surface Mount Technology)?", "id": "Teknologi Pemasangan Komponen Permukaan PCB." },
    { "en": "Apa Itu 'THT' (Through Hole Technology)?", "id": "Teknologi Komponen Melalui Lubang PCB." },
    { "en": "Apa Itu 'Reflow Soldering' (Penyolderan Aliran)?", "id": "Metode Penyolderan Massal Komponen SMD." },
    { "en": "Apa Itu 'Wave Soldering' (Penyolderan Gelombang)?", "id": "Metode Penyolderan Massal Komponen THT." },
    { "en": "Apa Itu 'Flux' (Fluks Solder)?", "id": "Cairan Pembersih Permukaan Logam Solder." },
    { "en": "Apa Itu 'Rosin' (Damar Solder)?", "id": "Bahan Dasar Fluks Penyolderan Aman." },
    { "en": "Apa Itu 'Cold Solder Joint' (Sambungan Solder Dingin)?", "id": "Koneksi Solder Retak, Tidak Sempurna." },
    { "en": "Apa Itu 'Solder Bridge' (Jembatan Solder)?", "id": "Hubung Singkat Solder Antar Jalur." },
    { "en": "Apa Itu 'Multicore Cable' (Kabel Inti Banyak)?", "id": "Kabel Dengan Beberapa Inti Penghantar." },
    { "en": "Apa Itu 'Armoured Cable' (Kabel Lapis Baja)?", "id": "Kabel Dengan Perlindungan Mekanis Baja." },
    { "en": "Apa Itu 'Conduit' (Pipa Kabel)?", "id": "Pipa Pelindung Jalur Kabel Listrik." },
    { "en": "Apa Itu 'Trunking' (Kanal Kabel)?", "id": "Kotak Pelindung Jalur Kabel Luar." },
    { "en": "Apa Itu 'Cable Tray' (Baki Kabel)?", "id": "Struktur Penyangga Kumpulan Kabel Besar." },
    { "en": "Apa Itu 'Terminal Block' (Blok Terminal)?", "id": "Titik Sambungan Kabel Dalam Panel." },
    { "en": "Apa Itu 'Ferrule' (Sepatu Kabel Kecil)?", "id": "Pelindung Ujung Serabut Kabel Listrik." },
    { "en": "Apa Itu 'Heat Shrink' (Selongsong Bakar)?", "id": "Isolator Kabel Yang Menyusut Dipanaskan." },
    { "en": "Apa Itu 'Cable Tie' (Pengikat Kabel)?", "id": "Tali Plastik Pengikat Kerapian Kabel." },
    { "en": "Apa Itu 'Bus Duct' (Saluran Bus)?", "id": "Sistem Distribusi Arus Besar Tertutup." },
    { "en": "Apa Itu 'Neutral Link' (Tautan Netral)?", "id": "Titik Kumpul Kabel Netral Listrik." },
    { "en": "Apa Itu 'Earth Bar' (Batang Arde)?", "id": "Terminal Utama Penghubung Kabel Pembumian." },
    { "en": "Apa Itu 'Lightning Rod' (Batang Petir)?", "id": "Ujung Logam Penangkap Sambaran Petir." },
    { "en": "Apa Itu 'Down Conductor' (Penghantar Turun)?", "id": "Kabel Penyalur Petir Ke Tanah." },
    { "en": "Apa Itu 'Earth Pit' (Sumur Pembumian)?", "id": "Lubang Titik Penanaman Elektroda Tanah." },
    { "en": "Apa Itu 'Soil Resistivity' (Resistivitas Tanah)?", "id": "Ukuran Hambatan Jenis Tanah Listrik." },
    { "en": "Apa Itu 'Step Voltage' (Tegangan Langkah)?", "id": "Beda Potensial Antara Dua Kaki." },
    { "en": "Apa Itu 'Touch Voltage' (Tegangan Sentuh)?", "id": "Beda Potensial Antara Tangan, Kaki." },
    { "en": "Apa Itu 'Mesh Grounding' (Pembumian Jala)?", "id": "Sistem Arde Berbentuk Jaring Logam." },
    { "en": "Apa Itu 'Galvanic Isolation' (Isolasi Galvanik)?", "id": "Pemisahan Sirkuit Tanpa Aliran Arus." },
    { "en": "Apa Itu 'Optoisolator' (Isolator Cahaya)?", "id": "Pengirim Sinyal Melalui Media Optik." },
    { "en": "Apa Itu 'SSR' (Solid State Relay)?", "id": "Saklar Elektronik Tanpa Kontak Mekanik." },
    { "en": "Apa Itu 'Zero Crossing' (Titik Nol)?", "id": "Deteksi Saat Gelombang Mencapai Nol." },
    { "en": "Apa Itu 'Snubber Circuit' (Rangkaian Peredam)?", "id": "Peredam Lonjakan Tegangan Pada Saklar." },
    { "en": "Apa Itu 'Flyback Diode' (Dioda Flyback)?", "id": "Dioda Pelindung Dari Arus Balik." },
    { "en": "Apa Itu 'Freewheeling Diode' (Dioda Arus Bebas)?", "id": "Dioda Pelancar Aliran Arus Induktif." },
    { "en": "Apa Itu 'Clamp Circuit' (Rangkaian Klem)?", "id": "Penggeser Level Tegangan Sinyal AC." },
    { "en": "Apa Itu 'Clipper Circuit' (Rangkaian Pemotong)?", "id": "Pemotong Amplitudo Sinyal Di Atas." },
    { "en": "Apa Itu 'Multivibrator' (Multivibrator)?", "id": "Rangkaian Penghasil Sinyal Kotak Elektronik." },
    { "en": "Apa Itu 'Monostable' (Monostabil)?", "id": "Rangkaian Dengan Satu Kondisi Stabil." },
    { "en": "Apa Itu 'Bistable' (Bistabil)?", "id": "Rangkaian Dengan Dua Kondisi Stabil." },
    { "en": "Apa Itu 'Astable' (Astabil)?", "id": "Rangkaian Tanpa Kondisi Stabil Tetap." },
    { "en": "Apa Itu 'Schmitt Trigger' (Pemicu Schmitt)?", "id": "Komparator Dengan Efek Histeresis Logika." },
    { "en": "Apa Itu 'Comparator' (Komparator)?", "id": "Sirkuit Pembanding Dua Nilai Tegangan." },
    { "en": "Apa Itu 'Voltage Follower' (Pengikut Tegangan)?", "id": "Penguat Dengan Penguatan Bernilai Satu." },
    { "en": "Apa Itu 'Instrumentation Amplifier' (Penguat Instrumentasi)?", "id": "Penguat Sinyal Sensor Sangat Presisi." },
    { "en": "Apa Itu 'Differential Amplifier' (Penguat Diferensial)?", "id": "Penguat Selisih Antara Dua Masukan." },
    { "en": "Apa Itu 'Inverting Amplifier' (Penguat Membalik)?", "id": "Penguat Yang Membalik Fase Sinyal." },
    { "en": "Apa Itu 'Non-Inverting Amplifier' (Penguat Tak Membalik)?", "id": "Penguat Tanpa Membalik Fase Sinyal." },
    { "en": "Apa Itu 'Integrator Circuit' (Rangkaian Integrator)?", "id": "Rangkaian Penghasil Output Integral Input." },
    { "en": "Apa Itu 'Differentiator Circuit' (Rangkaian Diferensiator)?", "id": "Rangkaian Penghasil Output Turunan Input." },
    { "en": "Apa Itu 'Active Filter' (Filter Aktif)?", "id": "Penyaring Sinyal Dengan Komponen Aktif." },
    { "en": "Apa Itu 'Passive Filter' (Filter Pasif)?", "id": "Penyaring Sinyal Tanpa Komponen Aktif." },
    { "en": "Apa Itu 'Butterworth Filter'?", "id": "Filter Dengan Respon Frekuensi Datar." },
    { "en": "Apa Itu 'Chebyshev Filter'?", "id": "Filter Dengan Karakteristik Penurunan Tajam." },
    { "en": "Apa Itu 'Notch Filter' (Filter Takik)?", "id": "Penghilang Satu Frekuensi Spesifik Saja." },
    { "en": "Apa Itu 'Band Pass Filter' (Filter Lolos Pita)?", "id": "Penyaring Sinyal Dalam Rentang Tertentu." },
    { "en": "Apa Itu 'Band Stop Filter' (Filter Tolak Pita)?", "id": "Penghambat Sinyal Dalam Rentang Tertentu." },
    { "en": "Apa Itu 'Sampling Rate' (Laju Sampel)?", "id": "Frekuensi Pengambilan Data Sinyal Analog." },
    { "en": "Apa Itu 'Nyquist Theorem' (Teorema Nyquist)?", "id": "Syarat Minimal Kecepatan Sampling Sinyal." },
    { "en": "Apa Itu 'Aliasing'?", "id": "Distorsi Sinyal Akibat Kurang Sampel." },
    { "en": "Apa Itu 'Quantization Noise' (Derau Kuantisasi)?", "id": "Kesalahan Pembulatan Saat Konversi Digital." },
    { "en": "Apa Itu 'Power Analyzer' (Penganalisis Daya)?", "id": "Alat Ukur Kualitas Daya Listrik." },
    { "en": "Apa Itu 'TDR' (Time Domain Reflectometer)?", "id": "Alat Deteksi Kerusakan Kabel Listrik." },
    { "en": "Apa Itu 'Insulation Tester' (Penguji Isolasi)?", "id": "Alat Ukur Ketahanan Isolasi Kabel." },
    { "en": "Apa Itu 'Earth Tester' (Penguji Arde)?", "id": "Alat Ukur Resistansi Tanah Pembumian." },
    { "en": "Apa Itu 'NAND Gate' (Not AND Gate)?", "id": "Output Rendah Jika Semua Tinggi." },
    { "en": "Apa Itu 'NOR Gate' (Not OR Gate)?", "id": "Output Rendah Jika Salah Satu Tinggi." },
    { "en": "Apa Itu 'XOR Gate' (Exclusive OR Gate)?", "id": "Output Tinggi Jika Masukan Berbeda." },
    { "en": "Apa Itu 'XNOR Gate' (Exclusive Not OR Gate)?", "id": "Output Tinggi Jika Masukan Sama." },
    { "en": "Apa Itu 'Half Adder' (Penambah Setengah)?", "id": "Penjumlah Dua Bit Tanpa Carry." },
    { "en": "Apa Itu 'Full Adder' (Penambah Penuh)?", "id": "Penjumlah Tiga Bit Dengan Carry." },
    { "en": "Apa Itu 'Half Subtractor' (Pengurang Setengah)?", "id": "Pengurang Dua Bit Tanpa Pinjaman." },
    { "en": "Apa Itu 'Full Subtractor' (Pengurang Penuh)?", "id": "Pengurang Tiga Bit Dengan Pinjaman." },
    { "en": "Apa Itu 'Binary Code' (Kode Biner)?", "id": "Sistem Bilangan Berbasis Dua Digital." },
    { "en": "Apa Itu 'Hexadecimal' (Heksadesimal)?", "id": "Sistem Bilangan Berbasis Enam Belas." },
    { "en": "Apa Itu 'Octal' (Oktal)?", "id": "Sistem Bilangan Berbasis Delapan Digital." },
    { "en": "Apa Itu 'BCD' (Binary Coded Decimal)?", "id": "Sandi Desimal Dalam Bentuk Biner." },
    { "en": "Apa Itu 'Gray Code' (Sandi Gray)?", "id": "Kode Biner Dengan Satu Perubahan." },
    { "en": "Apa Itu 'Parity Bit' (Bit Paritas)?", "id": "Bit Tambahan Untuk Deteksi Kesalahan." },
    { "en": "Apa Itu 'ASCII' (American Standard Code For Information Interchange)?", "id": "Standar Kode Karakter Perangkat Elektronik." },
    { "en": "Apa Itu 'Li-ion' (Lithium Ion) Battery?", "id": "Baterai Isi Ulang Gadget Modern." },
    { "en": "Apa Itu 'Ni-MH' (Nickel Metal Hydride) Battery?", "id": "Baterai Ramah Lingkungan Kapasitas Tinggi." },
    { "en": "Apa Itu 'Supercapacitor' (Superkapasitor)?", "id": "Penyimpan Energi Kapasitansi Sangat Besar." },
    { "en": "Apa Itu 'Fuel Cell' (Sel Bahan Bakar)?", "id": "Pembangkit Listrik Dari Reaksi Kimia." },
    { "en": "Apa Itu 'Wind Turbine' (Turbin Angin)?", "id": "Pengubah Energi Angin Menjadi Listrik." },
    { "en": "Apa Itu 'Hydroelectric' (Hidroelektrik)?", "id": "Pembangkit Listrik Berbasis Aliran Air." },
    { "en": "Apa Itu 'Geothermal' (Panas Bumi)?", "id": "Pembangkit Listrik Dari Panas Bumi." },
    { "en": "Apa Itu 'Biomass' (Biomassa)?", "id": "Energi Listrik Dari Bahan Organik." },
    { "en": "Apa Itu 'Micro-Hydro' (Mikro-Hidro)?", "id": "Pembangkit Listrik Air Skala Kecil." },
    { "en": "Apa Itu 'Smart Grid' (Jaringan Cerdas)?", "id": "Jaringan Listrik Dengan Teknologi Digital." },
    { "en": "Apa Itu 'Net Metering' (Meteran Netto)?", "id": "Sistem Ekspor Impor Energi Surya." },
    { "en": "Apa Itu 'Islanded Mode' (Mode Terisolasi)?", "id": "Sistem Mandiri Tanpa Jala Utama." },
    { "en": "Apa Itu 'Blackout' (Padam Total)?", "id": "Kondisi Terputusnya Aliran Listrik Total." },
    { "en": "Apa Itu 'Brownout' (Penurunan Tegangan)?", "id": "Penurunan Tegangan Listrik Di Wilayah." },
    { "en": "Apa Itu 'Load Shedding' (Pelepasan Beban)?", "id": "Pemutusan Listrik Bergilir Demi Keseimbangan." },
    { "en": "Apa Itu 'Base Load' (Beban Dasar)?", "id": "Permintaan Listrik Minimum Terus Menerus." },
    { "en": "Apa Itu 'Peak Load' (Beban Puncak)?", "id": "Konsumsi Listrik Tertinggi Suatu Waktu." },
    { "en": "Apa Itu 'Lumen'?", "id": "Satuan Ukuran Total Cahaya Keluar." },
    { "en": "Apa Itu 'Lux'?", "id": "Satuan Intensitas Cahaya Per Luas." },
    { "en": "Apa Itu 'Candela'?", "id": "Satuan Intensitas Cahaya Sumber Titik." },
    { "en": "Apa Itu 'Color Temperature' (Suhu Warna)?", "id": "Ukuran Warna Cahaya Lampu Listrik." },
    { "en": "Apa Itu 'CRI' (Color Rendering Index)?", "id": "Akurasi Cahaya Menampilkan Warna Asli." },
    { "en": "Apa Itu 'Incandescent Lamp' (Lampu Pijar)?", "id": "Lampu Penghasil Cahaya Dari Pemanasan." },
    { "en": "Apa Itu 'Halogen Lamp' (Lampu Halogen)?", "id": "Lampu Pijar Berisi Gas Halogen." },
    { "en": "Apa Itu 'Fluorescent Lamp' (Lampu Neon)?", "id": "Lampu Berbasis Eksitasi Gas Merkuri." },
    { "en": "Apa Itu 'CFL' (Compact Fluorescent Lamp)?", "id": "Lampu Neon Bentuk Kecil Kompak." },
    { "en": "Apa Itu 'HID Lamp' (High Intensity Discharge)?", "id": "Lampu Pelepasan Gas Intensitas Tinggi." },
    { "en": "Apa Itu 'Ballast' (Balas)?", "id": "Alat Pengatur Arus Lampu Tabung." },
    { "en": "Apa Itu 'Starter' (Pemantik)?", "id": "Alat Pemicu Awal Lampu Neon." },
    { "en": "Apa Itu 'Dimmer' (Peredup)?", "id": "Alat Pengatur Intensitas Cahaya Lampu." },
    { "en": "Apa Itu 'Photocell' (Sel Foto)?", "id": "Saklar Cahaya Berdasarkan Kondisi Gelap." },
    { "en": "Apa Itu 'DIP' (Dual In-line Package)?", "id": "Bentuk IC Dengan Dua Baris." },
    { "en": "Apa Itu 'SOIC' (Small Outline Integrated Circuit)?", "id": "Kemasan IC Permukaan Ukuran Kecil." },
    { "en": "Apa Itu 'QFP' (Quad Flat Package)?", "id": "Kemasan IC Persegi Empat Sisi." },
    { "en": "Apa Itu 'BGA' (Ball Grid Array)?", "id": "Kemasan IC Dengan Bola Solder." },
    { "en": "Apa Itu 'Silicon Wafer' (Wafer Silikon)?", "id": "Bahan Dasar Pembuatan Chip Semi-konduktor." },
    { "en": "Apa Itu 'Cleanroom' (Ruang Bersih)?", "id": "Tempat Produksi Chip Bebas Debu." },
    { "en": "Apa Itu 'VLSI' (Very Large Scale Integration)?", "id": "Integrasi Jutaan Transistor Dalam Chip." },
    { "en": "Apa Itu 'FPGA' (Field Programmable Gate Array)?", "id": "Sirkuit Terintegrasi Yang Dapat Diprogram." },
    { "en": "Apa Itu 'ASIC' (Application Specific Integrated Circuit)?", "id": "Chip Untuk Fungsi Khusus Saja." },
    { "en": "Apa Itu 'DSP' (Digital Signal Processor)?", "id": "Prosesor Pengolah Sinyal Digital Cepat." },
    { "en": "Apa Itu 'SoC' (System On Chip)?", "id": "Satu Chip Berisi Seluruh Sistem." },
    { "en": "Apa Itu 'GPIO' (General Purpose Input Output)?", "id": "Pin Masukan Keluaran Umum Mikrokontroler." },
    { "en": "Apa Itu 'PWM Inverter' (Inverter Modulasi Pulsa)?", "id": "Inverter Berbasis Teknik Lebar Pulsa." },
    { "en": "Apa Itu 'Cycloconverter' (Siklokonverter)?", "id": "Pengubah Frekuensi Arus Bolak Balik." },
    { "en": "Apa Itu 'Chopper' (Pencacah Arus)?", "id": "Pengubah Tegangan Searah Menjadi Variabel." },
    { "en": "Apa Itu 'Snubber' (Peredam Tegangan)?", "id": "Sirkuit Pelindung Komponen Dari Transien." },
    { "en": "Apa Itu 'ZVS' (Zero Voltage Switching)?", "id": "Pensakelaran Saat Tegangan Bernilai Nol." },
    { "en": "Apa Itu 'ZCS' (Zero Current Switching)?", "id": "Pensakelaran Saat Arus Bernilai Nol." },
    { "en": "Apa Itu 'Power Diode' (Dioda Daya)?", "id": "Dioda Khusus Untuk Arus Besar." },
    { "en": "Apa Itu 'Thyristor' (Tiristor)?", "id": "Komponen Semi-konduktor Pengendali Daya Tinggi." },
    { "en": "Apa Itu 'GTO' (Gate Turn-Off Thyristor)?", "id": "Tiristor Yang Dapat Dimatikan Gerbang." },
    { "en": "Apa Itu 'Phase Angle' (Sudut Fase)?", "id": "Posisi Waktu Gelombang Terhadap Referensi." },
    { "en": "Apa Itu 'Skin Effect' (Efek Kulit)?", "id": "Kecenderungan Arus Mengalir Di Permukaan." },
    { "en": "Apa Itu 'Proximity Effect' (Efek Kedekatan)?", "id": "Interferensi Arus Antar Konduktor Dekat." },
    { "en": "Apa Itu 'Magnetic Saturation' (Saturasi Magnetik)?", "id": "Kondisi Inti Besi Penuh Magnet." },
    { "en": "Apa Itu 'Leakage Flux' (Fluks Bocor)?", "id": "Magnet Yang Tidak Melalui Jalur." },
    { "en": "Apa Itu 'Reluctance' (Reluktansi)?", "id": "Hambatan Terhadap Jalur Fluks Magnetik." },
    { "en": "Apa Itu 'Magnetomotive Force' (Gaya Gerak Magnet)?", "id": "Penyebab Terjadinya Fluks Dalam Sirkuit." },
    { "en": "Apa Itu 'Coercivity' (Koersivitas)?", "id": "Ketahanan Bahan Terhadap Demagnetisasi Magnet." },
    { "en": "Apa Itu 'Remanence' (Remanensi)?", "id": "Sisa Magnetisme Setelah Medan Dihilangkan." },
    { "en": "Apa Itu 'Core Loss' (Rugi Inti)?", "id": "Energi Hilang Pada Inti Besi." },
    { "en": "Apa Itu 'Copper Loss' (Rugi Tembaga)?", "id": "Energi Hilang Akibat Hambatan Lilitan." },
    { "en": "Apa Itu 'Voltage Regulation' (Regulasi Tegangan)?", "id": "Kemampuan Menjaga Tegangan Tetap Stabil." },
    { "en": "Apa Itu 'No Load' (Tanpa Beban)?", "id": "Kondisi Alat Listrik Tidak Terbebani." },
    { "en": "Apa Itu 'Full Load' (Beban Penuh)?", "id": "Kondisi Alat Bekerja Kapasitas Maksimal." },
    { "en": "Apa Itu 'Overload' (Beban Lebih)?", "id": "Kondisi Beban Melebihi Kapasitas Alat." },
    { "en": "Apa Itu 'Universal Motor' (Motor Universal)?", "id": "Motor Beroperasi Arus Bolak, Searah." },
    { "en": "Apa Itu 'Shaded Pole Motor' (Motor Kutub Bayangan)?", "id": "Motor Induksi Sederhana Daya Rendah." },
    { "en": "Apa Itu 'Reluctance Motor' (Motor Reluktansi)?", "id": "Motor Berputar Berdasarkan Variasi Reluktansi." },
    { "en": "Apa Itu 'Hysteresis Motor' (Motor Histeresis)?", "id": "Motor Dengan Torsi Putaran Sangat Halus." },
    { "en": "Apa Itu 'Linear Motor' (Motor Linear)?", "id": "Motor Penghasil Gerak Garis Lurus." },
    { "en": "Apa Itu 'Damping' (Peredaman)?", "id": "Pengurangan Getaran Pada Sistem Alat." },
    { "en": "Apa Itu 'Parallax Error' (Kesalahan Paralaks)?", "id": "Kesalahan Baca Alat Ukur Analog." },
    { "en": "Apa Itu 'Calibration' (Kalibrasi)?", "id": "Proses Penyesuaian Akurasi Alat Ukur." },
    { "en": "Apa Itu 'Sensitivity' (Sensitivitas)?", "id": "Respon Alat Terhadap Perubahan Kecil." },
    { "en": "Apa Itu 'Resolution' (Resolusi)?", "id": "Perubahan Terkecil Yang Dapat Terdeteksi." },
    { "en": "Apa Itu 'Accuracy' (Akurasi)?", "id": "Kedekatan Hasil Ukur Dengan Sebenarnya." },
    { "en": "Apa Itu 'Precision' (Presisi)?", "id": "Konsistensi Hasil Pengukuran Berulang Kali." },
    { "en": "Apa Itu 'Tolerance' (Toleransi)?", "id": "Batas Variasi Nilai Yang Diizinkan." },
    { "en": "Apa Itu 'IEEE' (Institute Of Electrical And Electronics Engineers)?", "id": "Organisasi Standar Teknologi Elektro Global." },
    { "en": "Apa Itu 'IEC' (International Electrotechnical Commission)?", "id": "Standar Internasional Untuk Teknologi Listrik." },
    { "en": "Apa Itu 'NEMA' (National Electrical Manufacturers Association)?", "id": "Standar Industri Peralatan Listrik Amerika." },
    { "en": "Apa Itu 'PUIL' (Persyaratan Umum Instalasi Listrik)?", "id": "Standar Aturan Instalasi Listrik Indonesia." },
    { "en": "Apa Itu 'RoHS' (Restriction Of Hazardous Substances)?", "id": "Larangan Bahan Berbahaya Produk Elektronika." },
    { "en": "Apa Itu 'Form Factor' (Faktor Bentuk)?", "id": "Rasio Nilai RMS Terhadap Rata-Rata." },
    { "en": "Apa Itu 'Crest Factor' (Faktor Puncak)?", "id": "Rasio Nilai Puncak Terhadap RMS." },
    { "en": "Apa Itu 'Modulation Index' (Indeks Modulasi)?", "id": "Ukuran Kedalaman Modulasi Sinyal Informasi." },
    { "en": "Apa Itu 'Carrier Wave' (Gelombang Pembawa)?", "id": "Gelombang Tinggi Pengangkut Sinyal Informasi." },
    { "en": "Apa Itu 'Sideband' (Pita Samping)?", "id": "Rentang Frekuensi Hasil Proses Modulasi." },
    { "en": "Apa Itu 'PID' (Proportional Integral Derivative Controller)?", "id": "Sistem Kendali Kontinu Umpan Balik." },
    { "en": "Apa Itu 'Open Loop' (Sistem Terbuka)?", "id": "Sistem Kendali Tanpa Umpan Balik." },
    { "en": "Apa Itu 'Closed Loop' (Sistem Tertutup)?", "id": "Sistem Kendali Dengan Umpan Balik." },
    { "en": "Apa Itu 'Transfer Function' (Fungsi Transfer)?", "id": "Model Matematika Perbandingan Output Input." },
    { "en": "Apa Itu 'Settling Time' (Waktu Penyelesaian)?", "id": "Waktu Respon Mencapai Keadaan Tunak." },
    { "en": "Apa Itu 'Overshoot' (Lewatan)?", "id": "Nilai Respon Melebihi Target Akhir." },
    { "en": "Apa Itu 'Doping' (Pendopingan)?", "id": "Penambahan Pengotor Ke Dalam Semikonduktor." },
    { "en": "Apa Itu 'Hole' (Lubang Elektron)?", "id": "Kekosongan Elektron Dalam Ikatan Atom." },
    { "en": "Apa Itu 'Electron' (Elektron)?", "id": "Partikel Atom Bermuatan Listrik Negatif." },
    { "en": "Apa Itu 'Proton'?", "id": "Partikel Atom Bermuatan Listrik Positif." },
    { "en": "Apa Itu 'Neutron'?", "id": "Partikel Atom Tidak Bermuatan Listrik." },
    { "en": "Apa Itu 'Valence Band' (Pita Valensi)?", "id": "Orbit Terluar Elektron Suatu Atom." },
    { "en": "Apa Itu 'Conduction Band' (Pita Konduksi)?", "id": "Tingkat Energi Elektron Bebas Mengalir." },
    { "en": "Apa Itu 'Band Gap' (Celah Pita)?", "id": "Pemisah Energi Antara Dua Pita." },
    { "en": "Apa Itu 'Isolator Switch' (Saklar Isolasi)?", "id": "Pemutus Rangkaian Saat Tanpa Beban." },
    { "en": "Apa Itu 'Earthing Switch' (Saklar Pembumian)?", "id": "Alat Pembumian Bagian Rangkaian Padam." },
    { "en": "Apa Itu 'Bus Coupler' (Penghubung Busbar)?", "id": "Alat Penghubung Dua Sistem Busbar." },
    { "en": "Apa Itu 'Lightning Arrester' (Penangkap Petir)?", "id": "Pelindung Peralatan Dari Tegangan Surja." },
    { "en": "Apa Itu 'Sag' (Andongan Kabel)?", "id": "Lendutan Kabel Antara Dua Tiang." },
    { "en": "Apa Itu 'Span' (Rentang Jarak)?", "id": "Jarak Mendatar Antara Dua Penyangga." },
    { "en": "Apa Itu 'Stay Wire' (Kawat Tarik)?", "id": "Kawat Penahan Beban Tarik Tiang." },
    { "en": "Apa Itu 'Insulator String' (Rantai Isolator)?", "id": "Kumpulan Isolator Gantung Saluran Udara." },
    { "en": "Apa Itu 'Suspension Insulator' (Isolator Gantung)?", "id": "Isolator Penahan Beban Kabel Transmisi." },
    { "en": "Apa Itu 'Pin Insulator' (Isolator Tumpu)?", "id": "Isolator Penyangga Kabel Pada Tiang." },
    { "en": "Apa Itu 'Shackle Insulator' (Isolator Sekel)?", "id": "Isolator Untuk Ujung Saluran Rendah." },
    { "en": "Apa Itu 'Current Transformer' (Transformator Arus)?", "id": "Penurun Arus Besar Untuk Pengukuran." },
    { "en": "Apa Itu 'Potential Transformer' (Transformator Potensial)?", "id": "Penurun Tegangan Tinggi Untuk Pengukuran." },
    { "en": "Apa Itu 'Polarity' (Polaritas)?", "id": "Sifat Kutub Positif Atau Negatif." },
    { "en": "Apa Itu 'Self Inductance' (Induktansi Diri)?", "id": "Induksi Tegangan Dalam Kumparan Sendiri." },
    { "en": "Apa Itu 'Mutual Inductance' (Induktansi Timbal Balik)?", "id": "Induksi Antara Dua Kumparan Berdekatan." },
    { "en": "Apa Itu 'Coupling Coefficient' (Koefisien Gandeng)?", "id": "Ukuran Transfer Fluks Antar Kumparan." },
    { "en": "Apa Itu 'Magnetizing Current' (Arus Magnetisasi)?", "id": "Arus Penghasil Fluks Magnetik Utama." },
    { "en": "Apa Itu 'Load Factor' (Faktor Beban)?", "id": "Rasio Beban Rata-Rata Terhadap Puncak." },
    { "en": "Apa Itu 'Demand Factor' (Faktor Kebutuhan)?", "id": "Rasio Beban Maksimum Terhadap Terpasang." },
    { "en": "Apa Itu 'Diversity Factor' (Faktor Keanekaragaman)?", "id": "Rasio Total Puncak Terhadap Gabungan." },
    { "en": "Apa Itu 'Power Density' (Kerapatan Daya)?", "id": "Jumlah Daya Listrik Per Volume." },
    { "en": "Apa Itu 'Specific Gravity' (Berat Jenis)?", "id": "Ukuran Kepekatan Cairan Elektrolit Aki." },
    { "en": "Apa Itu 'Hydrometer' (Hidrometer)?", "id": "Alat Ukur Massa Jenis Cairan." },
    { "en": "Apa Itu 'Internal Resistance' (Hambatan Dalam)?", "id": "Hambatan Listrik Di Dalam Baterai." },
    { "en": "Apa Itu 'Short Circuit Current' (Arus Hubung Singkat)?", "id": "Aliran Arus Maksimum Saat Gangguan." },
    { "en": "Apa Itu 'Breaking Capacity' (Kapasitas Pemutus)?", "id": "Arus Maksimum Yang Aman Diputus." },
    { "en": "Apa Itu 'Making Capacity' (Kapasitas Masuk)?", "id": "Arus Maksimum Saat Penutupan Saklar." },
    { "en": "Apa Itu 'Arc Quenching' (Pemadaman Busur Api)?", "id": "Proses Penghilangan Percikan Listrik Saklar." },
    { "en": "Apa Itu 'SF6' (Sulfur Hexafluoride) Gas?", "id": "Media Isolasi Pemadam Busur Api." },
    { "en": "Apa Itu 'Vacuum Circuit Breaker' (Pemutus Sirkuit Vakum)?", "id": "Pemutus Arus Berbasis Media Vakum." },
    { "en": "Apa Itu 'Air Blast Circuit Breaker' (Pemutus Udara Tekan)?", "id": "Pemutus Menggunakan Hembusan Udara Tekan." },
    { "en": "Apa Itu 'Oil Circuit Breaker' (Pemutus Minyak)?", "id": "Pemutus Arus Berbasis Media Minyak." },
    { "en": "Apa Itu 'Earthing' (Pembumian)?", "id": "Pelepasan Arus Gangguan Ke Bumi." },
    { "en": "Apa Itu 'Grounding' (Pentanahan)?", "id": "Penghubung Bagian Aktif Listrik Bumi." },
    { "en": "Apa Itu 'Dead Short' (Hubung Singkat Penuh)?", "id": "Hubungan Tanpa Hambatan Antar Penghantar." },
    { "en": "Apa Itu 'Earth Fault' (Gangguan Tanah)?", "id": "Kebocoran Arus Listrik Ke Tanah." },
    { "en": "Apa Itu 'L-L Fault' (Line To Line Fault)?", "id": "Gangguan Hubung Singkat Antar Fase." },
    { "en": "Apa Itu 'L-G Fault' (Line To Ground Fault)?", "id": "Gangguan Hubung Singkat Fase Tanah." },
    { "en": "Apa Itu 'Symmetrical Fault' (Gangguan Simetris)?", "id": "Gangguan Listrik Pada Semua Fase." },
    { "en": "Apa Itu 'Unsymmetrical Fault' (Gangguan Tak Simetris)?", "id": "Gangguan Listrik Sebagian Fase Saja." },
    { "en": "Apa Itu 'Relay Setting' (Setelan Relay)?", "id": "Nilai Batas Operasi Perangkat Proteksi." },
    { "en": "Apa Itu 'Pickup Current' (Arus Mulai)?", "id": "Arus Minimum Penggerak Kerja Relay." },
    { "en": "Apa Itu 'Reset Ratio' (Rasio Reset)?", "id": "Rasio Arus Lepas Terhadap Kerja." },
    { "en": "Apa Itu 'IDMT' (Inverse Definite Minimum Time Relay)?", "id": "Waktu Kerja Terbalik Terhadap Arus." },
    { "en": "Apa Itu 'Differential Relay' (Relay Diferensial)?", "id": "Pengaman Berdasarkan Selisih Arus Masuk." },
    { "en": "Apa Itu 'Distance Relay' (Relay Jarak)?", "id": "Proteksi Berdasarkan Ukuran Impedansi Jalur." },
    { "en": "Apa Itu 'Overcurrent Relay' (Relay Arus Lebih)?", "id": "Proteksi Terhadap Aliran Arus Berlebih." },
    { "en": "Apa Itu 'Buchholz Relay' (Relay Buchholz)?", "id": "Pengaman Internal Transformator Media Minyak." },
    { "en": "Apa Itu 'Transformer Breathing' (Pernapasan Transformator)?", "id": "Pertukaran Udara Akibat Perubahan Suhu." },
    { "en": "Apa Itu 'Conservator Tank' (Tangki Konservator)?", "id": "Wadah Cadangan Minyak Pendingin Transformator." },
    { "en": "Apa Itu 'Silica Gel' (Gel Silika)?", "id": "Bahan Penyerap Kelembapan Udara Transformator." },
    { "en": "Apa Itu 'Breather' (Alat Pernapasan)?", "id": "Lubang Sirkulasi Udara Kering Transformator." },
    { "en": "Apa Itu 'Oil Gauge' (Indikator Minyak)?", "id": "Alat Pemantau Ketinggian Minyak Transformator." },
    { "en": "Apa Itu 'Temperature Indicator' (Indikator Suhu)?", "id": "Alat Pemantau Panas Kumparan Transformator." },
    { "en": "Apa Itu 'Tap Changer' (Pengubah Sadapan)?", "id": "Alat Pengatur Tegangan Output Transformator." },
    { "en": "Apa Itu 'OLTC' (On Load Tap Changer)?", "id": "Pengubah Tegangan Saat Kondisi Beroperasi." },
    { "en": "Apa Itu 'Vector Group' (Grup Vektor)?", "id": "Konfigurasi Hubungan Lilitan Transformator Tiga." },
    { "en": "Apa Itu 'Star Connection' (Hubungan Bintang)?", "id": "Koneksi Tiga Fase Dengan Netral." },
    { "en": "Apa Itu 'Delta Connection' (Hubungan Delta)?", "id": "Koneksi Tiga Fase Bentuk Segitiga." },
    { "en": "Apa Itu 'Line Voltage' (Tegangan Jalur)?", "id": "Tegangan Antara Dua Kabel Fase." },
    { "en": "Apa Itu 'Phase Voltage' (Tegangan Fase)?", "id": "Tegangan Antara Fase Dan Netral." },
    { "en": "Apa Itu 'Line Current' (Arus Jalur)?", "id": "Arus Yang Mengalir Pada Kabel." },
    { "en": "Apa Itu 'Phase Current' (Arus Fase)?", "id": "Arus Yang Mengalir Dalam Lilitan." },
    { "en": "Apa Itu 'Balanced Load' (Beban Seimbang)?", "id": "Beban Sama Besar Di Setiap." },
    { "en": "Apa Itu 'Unbalanced Load' (Beban Tak Seimbang)?", "id": "Beban Berbeda Di Antara Fase." },
    { "en": "Apa Itu 'Phase Sequence' (Urutan Fase)?", "id": "Urutan Mencapai Puncak Gelombang Tegangan." },
    { "en": "Apa Itu 'Phase Shifter' (Penggeser Fase)?", "id": "Alat Pengubah Sudut Fase Sinyal." },
    { "en": "Apa Itu 'Power System Stability' (Stabilitas Sistem Daya)?", "id": "Kemampuan Menjaga Operasi Tetap Sinkron." },
    { "en": "Apa Itu 'Load Flow' (Aliran Beban)?", "id": "Analisis Distribusi Daya Dalam Jaringan." },
    { "en": "Apa Itu 'Voltage Profile' (Profil Tegangan)?", "id": "Gambaran Tegangan Di Sepanjang Jalur." },
    { "en": "Apa Itu 'Spinning Reserve' (Cadangan Putar)?", "id": "Kapasitas Pembangkit Siap Pakai Segera." },
    { "en": "Apa Itu 'Exciter' (Pembangkit Eksitasi)?", "id": "Sumber Arus DC Untuk Generator." },
    { "en": "Apa Itu 'Governor' (Gubernur Mesin)?", "id": "Alat Pengatur Kecepatan Putaran Mesin." },
    { "en": "Apa Itu 'Prime Mover' (Penggerak Mula)?", "id": "Mesin Pemutar Poros Generator Utama." },
    { "en": "Apa Itu 'Step Potential' (Potensial Langkah)?", "id": "Beda Tegangan Antara Dua Langkah." },
    { "en": "Apa Itu 'Current Rating' (Peringkat Arus)?", "id": "Batas Arus Maksimum Operasi Aman." },
    { "en": "Apa Itu 'Voltage Rating' (Peringkat Tegangan)?", "id": "Batas Tegangan Kerja Maksimum Aman." },
    { "en": "Apa Itu 'Temperature Rise' (Kenaikan Suhu)?", "id": "Selisih Suhu Alat Dengan Sekitar." },
    { "en": "Apa Itu 'Ambient Temperature' (Suhu Sekitar)?", "id": "Suhu Lingkungan Tempat Alat Bekerja." },
    { "en": "Apa Itu 'Service Factor' (Faktor Layanan)?", "id": "Kapasitas Beban Lebih Motor Diizinkan." },
    { "en": "Apa Itu 'Gelombang Radio' (Radio Wave)?", "id": "Radiasi Elektromagnetik Transmisi Data Nirkabel." },
    { "en": "Apa Itu 'Kedalaman Modulasi' (Modulation Depth)?", "id": "Rasio Amplitudo Sinyal Terhadap Pembawa." },
    { "en": "Apa Itu 'SNR' (Signal To Noise Ratio)?", "id": "Perbandingan Antara Sinyal Dan Derau." },
    { "en": "Apa Itu 'Atenuasi' (Attenuation)?", "id": "Penurunan Kekuatan Sinyal Dalam Medium." },
    { "en": "Apa Itu 'Penguatan Transduser' (Transducer Gain)?", "id": "Rasio Daya Output Terhadap Input." },
    { "en": "Apa Itu 'dBm' (Decibel-milliwatt)?", "id": "Satuan Level Daya Referensi Miliwatt." },
    { "en": "Apa Itu 'Bitrate' (Laju Bit)?", "id": "Jumlah Bit Yang Ditransfer Perdetik." },
    { "en": "Apa Itu 'Baud Rate' (Laju Baud)?", "id": "Kecepatan Perubahan Simbol Per Detik." },
    { "en": "Apa Itu 'Full Duplex' (Dupleks Penuh)?", "id": "Komunikasi Dua Arah Secara Bersamaan." },
    { "en": "Apa Itu 'Simplex' (Simpleks)?", "id": "Komunikasi Data Hanya Satu Arah." },
    { "en": "Apa Itu 'Half Duplex' (Dupleks Setengah)?", "id": "Komunikasi Dua Arah Bergantian Waktu." },
    { "en": "Apa Itu 'Multiplexing' (Multipleksing)?", "id": "Penggabungan Banyak Sinyal Satu Saluran." },
    { "en": "Apa Itu 'TDM' (Time Division Multiplexing)?", "id": "Pembagian Saluran Berdasarkan Slot Waktu." },
    { "en": "Apa Itu 'FDM' (Frequency Division Multiplexing)?", "id": "Pembagian Saluran Berdasarkan Rentang Frekuensi." },
    { "en": "Apa Itu 'WDM' (Wavelength Division Multiplexing)?", "id": "Multipleksing Berdasarkan Perbedaan Panjang Gelombang." },
    { "en": "Apa Itu 'Packet Switching' (Pensakelaran Paket)?", "id": "Metode Pengiriman Data Berupa Paket." },
    { "en": "Apa Itu 'Circuit Switching' (Pensakelaran Sirkuit)?", "id": "Metode Komunikasi Jalur Fisik Tetap." },
    { "en": "Apa Itu 'LAN' (Local Area Network)?", "id": "Jaringan Komputer Area Skala Kecil." },
    { "en": "Apa Itu 'WAN' (Wide Area Network)?", "id": "Jaringan Komputer Mencakup Area Luas." },
    { "en": "Apa Itu 'PAN' (Personal Area Network)?", "id": "Jaringan Nirkabel Jarak Sangat Dekat." },
    { "en": "Apa Itu 'Router' (Perute)?", "id": "Perangkat Penghubung Antar Jaringan Berbeda." },
    { "en": "Apa Itu 'Network Switch' (Saklar Jaringan)?", "id": "Penghubung Perangkat Dalam Satu Jaringan." },
    { "en": "Apa Itu 'Network Hub' (Hub Jaringan)?", "id": "Titik Pusat Koneksi Jaringan Sederhana." },
    { "en": "Apa Itu 'Default Gateway' (Gerbang Default)?", "id": "Titik Keluar Menuju Jaringan Lain." },
    { "en": "Apa Itu 'Network Bridge' (Jembatan Jaringan)?", "id": "Penghubung Dua Segmen Jaringan Sama." },
    { "en": "Apa Itu 'Firewall' (Tembok Api)?", "id": "Sistem Keamanan Penyaring Lalu Lintas." },
    { "en": "Apa Itu 'IP' (Internet Protocol) Address?", "id": "Alamat Identitas Perangkat Dalam Jaringan." },
    { "en": "Apa Itu 'MAC' (Media Access Control) Address?", "id": "Alamat Fisik Unik Perangkat Jaringan." },
    { "en": "Apa Itu 'TCP' (Transmission Control Protocol)?", "id": "Protokol Pengiriman Data Terjamin, Akurat." },
    { "en": "Apa Itu 'UDP' (User Datagram Protocol)?", "id": "Protokol Pengiriman Data Cepat, Ringan." },
    { "en": "Apa Itu 'HTTP' (Hypertext Transfer Protocol)?", "id": "Protokol Pengiriman Dokumen Web Digital." },
    { "en": "Apa Itu 'FTP' (File Transfer Protocol)?", "id": "Protokol Untuk Pengiriman Berkas Digital." },
    { "en": "Apa Itu 'SMTP' (Simple Mail Transfer Protocol)?", "id": "Protokol Untuk Pengiriman Surat Elektronik." },
    { "en": "Apa Itu 'DNS' (Domain Name System)?", "id": "Penerjemah Nama Domain Menjadi IP." },
    { "en": "Apa Itu 'DHCP' (Dynamic Host Configuration Protocol)?", "id": "Pemberi Alamat IP Otomatis Perangkat." },
    { "en": "Apa Itu 'ICMP' (Internet Control Message Protocol)?", "id": "Protokol Pelaporan Kesalahan Dalam Jaringan." },
    { "en": "Apa Itu 'VPN' (Virtual Private Network)?", "id": "Koneksi Jaringan Pribadi Yang Aman." },
    { "en": "Apa Itu 'VLAN' (Virtual Local Area Network)?", "id": "Kelompok Perangkat Jaringan Secara Logis." },
    { "en": "Apa Itu 'Throughput' (Throughput Jaringan)?", "id": "Kecepatan Transfer Data Sebenarnya, Nyata." },
    { "en": "Apa Itu 'Latency' (Latensi)?", "id": "Waktu Tunda Pengiriman Data Jaringan." },
    { "en": "Apa Itu 'Jitter' (Variasi Latensi)?", "id": "Variasi Waktu Kedatangan Paket Data." },
    { "en": "Apa Itu 'Packet Loss' (Kehilangan Paket)?", "id": "Gagalnya Paket Data Sampai Tujuan." },
    { "en": "Apa Itu 'BER' (Bit Error Rate)?", "id": "Rasio Kesalahan Bit Selama Transmisi." },
    { "en": "Apa Itu 'Spectrum Analyzer' (Penganalisis Spektrum)?", "id": "Alat Ukur Amplitudo Terhadap Frekuensi." },
    { "en": "Apa Itu 'Logic Analyzer' (Penganalisis Logika)?", "id": "Alat Pemantau Banyak Sinyal Digital." },
    { "en": "Apa Itu 'Frequency Counter' (Pencacah Frekuensi)?", "id": "Alat Ukur Jumlah Pulsa Detik." },
    { "en": "Apa Itu 'Network Analyzer' (Penganalisis Jaringan)?", "id": "Alat Ukur Parameter Jaringan Radio." },
    { "en": "Apa Itu 'Wattmeter' (Alat Ukur Watt)?", "id": "Alat Ukur Konsumsi Daya Nyata." },
    { "en": "Apa Itu 'Cos Phi Meter' (Alat Ukur Cos)?", "id": "Alat Ukur Faktor Daya Listrik." },
    { "en": "Apa Itu 'Phase Sequence Indicator' (Indikator Urutan Fase)?", "id": "Alat Penentu Arah Urutan Fase." },
    { "en": "Apa Itu 'Tachometer' (Takometer)?", "id": "Alat Ukur Kecepatan Putaran Poros." },
    { "en": "Apa Itu 'Anemometer' (Anemometer)?", "id": "Alat Ukur Kecepatan Aliran Angin." },
    { "en": "Apa Itu 'Flow Meter' (Pengukur Aliran)?", "id": "Alat Ukur Laju Aliran Fluida." },
    { "en": "Apa Itu 'Pressure Transmitter' (Pemancar Tekanan)?", "id": "Pengubah Sinyal Tekanan Menjadi Elektrik." },
    { "en": "Apa Itu 'Level Sensor' (Sensor Ketinggian)?", "id": "Sensor Pendeteksi Ketinggian Permukaan Cairan." },
    { "en": "Apa Itu 'Ultrasonic Transducer' (Transduser Ultrasonik)?", "id": "Pengubah Energi Listrik Menjadi Suara." },
    { "en": "Apa Itu 'LVDT' (Linear Variable Differential Transformer)?", "id": "Sensor Pengukur Pergeseran Posisi Linear." },
    { "en": "Apa Itu 'RVDT' (Rotary Variable Differential Transformer)?", "id": "Sensor Pengukur Pergeseran Posisi Sudut." },
    { "en": "Apa Itu 'Resolver' (Resolfer)?", "id": "Sensor Rotasi Untuk Sudut Presisi." },
    { "en": "Apa Itu 'Synchro' (Sinkro)?", "id": "Transduser Sudut Berbasis Medan Magnet." },
    { "en": "Apa Itu 'Thermopile' (Termopil)?", "id": "Kumpulan Termokopel Penghasil Tegangan Tinggi." },
    { "en": "Apa Itu 'Pyrometer' (Pirometer)?", "id": "Alat Ukur Suhu Tanpa Sentuh." },
    { "en": "Apa Itu 'Bolometer' (Bolometer)?", "id": "Sensor Pengukur Daya Radiasi Elektromagnetik." },
    { "en": "Apa Itu 'Photoresistor' (Fotoresistor)?", "id": "Resistor Sensitif Terhadap Cahaya Luar." },
    { "en": "Apa Itu 'Photovoltaic Cell' (Sel Fotovoltaik)?", "id": "Pengubah Energi Cahaya Menjadi Listrik." },
    { "en": "Apa Itu 'RTOS' (Real Time Operating System)?", "id": "Sistem Operasi Respon Waktu Cepat." },
    { "en": "Apa Itu 'Firmware' (Perangkat Tegar)?", "id": "Perangkat Lunak Tetap Dalam Perangkat." },
    { "en": "Apa Itu 'Bootloader' (Pemuat Bot)?", "id": "Program Awal Pengaktif Sistem Operasi." },
    { "en": "Apa Itu 'Compiler' (Kompilator)?", "id": "Penerjemah Kode Program Menjadi Mesin." },
    { "en": "Apa Itu 'Interpreter' (Interpretator)?", "id": "Penerjemah Kode Program Secara Langsung." },
    { "en": "Apa Itu 'Debugger' (Pendebung)?", "id": "Alat Pendeteksi Kesalahan Kode Program." },
    { "en": "Apa Itu 'Emulator' (Emulator)?", "id": "Peniru Fungsi Perangkat Keras Lain." },
    { "en": "Apa Itu 'Simulator' (Simulator)?", "id": "Peniru Perilaku Sistem Secara Matematis." },
    { "en": "Apa Itu 'API' (Application Programming Interface)?", "id": "Antarmuka Penghubung Antar Program Aplikasi." },
    { "en": "Apa Itu 'SDK' (Software Development Kit)?", "id": "Kumpulan Alat Pengembangan Perangkat Lunak." },
    { "en": "Apa Itu 'IDE' (Integrated Development Environment)?", "id": "Lingkungan Kerja Terpadu Penulisan Kode." },
    { "en": "Apa Itu 'GUI' (Graphical User Interface)?", "id": "Antarmuka Pengguna Berbasis Grafis Visual." },
    { "en": "Apa Itu 'CLI' (Command Line Interface)?", "id": "Antarmuka Pengguna Berbasis Baris Perintah." },
    { "en": "Apa Itu 'Cloud Computing' (Komputasi Awan)?", "id": "Layanan Komputasi Melalui Jaringan Internet." },
    { "en": "Apa Itu 'Edge Computing' (Komputasi Pinggir)?", "id": "Pemrosesan Data Dekat Sumber Data." },
    { "en": "Apa Itu 'IoT' (Internet Of Things)?", "id": "Jaringan Benda Terhubung Koneksi Internet." },
    { "en": "Apa Itu 'M2M' (Machine To Machine)?", "id": "Komunikasi Otomatis Antar Perangkat Mesin." },
    { "en": "Apa Itu 'RFID' (Radio Frequency Identification)?", "id": "Identifikasi Objek Menggunakan Gelombang Radio." },
    { "en": "Apa Itu 'NFC' (Near Field Communication)?", "id": "Komunikasi Nirkabel Jarak Sangat Dekat." },
    { "en": "Apa Itu 'Li-Fi' (Light Fidelity)?", "id": "Transmisi Data Cepat Menggunakan Cahaya." },
    { "en": "Apa Itu '5G' (Fifth Generation) Network?", "id": "Teknologi Jaringan Seluler Generasi Kelima." },
    { "en": "Apa Itu 'SRAM' (Static Random Access Memory)?", "id": "Memori Data Statis Kecepatan Tinggi." },
    { "en": "Apa Itu 'DRAM' (Dynamic Random Access Memory)?", "id": "Memori Data Dinamis Kapasitas Besar." },
    { "en": "Apa Itu 'Flash Memory' (Memori Flash)?", "id": "Penyimpanan Data Non-volatil Dapat Dihapus." },
    { "en": "Apa Itu 'Interrupt Latency' (Latensi Interupsi)?", "id": "Waktu Respon Terhadap Sinyal Interupsi." },
    { "en": "Apa Itu 'Dead Time' (Waktu Mati)?", "id": "Jeda Waktu Antar Dua Saklar." },
    { "en": "Apa Itu 'Propagation Delay' (Tunda Rambat)?", "id": "Waktu Tempuh Sinyal Melalui Gerbang." },
    { "en": "Apa Itu 'Hold Time' (Waktu Tahan)?", "id": "Durasi Data Stabil Setelah Detak." },
    { "en": "Apa Itu 'Setup Time' (Waktu Persiapan)?", "id": "Durasi Data Stabil Sebelum Detak." },
    { "en": "Apa Itu 'Fan-Out' (Beban Keluaran)?", "id": "Jumlah Beban Maksimum Output Gerbang." },
    { "en": "Apa Itu 'Fan-In' (Beban Masukan)?", "id": "Jumlah Input Maksimum Gerbang Logika." },
    { "en": "Apa Itu 'Logic Threshold' (Ambang Logika)?", "id": "Batas Tegangan Penentu Status Logika." },
    { "en": "Apa Itu 'Noise Margin' (Margin Derau)?", "id": "Batas Ketahanan Terhadap Gangguan Sinyal." },
    { "en": "Apa Itu 'Power Dissipation' (Disipasi Daya)?", "id": "Daya Terbuang Menjadi Panas Komponen." },
    { "en": "Apa Itu 'Voltage Regulator Module' (Modul Regulator)?", "id": "Penyuplai Tegangan Konstan Untuk Prosesor." },
    { "en": "Apa Itu 'Faraday's Law' (Hukum Faraday)?", "id": "Induksi Magnet Menghasilkan Tegangan Listrik." },
    { "en": "Apa Itu 'Lenz's Law' (Hukum Lenz)?", "id": "Arah Arus Induksi Melawan Perubahan." },
    { "en": "Apa Itu 'Ampere's Law' (Hukum Ampere)?", "id": "Hubungan Arus Listrik Dan Magnet." },
    { "en": "Apa Itu 'Biot-Savart Law' (Hukum Biot-Savart)?", "id": "Medan Magnet Akibat Arus Listrik." },
    { "en": "Apa Itu 'Gauss's Law' (Hukum Gauss)?", "id": "Total Fluks Listrik Ruang Tertutup." },
    { "en": "Apa Itu 'Lorentz Force' (Gaya Lorentz)?", "id": "Gaya Pada Muatan Dalam Medan." },
    { "en": "Apa Itu 'Fleming's Left Hand Rule' (Aturan Tangan Kiri Fleming)?", "id": "Penentu Arah Gaya Magnetik Motor." },
    { "en": "Apa Itu 'Fleming's Right Hand Rule' (Aturan Tangan Kanan Fleming)?", "id": "Penentu Arah Arus Generator." },
    { "en": "Apa Itu 'Work Function' (Fungsi Kerja)?", "id": "Energi Minimum Melepas Elektron Logam." },
    { "en": "Apa Itu 'Photoelectric Effect' (Efek Fotolistrik)?", "id": "Pelepasan Elektron Akibat Cahaya Matahari." },
    { "en": "Apa Itu 'Seebeck Effect' (Efek Seebeck)?", "id": "Perubahan Suhu Menjadi Tegangan Listrik." },
    { "en": "Apa Itu 'Peltier Effect' (Efek Peltier)?", "id": "Perpindahan Panas Akibat Arus Listrik." },
    { "en": "Apa Itu 'Thomson Effect' (Efek Thomson)?", "id": "Penyerapan Panas Pada Penghantar Berarus." },
    { "en": "Apa Itu 'Hall Effect' (Efek Hall)?", "id": "Beda Potensial Akibat Medan Magnet." },
    { "en": "Apa Itu 'Joule Heating' (Pemanasan Joule)?", "id": "Panas Akibat Aliran Arus Listrik." },
    { "en": "Apa Itu 'Piezoelectric Effect' (Efek Piezoelektrik)?", "id": "Listrik Dari Tekanan Mekanik Bahan." },
    { "en": "Apa Itu 'Reverse Piezoelectric Effect' (Efek Piezoelektrik Terbalik)?", "id": "Gerakan Mekanik Dari Tegangan Listrik." },
    { "en": "Apa Itu 'Stray Capacitance' (Kapasitansi Liar)?", "id": "Kapasitansi Tak Diinginkan Antar Penghantar." },
    { "en": "Apa Itu 'Stray Inductance' (Induktansi Liar)?", "id": "Induktansi Tak Diinginkan Pada Jalur." },
    { "en": "Apa Itu 'Mutual Flux' (Fluks Bersama)?", "id": "Fluks Menembus Dua Lilitan Berdekatan." },
    { "en": "Apa Itu 'Magnetic Shielding' (Pelindung Magnetik)?", "id": "Bahan Penahan Pengaruh Medan Magnet." },
    { "en": "Apa Itu 'Tesla Coil' (Kumparan Tesla)?", "id": "Transformator Resonansi Tegangan Sangat Tinggi." },
    { "en": "Apa Itu 'Barkhausen Effect' (Efek Barkhausen)?", "id": "Perubahan Magnetisasi Secara Tiba-Tiba, Tajam." },
    { "en": "Apa Itu 'Curie Temperature' (Suhu Curie)?", "id": "Suhu Hilangnya Sifat Magnetik Bahan." },
    { "en": "Apa Itu 'Diamagnetism' (Diamagnetisme)?", "id": "Bahan Penolak Medan Magnet Luar." },
    { "en": "Apa Itu 'Paramagnetism' (Paramagnetisme)?", "id": "Bahan Penarik Lemah Medan Magnet." },
    { "en": "Apa Itu 'Ferromagnetism' (Ferromagnetisme)?", "id": "Bahan Penarik Kuat Medan Magnet." },
    { "en": "Apa Itu 'Antiferromagnetism' (Antiferromagnetisme)?", "id": "Momen Magnetik Berlawanan, Saling Menghilangkan." },
    { "en": "Apa Itu 'Ferrimagnetism' (Ferrimagnetisme)?", "id": "Momen Magnetik Berlawanan, Tidak Seimbang." },
    { "en": "Apa Itu 'Hysteresis Loop' (Loop Histeresis)?", "id": "Kurva Hubungan Intensitas Magnet, Fluks." },
    { "en": "Apa Itu 'Retentivity' (Retentivitas)?", "id": "Kemampuan Bahan Menyimpan Sisa Magnet." },
    { "en": "Apa Itu 'Coercive Force' (Gaya Koersif)?", "id": "Gaya Untuk Menghilangkan Sisa Magnet." },
    { "en": "Apa Itu 'Soft Magnetic Material' (Bahan Magnet Lunak)?", "id": "Bahan Mudah Dimagnetisasi Dan Dihilangkan." },
    { "en": "Apa Itu 'Hard Magnetic Material' (Bahan Magnet Keras)?", "id": "Bahan Sulit Dimagnetisasi, Magnet Permanen." },
    { "en": "Apa Itu 'Eddy Current Brake' (Rem Arus Eddy)?", "id": "Pengereman Menggunakan Induksi Medan Magnet." },
    { "en": "Apa Itu 'Magnetic Levitation' (Levitasi Magnetik)?", "id": "Pengangkatan Benda Menggunakan Gaya Magnet." },
    { "en": "Apa Itu 'Solenoid Actuator' (Aktuator Solenoid)?", "id": "Penggerak Mekanik Berbasis Gaya Elektromagnetik." },
    { "en": "Apa Itu 'Reluctance Torque' (Torsi Reluktansi)?", "id": "Gaya Putar Akibat Variasi Reluktansi." },
    { "en": "Apa Itu 'Cogging Torque' (Torsi Cogging)?", "id": "Interaksi Magnetik Antar Gigi Rotor." },
    { "en": "Apa Itu 'Torque Ripple' (Riak Torsi)?", "id": "Variasi Periodik Pada Output Torsi." },
    { "en": "Apa Itu 'Servomechanism' (Servomekanisme)?", "id": "Sistem Kendali Otomatis Posisi Presisi." },
    { "en": "Apa Itu 'Deadband' (Pita Mati)?", "id": "Rentang Input Tanpa Respon Output." },
    { "en": "Apa Itu 'Backlash' (Spasi Mekanik)?", "id": "Celah Antar Gigi Roda Mekanik." },
    { "en": "Apa Itu 'Limit Switch' (Saklar Pembatas)?", "id": "Saklar Penghenti Gerakan Batas Maksimal." },
    { "en": "Apa Itu 'Proximity Sensor Inductive' (Sensor Kedekatan Induktif)?", "id": "Pendeteksi Logam Tanpa Kontak Fisik." },
    { "en": "Apa Itu 'Proximity Sensor Capacitive' (Sensor Kedekatan Kapasitif)?", "id": "Pendeteksi Objek Apapun Tanpa Kontak." },
    { "en": "Apa Itu 'Laser Sensor' (Sensor Laser)?", "id": "Pengukur Jarak Presisi Cahaya Laser." },
    { "en": "Apa Itu 'Color Sensor' (Sensor Warna)?", "id": "Pendeteksi Nilai Warna Permukaan Benda." },
    { "en": "Apa Itu 'Gas Sensor' (Sensor Gas)?", "id": "Pendeteksi Konsentrasi Gas Di Udara." },
    { "en": "Apa Itu 'Flame Sensor' (Sensor Api)?", "id": "Pendeteksi Keberadaan Gelombang Cahaya Api." },
    { "en": "Apa Itu 'Vibration Sensor' (Sensor Getaran)?", "id": "Sensor Pengukur Frekuensi Getaran Mekanik." },
    { "en": "Apa Itu 'Tilt Sensor' (Sensor Kemiringan)?", "id": "Sensor Pendeteksi Perubahan Sudut Kemiringan." },
    { "en": "Apa Itu 'Water Level Sensor' (Sensor Level Air)?", "id": "Pendeteksi Ketinggian Cairan Dalam Wadah." },
    { "en": "Apa Itu 'pH Sensor' (Sensor Keasaman)?", "id": "Sensor Pengukur Tingkat Keasaman Larutan." },
    { "en": "Apa Itu 'Conductivity Sensor' (Sensor Konduktivitas)?", "id": "Sensor Pengukur Kemampuan Hantar Listrik." },
    { "en": "Apa Itu 'Turbidity Sensor' (Sensor Kekeruhan)?", "id": "Sensor Pengukur Tingkat Kejernihan Cairan." },
    { "en": "Apa Itu 'Flow Switch' (Saklar Aliran)?", "id": "Saklar Aktif Saat Ada Aliran." },
    { "en": "Apa Itu 'Pressure Switch' (Saklar Tekanan)?", "id": "Saklar Aktif Saat Batas Tekanan." },
    { "en": "Apa Itu 'Pneumatic System' (Sistem Pneumatik)?", "id": "Sistem Penggerak Menggunakan Udara Tekan." },
    { "en": "Apa Itu 'Hydraulic System' (Sistem Hidrolik)?", "id": "Sistem Penggerak Menggunakan Cairan Tekanan." },
    { "en": "Apa Itu 'Air Compressor' (Kompresor Udara)?", "id": "Mesin Penghasil Udara Bertekanan Tinggi." },
    { "en": "Apa Itu 'Control Valve' (Katup Kendali)?", "id": "Alat Pengatur Laju Aliran Fluida." },
    { "en": "Apa Itu 'Actuator' (Aktuator)?", "id": "Bagian Pelaksana Perintah Sistem Kendali." },
    { "en": "Apa Itu 'Positioner' (Penentu Posisi)?", "id": "Alat Penjamin Posisi Katup Sesuai." },
    { "en": "Apa Itu 'Feedback Signal' (Sinyal Umpan Balik)?", "id": "Sinyal Kondisi Nyata Menuju Pengendali." },
    { "en": "Apa Itu 'Set Point' (Titik Setelan)?", "id": "Nilai Target Yang Ingin Dicapai." },
    { "en": "Apa Itu 'Error Signal' (Sinyal Kesalahan)?", "id": "Selisih Antara Target Dan Kenyataan." },
    { "en": "Apa Itu 'Process Variable' (Variabel Proses)?", "id": "Parameter Yang Sedang Diukur, Dikendalikan." },
    { "en": "Apa Itu 'Disturbance' (Gangguan Sistem)?", "id": "Faktor Luar Pengganggu Kestabilan Sistem." },
    { "en": "Apa Itu 'Steady State' (Keadaan Tunak)?", "id": "Kondisi Sistem Saat Sudah Stabil." },
    { "en": "Apa Itu 'Rise Time' (Waktu Naik)?", "id": "Waktu Mencapai Sembilan Puluh Persen." },
    { "en": "Apa Itu 'Fall Time' (Waktu Turun)?", "id": "Waktu Penurunan Sinyal Hingga Stabil." },
    { "en": "Apa Itu 'Damping Ratio' (Rasio Redaman)?", "id": "Ukuran Kecepatan Peluruhan Getaran Sistem." },
    { "en": "Apa Itu 'Natural Frequency' (Frekuensi Alami)?", "id": "Frekuensi Sistem Bergetar Tanpa Paksaan." },
    { "en": "Apa Itu 'State Space' (Ruang Keadaan)?", "id": "Model Matematika Sistem Kendali Modern." },
    { "en": "Apa Itu 'Controllability' (Keterkendalian)?", "id": "Kemampuan Mengatur Keadaan Sistem Input." },
    { "en": "Apa Itu 'Observability' (Keteramatan)?", "id": "Kemampuan Mengetahui Keadaan Dari Output." },
    { "en": "Apa Itu 'Bode Plot' (Diagram Bode)?", "id": "Grafik Respon Frekuensi Sistem Kendali." },
    { "en": "Apa Itu 'Nyquist Plot' (Diagram Nyquist)?", "id": "Grafik Penentu Stabilitas Sistem Tertutup." },
    { "en": "Apa Itu 'Root Locus' (Lokus Akar)?", "id": "Lintasan Akar Persamaan Karakteristik Sistem." },
    { "en": "Apa Itu 'Lead Compensator' (Kompensator Lead)?", "id": "Rangkaian Perbaikan Respon Kecepatan Sistem." },
    { "en": "Apa Itu 'Lag Compensator' (Kompensator Lag)?", "id": "Rangkaian Perbaikan Akurasi Keadaan Tunak." },
    { "en": "Apa Itu 'Fuzzy Logic' (Logika Samar)?", "id": "Logika Berdasarkan Derajat Kebenaran Variabel." },
    { "en": "Apa Itu 'Neural Network' (Jaringan Saraf)?", "id": "Sistem Komputasi Meniru Otak Manusia." },
    { "en": "Apa Itu 'Genetic Algorithm' (Algoritma Genetik)?", "id": "Metode Optimasi Berdasarkan Evolusi Alami." },
    { "en": "Apa Itu 'Machine Learning' (Pembelajaran Mesin)?", "id": "Pengembangan Algoritma Belajar Dari Data." },
    { "en": "Apa Itu 'Artificial Intelligence' (Kecerdasan Buatan)?", "id": "Kemampuan Mesin Meniru Kecerdasan Manusia." },
    { "en": "Apa Itu 'Big Data' (Data Besar)?", "id": "Kumpulan Data Volume Sangat Besar." },
    { "en": "Apa Itu 'Data Mining' (Penggalian Data)?", "id": "Proses Menemukan Pola Dalam Data." },
    { "en": "Apa Itu 'Cyber Security' (Keamanan Siber)?", "id": "Perlindungan Sistem Dari Serangan Digital." },
    { "en": "Apa Itu 'Encryption' (Enkripsi)?", "id": "Proses Pengacakan Data Menjadi Rahasia." },
    { "en": "Apa Itu 'Decryption' (Dekripsi)?", "id": "Proses Pembacaan Kembali Data Enkripsi." },
    { "en": "Apa Itu 'Blockchain' (Rantai Blok)?", "id": "Sistem Pencatatan Transaksi Digital Terdistribusi." },
    { "en": "Apa Itu 'Smart Contract' (Kontrak Cerdas)?", "id": "Perjanjian Digital Yang Berjalan Otomatis." },
    { "en": "Apa Itu 'Server' (Peladen)?", "id": "Komputer Penyedia Layanan Bagi Klien." },
    { "en": "Apa Itu 'Client' (Klien)?", "id": "Perangkat Pengguna Layanan Dari Server." },
    { "en": "Apa Itu 'Data Center' (Pusat Data)?", "id": "Fasilitas Penyimpanan Server Skala Besar." },
    { "en": "Apa Itu 'Bandwidth' (Lebar Pita Jaringan)?", "id": "Kapasitas Maksimal Transmisi Data Jaringan." },
    { "en": "Apa Itu 'Ping' (Packet Internet Groper)?", "id": "Alat Uji Koneksi Antar Perangkat." },
    { "en": "Apa Itu 'Uptime' (Waktu Aktif)?", "id": "Durasi Sistem Beroperasi Tanpa Gangguan." },
    { "en": "Apa Itu 'LV' (Low Voltage)?", "id": "Tegangan Rendah Di Bawah Seribu Volt." },
    { "en": "Apa Itu 'MV' (Medium Voltage)?", "id": "Tegangan Menengah, Satu Sampai Tigapuluh Kilovolt." },
    { "en": "Apa Itu 'HV' (High Voltage)?", "id": "Tegangan Tinggi Di Atas Tigapuluh Kilovolt." },
    { "en": "Apa Itu 'EHV' (Extra High Voltage)?", "id": "Tegangan Ekstra Tinggi, Ratusan Kilovolt." },
    { "en": "Apa Itu 'UHV' (Ultra High Voltage)?", "id": "Tegangan Sangat Tinggi, Ribuan Kilovolt." },
    { "en": "Apa Itu 'VDR' (Voltage Dependent Resistor)?", "id": "Hambatan Berubah Berdasarkan Tegangan Masuk." },
    { "en": "Apa Itu 'ESD Protection' (Pelindung Pelepasan Statis)?", "id": "Pelindung Dari Lonjakan Listrik Statis." },
    { "en": "Apa Itu 'Ferrite Core' (Inti Ferit)?", "id": "Inti Magnetik Penekan Gangguan Frekuensi." },
    { "en": "Apa Itu 'Choke Coil' (Kumparan Cekik)?", "id": "Induktor Penghambat Arus Frekuensi Tinggi." },
    { "en": "Apa Itu 'Decoupling Capacitor' (Kapasitor Pemisah)?", "id": "Kapasitor Penstabil Tegangan Jalur Daya." },
    { "en": "Apa Itu 'Bypass Capacitor' (Kapasitor Pintas)?", "id": "Kapasitor Penyalur Sinyal AC Ke Tanah." },
    { "en": "Apa Itu 'Pull-Up Resistor' (Resistor Tarik Atas)?", "id": "Resistor Penjamin Logika Tinggi Masukan." },
    { "en": "Apa Itu 'Pull-Down Resistor' (Resistor Tarik Bawah)?", "id": "Resistor Penjamin Logika Rendah Masukan." },
    { "en": "Apa Itu 'Voltage Clamp' (Penjepit Tegangan)?", "id": "Pembatas Nilai Tegangan Maksimum Sinyal." },
    { "en": "Apa Itu 'Current Limiter' (Pembatas Arus)?", "id": "Rangkaian Pembatas Aliran Arus Listrik." },
    { "en": "Apa Itu 'Surge Arrester' (Penangkap Surja)?", "id": "Alat Pengalih Lonjakan Tegangan Tanah." },
    { "en": "Apa Itu 'Ground Rod' (Batang Arde)?", "id": "Logam Penyalur Arus Gangguan Tanah." },
    { "en": "Apa Itu 'Grounding Grid' (Jala Pembumian)?", "id": "Jaringan Kabel Tanam Pembumian Area." },
    { "en": "Apa Itu 'Isolation Barrier' (Penghalang Isolasi)?", "id": "Batas Pemisah Listrik Antar Sirkuit." },
    { "en": "Apa Itu 'Flyback Transformer' (Transformator Flyback)?", "id": "Pembangkit Tegangan Tinggi Pulsa Cepat." },
    { "en": "Apa Itu 'Current Sensor' (Sensor Arus)?", "id": "Sensor Pengukur Besaran Aliran Arus." },
    { "en": "Apa Itu 'Voltage Sensor' (Sensor Tegangan)?", "id": "Sensor Pengukur Besaran Potensial Tegangan." },
    { "en": "Apa Itu 'Phase Detector' (Pendeteksi Fase)?", "id": "Pendeteksi Perbedaan Fase Antar Sinyal." },
    { "en": "Apa Itu 'Frequency Mixer' (Pencampur Frekuensi)?", "id": "Sirkuit Penggabung Dua Frekuensi Berbeda." },
    { "en": "Apa Itu 'Local Oscillator' (Osilator Lokal)?", "id": "Pembangkit Frekuensi Referensi Sistem Radio." },
    { "en": "Apa Itu 'Intermediate Frequency' (Frekuensi Antara)?", "id": "Frekuensi Hasil Konversi Sistem Penerima." },
    { "en": "Apa Itu 'LNA' (Low Noise Amplifier)?", "id": "Penguat Sinyal Lemah Minim Derau." },
    { "en": "Apa Itu 'PA' (Power Amplifier)?", "id": "Penguat Daya Sinyal Sebelum Antena." },
    { "en": "Apa Itu 'Duplexer' (Duplekser)?", "id": "Alat Pembagi Jalur Kirim Terima." },
    { "en": "Apa Itu 'Diplexer' (Diplekser)?", "id": "Pemisah Dua Frekuensi Satu Antena." },
    { "en": "Apa Itu 'Impedance Bridge' (Jembatan Impedansi)?", "id": "Alat Pengukur Impedansi Komponen Listrik." },
    { "en": "Apa Itu 'VNA' (Vector Network Analyzer)?", "id": "Alat Ukur Karakteristik Jaringan Radio." },
    { "en": "Apa Itu 'Spectrum Occupancy' (Okupansi Spektrum)?", "id": "Ukuran Penggunaan Pita Frekuensi Radio." },
    { "en": "Apa Itu 'Signal Integrity' (Integritas Sinyal)?", "id": "Kualitas Sinyal Listrik Dalam Jalur." },
    { "en": "Apa Itu 'Eye Diagram' (Diagram Mata)?", "id": "Visualisasi Analisis Kualitas Data Digital." },
    { "en": "Apa Itu 'Bit Error' (Kesalahan Bit)?", "id": "Kesalahan Bit Selama Proses Transmisi." },
    { "en": "Apa Itu 'Hamming Code' (Kode Hamming)?", "id": "Metode Koreksi Kesalahan Data Digital." },
    { "en": "Apa Itu 'CRC' (Cyclic Redundancy Check)?", "id": "Metode Deteksi Kesalahan Pengiriman Data." },
    { "en": "Apa Itu 'FEC' (Forward Error Correction)?", "id": "Teknik Perbaikan Kesalahan Data Otomatis." },
    { "en": "Apa Itu 'Data Compression' (Kompresi Data)?", "id": "Pengurangan Ukuran Berkas Tanpa Hilang." },
    { "en": "Apa Itu 'Lossless Compression' (Kompresi Tanpa Hilang)?", "id": "Kompresi Data Tanpa Kehilangan Informasi." },
    { "en": "Apa Itu 'Lossy Compression' (Kompresi Dengan Kehilangan)?", "id": "Kompresi Data Dengan Menghapus Detail." },
    { "en": "Apa Itu 'Digital Filtering' (Penyaringan Digital)?", "id": "Proses Penyaringan Sinyal Secara Digital." },
    { "en": "Apa Itu 'FFT' (Fast Fourier Transform)?", "id": "Algoritma Analisis Frekuensi Sinyal Cepat." },
    { "en": "Apa Itu 'DCT' (Discrete Cosine Transform)?", "id": "Transformasi Data Untuk Kompresi Gambar." },
    { "en": "Apa Itu 'Wavelet Transform' (Transformasi Wavelet)?", "id": "Metode Analisis Sinyal Dalam Waktu." },
    { "en": "Apa Itu 'PCM' (Pulse Code Modulation)?", "id": "Metode Pengubahan Analog Ke Digital." },
    { "en": "Apa Itu 'Delta Modulation' (Modulasi Delta)?", "id": "Teknik Konversi Sinyal Menjadi Digital." },
    { "en": "Apa Itu 'Adaptive Modulation' (Modulasi Adaptif)?", "id": "Penyesuaian Modulasi Berdasarkan Kondisi Saluran." },
    { "en": "Apa Itu 'OFDM' (Orthogonal Frequency Division Multiplexing)?", "id": "Teknik Transmisi Data Kecepatan Tinggi." },
    { "en": "Apa Itu 'MIMO' (Multiple Input Multiple Output)?", "id": "Teknologi Multi Antena Pengiriman Data." },
    { "en": "Apa Itu 'Beamforming' (Pembentukan Berkas)?", "id": "Teknik Pengarahan Sinyal Nirkabel Presisi." },
    { "en": "Apa Itu 'Carrier Aggregation' (Agregasi Pembawa)?", "id": "Penggabungan Beberapa Pita Frekuensi Seluler." },
    { "en": "Apa Itu 'Handover' (Serah Terima)?", "id": "Perpindahan Koneksi Antar Menara Seluler." },
    { "en": "Apa Itu 'Roaming' (Jelajah)?", "id": "Layanan Komunikasi Di Luar Jangkauan." },
    { "en": "Apa Itu 'Base Station' (Stasiun Basis)?", "id": "Stasiun Pemancar Penerima Sinyal Seluler." },
    { "en": "Apa Itu 'Core Network' (Jaringan Inti)?", "id": "Jaringan Pusat Pengendali Komunikasi Seluler." },
    { "en": "Apa Itu 'Backhaul' (Jalur Balik)?", "id": "Jalur Penghubung Antena Ke Pusat." },
    { "en": "Apa Itu 'Latency Time' (Waktu Latensi)?", "id": "Waktu Tunggu Pengiriman Paket Data." },
    { "en": "Apa Itu 'Jitter Buffer' (Penyangga Jitter)?", "id": "Penyimpan Data Pengurang Variasi Latensi." },
    { "en": "Apa Itu 'QoS' (Quality Of Service)?", "id": "Jaminan Kualitas Layanan Jaringan Data." },
    { "en": "Apa Itu 'Traffic Shaping' (Pembentukan Lalu Lintas)?", "id": "Pengaturan Laju Aliran Data Jaringan." },
    { "en": "Apa Itu 'Packet Filtering' (Penyaringan Paket)?", "id": "Penyaringan Data Berdasarkan Aturan Tertentu." },
    { "en": "Apa Itu 'DPI' (Deep Packet Inspection)?", "id": "Pemeriksaan Detail Isi Paket Data." },
    { "en": "Apa Itu 'Firewall Policy' (Kebijakan Tembok Api)?", "id": "Aturan Keamanan Pada Dinding Api." },
    { "en": "Apa Itu 'Encryption Key' (Kunci Enkripsi)?", "id": "Kunci Rahasia Untuk Mengacak Data." },
    { "en": "Apa Itu 'Digital Signature' (Tanda Tangan Digital)?", "id": "Verifikasi Keaslian Dokumen Elektronik Digital." },
    { "en": "Apa Itu 'PKI' (Public Key Infrastructure)?", "id": "Kerangka Kerja Keamanan Data Digital." },
    { "en": "Apa Itu 'SSL' (Secure Sockets Layer)?", "id": "Protokol Keamanan Koneksi Internet Lama." },
    { "en": "Apa Itu 'TLS' (Transport Layer Security)?", "id": "Protokol Keamanan Koneksi Internet Modern." },
    { "en": "Apa Itu 'VPS' (Virtual Private Server)?", "id": "Server Virtual Dalam Satu Fisik." },
    { "en": "Apa Itu 'Containerization' (Kontainerisasi)?", "id": "Pengemasan Aplikasi Beserta Lingkungan Kerjanya." },
    { "en": "Apa Itu 'Microservices' (Layanan Mikro)?", "id": "Arsitektur Pengembangan Aplikasi Terpisah Kecil." },
    { "en": "Apa Itu 'Load Balancer' (Penyeimbang Beban)?", "id": "Pembagi Beban Kerja Antar Server." },
    { "en": "Apa Itu 'CDN' (Content Delivery Network)?", "id": "Jaringan Server Pendistribusi Konten Cepat." },
    { "en": "Apa Itu 'Edge Gateway' (Gerbang Pinggir)?", "id": "Gerbang Penghubung Perangkat Ke Awan." },
    { "en": "Apa Itu 'Fog Computing' (Komputasi Kabut)?", "id": "Perluasan Komputasi Awan Ke Pinggir." },
    { "en": "Apa Itu 'IIoT' (Industrial Internet Of Things)?", "id": "Internet Benda Untuk Sektor Industri." },
    { "en": "Apa Itu 'Smart Factory' (Pabrik Cerdas)?", "id": "Pabrik Otomatis Berbasis Teknologi Digital." },
    { "en": "Apa Itu 'Digital Twin' (Kembaran Digital)?", "id": "Replika Digital Dari Aset Fisik." },
    { "en": "Apa Itu 'Predictive Maintenance' (Pemeliharaan Prediktif)?", "id": "Pemeliharaan Berdasarkan Prediksi Kerusakan Data." },
    { "en": "Apa Itu 'Condition Monitoring' (Pemantauan Kondisi)?", "id": "Pemantauan Kondisi Mesin Secara Terus-Menerus." },
    { "en": "Apa Itu 'Remote Monitoring' (Pemantauan Jarak Jauh)?", "id": "Pemantauan Jarak Jauh Melalui Jaringan." },
    { "en": "Apa Itu 'Telemetry' (Telemetri)?", "id": "Pengiriman Data Pengukuran Jarak Jauh." },
    { "en": "Apa Itu 'Supervisory Control' (Kontrol Pengawasan)?", "id": "Pengawasan Dan Pengendalian Tingkat Atas." },
    { "en": "Apa Itu 'Data Logging' (Pencatatan Data)?", "id": "Pencatatan Data Kejadian Secara Berurutan." },
    { "en": "Apa Itu 'Real Time Data' (Data Waktu Nyata)?", "id": "Data Yang Diproses Saat Terjadi." },
    { "en": "Apa Itu 'Historical Data' (Data Historis)?", "id": "Kumpulan Data Rekaman Masa Lalu." },
    { "en": "Apa Itu 'Data Analytics' (Analisis Data)?", "id": "Proses Analisis Data Untuk Informasi." },
    { "en": "Apa Itu 'Machine Vision' (Visi Mesin)?", "id": "Sistem Penglihatan Mesin Berbasis Kamera." },
    { "en": "Apa Itu 'Object Detection' (Deteksi Objek)?", "id": "Kemampuan Mesin Mendeteksi Keberadaan Objek." },
    { "en": "Apa Itu 'Image Processing' (Pengolahan Citra)?", "id": "Pengolahan Gambar Digital Secara Matematis." },
    { "en": "Apa Itu 'Pattern Recognition' (Pengenalan Pola)?", "id": "Pengenalan Pola Dalam Kumpulan Data." },
    { "en": "Apa Itu 'Speech Recognition' (Pengenalan Suara)?", "id": "Pengenalan Suara Manusia Oleh Mesin." },
    { "en": "Apa Itu 'NLP' (Natural Language Processing)?", "id": "Pengolahan Bahasa Alami Oleh Komputer." },
    { "en": "Apa Itu 'Robotics' (Robotika)?", "id": "Bidang Teknologi Perancangan Robot Otomatis." },
    { "en": "Apa Itu 'Cobot' (Collaborative Robot)?", "id": "Robot Pendamping Kerja Manusia Aman." },
    { "en": "Apa Itu 'Autonomous System' (Sistem Otonom)?", "id": "Sistem Mandiri Tanpa Intervensi Manusia." },
    { "en": "Apa Itu 'Humanoid Robot' (Robot Humanoid)?", "id": "Robot Dengan Bentuk Menyerupai Manusia." },
    { "en": "Apa Itu 'BMS' (Battery Management System)?", "id": "Sistem Pengelola, Pelindung Sel Baterai." },
    { "en": "Apa Itu 'SOC' (State Of Charge)?", "id": "Persentase Kapasitas Energi Baterai Tersisa." },
    { "en": "Apa Itu 'SOH' (State Of Health)?", "id": "Ukuran Kondisi Kesehatan Fisik Baterai." },
    { "en": "Apa Itu 'DOD' (Depth Of Discharge)?", "id": "Persentase Energi Baterai Yang Terpakai." },
    { "en": "Apa Itu 'BEV' (Battery Electric Vehicle)?", "id": "Kendaraan Listrik Berbasis Baterai Penuh." },
    { "en": "Apa Itu 'HEV' (Hybrid Electric Vehicle)?", "id": "Kendaraan Listrik Dengan Mesin Bakar." },
    { "en": "Apa Itu 'PHEV' (Plug-In Hybrid Electric Vehicle)?", "id": "Kendaraan Hibrida Dengan Pengisian Kabel." },
    { "en": "Apa Itu 'FCEV' (Fuel Cell Electric Vehicle)?", "id": "Kendaraan Listrik Berbahan Bakar Hidrogen." },
    { "en": "Apa Itu 'V2G' (Vehicle To Grid)?", "id": "Aliran Daya Kendaraan Ke Jaringan." },
    { "en": "Apa Itu 'V2H' (Vehicle To Home)?", "id": "Aliran Daya Kendaraan Ke Rumah." },
    { "en": "Apa Itu 'Smart Meter' (Meteran Cerdas)?", "id": "Alat Ukur Listrik Digital Interaktif." },
    { "en": "Apa Itu 'AMR' (Automatic Meter Reading)?", "id": "Pembacaan Meteran Jarak Jauh Otomatis." },
    { "en": "Apa Itu 'AMI' (Advanced Metering Infrastructure)?", "id": "Infrastruktur Pengukuran Listrik Dua Arah." },
    { "en": "Apa Itu 'Microgrid' (Jaringan Mikro)?", "id": "Jaringan Listrik Lokal Skala Kecil." },
    { "en": "Apa Itu 'ESS' (Energy Storage System)?", "id": "Sistem Penyimpanan Energi Skala Besar." },
    { "en": "Apa Itu 'Flywheel' (Roda Gila)?", "id": "Penyimpan Energi Mekanik Bentuk Putaran." },
    { "en": "Apa Itu 'PHS' (Pumped Hydro Storage)?", "id": "Penyimpanan Energi Melalui Elevasi Air." },
    { "en": "Apa Itu 'CAES' (Compressed Air Energy Storage)?", "id": "Penyimpanan Energi Udara Bertekanan Tinggi." },
    { "en": "Apa Itu 'Virtual Power Plant' (Pembangkit Virtual)?", "id": "Penggabungan Sumber Energi Terdistribusi Digital." },
    { "en": "Apa Itu 'Demand Response' (Respon Permintaan)?", "id": "Penyesuaian Beban Berdasarkan Kondisi Jaringan." },
    { "en": "Apa Itu 'Peak Shaving' (Pemangkasan Puncak)?", "id": "Pengurangan Beban Saat Puncak Pemakaian." },
    { "en": "Apa Itu 'Load Shifting' (Pergeseran Beban)?", "id": "Pemindahan Waktu Pemakaian Energi Listrik." },
    { "en": "Apa Itu 'Distributed Generation' (Pembangkit Terdistribusi)?", "id": "Sumber Energi Dekat Lokasi Konsumen." },
    { "en": "Apa Itu 'Modbus'?", "id": "Protokol Komunikasi Industri Serial Standar." },
    { "en": "Apa Itu 'Profibus' (Process Field Bus)?", "id": "Standar Jaringan Komunikasi Lapangan Industri." },
    { "en": "Apa Itu 'Profinet' (Process Field Net)?", "id": "Protokol Komunikasi Industri Berbasis Ethernet." },
    { "en": "Apa Itu 'EtherCAT' (Ethernet For Control Automation Technology)?", "id": "Protokol Ethernet Industri Sangat Cepat." },
    { "en": "Apa Itu 'CAN Bus' (Controller Area Network Bus)?", "id": "Protokol Komunikasi Internal Kendaraan Otomotif." },
    { "en": "Apa Itu 'BACnet' (Building Automation And Control Networks)?", "id": "Protokol Komunikasi Otomasi Gedung Pintar." },
    { "en": "Apa Itu 'KNX'?", "id": "Standar Dunia Untuk Kontrol Rumah." },
    { "en": "Apa Itu 'DALI' (Digital Addressable Lighting Interface)?", "id": "Protokol Kendali Lampu Digital Pintar." },
    { "en": "Apa Itu 'SCADA Master' (Master SCADA)?", "id": "Pusat Kendali Dan Pengolahan Data." },
    { "en": "Apa Itu 'RTU' (Remote Terminal Unit)?", "id": "Unit Terminal Komunikasi Jarak Jauh." },
    { "en": "Apa Itu 'MTU' (Master Terminal Unit)?", "id": "Unit Terminal Pusat Kendali Utama." },
    { "en": "Apa Itu 'Fieldbus'?", "id": "Jaringan Komunikasi Antar Perangkat Lapangan." },
    { "en": "Apa Itu 'Gateway' (Gerbang Jaringan)?", "id": "Penghubung Dua Protokol Komunikasi Berbeda." },
    { "en": "Apa Itu 'Switchgear' (Perangkat Hubung)?", "id": "Kumpulan Komponen Pemutus Arus Listrik." },
    { "en": "Apa Itu 'Load Break Switch' (Saklar Beban)?", "id": "Saklar Pemutus Saat Arus Beban." },
    { "en": "Apa Itu 'Disconnector' (Pemisah Listrik)?", "id": "Alat Pemutus Sirkuit Tanpa Beban." },
    { "en": "Apa Itu 'CT Ratio' (Rasio CT)?", "id": "Perbandingan Arus Primer Dan Sekunder." },
    { "en": "Apa Itu 'Burden' (Beban CT)?", "id": "Beban Impedansi Pada Sekunder CT." },
    { "en": "Apa Itu 'Accuracy Class' (Kelas Akurasi)?", "id": "Batas Kesalahan Pengukuran Alat Ukur." },
    { "en": "Apa Itu 'Instrument Security Factor' (Faktor Keamanan)?", "id": "Pembatas Arus Masuk Ke Meter." },
    { "en": "Apa Itu 'Knee Point Voltage' (Tegangan Knee)?", "id": "Batas Saturasi Magnetik Inti CT." },
    { "en": "Apa Itu 'Busbar Protection' (Proteksi Busbar)?", "id": "Sistem Pengaman Jalur Utama Panel." },
    { "en": "Apa Itu 'Breaker Failure Protection' (Proteksi Gagal Breaker)?", "id": "Pengaman Saat Pemutus Gagal Bekerja." },
    { "en": "Apa Itu 'Islanding' (Penyuasan Mandiri)?", "id": "Kondisi Pembangkit Terpisah Jala Utama." },
    { "en": "Apa Itu 'Anti-Islanding' (Anti-Penyuasan)?", "id": "Sistem Pencegah Operasi Pembangkit Mandiri." },
    { "en": "Apa Itu 'Power Swing' (Ayunan Daya)?", "id": "Variasi Aliran Daya Akibat Gangguan." },
    { "en": "Apa Itu 'Out Of Step' (Lepas Sinkron)?", "id": "Kondisi Generator Kehilangan Sinkronisasi Jaringan." },
    { "en": "Apa Itu 'Black Start' (Mulai Hitam)?", "id": "Menyalakan Pembangkit Tanpa Sumber Luar." },
    { "en": "Apa Itu 'Spinning Reserve' (Cadangan Putar)?", "id": "Kapasitas Generator Siap Beban Segera." },
    { "en": "Apa Itu 'Operating Reserve' (Cadangan Operasi)?", "id": "Total Kapasitas Daya Cadangan Sistem." },
    { "en": "Apa Itu 'Frequency Regulation' (Regulasi Frekuensi)?", "id": "Upaya Menjaga Kestabilan Frekuensi Jaringan." },
    { "en": "Apa Itu 'AGC' (Automatic Generation Control)?", "id": "Kendali Otomatis Output Daya Pembangkit." },
    { "en": "Apa Itu 'Tie-Line' (Jalur Ikat)?", "id": "Jalur Penghubung Antar Area Sistem." },
    { "en": "Apa Itu 'Area Control Error' (Kesalahan Kendali Area)?", "id": "Selisih Aliran Daya Wilayah Tertentu." },
    { "en": "Apa Itu 'Voltage Stability' (Stabilitas Tegangan)?", "id": "Kemampuan Menjaga Level Tegangan Stabil." },
    { "en": "Apa Itu 'Angle Stability' (Stabilitas Sudut)?", "id": "Kemampuan Rotor Tetap Sinkron Jaringan." },
    { "en": "Apa Itu 'Load shedding' (Pelepasan Beban)?", "id": "Pemutusan Beban Untuk Menjaga Frekuensi." },
    { "en": "Apa Itu 'Fault Current' (Arus Gangguan)?", "id": "Aliran Arus Saat Terjadi Kerusakan." },
    { "en": "Apa Itu 'Earth Fault Protection' (Proteksi Hubung Tanah)?", "id": "Pengaman Kebocoran Arus Ke Bumi." },
    { "en": "Apa Itu 'Arc Flash' (Kilatan Busur)?", "id": "Pelepasan Energi Cahaya Panas Listrik." },
    { "en": "Apa Itu 'Incident Energy' (Energi Insiden)?", "id": "Panas Hasil Ledakan Busur Api." },
    { "en": "Apa Itu 'PPE' (Personal Protective Equipment)?", "id": "Alat Pelindung Diri Kerja Listrik." },
    { "en": "Apa Itu 'LOTO' (Lock Out Tag Out)?", "id": "Prosedur Keamanan Isolasi Energi Berbahaya." },
    { "en": "Apa Itu 'Hot Work' (Pekerjaan Bertegangan)?", "id": "Pekerjaan Pada Sistem Listrik Aktif." },
    { "en": "Apa Itu 'Single Line Diagram' (Diagram Garis Tunggal)?", "id": "Gambaran Sederhana Sistem Tenaga Listrik." },
    { "en": "Apa Itu 'Wiring Diagram' (Diagram Pengabelan)?", "id": "Gambar Detail Koneksi Fisik Kabel." },
    { "en": "Apa Itu 'Schematic Diagram' (Diagram Skematik)?", "id": "Gambar Fungsi Logika Rangkaian Listrik." },
    { "en": "Apa Itu 'Logic Diagram' (Diagram Logika)?", "id": "Representasi Visual Alur Logika Digital." },
    { "en": "Apa Itu 'Ladder Diagram' (Diagram Tangga)?", "id": "Bahasa Pemrograman Visual Kontrol PLC." },
    { "en": "Apa Itu 'Function Block Diagram' (Diagram Blok Fungsi)?", "id": "Metode Pemrograman PLC Berbasis Blok." },
    { "en": "Apa Itu 'Sequential Function Chart' (Bagan Fungsi Sekuensial)?", "id": "Metode Pemrograman PLC Langkah Berurutan." },
    { "en": "Apa Itu 'Structured Text' (Teks Terstruktur)?", "id": "Bahasa Pemrograman PLC Berbasis Teks." },
    { "en": "Apa Itu 'Instruction List' (Daftar Instruksi)?", "id": "Bahasa Pemrograman PLC Tingkat Rendah." },
    { "en": "Apa Itu 'Human Machine Interface' (Antarmuka Manusia Mesin)?", "id": "Panel Monitor Kontrol Proses Industri." },
    { "en": "Apa Itu 'Industrial PC' (Komputer Industri)?", "id": "Komputer Tangguh Untuk Lingkungan Pabrik." },
    { "en": "Apa Itu 'Data Historian' (Penyimpan Sejarah)?", "id": "Database Khusus Penyimpan Data Proses." },
    { "en": "Apa Itu 'OPC' (Open Platform Communications)?", "id": "Standar Pertukaran Data Industri Terbuka." },
    { "en": "Apa Itu 'MQTT' (Message Queuing Telemetry Transport)?", "id": "Protokol Komunikasi Ringan Untuk IoT." },
    { "en": "Apa Itu 'CoAP' (Constrained Application Protocol)?", "id": "Protokol Web Untuk Perangkat Terbatas." },
    { "en": "Apa Itu 'LoRaWAN' (Long Range Wide Area Network)?", "id": "Protokol Jaringan Nirkabel Jarak Jauh." },
    { "en": "Apa Itu 'NB-IoT' (Narrowband Internet Of Things)?", "id": "Standar Komunikasi IoT Berbasis Seluler." },
    { "en": "Apa Itu 'Sigfox'?", "id": "Jaringan Komunikasi IoT Daya Rendah." },
    { "en": "Apa Itu 'Smart City' (Kota Cerdas)?", "id": "Integrasi Teknologi Digital Manajemen Kota." },
    { "en": "Apa Itu 'Smart Home' (Rumah Cerdas)?", "id": "Sistem Otomasi Peralatan Dalam Rumah." },
    { "en": "Apa Itu 'Home Automation' (Otomasi Rumah)?", "id": "Kendali Otomatis Lampu Dan Perangkat." },
    { "en": "Apa Itu 'Energy Audit' (Audit Energi)?", "id": "Proses Analisis Konsumsi Energi Bangunan." },
    { "en": "Apa Itu 'Energy Management System' (Sistem Manajemen Energi)?", "id": "Sistem Optimasi Penggunaan Daya Listrik." },
    { "en": "Apa Itu 'Power Quality Meter' (Meter Kualitas Daya)?", "id": "Alat Pantau Gangguan Gelombang Listrik." },
    { "en": "Apa Itu 'Harmonic Filter' (Filter Harmonisa)?", "id": "Alat Pengurang Gangguan Harmonisa Listrik." },
    { "en": "Apa Itu 'Passive Harmonic Filter' (Filter Harmonisa Pasif)?", "id": "Filter Menggunakan Komponen Pasif Saja." },
    { "en": "Apa Itu 'Active Harmonic Filter' (Filter Harmonisa Aktif)?", "id": "Filter Berbasis Elektronika Daya Aktif." },
    { "en": "Apa Itu 'Surge Protection Device' (Perangkat Perlindungan Surja)?", "id": "Komponen Pelindung Dari Tegangan Kejut." },
    { "en": "Apa Itu 'Isolation Monitor' (Monitor Isolasi)?", "id": "Alat Pantau Ketahanan Isolasi Sistem." },
    { "en": "Apa Itu 'Residual Current Monitor' (Monitor Arus Sisa)?", "id": "Alat Pantau Kebocoran Arus Kontinu." },
    { "en": "Apa Itu 'Fault Locator' (Pencari Gangguan)?", "id": "Alat Penentu Lokasi Kerusakan Kabel." },
    { "en": "Apa Itu 'Cable Fault' (Gangguan Kabel)?", "id": "Kondisi Kerusakan Pada Isolasi Kabel." },
    { "en": "Apa Itu 'Dielectric Strength' (Kekuatan Dielektrik)?", "id": "Kemampuan Bahan Menahan Tegangan Tinggi." },
    { "en": "Apa Itu 'FACTS' (Flexible Alternating Current Transmission Systems)?", "id": "Teknologi Kendali Daya Jaringan Transmisi." },
    { "en": "Apa Itu 'UPFC' (Unified Power Flow Controller)?", "id": "Alat Pengatur Aliran Daya Fleksibel." },
    { "en": "Apa Itu 'TCSC' (Thyristor Controlled Series Capacitor)?", "id": "Kapasitor Seri Terkendali Melalui Tiristor." },
    { "en": "Apa Itu 'SSSC' (Static Synchronous Series Compensator)?", "id": "Kompensator Seri Sinkron Berbasis Inverter." },
    { "en": "Apa Itu 'IPFC' (Interline Power Flow Controller)?", "id": "Pengatur Aliran Daya Antar Jalur." },
    { "en": "Apa Itu 'TCPS' (Thyristor Controlled Phase Shifter)?", "id": "Penggeser Fase Terkendali Melalui Tiristor." },
    { "en": "Apa Itu 'PFC' (Power Factor Correction)?", "id": "Proses Perbaikan Nilai Faktor Daya." },
    { "en": "Apa Itu 'HVDC Light' (High Voltage Direct Current Light)?", "id": "Sistem Transmisi Searah Kapasitas Kecil." },
    { "en": "Apa Itu 'BIPV' (Building Integrated Photovoltaics)?", "id": "Panel Surya Terintegrasi Struktur Bangunan." },
    { "en": "Apa Itu 'CSP' (Concentrated Solar Power)?", "id": "Pembangkit Listrik Tenaga Surya Konsentrasi." },
    { "en": "Apa Itu 'TSR' (Tip Speed Ratio)?", "id": "Rasio Kecepatan Ujung Bilah Turbin." },
    { "en": "Apa Itu 'Betz Limit' (Batas Betz)?", "id": "Efisiensi Teoritis Maksimum Turbin Angin." },
    { "en": "Apa Itu 'DNI' (Direct Normal Irradiance)?", "id": "Radiasi Matahari Langsung Tegak Lurus." },
    { "en": "Apa Itu 'GHI' (Global Horizontal Irradiance)?", "id": "Total Radiasi Matahari Permukaan Horizontal." },
    { "en": "Apa Itu 'Hazardous Area' (Area Berbahaya)?", "id": "Lokasi Berisiko Ledakan Gas, Debu." },
    { "en": "Apa Itu 'Intrinsically Safe' (Aman Secara Intrinsik)?", "id": "Sistem Energi Rendah Pencegah Ledakan." },
    { "en": "Apa Itu 'Flameproof' (Tahan Api)?", "id": "Selungkup Penahan Ledakan Internal Peralatan." },
    { "en": "Apa Itu 'Explosion Proof' (Tahan Ledakan)?", "id": "Peralatan Penahan Ledakan Atmosfer Luar." },
    { "en": "Apa Itu 'IP Rating' (Ingress Protection Rating)?", "id": "Standar Perlindungan Debu Dan Air." },
    { "en": "Apa Itu 'Flux Linkage' (Tautan Fluks)?", "id": "Total Fluks Magnetik Menembus Lilitan." },
    { "en": "Apa Itu 'Magnetostriction' (Magnetostriksi)?", "id": "Perubahan Bentuk Bahan Akibat Magnet." },
    { "en": "Apa Itu 'Barkhausen Noise' (Derau Barkhausen)?", "id": "Loncatan Magnetisasi Kecil Dalam Bahan." },
    { "en": "Apa Itu 'Kramer-Kronig Relation' (Hubungan Kramer-Kronig)?", "id": "Hubungan Matematis Refleksi Dan Absorpsi." },
    { "en": "Apa Itu 'Skin Depth' (Kedalaman Kulit)?", "id": "Jarak Penetrasi Arus Dalam Konduktor." },
    { "en": "Apa Itu 'Proximity Effect' (Efek Kedekatan)?", "id": "Distribusi Arus Akibat Konduktor Lain." },
    { "en": "Apa Itu 'Permittivity' (Permitivitas)?", "id": "Kemampuan Bahan Menyimpan Energi Listrik." },
    { "en": "Apa Itu 'Permeability' (Permeabilitas)?", "id": "Kemampuan Bahan Menghantarkan Fluks Magnetik." },
    { "en": "Apa Itu 'Susceptibility' (Suseptibilitas)?", "id": "Ukuran Respon Bahan Terhadap Medan." },
    { "en": "Apa Itu 'Reluctivity' (Reluktivitas)?", "id": "Kebalikan Dari Sifat Permeabilitas Bahan." },
    { "en": "Apa Itu 'Retentivity' (Retentivitas)?", "id": "Sisa Magnetisme Setelah Medan Hilang." },
    { "en": "Apa Itu 'Specific Resistance' (Hambatan Jenis)?", "id": "Hambatan Per Satuan Panjang Bahan." },
    { "en": "Apa Itu 'Temperature Coefficient' (Koefisien Suhu)?", "id": "Perubahan Nilai Hambatan Akibat Suhu." },
    { "en": "Apa Itu 'Load Profile' (Profil Beban)?", "id": "Grafik Konsumsi Listrik Terhadap Waktu." },
    { "en": "Apa Itu 'Demand Charge' (Biaya Beban)?", "id": "Biaya Berdasarkan Pemakaian Daya Puncak." },
    { "en": "Apa Itu 'Tariff' (Tarif Listrik)?", "id": "Struktur Harga Penjualan Energi Listrik." },
    { "en": "Apa Itu 'Time Of Use' (Waktu Penggunaan)?", "id": "Harga Listrik Berdasarkan Waktu Pemakaian." },
    { "en": "Apa Itu 'Feeder' (Penyulang)?", "id": "Kabel Distribusi Utama Dari Gardu." },
    { "en": "Apa Itu 'Ring Main Unit' (Unit Cincin Utama)?", "id": "Perangkat Hubung Distribusi Kabel Melingkar." },
    { "en": "Apa Itu 'Step-Down Substation' (Gardu Penurun Tegangan)?", "id": "Gardu Pengubah Tegangan Menjadi Rendah." },
    { "en": "Apa Itu 'Step-Up Substation' (Gardu Penaik Tegangan)?", "id": "Gardu Pengubah Tegangan Menjadi Tinggi." },
    { "en": "Apa Itu 'Gas Insulated Substation' (Gardu Terisolasi Gas)?", "id": "Gardu Kompak Menggunakan Gas SF6." },
    { "en": "Apa Itu 'Air Insulated Substation' (Gardu Terisolasi Udara)?", "id": "Gardu Terbuka Menggunakan Isolasi Udara." },
    { "en": "Apa Itu 'Busbar Trunking' (Saluran Busbar)?", "id": "Sistem Distribusi Daya Rel Tembaga." },
    { "en": "Apa Itu 'Current Rating' (Peringkat Arus)?", "id": "Kapasitas Arus Maksimum Operasi Aman." },
    { "en": "Apa Itu 'Short Circuit Rating' (Peringkat Hubung Singkat)?", "id": "Ketahanan Arus Gangguan Maksimum Alat." },
    { "en": "Apa Itu 'Utilization Factor' (Faktor Utilisasi)?", "id": "Rasio Beban Maksimum Terhadap Kapasitas." },
    { "en": "Apa Itu 'Plant Factor' (Faktor Pembangkit)?", "id": "Rasio Energi Hasil Terhadap Kapasitas." },
    { "en": "Apa Itu 'Capacity Factor' (Faktor Kapasitas)?", "id": "Rasio Output Rata-Rata Terhadap Maksimum." },
    { "en": "Apa Itu 'Reserve Margin' (Margin Cadangan)?", "id": "Kelebihan Kapasitas Di Atas Beban." },
    { "en": "Apa Itu 'Reliability' (Keandalan Sistem)?", "id": "Kemampuan Sistem Beroperasi Tanpa Gagal." },
    { "en": "Apa Itu 'SAIDI' (System Average Interruption Duration Index)?", "id": "Indeks Durasi Rata-Rata Gangguan Sistem." },
    { "en": "Apa Itu 'SAIFI' (System Average Interruption Frequency Index)?", "id": "Indeks Frekuensi Rata-Rata Gangguan Sistem." },
    { "en": "Apa Itu 'CAIDI' (Customer Average Interruption Duration Index)?", "id": "Durasi Rata-Rata Gangguan Per Pelanggan." },
    { "en": "Apa Itu 'Power Outage' (Padam Listrik)?", "id": "Kondisi Hilangnya Suplai Daya Listrik." },
    { "en": "Apa Itu 'Voltage Dip' (Kedipan Tegangan)?", "id": "Penurunan Tegangan Sesaat Skala Besar." },
    { "en": "Apa Itu 'Sag' (Penurunan Tegangan)?", "id": "Penurunan Tegangan Dalam Durasi Singkat." },
    { "en": "Apa Itu 'Swell' (Kenaikan Tegangan)?", "id": "Kenaikan Tegangan Dalam Durasi Singkat." },
    { "en": "Apa Itu 'Total Harmonic Distortion' (Distorsi Harmonisa Total)?", "id": "Persentase Kandungan Harmonisa Dalam Sinyal." },
    { "en": "Apa Itu 'Interharmonic' (Antarharmonisa)?", "id": "Frekuensi Bukan Kelipatan Bulat Dasar." },
    { "en": "Apa Itu 'Notching' (Takikan Tegangan)?", "id": "Gangguan Tegangan Akibat Pensakelaran Komutasi." },
    { "en": "Apa Itu 'Noise' (Derau)?", "id": "Sinyal Listrik Tidak Diinginkan, Acak." },
    { "en": "Apa Itu 'Unbalance' (Ketidakseimbangan)?", "id": "Perbedaan Nilai Tegangan Antar Fase." },
    { "en": "Apa Itu 'DC Offset' (Ofset Searah)?", "id": "Keberadaan Komponen Searah Sinyal Bolak-Balik." },
    { "en": "Apa Itu 'Voltage Deviation' (Deviasi Tegangan)?", "id": "Penyimpangan Tegangan Dari Nilai Nominal." },
    { "en": "Apa Itu 'Frequency Deviation' (Deviasi Frekuensi)?", "id": "Penyimpangan Frekuensi Dari Nilai Standar." },
    { "en": "Apa Itu 'Phase Jump' (Loncatan Fase)?", "id": "Perubahan Sudut Fase Secara Tiba-Tiba." },
    { "en": "Apa Itu 'Electromagnetic Compatibility' (Kompatibilitas Elektromagnetik)?", "id": "Kemampuan Perangkat Berdampingan Tanpa Gangguan." },
    { "en": "Apa Itu 'Earthing Resistance' (Tahanan Pentanahan)?", "id": "Nilai Hambatan Jalur Menuju Bumi." },
    { "en": "Apa Itu 'Touch Potential' (Potensial Sentuh)?", "id": "Tegangan Antara Objek Dan Kaki." },
    { "en": "Apa Itu 'Step Potential' (Potensial Langkah)?", "id": "Tegangan Antara Dua Kaki Terbuka." },
    { "en": "Apa Itu 'Mesh Potential' (Potensial Jala)?", "id": "Tegangan Maksimum Dalam Jaring Arde." },
    { "en": "Apa Itu 'Counterpoise' (Kawat Pembumian)?", "id": "Kawat Tanam Sejajar Jalur Transmisi." },
    { "en": "Apa Itu 'Bonding' (Ikatan Penyatuan)?", "id": "Penghubung Logam Untuk Menyamakan Potensial." },
    { "en": "Apa Itu 'Static Discharge' (Pelepasan Statis)?", "id": "Aliran Listrik Statis Ke Tanah." },
    { "en": "Apa Itu 'Lightning Strike' (Sambaran Petir)?", "id": "Pelepasan Listrik Atmosfer Ke Bumi." },
    { "en": "Apa Itu 'Isokeraunic Level' (Tingkat Isokeraunik)?", "id": "Jumlah Hari Petir Per Tahun." },
    { "en": "Apa Itu 'Rolling Sphere Method' (Metode Bola Bergulir)?", "id": "Cara Menentukan Area Proteksi Petir." },
    { "en": "Apa Itu 'Faraday Cage' (Sangkar Faraday)?", "id": "Ruang Logam Pelindung Medan Listrik." },
    { "en": "Apa Itu 'Shielding Angle' (Sudut Lindung)?", "id": "Sudut Proteksi Kawat Tanah Transmisi." },
    { "en": "Apa Itu 'Arc Horn' (Tanduk Busur)?", "id": "Logam Pelindung Isolator Dari Kilatan." },
    { "en": "Apa Itu 'Grading Ring' (Cincin Gradasi)?", "id": "Alat Perata Distribusi Tegangan Isolator." },
    { "en": "Apa Itu 'Corona Ring' (Cincin Korona)?", "id": "Alat Pengurang Efek Korona Tegangan." },
    { "en": "Apa Itu 'Stockbridge Damper' (Peredam Stockbridge)?", "id": "Alat Peredam Getaran Kabel Transmisi." },
    { "en": "Apa Itu 'Spacer' (Pemisah Kabel)?", "id": "Alat Menjaga Jarak Antar Penghantar." },
    { "en": "Apa Itu 'Bundle Conductor' (Penghantar Berkas)?", "id": "Penggunaan Beberapa Kabel Per Fase." },
    { "en": "Apa Itu 'Sag Template' (Templat Andongan)?", "id": "Alat Bantu Desain Profil Kabel." },
    { "en": "Apa Itu 'Tensioner' (Alat Penarik)?", "id": "Mesin Penarik Kabel Saluran Udara." },
    { "en": "Apa Itu 'Puller' (Mesin Penarik)?", "id": "Alat Penarik Kabel Melalui Pipa." },
    { "en": "Apa Itu 'Cable Drum' (Has pel Kabel)?", "id": "Gulungan Besar Tempat Penyimpanan Kabel." },
    { "en": "Apa Itu 'Trench' (Parit Kabel)?", "id": "Galian Tanah Tempat Penanaman Kabel." },
    { "en": "Apa Itu 'Duct Bank' (Deretan Pipa)?", "id": "Kumpulan Pipa Kabel Dalam Beton." },
    { "en": "Apa Itu 'Joint Pit' (Lubang Sambung)?", "id": "Ruang Untuk Melakukan Penyambungan Kabel." },
    { "en": "Apa Itu 'Cable Jointing' (Penyambungan Kabel)?", "id": "Proses Menghubungkan Dua Ujung Kabel." },
    { "en": "Apa Itu 'Termination' (Terminasi Kabel)?", "id": "Penyambungan Ujung Kabel Ke Peralatan." },
    { "en": "Apa Itu 'Cleat' (Penjepit Kabel)?", "id": "Alat Pengikat Kabel Pada Struktur." },
    { "en": "Apa Itu 'Bending Radius' (Radius Tekuk)?", "id": "Batas Lengkungan Minimum Kabel Listrik." },
    { "en": "Apa Itu 'Pulling Tension' (Tegangan Tarik)?", "id": "Gaya Tarik Saat Pemasangan Kabel." },
    { "en": "Apa Itu 'Sidewall Pressure' (Tekanan Dinding)?", "id": "Tekanan Kabel Pada Tikungan Pipa." },
    { "en": "Apa Itu 'Fill Ratio' (Rasio Isi)?", "id": "Persentase Luas Kabel Dalam Pipa." },
    { "en": "Apa Itu 'Derating Factor' (Faktor Penurunan)?", "id": "Koreksi Kapasitas Arus Akibat Suhu." },
    { "en": "Apa Itu 'Ah' (Ampere Hour)?", "id": "Kapasitas Total Penyimpanan Arus Baterai." },
    { "en": "Apa Itu 'Wh' (Watt Hour)?", "id": "Total Energi Listrik Yang Digunakan." },
    { "en": "Apa Itu 'VAR' (Volt Ampere Reactive)?", "id": "Satuan Ukuran Untuk Daya Reaktif." },
    { "en": "Apa Itu 'VA' (Volt Ampere)?", "id": "Satuan Ukuran Untuk Daya Semu." },
    { "en": "Apa Itu 'RMS' (Root Mean Square)?", "id": "Nilai Efektif Tegangan Arus Bolak-Balik." },
    { "en": "Apa Itu 'Peak To Peak Voltage'?", "id": "Selisih Tegangan Puncak Atas, Bawah." },
    { "en": "Apa Itu 'Average Voltage' (Tegangan Rata-Rata)?", "id": "Nilai Rata-Rata Satu Siklus Gelombang." },
    { "en": "Apa Itu 'Phase Angle' (Sudut Fase)?", "id": "Perbedaan Waktu Antar Dua Gelombang." },
    { "en": "Apa Itu 'Power Factor' (Faktor Daya)?", "id": "Perbandingan Daya Aktif Dan Semu." },
    { "en": "Apa Itu 'Lagging Current' (Arus Tertinggal)?", "id": "Arus Tertinggal Dari Tegangan Induktif." },
    { "en": "Apa Itu 'Leading Current' (Arus Mendahului)?", "id": "Arus Mendahului Tegangan Beban Kapasitif." },
    { "en": "Apa Itu 'Eddy Current' (Arus Eddy)?", "id": "Arus Induksi Merugi Dalam Inti." },
    { "en": "Apa Itu 'Hysteresis Loss' (Rugi Histeresis)?", "id": "Energi Hilang Akibat Magnetisasi Bolak-Balik." },
    { "en": "Apa Itu 'Copper Loss' (Rugi Tembaga)?", "id": "Panas Akibat Hambatan Kawat Penghantar." },
    { "en": "Apa Itu 'Iron Loss' (Rugi Besi)?", "id": "Total Kerugian Energi Inti Magnetik." },
    { "en": "Apa Itu 'No Load Test' (Uji Tanpa Beban)?", "id": "Pengujian Alat Listrik Tanpa Terhubung." },
    { "en": "Apa Itu 'Short Circuit Test' (Uji Hubung Singkat)?", "id": "Pengujian Ketahanan Arus Maksimum Alat." },
    { "en": "Apa Itu 'Voltage Regulation' (Regulasi Tegangan)?", "id": "Kemampuan Menjaga Tegangan Tetap Konstan." },
    { "en": "Apa Itu 'Efficiency' (Efisiensi Alat)?", "id": "Rasio Output Terhadap Input Daya." },
    { "en": "Apa Itu 'Saturation' (Saturasi Inti)?", "id": "Kondisi Magnetik Maksimal Inti Besi." },
    { "en": "Apa Itu 'Permeability' (Permeabilitas Magnetik)?", "id": "Kemampuan Bahan Menghantarkan Fluks Magnetik." },
    { "en": "Apa Itu 'Reluctance' (Reluktansi Magnetik)?", "id": "Hambatan Terhadap Jalur Fluks Magnet." },
    { "en": "Apa Itu 'Magnetomotive Force' (Gaya Gerak Magnet)?", "id": "Penyebab Timbulnya Fluks Dalam Inti." },
    { "en": "Apa Itu 'Self Inductance' (Induktansi Diri)?", "id": "Induksi Tegangan Pada Kumparan Sendiri." },
    { "en": "Apa Itu 'Mutual Inductance' (Induktansi Bersama)?", "id": "Induksi Tegangan Antar Dua Kumparan." },
    { "en": "Apa Itu 'Coupling Coefficient' (Koefisien Gandeng)?", "id": "Tingkat Hubungan Fluks Antar Lilitan." },
    { "en": "Apa Itu 'Step-Up Transformer' (Transformator Penaik)?", "id": "Alat Peningkat Level Tegangan Listrik." },
    { "en": "Apa Itu 'Step-Down Transformer' (Transformator Penurun)?", "id": "Alat Penurun Level Tegangan Listrik." },
    { "en": "Apa Itu 'Isolation Transformer' (Transformator Isolasi)?", "id": "Transformator Pemisah Sirkuit Masuk, Keluar." },
    { "en": "Apa Itu 'Auto Transformer' (Auto Transformator)?", "id": "Transformator Dengan Satu Lilitan Bersama." },
    { "en": "Apa Itu 'Instrument Transformer' (Transformator Instrumen)?", "id": "Transformator Khusus Untuk Alat Ukur." },
    { "en": "Apa Itu 'CT' (Current Transformer)?", "id": "Transformator Pengubah Arus Skala Besar." },
    { "en": "Apa Itu 'PT' (Potential Transformer)?", "id": "Transformator Pengubah Tegangan Skala Besar." },
    { "en": "Apa Itu 'Burden' (Beban Instrumen)?", "id": "Impedansi Eksternal Pada Sekunder Transformator." },
    { "en": "Apa Itu 'Polarity Test' (Uji Polaritas)?", "id": "Penentuan Arah Aliran Arus Lilitan." },
    { "en": "Apa Itu 'Vector Group' (Grup Vektor)?", "id": "Konfigurasi Hubungan Transformator Tiga Fase." },
    { "en": "Apa Itu 'Delta Connection' (Hubungan Delta)?", "id": "Koneksi Tiga Fase Tanpa Netral." },
    { "en": "Apa Itu 'Phase Voltage' (Tegangan Fase)?", "id": "Beda Potensial Antara Fase, Netral." },
    { "en": "Apa Itu 'Line Voltage' (Tegangan Saluran)?", "id": "Beda Potensial Antara Dua Fase." },
    { "en": "Apa Itu 'Balanced Load' (Beban Seimbang)?", "id": "Beban Sama Besar Tiap Fase." },
    { "en": "Apa Itu 'Neutral Current' (Arus Netral)?", "id": "Arus Akibat Ketidakseimbangan Beban Fase." },
    { "en": "Apa Itu 'Floating Neutral' (Netral Mengambang)?", "id": "Kondisi Titik Netral Tidak Terbumikan." },
    { "en": "Apa Itu 'Earth Fault' (Gangguan Tanah)?", "id": "Kebocoran Arus Menuju Bumi Langsung." },
    { "en": "Apa Itu 'Short Circuit' (Hubung Singkat)?", "id": "Kontak Langsung Antar Jalur Listrik." },
    { "en": "Apa Itu 'Open Circuit' (Rangkaian Terbuka)?", "id": "Jalur Listrik Yang Terputus Total." },
    { "en": "Apa Itu 'Protective Relay' (Relay Proteksi)?", "id": "Alat Pendeteksi Gangguan Sistem Listrik." },
    { "en": "Apa Itu 'Circuit Breaker' (Pemutus Sirkuit)?", "id": "Saklar Otomatis Pemutus Arus Gangguan." },
    { "en": "Apa Itu 'Fuse' (Sekring)?", "id": "Kawat Pemutus Arus Berlebih Sesaat." },
    { "en": "Apa Itu 'Isolator' (Saklar Pemisah)?", "id": "Alat Pemutus Sirkuit Tanpa Beban." },
    { "en": "Apa Itu 'Busbar' (Rel Tembaga)?", "id": "Batang Logam Penghantar Arus Besar." },
    { "en": "Apa Itu 'Feeder' (Penyulang)?", "id": "Jalur Distribusi Dari Gardu Induk." },
    { "en": "Apa Itu 'Substation' (Gardu Induk)?", "id": "Pusat Pengaturan Dan Transformasi Tegangan." },
    { "en": "Apa Itu 'Grid' (Jaringan Listrik)?", "id": "Sistem Interkoneksi Pembangkit Dan Beban." },
    { "en": "Apa Itu 'Load Shedding' (Pelepasan Beban)?", "id": "Pemutusan Beban Darurat Jaga Frekuensi." },
    { "en": "Apa Itu 'Black Start' (Mulai Hitam)?", "id": "Proses Menyalakan Pembangkit Secara Mandiri." },
    { "en": "Apa Itu 'AVR' (Automatic Voltage Regulator)?", "id": "Sistem Penstabil Tegangan Keluaran Generator." },
    { "en": "Apa Itu 'Exciter' (Pembangkit Eksitasi)?", "id": "Sumber Arus Magnet Untuk Generator." },
    { "en": "Apa Itu 'Synchronous Machine' (Mesin Sinkron)?", "id": "Mesin Listrik Berputar Kecepatan Tetap." },
    { "en": "Apa Itu 'Induction Machine' (Mesin Induksi)?", "id": "Mesin Listrik Berdasarkan Medan Putar." },
    { "en": "Apa Itu 'Slip' (Slip Motor)?", "id": "Selisih Kecepatan Medan Dan Rotor." },
    { "en": "Apa Itu 'Torque' (Torsi)?", "id": "Gaya Putar Pada Poros Mesin." },
    { "en": "Apa Itu 'Starting Current' (Arus Awal)?", "id": "Lonjakan Arus Saat Motor Dinyalakan." },
    { "en": "Apa Itu 'Soft Starter' (Starter Lembut)?", "id": "Alat Pengurang Lonjakan Arus Motor." },
    { "en": "Apa Itu 'Servo Motor' (Motor Servo)?", "id": "Motor Dengan Kendali Posisi Presisi." },
    { "en": "Apa Itu 'Stepper Motor' (Motor Langkah)?", "id": "Motor Bergerak Dalam Sudut Bertahap." },
    { "en": "Apa Itu 'THD' (Total Harmonic Distortion)?", "id": "Persentase Total Distorsi Gelombang Listrik." },
    { "en": "Apa Itu 'Surge' (Surja)?", "id": "Lonjakan Tegangan Tinggi Waktu Singkat." },
    { "en": "Apa Itu 'Transient' (Transien)?", "id": "Perubahan Status Energi Secara Mendadak." },
    { "en": "Apa Itu 'Grounding' (Pentanahan)?", "id": "Sistem Pembuangan Arus Ke Bumi." },
    { "en": "Apa Itu 'Bonding' (Ikatan Logam)?", "id": "Penyambungan Logam Untuk Menyamakan Potensial." },
    { "en": "Apa Itu 'Dielectric' (Dielektrik)?", "id": "Bahan Non-Konduktor Penyimpan Medan Listrik." },
    { "en": "Apa Itu 'Semiconductor' (Semikonduktor)?", "id": "Bahan Dengan Konduktivitas Listrik Menengah." },
    { "en": "Apa Itu 'P-Type' (Tipe-P) Semiconductor?", "id": "Semikonduktor Dengan Kelebihan Lubang Positif." },
    { "en": "Apa Itu 'N-Type' (Tipe-N) Semiconductor?", "id": "Semikonduktor Dengan Kelebihan Elektron Negatif." },
    { "en": "Apa Itu 'PN Junction' (Sambungan PN)?", "id": "Pertemuan Bahan Semikonduktor Tipe-P, Tipe-N." },
    { "en": "Apa Itu 'Anode' (Anoda)?", "id": "Terminal Positif Pada Komponen Elektronika." },
    { "en": "Apa Itu 'Cathode' (Katoda)?", "id": "Terminal Negatif Pada Komponen Elektronika." },
    { "en": "Apa Itu 'Bias' (Prategangan)?", "id": "Pemberian Tegangan Kerja Komponen Aktif." },
    { "en": "Apa Itu 'Rectification' (Penyearahan)?", "id": "Proses Pengubahan AC Menjadi DC." },
    { "en": "Apa Itu 'Inversion' (Inversi Daya)?", "id": "Proses Pengubahan DC Menjadi AC." },
    { "en": "Apa Itu 'Switching' (Pensakelaran)?", "id": "Proses Pemutusan Sambungan Secara Cepat." },
    { "en": "Apa Itu 'Duty Cycle' (Siklus Kerja)?", "id": "Rasio Waktu Aktif Terhadap Periode." },
    { "en": "Apa Itu 'PWM' (Pulse Width Modulation)?", "id": "Modulasi Lebar Pulsa Untuk Kendali." },
    { "en": "Apa Itu 'Sampling' (Pencuplikan)?", "id": "Proses Pengambilan Data Sinyal Analog." },
    { "en": "Apa Itu 'Quantization' (Kuantisasi)?", "id": "Pembulatan Nilai Sinyal Ke Digital." },
    { "en": "Apa Itu 'Aliasing' (Aliasing Sinyal)?", "id": "Distorsi Akibat Laju Sampling Rendah." },
    { "en": "Apa Itu 'Bandwidth' (Lebar Pita)?", "id": "Rentang Frekuensi Operasi Suatu Sistem." },
    { "en": "Apa Itu 'Gain' (Penguatan Sinyal)?", "id": "Rasio Output Terhadap Input Rangkaian." },
    { "en": "Apa Itu 'Noise' (Derau)?", "id": "Sinyal Pengganggu Tidak Diinginkan, Acak." },
    { "en": "Apa Itu 'Resistor Shunt'?", "id": "Hambatan Paralel Pengukur Arus Besar." },
    { "en": "Apa Itu 'Galvanometer'?", "id": "Alat Deteksi Arus Listrik Kecil." },
    { "en": "Apa Itu 'Potensiometer'?", "id": "Resistor Variabel Tiga Terminal Putar." },
    { "en": "Apa Itu 'Rheostat'?", "id": "Resistor Variabel Pengatur Arus Listrik." },
    { "en": "Apa Itu 'Wheatstone Bridge' (Jembatan Wheatstone)?", "id": "Sirkuit Pengukur Hambatan Sangat Teliti." },
    { "en": "Apa Itu 'Digital Multimeter' (Multimeter Digital)?", "id": "Alat Ukur Listrik Tampilan Angka." },
    { "en": "Apa Itu 'CRO' (Cathode Ray Oscilloscope)?", "id": "Alat Visualisasi Sinyal Elektronik Tabung." },
    { "en": "Apa Itu 'Function Generator' (Generator Fungsi)?", "id": "Pembangkit Berbagai Bentuk Gelombang Listrik." },
    { "en": "Apa Itu 'Passive Probe' (Probe Pasif)?", "id": "Kabel Penghubung Sinyal Ke Osiloskop." },
    { "en": "Apa Itu 'Spectrum Analyzer' (Penganalisis Spektrum)?", "id": "Alat Analisis Frekuensi Sinyal Radio." },
    { "en": "Apa Itu 'Logic Probe' (Probe Logika)?", "id": "Alat Cek Status Logika Digital." },
    { "en": "Apa Itu 'Wattmeter'?", "id": "Alat Ukur Konsumsi Daya Aktif." },
    { "en": "Apa Itu 'Energy Meter' (Meteran Energi)?", "id": "Alat Ukur Total Konsumsi Energi." },
    { "en": "Apa Itu 'Megger' (Insulation Tester)?", "id": "Alat Ukur Tahanan Isolasi Kabel." },
    { "en": "Apa Itu 'Earth Tester' (Penguji Tanah)?", "id": "Alat Ukur Resistansi Pembumian Listrik." },
    { "en": "Apa Itu 'Clamp Meter' (Tang Ampere)?", "id": "Alat Ukur Arus Tanpa Memutus." },
    { "en": "Apa Itu 'Frequency Meter' (Meter Frekuensi)?", "id": "Alat Ukur Jumlah Siklus Detik." },
    { "en": "Apa Itu 'Phase Meter' (Meter Fase)?", "id": "Alat Ukur Perbedaan Sudut Fase." },
    { "en": "Apa Itu 'LCR Meter' (Inductance Capacitance Resistance Meter)?", "id": "Alat Ukur Induktansi, Kapasitansi, Hambatan." },
    { "en": "Apa Itu 'Signal Tracer' (Penelusur Sinyal)?", "id": "Alat Pelacak Kerusakan Jalur Sinyal." },
    { "en": "Apa Itu 'Capacitance Meter' (Meter Kapasitansi)?", "id": "Alat Ukur Nilai Kapasitas Kapasitor." },
    { "en": "Apa Itu 'Sound Level Meter' (Meter Tingkat Suara)?", "id": "Alat Ukur Intensitas Kebisingan Suara." },
    { "en": "Apa Itu 'Lux Meter' (Meter Cahaya)?", "id": "Alat Ukur Intensitas Cahaya Ruangan." },
    { "en": "Apa Itu 'Tachometer'?", "id": "Alat Ukur Kecepatan Putaran Mesin." },
    { "en": "Apa Itu 'Thermometer Digital' (Termometer Digital)?", "id": "Alat Ukur Suhu Tampilan Digital." },
    { "en": "Apa Itu 'Pyrometer'?", "id": "Alat Ukur Suhu Tinggi Jarak-Jauh." },
    { "en": "Apa Itu 'Hygrometer'?", "id": "Alat Ukur Kelembapan Udara Sekitar." },
    { "en": "Apa Itu 'Anemometer'?", "id": "Alat Ukur Kecepatan Aliran Angin." },
    { "en": "Apa Itu 'Flow Meter'?", "id": "Alat Ukur Laju Aliran Fluida." },
    { "en": "Apa Itu 'Manometer'?", "id": "Alat Ukur Tekanan Fluida Gas." },
    { "en": "Apa Itu 'Barometer'?", "id": "Alat Ukur Tekanan Udara Atmosfer." },
    { "en": "Apa Itu 'Voltmeter'?", "id": "Alat Ukur Beda Potensial Listrik." },
    { "en": "Apa Itu 'Ammeter' (Amperemeter)?", "id": "Alat Ukur Kuat Arus Listrik." },
    { "en": "Apa Itu 'Ohmmeter'?", "id": "Alat Ukur Nilai Hambatan Listrik." },
    { "en": "Apa Itu 'Power Quality Analyzer' (Penganalisis Kualitas Daya)?", "id": "Alat Ukur Gangguan Listrik Industri." },
    { "en": "Apa Itu 'TDR' (Time Domain Reflectometer)?", "id": "Alat Deteksi Titik Putus Kabel." },
    { "en": "Apa Itu 'Optical Power Meter' (Meter Daya Optik)?", "id": "Alat Ukur Kekuatan Cahaya Fiber." },
    { "en": "Apa Itu 'OTDR' (Optical Time Domain Reflectometer)?", "id": "Alat Analisis Kerusakan Kabel Optik." },
    { "en": "Apa Itu 'Logic Analyzer' (Penganalisis Logika)?", "id": "Alat Pantau Banyak Sinyal Digital." },
    { "en": "Apa Itu 'Network Analyzer' (Penganalisis Jaringan)?", "id": "Alat Ukur Parameter Sinyal Radio." },
    { "en": "Apa Itu 'Distortion Meter' (Meter Distorsi)?", "id": "Alat Ukur Ketidakmurnian Sinyal Audio." },
    { "en": "Apa Itu 'Bridge Rectifier' (Penyearah Jembatan)?", "id": "Rangkaian Empat Dioda Penyearah Penuh." },
    { "en": "Apa Itu 'Zener Diode' (Dioda Zener)?", "id": "Dioda Pengatur Tegangan Tetap Stabil." },
    { "en": "Apa Itu 'Varactor Diode' (Dioda Varaktor)?", "id": "Dioda Dengan Kapasitansi Variabel Tegangan." },
    { "en": "Apa Itu 'Schottky Diode' (Dioda Schottky)?", "id": "Dioda Pensakelaran Cepat, Tegangan Rendah." },
    { "en": "Apa Itu 'Tunnel Diode' (Dioda Terowongan)?", "id": "Dioda Kecepatan Tinggi Efek Kuantum." },
    { "en": "Apa Itu 'Photodiode' (Fotodioda)?", "id": "Dioda Sensitif Terhadap Cahaya Masuk." },
    { "en": "Apa Itu 'LED' (Light Emitting Diode)?", "id": "Dioda Pemancar Cahaya Arus Listrik." },
    { "en": "Apa Itu 'Laser Diode' (Dioda Laser)?", "id": "Dioda Pemancar Cahaya Koheren Fokus." },
    { "en": "Apa Itu 'Step Recovery Diode' (Dioda Step Recovery)?", "id": "Dioda Pembangkit Pulsa Sangat Tajam." },
    { "en": "Apa Itu 'PIN Diode' (Positive Intrinsic Negative Diode)?", "id": "Dioda Pengendali Sinyal Frekuensi Tinggi." },
    { "en": "Apa Itu 'Gunn Diode' (Dioda Gunn)?", "id": "Dioda Pembangkit Gelombang Mikro Aktif." },
    { "en": "Apa Itu 'Impatt Diode' (Impact Ionization Avalanche Transit-Time Diode)?", "id": "Dioda Pembangkit Daya Gelombang Mikro." },
    { "en": "Apa Itu 'BJT' (Bipolar Junction Transistor)?", "id": "Transistor Pengendali Arus Dua Kutub." },
    { "en": "Apa Itu 'FET' (Field Effect Transistor)?", "id": "Transistor Pengendali Tegangan Medan Listrik." },
    { "en": "Apa Itu 'UJT' (Uni-Junction Transistor)?", "id": "Transistor Satu Sambungan Pemicu Pulsa." },
    { "en": "Apa Itu 'Darlington Pair' (Pasangan Darlington)?", "id": "Dua Transistor Penguatan Arus Tinggi." },
    { "en": "Apa Itu 'Phototransistor' (Fototransistor)?", "id": "Transistor Aktif Melalui Intensitas Cahaya." },
    { "en": "Apa Itu 'Optocoupler'?", "id": "Isolator Sinyal Berbasis Cahaya Internal." },
    { "en": "Apa Itu 'SCR' (Silicon Controlled Rectifier)?", "id": "Dioda Terkendali Untuk Saklar Daya." },
    { "en": "Apa Itu 'TRIAC' (Triode For Alternating Current)?", "id": "Saklar Elektronik Dua Arah AC." },
    { "en": "Apa Itu 'DIAC' (Diode For Alternating Current)?", "id": "Dioda Pemicu Saklar Arus Bolak-Balik." },
    { "en": "Apa Itu 'IGBT' (Insulated Gate Bipolar Transistor)?", "id": "Transistor Daya Tinggi Efisiensi Cepat." },
    { "en": "Apa Itu 'MOSFET' (Metal Oxide Semiconductor Field Effect Transistor)?", "id": "Transistor Efek Medan Gerbang Terisolasi." },
    { "en": "Apa Itu 'Varistor'?", "id": "Hambatan Pelindung Dari Lonjakan Tegangan." },
    { "en": "Apa Itu 'NTC' (Negative Temperature Coefficient)?", "id": "Hambatan Turun Saat Suhu Naik." },
    { "en": "Apa Itu 'PTC' (Positive Temperature Coefficient)?", "id": "Hambatan Naik Saat Suhu Naik." },
    { "en": "Apa Itu 'LDR' (Light Dependent Resistor)?", "id": "Hambatan Berubah Sesuai Intensitas Cahaya." },
    { "en": "Apa Itu 'Thermistor'?", "id": "Resistor Sensitif Terhadap Perubahan Suhu." },
    { "en": "Apa Itu 'Trimmer'?", "id": "Kapasitor Atau Resistor Variabel Kecil." },
    { "en": "Apa Itu 'Ferrite Bead'?", "id": "Komponen Penekan Gangguan Frekuensi Tinggi." },
    { "en": "Apa Itu 'Choke' (Kumparan Cekik)?", "id": "Induktor Penghambat Arus Frekuensi Tinggi." },
    { "en": "Apa Itu 'Solenoid'?", "id": "Kumparan Pembangkit Gaya Gerak Mekanik." },
    { "en": "Apa Itu 'SSR' (Solid State Relay)?", "id": "Relay Elektronik Tanpa Kontak Mekanik." },
    { "en": "Apa Itu 'Reed Switch' (Saklar Buluh)?", "id": "Saklar Aktif Melalui Medan Magnet." },
    { "en": "Apa Itu 'Buzzer'?", "id": "Komponen Penghasil Bunyi Peringatan Listrik." },
    { "en": "Apa Itu 'Crystal Oscillator' (Osilator Kristal)?", "id": "Pembangkit Frekuensi Detak Sangat Stabil." },
    { "en": "Apa Itu 'Heat Sink' (Pendingin)?", "id": "Logam Pembuang Panas Komponen Elektronika." },
    { "en": "Apa Itu 'Thermal Paste'?", "id": "Cairan Penghantar Panas Ke Pendingin." },
    { "en": "Apa Itu 'PCB' (Printed Circuit Board)?", "id": "Papan Jalur Penghubung Komponen Elektronik." },
    { "en": "Apa Itu 'Solder'?", "id": "Paduan Logam Penyambung Komponen Listrik." },
    { "en": "Apa Itu 'Breadboard'?", "id": "Papan Percobaan Rangkaian Tanpa Solder." },
    { "en": "Apa Itu 'Jumper Wire' (Kabel Jumper)?", "id": "Kabel Penghubung Antar Titik Rangkaian." },
    { "en": "Apa Itu 'IC' (Integrated Circuit)?", "id": "Kumpulan Komponen Dalam Chip Tunggal." },
    { "en": "Apa Itu 'Microcontroller' (Mikrokontroler)?", "id": "Komputer Kecil Dalam Satu Chip." },
    { "en": "Apa Itu 'Microprocessor' (Mikroprosesor)?", "id": "Unit Pemroses Pusat Data Komputer." },
    { "en": "Apa Itu 'RAM' (Random Access Memory)?", "id": "Memori Penyimpanan Data Sementara Cepat." },
    { "en": "Apa Itu 'ROM' (Read Only Memory)?", "id": "Memori Penyimpanan Data Permanen Baca." },
    { "en": "Apa Itu 'Flash Memory'?", "id": "Penyimpanan Data Non-Volatil Kecepatan Tinggi." },
    { "en": "Apa Itu 'UART' (Universal Asynchronous Receiver Transmitter)?", "id": "Protokol Komunikasi Data Serial Asinkron." },
    { "en": "Apa Itu 'SPI' (Serial Peripheral Interface)?", "id": "Protokol Komunikasi Data Serial Sinkron." },
    { "en": "Apa Itu 'I2C' (Inter-Integrated Circuit)?", "id": "Protokol Komunikasi Antar Chip Sederhana." },
    { "en": "Apa Itu 'USB' (Universal Serial Bus)?", "id": "Standar Koneksi Perangkat Luar Universal." },
    { "en": "Apa Itu 'VSC' (Voltage Source Converter)?", "id": "Konverter Tegangan Searah Menjadi Bolak-Balik." },
    { "en": "Apa Itu 'LCC' (Line Commutated Converter)?", "id": "Konverter Dengan Komutasi Jalur Listrik." },
    { "en": "Apa Itu 'DTC' (Direct Torque Control)?", "id": "Kendali Torsi Motor Secara Langsung." },
    { "en": "Apa Itu 'FOC' (Field Oriented Control)?", "id": "Kendali Motor Berorientasi Medan Magnet." },
    { "en": "Apa Itu 'SVPWM' (Space Vector Pulse Width Modulation)?", "id": "Teknik Modulasi Vektor Ruang Efisien." },
    { "en": "Apa Itu 'THD' (Total Harmonic Distortion)?", "id": "Persentase Distorsi Harmonisa Sinyal Listrik." },
    { "en": "Apa Itu 'EMI Filter' (Electromagnetic Interference Filter)?", "id": "Penyaring Gangguan Elektromagnetik Frekuensi Tinggi." },
    { "en": "Apa Itu 'Ferrite Ring' (Cincin Ferit)?", "id": "Komponen Penekan Derau Pada Kabel." },
    { "en": "Apa Itu 'Opto-Isolator' (Isolator Optik)?", "id": "Pengirim Sinyal Melalui Media Cahaya." },
    { "en": "Apa Itu 'Flyback Converter' (Konverter Flyback)?", "id": "Konverter Daya Dengan Isolasi Transformator." },
    { "en": "Apa Itu 'Forward Converter' (Konverter Forward)?", "id": "Konverter Daya Searah Dengan Transformator." },
    { "en": "Apa Itu 'Push-Pull Converter' (Konverter Push-Pull)?", "id": "Konverter Daya Dengan Dua Saklar." },
    { "en": "Apa Itu 'Full Bridge Converter' (Konverter Full Bridge)?", "id": "Konverter Daya Dengan Empat Saklar." },
    { "en": "Apa Itu 'Half Bridge Converter' (Konverter Half Bridge)?", "id": "Konverter Daya Dengan Dua Kapasitor." },
    { "en": "Apa Itu 'Snubber Circuit' (Rangkaian Snubber)?", "id": "Rangkaian Peredam Lonjakan Tegangan Saklar." },
    { "en": "Apa Itu 'Crowbar Circuit' (Rangkaian Crowbar)?", "id": "Proteksi Tegangan Lebih Hubung Singkat." },
    { "en": "Apa Itu 'Active Rectifier' (Penyearah Aktif)?", "id": "Penyearah Menggunakan Komponen Saklar Aktif." },
    { "en": "Apa Itu 'Power Factor Preregulator' (Praregulator Faktor Daya)?", "id": "Rangkaian Perbaikan Faktor Daya Aktif." },
    { "en": "Apa Itu 'Burden Resistor' (Resistor Beban)?", "id": "Resistor Pengubah Arus Menjadi Tegangan." },
    { "en": "Apa Itu 'Bleeder Resistor' (Resistor Bleeder)?", "id": "Resistor Pembuang Muatan Kapasitor Sisa." },
    { "en": "Apa Itu 'Inrush Current' (Arus Inrush)?", "id": "Lonjakan Arus Saat Perangkat Dinyalakan." },
    { "en": "Apa Itu 'Soft Start' (Mulai Lembut)?", "id": "Proses Peningkatan Tegangan Secara Bertahap." },
    { "en": "Apa Itu 'Hard Switching' (Pensakelaran Keras)?", "id": "Pensakelaran Saat Tegangan Arus Tinggi." },
    { "en": "Apa Itu 'Switching Frequency' (Frekuensi Pensakelaran)?", "id": "Kecepatan Saklar Bekerja Per Detik." },
    { "en": "Apa Itu 'Gate Driver' (Driver Gerbang)?", "id": "Sirkuit Penguat Sinyal Kendali Transistor." },
    { "en": "Apa Itu 'Dead Time' (Waktu Mati)?", "id": "Jeda Mencegah Hubung Singkat Saklar." },
    { "en": "Apa Itu 'Bootstrap Circuit' (Rangkaian Bootstrap)?", "id": "Penyuplai Tegangan Gerbang Transistor Atas." },
    { "en": "Apa Itu 'High Side Switch' (Saklar Sisi Tinggi)?", "id": "Saklar Di Antara Beban, Positif." },
    { "en": "Apa Itu 'Low Side Switch' (Saklar Sisi Rendah)?", "id": "Saklar Di Antara Beban, Ground." },
    { "en": "Apa Itu 'Current Mode Control' (Kendali Mode Arus)?", "id": "Sistem Kendali Berdasarkan Arus Induktor." },
    { "en": "Apa Itu 'Voltage Mode Control' (Kendali Mode Tegangan)?", "id": "Sistem Kendali Berdasarkan Tegangan Output." },
    { "en": "Apa Itu 'Linear Regulator' (Regulator Linear)?", "id": "Penstabil Tegangan Dengan Metode Disipasi." },
    { "en": "Apa Itu 'LDO' (Low Drop Out Regulator)?", "id": "Regulator Tegangan Selisih Masukan Rendah." },
    { "en": "Apa Itu 'Shunt Regulator' (Regulator Shunt)?", "id": "Regulator Tegangan Melalui Jalur Paralel." },
    { "en": "Apa Itu 'Voltage Reference' (Referensi Tegangan)?", "id": "Sumber Tegangan Sangat Stabil Presisi." },
    { "en": "Apa Itu 'Bandgap Reference' (Referensi Bandgap)?", "id": "Sirkuit Referensi Tegangan Kompensasi Suhu." },
    { "en": "Apa Itu 'Charge Pump' (Pompa Muatan)?", "id": "Konverter Tegangan Menggunakan Kapasitor Saklar." },
    { "en": "Apa Itu 'EDLC' (Electric Double-Layer Capacitor)?", "id": "Kapasitor Penyimpan Energi Lapisan Ganda." },
    { "en": "Apa Itu 'Li-Po' (Lithium Polymer Battery)?", "id": "Baterai Polimer Ringan Kepadatan Tinggi." },
    { "en": "Apa Itu 'LiFePO4' (Lithium Iron Phosphate Battery)?", "id": "Baterai Lithium Dengan Stabilitas Tinggi." },
    { "en": "Apa Itu 'C-Rate' (Laju Pengisian)?", "id": "Ukuran Kecepatan Pengisian Arus Baterai." },
    { "en": "Apa Itu 'Thermal Runaway' (Pelarian Termal)?", "id": "Kenaikan Suhu Tak Terkendali Baterai." },
    { "en": "Apa Itu 'Battery Balancing' (Penyeimbangan Baterai)?", "id": "Proses Penyamamaan Tegangan Sel Baterai." },
    { "en": "Apa Itu 'BMS' (Battery Management System)?", "id": "Sistem Pengawas Dan Pelindung Baterai." },
    { "en": "Apa Itu 'Coulomb Counting' (Penghitungan Coulomb)?", "id": "Metode Estimasi Kapasitas Tersisa Baterai." },
    { "en": "Apa Itu 'DOD' (Depth Of Discharge)?", "id": "Persentase Energi Yang Diambil Baterai." },
    { "en": "Apa Itu 'Self Discharge' (Pengosongan Mandiri)?", "id": "Hilangnya Muatan Baterai Saat Diam." },
    { "en": "Apa Itu 'Internal Resistance' (Resistansi Internal)?", "id": "Hambatan Aliran Arus Dalam Baterai." },
    { "en": "Apa Itu 'Wireless Power Transfer' (Transfer Daya Nirkabel)?", "id": "Pengiriman Energi Listrik Tanpa Kabel." },
    { "en": "Apa Itu 'Inductive Coupling' (Kopling Induktif)?", "id": "Transfer Energi Melalui Medan Magnet." },
    { "en": "Apa Itu 'Resonant Coupling' (Kopling Resonansi)?", "id": "Transfer Energi Nirkabel Jarak Menengah." },
    { "en": "Apa Itu 'Qi Standard' (Standar Qi)?", "id": "Standar Global Pengisian Daya Nirkabel." },
    { "en": "Apa Itu 'RFID' (Radio Frequency Identification)?", "id": "Identifikasi Objek Melalui Gelombang Radio." },
    { "en": "Apa Itu 'NFC' (Near Field Communication)?", "id": "Komunikasi Nirkabel Jarak Sangat Pendek." },
    { "en": "Apa Itu 'Surface Acoustic Wave' (Gelombang Akustik Permukaan)?", "id": "Gelombang Suara Pada Permukaan Bahan." },
    { "en": "Apa Itu 'MEMS' (Micro-Electro-Mechanical Systems)?", "id": "Sistem Mekanik Elektronik Skala Mikroskopis." },
    { "en": "Apa Itu 'Piezoelectric Actuator' (Aktuator Piezoelektrik)?", "id": "Penggerak Berbasis Perubahan Bentuk Listrik." },
    { "en": "Apa Itu 'Strain Gauge' (Pengukur Regangan)?", "id": "Sensor Pendeteksi Perubahan Bentuk Mekanik." },
    { "en": "Apa Itu 'Load Cell' (Sel Beban)?", "id": "Sensor Pengukur Berat Atau Gaya." },
    { "en": "Apa Itu 'LVDT' (Linear Variable Differential Transformer)?", "id": "Sensor Pengukur Posisi Linear Presisi." },
    { "en": "Apa Itu 'RVDT' (Rotary Variable Differential Transformer)?", "id": "Sensor Pengukur Posisi Sudut Presisi." },
    { "en": "Apa Itu 'Resolver' (Resolver Sudut)?", "id": "Sensor Rotasi Untuk Posisi Sudut." },
    { "en": "Apa Itu 'Hall Effect Sensor' (Sensor Efek Hall)?", "id": "Sensor Pendeteksi Keberadaan Medan Magnet." },
    { "en": "Apa Itu 'Magnetometer' (Alat Ukur Magnet)?", "id": "Sensor Pengukur Kekuatan Medan Magnet." },
    { "en": "Apa Itu 'Gyroscope' (Giroskop)?", "id": "Sensor Pengukur Kecepatan Sudut Rotasi." },
    { "en": "Apa Itu 'Accelerometer' (Akselerometer)?", "id": "Sensor Pengukur Percepatan Gerak Fisik." },
    { "en": "Apa Itu 'IMU' (Inertial Measurement Unit)?", "id": "Sensor Gabungan Akselerometer Dan Giroskop." },
    { "en": "Apa Itu 'LIDAR' (Light Detection And Ranging)?", "id": "Sensor Pemetaan Menggunakan Pantulan Cahaya." },
    { "en": "Apa Itu 'Ultrasonic Sensor' (Sensor Ultrasonik)?", "id": "Sensor Jarak Menggunakan Gelombang Suara." },
    { "en": "Apa Itu 'TOF Sensor' (Time Of Flight Sensor)?", "id": "Sensor Jarak Berdasarkan Waktu Pantulan." },
    { "en": "Apa Itu 'Solar Cell' (Sel Surya)?", "id": "Perangkat Pengubah Cahaya Menjadi Listrik." },
    { "en": "Apa Itu 'Peltier Cooler' (Pendingin Peltier)?", "id": "Pendingin Berbasis Arus Listrik Langsung." },
    { "en": "Apa Itu 'Seebeck Effect' (Efek Seebeck)?", "id": "Pembangkit Listrik Dari Perbedaan Suhu." },
    { "en": "Apa Itu 'Thermopile' (Termopil)?", "id": "Kumpulan Termokopel Pengukur Radiasi Panas." },
    { "en": "Apa Itu 'RTD' (Resistance Temperature Detector)?", "id": "Sensor Suhu Berbasis Perubahan Hambatan." },
    { "en": "Apa Itu 'Thermistor' (Termistor)?", "id": "Resistor Sensitif Terhadap Perubahan Suhu." },
    { "en": "Apa Itu 'NTC' (Negative Temperature Coefficient)?", "id": "Resistor Hambatan Turun Suhu Naik." },
    { "en": "Apa Itu 'PTC' (Positive Temperature Coefficient)?", "id": "Resistor Hambatan Naik Suhu Naik." },
    { "en": "Apa Itu 'HMI' (Human Machine Interface)?", "id": "Antarmuka Visual Antara Manusia, Mesin." },
    { "en": "Apa Itu 'DCS' (Distributed Control System)?", "id": "Sistem Kendali Industri Secara Terdistribusi." },
    { "en": "Apa Itu 'PID Controller' (Proportional Integral Derivative Controller)?", "id": "Sistem Kendali Umpan Balik Kontinu." },
    { "en": "Apa Itu 'Servo Drive' (Driver Servo)?", "id": "Pengendali Motor Servo Posisi Presisi." },
    { "en": "Apa Itu 'Modbus RTU' (Modbus Remote Terminal Unit)?", "id": "Protokol Komunikasi Industri Serial Standar." },
    { "en": "Apa Itu 'Profibus' (Process Field Bus)?", "id": "Standar Komunikasi Data Lapangan Industri." },
    { "en": "Apa Itu 'CAN Bus' (Controller Area Network Bus)?", "id": "Protokol Komunikasi Data Kendaraan Otomotif." },
    { "en": "Apa Itu 'ASIC' (Application Specific Integrated Circuit)?", "id": "Sirkuit Terintegrasi Untuk Fungsi Khusus." },
    { "en": "Apa Itu 'Digital Signal Processor' (Prosesor Sinyal Digital)?", "id": "Prosesor Khusus Pengolah Data Sinyal." },
    { "en": "Apa Itu 'Watchdog Timer' (Timer Penjaga)?", "id": "Sistem Pendeteksi Kegagalan Program Mikroprosesor." },
    { "en": "Apa Itu 'EDFA' (Erbium Doped Fiber Amplifier)?", "id": "Penguat Sinyal Cahaya Optik Erbium." },
    { "en": "Apa Itu 'FER' (Frame Error Rate)?", "id": "Rasio Kesalahan Bingkai Data Digital." },
    { "en": "Apa Itu 'PON' (Passive Optical Network)?", "id": "Jaringan Serat Optik Tanpa Daya." },
    { "en": "Apa Itu 'GPON' (Gigabit Passive Optical Network)?", "id": "Jaringan Optik Pasif Kecepatan Gigabit." },
    { "en": "Apa Itu 'EPON' (Ethernet Passive Optical Network)?", "id": "Jaringan Optik Pasif Berbasis Ethernet." },
    { "en": "Apa Itu 'OLT' (Optical Line Terminal)?", "id": "Terminal Jalur Optik Sisi Operator." },
    { "en": "Apa Itu 'ONT' (Optical Network Terminal)?", "id": "Terminal Jaringan Optik Sisi Pelanggan." },
    { "en": "Apa Itu 'ONU' (Optical Network Unit)?", "id": "Unit Jaringan Optik Pengguna Akhir." },
    { "en": "Apa Itu 'FTTx' (Fiber To The X)?", "id": "Arsitektur Jaringan Serat Optik Universal." },
    { "en": "Apa Itu 'FTTH' (Fiber To The Home)?", "id": "Jaringan Serat Optik Sampai Rumah." },
    { "en": "Apa Itu 'FTTB' (Fiber To The Building)?", "id": "Jaringan Serat Optik Sampai Gedung." },
    { "en": "Apa Itu 'FTTC' (Fiber To The Curb)?", "id": "Jaringan Serat Optik Sampai Trotoar." },
    { "en": "Apa Itu 'FTTN' (Fiber To The Node)?", "id": "Jaringan Serat Optik Sampai Simpul." },
    { "en": "Apa Itu 'FTTP' (Fiber To The Premises)?", "id": "Jaringan Serat Optik Sampai Lokasi." },
    { "en": "Apa Itu 'ODF' (Optical Distribution Frame)?", "id": "Rak Distribusi Kabel Serat Optik." },
    { "en": "Apa Itu 'OSP' (Outside Plant)?", "id": "Infrastruktur Jaringan Di Luar Ruangan." },
    { "en": "Apa Itu 'ISP' (Internet Service Provider)?", "id": "Perusahaan Penyedia Layanan Akses Internet." },
    { "en": "Apa Itu 'NAP' (Network Access Point)?", "id": "Titik Akses Pertukaran Data Jaringan." },
    { "en": "Apa Itu 'IXP' (Internet Exchange Point)?", "id": "Titik Fisik Pertukaran Lalu Lintas." },
    { "en": "Apa Itu 'BGP' (Border Gateway Protocol)?", "id": "Protokol Rute Utama Antar Jaringan." },
    { "en": "Apa Itu 'OSPF' (Open Shortest Path First)?", "id": "Protokol Pencari Jalur Terpendek Jaringan." },
    { "en": "Apa Itu 'MPLS' (Multiprotocol Label Switching)?", "id": "Teknik Penerusan Paket Melalui Label." },
    { "en": "Apa Itu 'PVLAN' (Private Virtual Local Area Network)?", "id": "Segmen Jaringan Lokal Virtual Pribadi." },
    { "en": "Apa Itu 'VXLAN' (Virtual Extensible Local Area Network)?", "id": "Teknologi Virtualisasi Jaringan Skala Besar." },
    { "en": "Apa Itu 'SDN' (Software Defined Networking)?", "id": "Pengelolaan Jaringan Melalui Perangkat Lunak." },
    { "en": "Apa Itu 'NFV' (Network Functions Virtualization)?", "id": "Virtualisasi Fungsi Perangkat Keras Jaringan." },
    { "en": "Apa Itu 'VNF' (Virtualized Network Function)?", "id": "Fungsi Jaringan Dalam Bentuk Virtual." },
    { "en": "Apa Itu 'MAN' (Metropolitan Area Network)?", "id": "Jaringan Komputer Skala Satu Kota." },
    { "en": "Apa Itu 'SAN' (Storage Area Network)?", "id": "Jaringan Khusus Untuk Penyimpanan Data." },
    { "en": "Apa Itu 'WWAN' (Wireless Wide Area Network)?", "id": "Jaringan Nirkabel Mencakup Area Luas." },
    { "en": "Apa Itu 'WLAN' (Wireless Local Area Network)?", "id": "Jaringan Lokal Nirkabel Skala Kecil." },
    { "en": "Apa Itu 'WPA' (Wi-Fi Protected Access)?", "id": "Standar Keamanan Koneksi Nirkabel Modern." },
    { "en": "Apa Itu 'WEP' (Wired Equivalent Privacy)?", "id": "Protokol Keamanan Jaringan Nirkabel Lama." },
    { "en": "Apa Itu 'AES' (Advanced Encryption Standard)?", "id": "Algoritma Enkripsi Data Sangat Aman." },
    { "en": "Apa Itu 'TKIP' (Temporal Key Integrity Protocol)?", "id": "Protokol Keamanan Enkripsi Data Nirkabel." },
    { "en": "Apa Itu 'SSID' (Service Set Identifier)?", "id": "Nama Identitas Jaringan Nirkabel Tertentu." },
    { "en": "Apa Itu 'BSSID' (Basic Service Set Identifier)?", "id": "Alamat Fisik Titik Akses Nirkabel." },
    { "en": "Apa Itu 'ESSID' (Extended Service Set Identifier)?", "id": "Identitas Jaringan Nirkabel Area Luas." },
    { "en": "Apa Itu 'CSMA/CD' (Carrier Sense Multiple Access Collision Detection)?", "id": "Metode Deteksi Tabrakan Data Jaringan." },
    { "en": "Apa Itu 'CSMA/CA' (Carrier Sense Multiple Access Collision Avoidance)?", "id": "Metode Pencegahan Tabrakan Data Nirkabel." },
    { "en": "Apa Itu 'ARQ' (Automatic Repeat Request)?", "id": "Protokol Pengiriman Ulang Data Otomatis." },
    { "en": "Apa Itu 'HARQ' (Hybrid Automatic Repeat Request)?", "id": "Kombinasi Koreksi Dan Pengiriman Ulang." },
    { "en": "Apa Itu 'COS' (Class Of Service)?", "id": "Klasifikasi Prioritas Lalu Lintas Jaringan." },
    { "en": "Apa Itu 'DiffServ' (Differentiated Services)?", "id": "Arsitektur Kualitas Layanan Jaringan Digital." },
    { "en": "Apa Itu 'IntServ' (Integrated Services)?", "id": "Arsitektur Reservasi Sumber Daya Jaringan." },
    { "en": "Apa Itu 'RSVP' (Resource Reservation Protocol)?", "id": "Protokol Reservasi Bandwidth Jalur Data." },
    { "en": "Apa Itu 'SIP' (Session Initiation Protocol)?", "id": "Protokol Memulai Sesi Komunikasi Multimedia." },
    { "en": "Apa Itu 'RTP' (Real-time Transport Protocol)?", "id": "Protokol Pengiriman Data Waktu Nyata." },
    { "en": "Apa Itu 'RTCP' (Real-time Transport Control Protocol)?", "id": "Protokol Kendali Transmisi Data Multimedia." },
    { "en": "Apa Itu 'VoIP' (Voice Over Internet Protocol)?", "id": "Layanan Telepon Melalui Jaringan Internet." },
    { "en": "Apa Itu 'IPTV' (Internet Protocol Television)?", "id": "Layanan Televisi Melalui Protokol Internet." },
    { "en": "Apa Itu 'LTE' (Long Term Evolution)?", "id": "Standar Komunikasi Nirkabel Kecepatan Tinggi." },
    { "en": "Apa Itu 'VoLTE' (Voice Over Long Term Evolution)?", "id": "Layanan Suara Melalui Jaringan LTE." },
    { "en": "Apa Itu 'EPC' (Evolved Packet Core)?", "id": "Arsitektur Inti Jaringan Seluler LTE." },
    { "en": "Apa Itu 'IMS' (IP Multimedia Subsystem)?", "id": "Kerangka Kerja Layanan Multimedia IP." },
    { "en": "Apa Itu 'HSS' (Home Subscriber Server)?", "id": "Database Pusat Data Pelanggan Seluler." },
    { "en": "Apa Itu 'MME' (Mobility Management Entity)?", "id": "Elemen Kendali Mobilitas Jaringan Seluler." },
    { "en": "Apa Itu 'SGW' (Serving Gateway)?", "id": "Gerbang Penyalur Data Paket Pengguna." },
    { "en": "Apa Itu 'PGW' (Packet Data Network Gateway)?", "id": "Gerbang Penghubung Jaringan Data Luar." },
    { "en": "Apa Itu 'RNC' (Radio Network Controller)?", "id": "Pengatur Sumber Daya Radio Seluler." },
    { "en": "Apa Itu 'NodeB' (Node B)?", "id": "Stasiun Pemancar Jaringan Seluler 3G." },
    { "en": "Apa Itu 'eNodeB' (Evolved Node B)?", "id": "Stasiun Pemancar Jaringan Seluler 4G." },
    { "en": "Apa Itu 'gNodeB' (Next Generation Node B)?", "id": "Stasiun Pemancar Jaringan Seluler 5G." },
    { "en": "Apa Itu 'URLLC' (Ultra-Reliable Low-Latency Communications)?", "id": "Komunikasi Sangat Handal, Latensi Rendah." },
    { "en": "Apa Itu 'eMBB' (Enhanced Mobile Broadband)?", "id": "Layanan Pita Lebar Seluler Cepat." },
    { "en": "Apa Itu 'mMTC' (Massive Machine Type Communications)?", "id": "Komunikasi Mesin Skala Sangat Besar." },
    { "en": "Apa Itu 'SIM' (Subscriber Identity Module)?", "id": "Modul Identitas Pelanggan Telepon Seluler." },
    { "en": "Apa Itu 'eSIM' (Embedded Subscriber Identity Module)?", "id": "Modul Identitas Pelanggan Tertanam Digital." },
    { "en": "Apa Itu 'IMEI' (International Mobile Equipment Identity)?", "id": "Nomor Identitas Unik Perangkat Seluler." },
    { "en": "Apa Itu 'IMSI' (International Mobile Subscriber Identity)?", "id": "Nomor Identitas Unik Pelanggan Seluler." },
    { "en": "Apa Itu 'PIN' (Personal Identification Number)?", "id": "Kode Keamanan Identitas Pribadi Pengguna." },
    { "en": "Apa Itu 'PUK' (Personal Unlocking Key)?", "id": "Kode Pembuka Blokir Kartu SIM." },
    { "en": "Apa Itu 'MCC' (Mobile Country Code)?", "id": "Kode Identitas Negara Asal Seluler." },
    { "en": "Apa Itu 'MNC' (Mobile Network Code)?", "id": "Kode Identitas Operator Jaringan Seluler." },
    { "en": "Apa Itu 'LAC' (Location Area Code)?", "id": "Kode Identitas Area Lokasi Seluler." },
    { "en": "Apa Itu 'CID' (Cell ID)?", "id": "Identitas Unik Sel Pemancar Seluler." },
    { "en": "Apa Itu 'RSRP' (Reference Signal Received Power)?", "id": "Ukuran Kekuatan Sinyal Referensi Diterima." },
    { "en": "Apa Itu 'RSRQ' (Reference Signal Received Quality)?", "id": "Ukuran Kualitas Sinyal Referensi Diterima." },
    { "en": "Apa Itu 'CQI' (Channel Quality Indicator)?", "id": "Indikator Kualitas Saluran Komunikasi Nirkabel." },
    { "en": "Apa Itu 'MCS' (Modulation And Coding Scheme)?", "id": "Skema Modulasi Dan Pengkodean Data." },
    { "en": "Apa Itu 'SISO' (Single Input Single Output)?", "id": "Sistem Antena Tunggal Kirim Terima." },
    { "en": "Apa Itu 'SIMO' (Single Input Multiple Output)?", "id": "Sistem Antena Tunggal Banyak Terima." },
    { "en": "Apa Itu 'MISO' (Multiple Input Single Output)?", "id": "Sistem Banyak Antena Satu Terima." },
    { "en": "Apa Itu 'MU-MIMO' (Multi-User Multiple Input Multiple Output)?", "id": "Teknologi MIMO Untuk Banyak Pengguna." },
    { "en": "Apa Itu 'TDD' (Time Division Duplex)?", "id": "Metode Dupleks Berdasarkan Slot Waktu." },
    { "en": "Apa Itu 'FDD' (Frequency Division Duplex)?", "id": "Metode Dupleks Berdasarkan Rentang Frekuensi." },
    { "en": "Apa Itu 'CP-OFDM' (Cyclic Prefix Orthogonal Frequency Division Multiplexing)?", "id": "Modulasi OFDM Dengan Prefiks Siklik." },
    { "en": "Apa Itu 'DFT-s-OFDM' (Discrete Fourier Transform Spread Orthogonal Frequency Division Multiplexing)?", "id": "Modulasi OFDM Dengan Sebaran DFT." },
    { "en": "Apa Itu 'PAPR' (Peak-To-Average Power Ratio)?", "id": "Rasio Daya Puncak Terhadap Rata-Rata." },
    { "en": "Apa Itu 'EVM' (Error Vector Magnitude)?", "id": "Ukuran Kualitas Modulasi Sinyal Digital." },
    { "en": "Apa Itu 'ACLR' (Adjacent Channel Leakage Ratio)?", "id": "Rasio Kebocoran Daya Saluran Tetangga." },
    { "en": "Apa Itu 'SEM' (Spectrum Emission Mask)?", "id": "Batas Emisi Spektrum Frekuensi Diizinkan." },
    { "en": "Apa Itu 'OBW' (Occupied Bandwidth)?", "id": "Lebar Pita Frekuensi Yang Terpakai." },
    { "en": "Apa Itu 'PSD' (Power Spectral Density)?", "id": "Kerapatan Daya Sinyal Terhadap Frekuensi." },
    { "en": "Apa Itu 'S-Parameter' (Scattering Parameter)?", "id": "Parameter Hamburan Sinyal Frekuensi Tinggi." },
    { "en": "Apa Itu 'RL' (Return Loss)?", "id": "Ukuran Daya Sinyal Yang Dipantulkan." },
    { "en": "Apa Itu 'IL' (Insertion Loss)?", "id": "Kehilangan Daya Akibat Pemasangan Komponen." },
    { "en": "Apa Itu 'OADM' (Optical Add-Drop Multiplexer)?", "id": "Perangkat Manipulasi Panjang Gelombang Optik." },
    { "en": "Apa Itu 'ROADM' (Reconfigurable Optical Add-Drop Multiplexer)?", "id": "Perangkat Manipulasi Optik Terprogram Jarak-Jauh." },
    { "en": "Apa Itu 'DWDM' (Dense Wavelength Division Multiplexing)?", "id": "Multipleksing Panjang Gelombang Sangat Padat." },
    { "en": "Apa Itu 'NTSC' (National Television System Committee)?", "id": "Standar Penyiaran Televisi Analog Amerika." },
    { "en": "Apa Itu 'PAL' (Phase Alternating Line)?", "id": "Standar Penyiaran Televisi Analog Eropa." },
    { "en": "Apa Itu 'SECAM' (Sequential Color With Memory)?", "id": "Standar Penyiaran Televisi Analog Perancis." },
    { "en": "Apa Itu 'UHD' (Ultra High Definition)?", "id": "Resolusi Gambar Layar Sangat Tinggi." },
    { "en": "Apa Itu 'HDR' (High Dynamic Range)?", "id": "Teknologi Peningkatan Kontras Warna Gambar." },
    { "en": "Apa Itu 'MPEG' (Moving Picture Experts Group)?", "id": "Standar Kompresi Data Video Digital." },
    { "en": "Apa Itu 'JPEG' (Joint Photographic Experts Group)?", "id": "Standar Kompresi Data Gambar Diam." },
    { "en": "Apa Itu 'MIDI' (Musical Instrument Digital Interface)?", "id": "Protokol Komunikasi Data Instrumen Musik." },
    { "en": "Apa Itu 'ASIO' (Audio Stream Input/Output)?", "id": "Driver Audio Latensi Sangat Rendah." },
    { "en": "Apa Itu 'VST' (Virtual Studio Technology)?", "id": "Antarmuka Efek Audio Perangkat Lunak." },
    { "en": "Apa Itu 'DAW' (Digital Audio Workstation)?", "id": "Perangkat Lunak Pengolah Musik Digital." },
    { "en": "Apa Itu 'SPL' (Sound Pressure Level)?", "id": "Ukuran Tingkat Tekanan Suara Akustik." },
    { "en": "Apa Itu 'TRS' (Tip Ring Sleeve)?", "id": "Konektor Audio Tiga Jalur Kontak." },
    { "en": "Apa Itu 'XLR' (External Line Return)?", "id": "Konektor Audio Profesional Tiga Pin." },
    { "en": "Apa Itu 'RCA' (Radio Corporation Of America)?", "id": "Konektor Audio Video Standar Konsumen." },
    { "en": "Apa Itu 'BNC' (Bayonet Neill-Concelman)?", "id": "Konektor Kabel Koaksial Pengunci Putar." },
    { "en": "Apa Itu 'SMA' (SubMiniature Version A)?", "id": "Konektor Frekuensi Radio Ukuran Kecil." },
    { "en": "Apa Itu 'SMB' (SubMiniature Version B)?", "id": "Konektor Frekuensi Radio Pasang Tekan." },
    { "en": "Apa Itu 'MCX' (Micro Coaxial Connector)?", "id": "Konektor Koaksial Mikro Ukuran Kecil." },
    { "en": "Apa Itu 'MMCX' (Micro-Miniature Coaxial)?", "id": "Konektor Koaksial Mikro Sangat Kecil." },
    { "en": "Apa Itu 'VHF' (Very High Frequency)?", "id": "Rentang Frekuensi Radio Sangat Tinggi." },
    { "en": "Apa Itu 'SHF' (Super High Frequency)?", "id": "Rentang Frekuensi Radio Super Tinggi." },
    { "en": "Apa Itu 'EHF' (Extremely High Frequency)?", "id": "Rentang Frekuensi Radio Ekstrem Tinggi." },
    { "en": "Apa Itu 'GNSS' (Global Navigation Satellite System)?", "id": "Sistem Navigasi Satelit Global Universal." },
    { "en": "Apa Itu 'GLONASS' (Global Navigation Satellite System)?", "id": "Sistem Navigasi Satelit Milik Rusia." },
    { "en": "Apa Itu 'GALILEO' (European Global Navigation Satellite System)?", "id": "Sistem Navigasi Satelit Milik Eropa." },
    { "en": "Apa Itu 'BEIDOU' (Chinese Navigation Satellite System)?", "id": "Sistem Navigasi Satelit Milik Cina." },
    { "en": "Apa Itu 'RTK' (Real-Time Kinematic)?", "id": "Teknik Pemosisian Satelit Sangat Presisi." },
    { "en": "Apa Itu 'AHRS' (Attitude And Heading Reference System)?", "id": "Sistem Referensi Sikap, Arah Pesawat." },
    { "en": "Apa Itu 'EFIS' (Electronic Flight Instrument System)?", "id": "Sistem Instrumen Penerbangan Elektronik Digital." },
    { "en": "Apa Itu 'PFD' (Primary Flight Display)?", "id": "Layar Utama Informasi Data Penerbangan." },
    { "en": "Apa Itu 'ND' (Navigation Display)?", "id": "Layar Penampil Informasi Navigasi Pesawat." },
    { "en": "Apa Itu 'FMS' (Flight Management System)?", "id": "Sistem Komputer Pengelola Rencana Terbang." },
    { "en": "Apa Itu 'MCP' (Mode Control Panel)?", "id": "Panel Kendali Mode Pilot Otomatis." },
    { "en": "Apa Itu 'CDU' (Control Display Unit)?", "id": "Antarmuka Masukan Data Komputer Pesawat." },
    { "en": "Apa Itu 'TCAS' (Traffic Collision Avoidance System)?", "id": "Sistem Pencegah Tabrakan Antar Pesawat." },
    { "en": "Apa Itu 'GPWS' (Ground Proximity Warning System)?", "id": "Peringatan Bahaya Kedekatan Dengan Tanah." },
    { "en": "Apa Itu 'ADS-B' (Automatic Dependent Surveillance-Broadcast)?", "id": "Sistem Pengawasan Pesawat Berbasis Siaran." },
    { "en": "Apa Itu 'ILS' (Instrument Landing System)?", "id": "Sistem Pemandu Pendaratan Pesawat Presisi." },
    { "en": "Apa Itu 'VOR' (VHF Omnidirectional Range)?", "id": "Pemancar Navigasi Radio Segala Arah." },
    { "en": "Apa Itu 'DME' (Distance Measuring Equipment)?", "id": "Alat Ukur Jarak Pesawat Radio." },
    { "en": "Apa Itu 'ADF' (Automatic Direction Finder)?", "id": "Alat Penentu Arah Pemancar Radio." },
    { "en": "Apa Itu 'NDB' (Non-Directional Beacon)?", "id": "Pemancar Radio Navigasi Tanpa Arah." },
    { "en": "Apa Itu 'RADAR' (Radio Detection And Ranging)?", "id": "Deteksi Objek Menggunakan Gelombang Radio." },
    { "en": "Apa Itu 'SONAR' (Sound Navigation And Ranging)?", "id": "Deteksi Objek Menggunakan Gelombang Suara." },
    { "en": "Apa Itu 'AIS' (Automatic Identification System)?", "id": "Sistem Identifikasi Kapal Laut Otomatis." },
    { "en": "Apa Itu 'ECDIS' (Electronic Chart Display And Information System)?", "id": "Sistem Pemetaan Laut Elektronik Digital." },
    { "en": "Apa Itu 'GMDSS' (Global Maritime Distress And Safety System)?", "id": "Sistem Komunikasi Keselamatan Maritim Global." },
    { "en": "Apa Itu 'EPIRB' (Emergency Position Indicating Radio Beacon)?", "id": "Pemancar Sinyal Darurat Lokasi Kecelakaan." },
    { "en": "Apa Itu 'SART' (Search And Rescue Transponder)?", "id": "Transponder Pencarian Dan Penyelamatan Maritim." },
    { "en": "Apa Itu 'VDR' (Voyage Data Recorder)?", "id": "Perekam Data Perjalanan Kapal Laut." },
    { "en": "Apa Itu 'ARPA' (Automatic Radar Plotting Aid)?", "id": "Alat Bantu Plot Radar Otomatis." },
    { "en": "Apa Itu 'L-Band'?", "id": "Rentang Frekuensi Satu Hingga Dua." },
    { "en": "Apa Itu 'S-Band'?", "id": "Rentang Frekuensi Dua Hingga Empat." },
    { "en": "Apa Itu 'C-Band'?", "id": "Rentang Frekuensi Empat Hingga Delapan." },
    { "en": "Apa Itu 'X-Band'?", "id": "Rentang Frekuensi Delapan Hingga Duabelas." },
    { "en": "Apa Itu 'Ku-Band'?", "id": "Rentang Frekuensi Duabelas Hingga Delapanbelas." },
    { "en": "Apa Itu 'K-Band'?", "id": "Rentang Frekuensi Delapanbelas Hingga Duapuluhtujuh." },
    { "en": "Apa Itu 'Ka-Band'?", "id": "Rentang Frekuensi Duapuluhtujuh Hingga Empatpuluh." },
    { "en": "Apa Itu 'VSAT' (Very Small Aperture Terminal)?", "id": "Stasiun Bumi Satelit Antena Kecil." },
    { "en": "Apa Itu 'BUC' (Block Upconverter)?", "id": "Pengubah Frekuensi Rendah Menjadi Tinggi." },
    { "en": "Apa Itu 'LNB' (Low Noise Block Downconverter)?", "id": "Penerima Sinyal Satelit Minim Derau." },
    { "en": "Apa Itu 'OMT' (Ortho-Mode Transducer)?", "id": "Pemisah Polarisasi Sinyal Gelombang Mikro." },
    { "en": "Apa Itu 'EIRP' (Equivalent Isotropically Radiated Power)?", "id": "Total Daya Pancar Antena Isotropik." },
    { "en": "Apa Itu 'G/T' (Gain-To-Noise Temperature)?", "id": "Rasio Penguatan Terhadap Suhu Derau." },
    { "en": "Apa Itu 'PER' (Packet Error Rate)?", "id": "Rasio Kesalahan Paket Data Digital." },
    { "en": "Apa Itu 'ISI' (Inter-Symbol Interference)?", "id": "Gangguan Antar Simbol Sinyal Digital." },
    { "en": "Apa Itu 'AWGN' (Additive White Gaussian Noise)?", "id": "Model Derau Putih Acak Matematis." },
    { "en": "Apa Itu 'CDMA' (Code Division Multiple Access)?", "id": "Akses Jamak Berdasarkan Kode Unik." },
    { "en": "Apa Itu 'TDMA' (Time Division Multiple Access)?", "id": "Akses Jamak Berdasarkan Slot Waktu." },
    { "en": "Apa Itu 'FDMA' (Frequency Division Multiple Access)?", "id": "Akses Jamak Berdasarkan Pita Frekuensi." },
    { "en": "Apa Itu 'OFDMA' (Orthogonal Frequency Division Multiple Access)?", "id": "Akses Jamak Berdasarkan Sub-Pembawa Orto-Gonal." },
    { "en": "Apa Itu 'SDMA' (Space Division Multiple Access)?", "id": "Akses Jamak Berdasarkan Ruang Geografis." },
    { "en": "Apa Itu 'CSMA' (Carrier Sense Multiple Access)?", "id": "Protokol Akses Media Deteksi Pembawa." },
    { "en": "Apa Itu 'LDPC' (Low-Density Parity-Check)?", "id": "Metode Koreksi Kesalahan Data Efisien." },
    { "en": "Apa Itu 'Turbo Code'?", "id": "Teknik Pengkodean Data Performa Tinggi." },
    { "en": "Apa Itu 'QAM' (Quadrature Amplitude Modulation)?", "id": "Modulasi Amplitudo Dan Fase Sekaligus." },
    { "en": "Apa Itu 'QPSK' (Quadrature Phase Shift Keying)?", "id": "Modulasi Pergeseran Fase Empat Kondisi." },
    { "en": "Apa Itu 'BPSK' (Binary Phase Shift Keying)?", "id": "Modulasi Pergeseran Fase Dua Kondisi." },
    { "en": "Apa Itu '8PSK' (Eight Phase Shift Keying)?", "id": "Modulasi Pergeseran Fase Delapan Kondisi." },
    { "en": "Apa Itu '16QAM' (Sixteen Quadrature Amplitude Modulation)?", "id": "Modulasi Dengan Enambelas Titik Konstelasi." },
    { "en": "Apa Itu '64QAM' (Sixty-Four Quadrature Amplitude Modulation)?", "id": "Modulasi Dengan Enampuluhempat Titik Konstelasi." },
    { "en": "Apa Itu '256QAM' (Two-Hundred Fifty-Six Quadrature Amplitude Modulation)?", "id": "Modulasi Dengan Duaratuslimapuluhenam Titik Konstelasi." },
    { "en": "Apa Itu 'DQPSK' (Differential Quadrature Phase Shift Keying)?", "id": "Modulasi Fase Kuadratur Secara Diferensial." },
    { "en": "Apa Itu 'GMSK' (Gaussian Minimum Shift Keying)?", "id": "Modulasi Pergeseran Minimum Filter Gaussian." },
    { "en": "Apa Itu 'CPFSK' (Continuous Phase Frequency Shift Keying)?", "id": "Modulasi Frekuensi Dengan Fase Kontinu." },
    { "en": "Apa Itu 'MSK' (Minimum Shift Keying)?", "id": "Modulasi Frekuensi Dengan Selisih Minimum." },
    { "en": "Apa Itu 'DVI' (Digital Visual Interface)?", "id": "Antarmuka Transmisi Video Digital Kabel." },
    { "en": "Apa Itu 'SODAR' (Sonic Detection And Ranging)?", "id": "Deteksi Atmosfer Menggunakan Gelombang Suara." },
    { "en": "Apa Itu 'Eb/N0' (Energy Per Bit To Noise Power Spectral Density)?", "id": "Rasio Energi Bit Terhadap Derau." },
    { "en": "Apa Itu 'MER' (Modulation Error Ratio)?", "id": "Ukuran Kualitas Sinyal Modulasi Digital." },
    { "en": "Apa Itu 'Viterbi Decoder' (Dekoder Viterbi)?", "id": "Algoritma Dekode Data Terenkode Konvolusi." },
    { "en": "Apa Itu 'IMD' (Intermodulation Distortion)?", "id": "Distorsi Akibat Campuran Dua Sinyal." },
    { "en": "Apa Itu 'MCU' (Multipoint Control Unit)?", "id": "Perangkat Pengelola Konferensi Video Banyak." },
    { "en": "Apa Itu 'DVB-S' (Digital Video Broadcasting-Satellite)?", "id": "Standar Penyiaran Video Digital Satelit." },
    { "en": "Apa Itu 'DVB-T' (Digital Video Broadcasting-Terrestrial)?", "id": "Standar Penyiaran Video Digital Terestrial." },
    { "en": "Apa Itu 'DVB-C' (Digital Video Broadcasting-Cable)?", "id": "Standar Penyiaran Video Digital Kabel." },
    { "en": "Apa Itu 'ATSC' (Advanced Television Systems Committee)?", "id": "Standar Televisi Digital Amerika Utara." },
    { "en": "Apa Itu 'ISDB-T' (Integrated Services Digital Broadcasting-Terrestrial)?", "id": "Standar Televisi Digital Jepang, Brasil." },
    { "en": "Apa Itu 'DTMB' (Digital Terrestrial Multimedia Broadcasting)?", "id": "Standar Televisi Digital Negara Cina." },
    { "en": "Apa Itu 'Supernode' (Simpul Super)?", "id": "Penggabungan Dua Simpul Tegangan Diketahui." },
    { "en": "Apa Itu 'Supermesh' (Mata Jala Super)?", "id": "Penggabungan Dua Loop Arus Diketahui." },
    { "en": "Apa Itu 'Millman's Theorem' (Teorema Millman)?", "id": "Penyederhanaan Banyak Cabang Tegangan Paralel." },
    { "en": "Apa Itu 'Reciprocity Theorem' (Teorema Resiprositas)?", "id": "Pertukaran Posisi Sumber Dan Respon." },
    { "en": "Apa Itu 'Substitution Theorem' (Teorema Substitusi)?", "id": "Penggantian Cabang Dengan Sumber Ekivalen." },
    { "en": "Apa Itu 'Compensation Theorem' (Teorema Kompensasi)?", "id": "Analisis Perubahan Arus Akibat Impedansi." },
    { "en": "Apa Itu 'Tellegen's Theorem' (Teorema Tellegen)?", "id": "Hukum Kekekalan Daya Total Rangkaian." },
    { "en": "Apa Itu 'Dual Circuit' (Rangkaian Dual)?", "id": "Rangkaian Dengan Persamaan Matematika Sama." },
    { "en": "Apa Itu 'Planar Graph' (Graf Planar)?", "id": "Representasi Rangkaian Tanpa Jalur Berpotongan." },
    { "en": "Apa Itu 'Tree' (Pohon Rangkaian)?", "id": "Himpunan Cabang Menghubungkan Semua Simpul." },
    { "en": "Apa Itu 'Co-Tree' (Tali Pohon)?", "id": "Himpunan Cabang Tidak Termasuk Pohon." },
    { "en": "Apa Itu 'Incidence Matrix' (Matriks Insidensi)?", "id": "Matriks Hubungan Antar Cabang, Simpul." },
    { "en": "Apa Itu 'Cut-Set' (Himpunan Potong)?", "id": "Kumpulan Cabang Pemisah Graf Rangkaian." },
    { "en": "Apa Itu 'Tie-Set' (Himpunan Ikat)?", "id": "Kumpulan Cabang Pembentuk Loop Tertutup." },
    { "en": "Apa Itu 'Active Network' (Jaringan Aktif)?", "id": "Rangkaian Yang Memiliki Sumber Energi." },
    { "en": "Apa Itu 'Passive Network' (Jaringan Pasif)?", "id": "Rangkaian Tanpa Adanya Sumber Energi." },
    { "en": "Apa Itu 'Lumped Circuit' (Rangkaian Terpusat)?", "id": "Rangkaian Dengan Dimensi Fisik Kecil." },
    { "en": "Apa Itu 'Distributed Circuit' (Rangkaian Terdistribusi)?", "id": "Rangkaian Dengan Parameter Sepanjang Jalur." },
    { "en": "Apa Itu 'Linear Time-Invariant' (Waktu-Invarian Linear)?", "id": "Sistem Dengan Karakteristik Tetap Waktu." },
    { "en": "Apa Itu 'Driving Point Impedance' (Impedansi Titik Gerak)?", "id": "Rasio Tegangan Terhadap Arus Input." },
    { "en": "Apa Itu 'Transfer Impedance' (Impedansi Transfer)?", "id": "Rasio Tegangan Input Arus Output." },
    { "en": "Apa Itu 'Two-Port Network' (Jaringan Dua Port)?", "id": "Rangkaian Dengan Dua Pasang Terminal." },
    { "en": "Apa Itu 'Z-Parameters' (Parameter Impedansi)?", "id": "Matriks Hubungan Tegangan Arus Port." },
    { "en": "Apa Itu 'Y-Parameters' (Parameter Admitansi)?", "id": "Matriks Hubungan Arus Tegangan Port." },
    { "en": "Apa Itu 'H-Parameters' (Parameter Hibrid)?", "id": "Matriks Campuran Tegangan Dan Arus." },
    { "en": "Apa Itu 'ABCD Parameters' (Parameter Transmisi)?", "id": "Parameter Untuk Analisis Jalur Transmisi." },
    { "en": "Apa Itu 'Symmetrical Network' (Jaringan Simetris)?", "id": "Rangkaian Dengan Karakteristik Port Identik." },
    { "en": "Apa Itu 'Reciprocal Network' (Jaringan Resiprokal)?", "id": "Rangkaian Memenuhi Syarat Teorema Resiprositas." },
    { "en": "Apa Itu 'Characteristic Impedance' (Impedansi Karakteristik)?", "id": "Impedansi Input Jalur Transmisi Tak-Hingga." },
    { "en": "Apa Itu 'Propagation Constant' (Konstanta Propagasi)?", "id": "Parameter Perubahan Amplitudo, Fase Gelombang." },
    { "en": "Apa Itu 'Attenuation Constant' (Konstanta Redaman)?", "id": "Ukuran Penurunan Kekuatan Sinyal Jarak." },
    { "en": "Apa Itu 'Phase Constant' (Konstanta Fase)?", "id": "Ukuran Perubahan Fase Sinyal Jarak." },
    { "en": "Apa Itu 'Image Impedance' (Impedansi Citra)?", "id": "Impedansi Penyesuai Beban Port Rangkaian." },
    { "en": "Apa Itu 'Low Pass Prototype' (Prototipe Lolos Rendah)?", "id": "Desain Filter Dasar Frekuensi Normalisasi." },
    { "en": "Apa Itu 'M-Derived Filter' (Filter Turunan-M)?", "id": "Filter Dengan Karakteristik Pemutusan Tajam." },
    { "en": "Apa Itu 'Constant-K Filter' (Filter Konstanta-K)?", "id": "Filter Dengan Impedansi Produk Tetap." },
    { "en": "Apa Itu 'Bridge-T Network' (Jaringan Jembatan-T)?", "id": "Rangkaian Filter Bentuk Huruf T." },
    { "en": "Apa Itu 'Lattice Network' (Jaringan Kisi)?", "id": "Rangkaian Simetris Berbentuk Struktur Jembatan." },
    { "en": "Apa Itu 'All-Pass Filter' (Filter Lolos-Semua)?", "id": "Filter Pengubah Fase Tanpa Redaman." },
    { "en": "Apa Itu 'Phase Equalizer' (Pemerata Fase)?", "id": "Rangkaian Koreksi Distorsi Fase Sinyal." },
    { "en": "Apa Itu 'Insertion Loss' (Rugi Sisipan)?", "id": "Pengurangan Daya Akibat Komponen Tambahan." },
    { "en": "Apa Itu 'Return Loss' (Rugi Balik)?", "id": "Daya Pantul Akibat Ketidaksesuaian Impedansi." },
    { "en": "Apa Itu 'Group Delay' (Tunda Kelompok)?", "id": "Laju Perubahan Fase Terhadap Frekuensi." },
    { "en": "Apa Itu 'Phase Velocity' (Kecepatan Fase)?", "id": "Laju Rambat Fase Gelombang Tunggal." },
    { "en": "Apa Itu 'Group Velocity' (Kecepatan Kelompok)?", "id": "Laju Rambat Energi Kelompok Gelombang." },
    { "en": "Apa Itu 'Smith Chart' (Diagram Smith)?", "id": "Alat Grafis Analisis Jalur Transmisi." },
    { "en": "Apa Itu 'Stub Matching' (Penyesuaian Stub)?", "id": "Teknik Penyesuaian Impedansi Kabel Pendek." },
    { "en": "Apa Itu 'Quarter-Wave Transformer' (Transformator Seperempat Gelombang)?", "id": "Penyesuai Impedansi Menggunakan Panjang Kabel." },
    { "en": "Apa Itu 'Voltage Reflection Coefficient' (Koefisien Refleksi Tegangan)?", "id": "Rasio Tegangan Pantul Terhadap Datang." },
    { "en": "Apa Itu 'Standing Wave Ratio' (Rasio Gelombang Berdiri)?", "id": "Ukuran Ketidaksesuaian Impedansi Jalur Transmisi." },
    { "en": "Apa Itu 'Maximum Realizable Gain' (Penguatan Maksimal Tercapai)?", "id": "Batas Penguatan Tertinggi Sistem Stabil." },
    { "en": "Apa Itu 'Stability Factor' (Faktor Stabilitas)?", "id": "Ukuran Ketahanan Terhadap Osilasi Liar." },
    { "en": "Apa Itu 'Neutralization Circuit' (Rangkaian Netralisasi)?", "id": "Pencegah Umpan Balik Internal Transistor." },
    { "en": "Apa Itu 'Cascode Amplifier' (Penguat Kaskode)?", "id": "Kombinasi Penguat Emitor, Basis Bersama." },
    { "en": "Apa Itu 'Differential Gain' (Penguatan Diferensial)?", "id": "Penguatan Selisih Dua Sinyal Input." },
    { "en": "Apa Itu 'Common Mode Gain' (Penguatan Mode Bersama)?", "id": "Penguatan Sinyal Rata-Rata Kedua Input." },
    { "en": "Apa Itu 'CMRR' (Common Mode Rejection Ratio)?", "id": "Rasio Penolakan Sinyal Mode Bersama." },
    { "en": "Apa Itu 'Slew Rate' (Laju Slew)?", "id": "Kecepatan Respon Maksimal Tegangan Output." },
    { "en": "Apa Itu 'Gain Bandwidth Product' (Produk Lebar Pita)?", "id": "Konstanta Performa Penguat Operasional Dasar." },
    { "en": "Apa Itu 'Input Offset Voltage' (Tegangan Ofset Input)?", "id": "Tegangan Input Penyeimbang Output Nol." },
    { "en": "Apa Itu 'Input Bias Current' (Arus Bias Input)?", "id": "Arus Rata-Rata Pada Terminal Input." },
    { "en": "Apa Itu 'Input Impedance' (Impedansi Input)?", "id": "Hambatan Terlihat Dari Sisi Input." },
    { "en": "Apa Itu 'Output Impedance' (Impedansi Output)?", "id": "Hambatan Terlihat Dari Sisi Output." },
    { "en": "Apa Itu 'Open Loop Gain' (Penguatan Loop Terbuka)?", "id": "Penguatan Tanpa Jalur Umpan Balik." },
    { "en": "Apa Itu 'Closed Loop Gain' (Penguatan Loop Tertutup)?", "id": "Penguatan Dengan Jalur Umpan Balik." },
    { "en": "Apa Itu 'Loop Gain' (Penguatan Loop)?", "id": "Total Penguatan Melalui Seluruh Lintasan." },
    { "en": "Apa Itu 'Phase Margin' (Margin Fase)?", "id": "Jarak Fase Ke Titik Ketidakstabilan." },
    { "en": "Apa Itu 'Gain Margin' (Margin Penguatan)?", "id": "Jarak Penguatan Ke Titik Ketidakstabilan." },
    { "en": "Apa Itu 'Barkhausen Criterion' (Kriteria Barkhausen)?", "id": "Syarat Terjadinya Osilasi Sinyal Listrik." },
    { "en": "Apa Itu 'Hartley Oscillator' (Osilator Hartley)?", "id": "Osilator Menggunakan Pembagi Tegangan Induktif." },
    { "en": "Apa Itu 'Colpitts Oscillator' (Osilator Colpitts)?", "id": "Osilator Menggunakan Pembagi Tegangan Kapasitif." },
    { "en": "Apa Itu 'Clapp Oscillator' (Osilator Clapp)?", "id": "Variasi Osilator Colpitts Stabilitas Tinggi." },
    { "en": "Apa Itu 'Wien Bridge Oscillator' (Osilator Jembatan Wien)?", "id": "Osilator Penghasil Gelombang Sinus Rendah." },
    { "en": "Apa Itu 'Phase Shift Oscillator' (Osilator Pergeseran Fase)?", "id": "Osilator Berbasis Jaringan Resistor Kapasitor." },
    { "en": "Apa Itu 'Miller Effect' (Efek Miller)?", "id": "Peningkatan Kapasitansi Input Akibat Penguatan." },
    { "en": "Apa Itu 'Bootstrap Circuit' (Rangkaian Bootstrap)?", "id": "Teknik Peningkatan Impedansi Input Rangkaian." },
    { "en": "Apa Itu 'Current Mirror' (Cermin Arus)?", "id": "Rangkaian Penyalin Nilai Arus Input." },
    { "en": "Apa Itu 'Active Load' (Beban Aktif)?", "id": "Penggunaan Transistor Sebagai Hambatan Beban." },
    { "en": "Apa Itu 'Bandgap Reference' (Referensi Bandgap)?", "id": "Referensi Tegangan Kompensasi Perubahan Suhu." },
    { "en": "Apa Itu 'Voltage Doubler' (Pengganda Tegangan)?", "id": "Rangkaian Penyearah Output Dua Kali." },
    { "en": "Apa Itu 'Voltage Tripler' (Penghasil Tegangan Tiga)?", "id": "Rangkaian Penyearah Output Tiga Kali." },
    { "en": "Apa Itu 'Precision Rectifier' (Penyearah Presisi)?", "id": "Penyearah Sinyal Kecil Menggunakan Op-Amp." },
    { "en": "Apa Itu 'Logarithmic Amplifier' (Penguat Logaritmik)?", "id": "Penguat Dengan Output Logaritma Input." },
    { "en": "Apa Itu 'Antilogarithmic Amplifier' (Penguat Antilogaritmik)?", "id": "Penguat Dengan Output Eksponensial Input." },
    { "en": "Apa Itu 'Peak Detector' (Pendeteksi Puncak)?", "id": "Rangkaian Penahan Nilai Maksimum Sinyal." },
    { "en": "Apa Itu 'Sample And Hold' (Sampel Dan Tahan)?", "id": "Rangkaian Penahan Nilai Sinyal Analog." },
    { "en": "Apa Itu 'Analog Switch' (Saklar Analog)?", "id": "Saklar Elektronik Untuk Sinyal Kontinu." },
    { "en": "Apa Itu 'Analog Multiplexer' (Multiplekser Analog)?", "id": "Pemilih Jalur Sinyal Analog Banyak." },
    { "en": "Apa Itu 'Phase Locked Loop' (Loop Terkunci Fase)?", "id": "Sistem Sinkronisasi Fase Sinyal Output." },
    { "en": "Apa Itu 'Voltage Controlled Oscillator' (Osilator Terkendali Tegangan)?", "id": "Osilator Dengan Frekuensi Variabel Tegangan." },
    { "en": "Apa Itu 'Frequency Synthesizer' (Sintesis Frekuensi)?", "id": "Pembangkit Banyak Frekuensi Referensi Tunggal." },
    { "en": "Apa Itu 'Digital Potentiometer' (Potensiometer Digital)?", "id": "Resistor Variabel Terkendali Sinyal Digital." },
    { "en": "Apa Itu 'Instrumentation Amplifier' (Penguat Instrumentasi)?", "id": "Penguat Diferensial Presisi Impedansi Tinggi." },
    { "en": "Apa Itu 'Isolation Amplifier' (Penguat Isolasi)?", "id": "Penguat Dengan Pemisahan Elektrik Total." },
    { "en": "Apa Itu 'Chopper Amplifier' (Penguat Pencacah)?", "id": "Penguat Sinyal Searah Sangat Stabil." },
    { "en": "Apa Itu 'Programmable Gain Amplifier' (Penguat Penguatan Terprogram)?", "id": "Penguat Dengan Nilai Penguatan Digital." },
    { "en": "Apa Itu 'Current Feedback Op-Amp' (Op-Amp Umpan Balik Arus)?", "id": "Penguat Operasional Kecepatan Tinggi Khusus." },
    { "en": "Apa Itu 'Transconductance Amplifier' (Penguat Transkonduktansi)?", "id": "Penguat Output Arus Input Tegangan." },
    { "en": "Apa Itu 'Transimpedance Amplifier' (Penguat Transimpedansi)?", "id": "Penguat Output Tegangan Input Arus." },
    { "en": "Apa Itu 'LSB' (Least Significant Bit)?", "id": "Posisi Bit Dengan Nilai Terendah." },
    { "en": "Apa Itu 'MSB' (Most Significant Bit)?", "id": "Posisi Bit Dengan Nilai Tertinggi." },
    { "en": "Apa Itu 'Binary' (Sistem Bilangan Biner)?", "id": "Sistem Bilangan Berbasis Dua Digital." },
    { "en": "Apa Itu 'Hexadecimal' (Sistem Bilangan Heksadesimal)?", "id": "Sistem Bilangan Berbasis Enam Belas." },
    { "en": "Apa Itu 'Octal' (Sistem Bilangan Oktal)?", "id": "Sistem Bilangan Berbasis Delapan Digital." },
    { "en": "Apa Itu 'BCD' (Binary Coded Decimal)?", "id": "Representasi Desimal Dalam Bentuk Biner." },
    { "en": "Apa Itu 'Boolean Algebra' (Aljabar Boolean)?", "id": "Matematika Logika Operasi Digital Dasar." },
    { "en": "Apa Itu 'Truth Table' (Tabel Kebenaran)?", "id": "Tabel Representasi Output Logika Digital." },
    { "en": "Apa Itu 'K-Map' (Karnaugh Map)?", "id": "Metode Penyederhanaan Fungsi Logika Visual." },
    { "en": "Apa Itu 'Logic Gate' (Gerbang Logika)?", "id": "Blok Dasar Pembentuk Sirkuit Digital." },
    { "en": "Apa Itu 'Combinational Logic' (Logika Kombinasional)?", "id": "Output Tergantung Pada Input Sekarang." },
    { "en": "Apa Itu 'Sequential Logic' (Logika Sekuensial)?", "id": "Output Tergantung Input Dan Memori." },
    { "en": "Apa Itu 'Flip-Flop' (Sirkuit Flip-Flop)?", "id": "Elemen Penyimpan Data Satu Bit." },
    { "en": "Apa Itu 'SR Flip-Flop' (Set-Reset Flip-Flop)?", "id": "Flip-Flop Dengan Input Set, Reset." },
    { "en": "Apa Itu 'JK Flip-Flop' (Jack-Kilby Flip-Flop)?", "id": "Flip-Flop Universal Dengan Kondisi Toggle." },
    { "en": "Apa Itu 'D Flip-Flop' (Data Flip-Flop)?", "id": "Flip-Flop Penunda Data Satu Detak." },
    { "en": "Apa Itu 'T Flip-Flop' (Toggle Flip-Flop)?", "id": "Flip-Flop Pengubah Status Setiap Detak." },
    { "en": "Apa Itu 'Master-Slave Flip-Flop'?", "id": "Dua Flip-Flop Penghilang Kondisi Balapan." },
    { "en": "Apa Itu 'Race Around Condition' (Kondisi Balapan)?", "id": "Kesalahan Status Output Akibat Pulsa." },
    { "en": "Apa Itu 'Latch' (Sirkuit Grendel)?", "id": "Penyimpan Data Berdasarkan Level Tegangan." },
    { "en": "Apa Itu 'Counter' (Sirkuit Pencacah)?", "id": "Rangkaian Digital Penghitung Pulsa Masuk." },
    { "en": "Apa Itu 'Asynchronous Counter' (Pencacah Asinkron)?", "id": "Pencacah Dengan Detak Berantai Berurutan." },
    { "en": "Apa Itu 'Synchronous Counter' (Pencacah Sinkron)?", "id": "Pencacah Dengan Detak Masuk Bersamaan." },
    { "en": "Apa Itu 'Up Counter' (Pencacah Naik)?", "id": "Pencacah Nilai Dari Rendah Tinggi." },
    { "en": "Apa Itu 'Down Counter' (Pencacah Turun)?", "id": "Pencacah Nilai Dari Tinggi Rendah." },
    { "en": "Apa Itu 'Modulo Counter' (Pencacah Modulo)?", "id": "Pencacah Dengan Batas Bilangan Tertentu." },
    { "en": "Apa Itu 'Ring Counter' (Pencacah Cincin)?", "id": "Register Geser Dengan Output Terhubung." },
    { "en": "Apa Itu 'Johnson Counter' (Pencacah Johnson)?", "id": "Pencacah Cincin Dengan Output Terbalik." },
    { "en": "Apa Itu 'Register' (Register Digital)?", "id": "Kumpulan Flip-Flop Penyimpan Data Bit." },
    { "en": "Apa Itu 'Shift Register' (Register Geser)?", "id": "Register Pemindah Bit Secara Berurutan." },
    { "en": "Apa Itu 'SISO' (Serial In Serial Out)?", "id": "Masukan Serial Dan Keluaran Serial." },
    { "en": "Apa Itu 'SIPO' (Serial In Parallel Out)?", "id": "Masukan Serial Dan Keluaran Paralel." },
    { "en": "Apa Itu 'PISO' (Parallel In Serial Out)?", "id": "Masukan Paralel Dan Keluaran Serial." },
    { "en": "Apa Itu 'PIPO' (Parallel In Parallel Out)?", "id": "Masukan Paralel Dan Keluaran Paralel." },
    { "en": "Apa Itu 'Multiplexer' (Pemilih Data)?", "id": "Pemilih Satu Output Dari Banyak." },
    { "en": "Apa Itu 'Demultiplexer' (Penyalur Data)?", "id": "Penyalur Satu Input Ke Banyak." },
    { "en": "Apa Itu 'Encoder' (Penyandi Digital)?", "id": "Pengubah Sinyal Aktif Menjadi Biner." },
    { "en": "Apa Itu 'Decoder' (Pendekode Digital)?", "id": "Pengubah Kode Biner Menjadi Sinyal." },
    { "en": "Apa Itu 'Priority Encoder' (Penyandi Prioritas)?", "id": "Penyandi Berdasarkan Nilai Input Tertinggi." },
    { "en": "Apa Itu 'Half Adder' (Penjumlah Setengah)?", "id": "Penjumlah Dua Bit Tanpa Carry." },
    { "en": "Apa Itu 'Full Adder' (Penjumlah Penuh)?", "id": "Penjumlah Tiga Bit Termasuk Carry." },
    { "en": "Apa Itu 'Half Subtractor' (Pengurang Setengah)?", "id": "Pengurang Dua Bit Tanpa Borrow." },
    { "en": "Apa Itu 'Full Subtractor' (Pengurang Penuh)?", "id": "Pengurang Tiga Bit Termasuk Borrow." },
    { "en": "Apa Itu 'Look-Ahead Carry Adder'?", "id": "Penjumlah Cepat Dengan Prediksi Carry." },
    { "en": "Apa Itu 'Magnitude Comparator' (Komparator Magnitudo)?", "id": "Pembanding Besaran Dua Bilangan Biner." },
    { "en": "Apa Itu 'Parity Generator' (Generator Paritas)?", "id": "Pembangkit Bit Pemeriksa Kesalahan Data." },
    { "en": "Apa Itu 'Parity Checker' (Pemeriksa Paritas)?", "id": "Pendeteksi Kesalahan Bit Saat Transmisi." },
    { "en": "Apa Itu 'PLA' (Programmable Logic Array)?", "id": "Sirkuit Logika Dengan Gerbang Terprogram." },
    { "en": "Apa Itu 'PAL' (Programmable Array Logic)?", "id": "Logika Array Dengan Output Tetap." },
    { "en": "Apa Itu 'CPLD' (Complex Programmable Logic Device)?", "id": "Kumpulan Blok Logika Terprogram Kompleks." },
    { "en": "Apa Itu 'FPGA' (Field Programmable Gate Array)?", "id": "Sirkuit Terintegrasi Fleksibel Dapat Diprogram." },
    { "en": "Apa Itu 'VHDL' (VHSIC Hardware Description Language)?", "id": "Bahasa Pemodelan Perangkat Keras Digital." },
    { "en": "Apa Itu 'Verilog' (Bahasa Pemodelan Verilog)?", "id": "Bahasa Deskripsi Standar Desain Digital." },
    { "en": "Apa Itu 'Fan-In' (Beban Masukan)?", "id": "Jumlah Maksimal Input Gerbang Logika." },
    { "en": "Apa Itu 'Fan-Out' (Beban Keluaran)?", "id": "Jumlah Maksimal Output Gerbang Logika." },
    { "en": "Apa Itu 'Propagation Delay' (Tunda Rambat)?", "id": "Waktu Sinyal Melewati Gerbang Logika." },
    { "en": "Apa Itu 'Power Dissipation' (Disipasi Daya)?", "id": "Daya Terbuang Menjadi Panas Chip." },
    { "en": "Apa Itu 'Noise Margin' (Margin Derau)?", "id": "Batas Toleransi Gangguan Sinyal Digital." },
    { "en": "Apa Itu 'TTL' (Transistor-Transistor Logic)?", "id": "Keluarga Logika Digital Berbasis Transistor." },
    { "en": "Apa Itu 'CMOS' (Complementary Metal-Oxide-Semiconductor)?", "id": "Teknologi IC Dengan Konsumsi Daya Rendah." },
    { "en": "Apa Itu 'ECL' (Emitter-Coupled Logic)?", "id": "Logika Digital Dengan Kecepatan Tertinggi." },
    { "en": "Apa Itu 'Tri-State Logic' (Logika Tiga Status)?", "id": "Status Logika Tinggi, Rendah, Impedansi." },
    { "en": "Apa Itu 'High Impedance State' (Status Impedansi Tinggi)?", "id": "Kondisi Output Terputus Secara Elektrik." },
    { "en": "Apa Itu 'Open Collector' (Kolektor Terbuka)?", "id": "Output Transistor Tanpa Resistor Internal." },
    { "en": "Apa Itu 'Pull-Up Resistor' (Resistor Tarik Atas)?", "id": "Resistor Penjamin Level Tegangan Tinggi." },
    { "en": "Apa Itu 'Pull-Down Resistor' (Resistor Tarik Bawah)?", "id": "Resistor Penjamin Level Tegangan Rendah." },
    { "en": "Apa Itu 'Schmitt Trigger' (Pemicu Schmitt)?", "id": "Gerbang Logika Dengan Efek Histeresis." },
    { "en": "Apa Itu 'Monostable Multivibrator'?", "id": "Rangkaian Dengan Satu Kondisi Stabil." },
    { "en": "Apa Itu 'Astable Multivibrator'?", "id": "Rangkaian Pembangkit Gelombang Kotak Kontinu." },
    { "en": "Apa Itu 'Bistable Multivibrator'?", "id": "Rangkaian Dengan Dua Kondisi Stabil." },
    { "en": "Apa Itu 'SRAM' (Static Random Access Memory)?", "id": "Memori RAM Statis Tanpa Refresh." },
    { "en": "Apa Itu 'DRAM' (Dynamic Random Access Memory)?", "id": "Memori RAM Dinamis Dengan Refresh." },
    { "en": "Apa Itu 'PROM' (Programmable Read Only Memory)?", "id": "Memori ROM Yang Dapat Diprogram." },
    { "en": "Apa Itu 'EPROM' (Erasable Programmable Read Only Memory)?", "id": "Memori ROM Dapat Dihapus Cahaya." },
    { "en": "Apa Itu 'Flash Memory' (Memori Flash)?", "id": "Penyimpanan Data Non-Volatil Kecepatan Tinggi." },
    { "en": "Apa Itu 'Cache Memory' (Memori Cache)?", "id": "Memori Penyangga Kecepatan Sangat Tinggi." },
    { "en": "Apa Itu 'Virtual Memory' (Memori Virtual)?", "id": "Teknik Perluasan Memori Menggunakan Disk." },
    { "en": "Apa Itu 'FIFO' (First In First Out)?", "id": "Metode Antrean Data Masuk Pertama." },
    { "en": "Apa Itu 'LIFO' (Last In First Out)?", "id": "Metode Antrean Data Masuk Terakhir." },
    { "en": "Apa Itu 'Address Bus' (Bus Alamat)?", "id": "Jalur Penentu Lokasi Data Memori." },
    { "en": "Apa Itu 'Data Bus' (Bus Data)?", "id": "Jalur Perpindahan Data Antar Komponen." },
    { "en": "Apa Itu 'Control Bus' (Bus Kendali)?", "id": "Jalur Perintah Operasi Sistem Komputer." },
    { "en": "Apa Itu 'Instruction Set' (Set Instruksi)?", "id": "Kumpulan Perintah Dasar Bagi Prosesor." },
    { "en": "Apa Itu 'Opcode' (Operation Code)?", "id": "Bagian Instruksi Penentu Jenis Operasi." },
    { "en": "Apa Itu 'Operand' (Data Operasi)?", "id": "Data Yang Diolah Oleh Instruksi." },
    { "en": "Apa Itu 'Program Counter' (Pencacah Program)?", "id": "Register Penunjuk Alamat Instruksi Selanjutnya." },
    { "en": "Apa Itu 'Stack Pointer' (Penunjuk Tumpukan)?", "id": "Register Penunjuk Alamat Tumpukan Memori." },
    { "en": "Apa Itu 'Accumulator' (Akumulator)?", "id": "Register Utama Penyimpan Hasil Operasi." },
    { "en": "Apa Itu 'Interrupt' (Interupsi Sistem)?", "id": "Sinyal Penghenti Sementara Program Utama." },
    { "en": "Apa Itu 'DMA' (Direct Memory Access)?", "id": "Transfer Data Langsung Tanpa CPU." },
    { "en": "Apa Itu 'Pipelining' (Pemrosesan Jalur Pipa)?", "id": "Teknik Eksekusi Instruksi Secara Simultan." },
    { "en": "Apa Itu 'CISC' (Complex Instruction Set Computer)?", "id": "Arsitektur Prosesor Dengan Instruksi Kompleks." },
    { "en": "Apa Itu 'RISC' (Reduced Instruction Set Computer)?", "id": "Arsitektur Prosesor Dengan Instruksi Sederhana." },
    { "en": "Apa Itu 'AFCI' (Arc Fault Circuit Interrupter)?", "id": "Pemutus Sirkuit Akibat Percikan Api." },
    { "en": "Apa Itu 'GFCI' (Ground Fault Circuit Interrupter)?", "id": "Pemutus Sirkuit Akibat Kebocoran Tanah." },
    { "en": "Apa Itu 'MOV' (Metal Oxide Varistor)?", "id": "Komponen Pelindung Lonjakan Tegangan Tinggi." },
    { "en": "Apa Itu 'GDT' (Gas Discharge Tube)?", "id": "Tabung Pelindung Surja Gas Mulia." },
    { "en": "Apa Itu 'TVS' (Transient Voltage Suppressor)?", "id": "Dioda Penekan Lonjakan Tegangan Transien." },
    { "en": "Apa Itu 'SELV' (Safety Extra Low Voltage)?", "id": "Sistem Tegangan Rendah Sangat Aman." },
    { "en": "Apa Itu 'PELV' (Protective Extra Low Voltage)?", "id": "Sistem Tegangan Rendah Dengan Proteksi." },
    { "en": "Apa Itu 'FELV' (Functional Extra Low Voltage)?", "id": "Tegangan Rendah Untuk Fungsi Tertentu." },
    { "en": "Apa Itu 'IP67' (Ingress Protection 67)?", "id": "Standar Tahan Debu, Rendaman Air." },
    { "en": "Apa Itu 'IP68' (Ingress Protection 68)?", "id": "Standar Tahan Debu, Tekanan Air." },
    { "en": "Apa Itu 'NEC' (National Electrical Code)?", "id": "Standar Instalasi Listrik Amerika Serikat." },
    { "en": "Apa Itu 'ANSI' (American National Standards Institute)?", "id": "Lembaga Standar Nasional Amerika Serikat." },
    { "en": "Apa Itu 'VDE' (Verband Der Elektrotechnik)?", "id": "Standar Teknologi Elektrik Negara Jerman." },
    { "en": "Apa Itu 'NFPA' (National Fire Protection Association)?", "id": "Asosiasi Nasional Perlindungan Kebakaran Amerika." },
    { "en": "Apa Itu 'OSHA' (Occupational Safety And Health Administration)?", "id": "Lembaga Kesehatan, Keselamatan Kerja Amerika." },
    { "en": "Apa Itu 'Arc Flash Suit' (Pakaian Kilatan Busur)?", "id": "Pakaian Pelindung Ledakan Cahaya Panas." },
    { "en": "Apa Itu 'Dielectric Boots' (Sepatu Dielektrik)?", "id": "Sepatu Isolasi Khusus Pekerja Listrik." },
    { "en": "Apa Itu 'Insulated Gloves' (Sarung Tangan Isolasi)?", "id": "Sarung Tangan Pelindung Sengatan Listrik." },
    { "en": "Apa Itu 'Voltage Detector' (Pendeteksi Tegangan)?", "id": "Alat Cek Keberadaan Tegangan Listrik." },
    { "en": "Apa Itu 'Non-Contact Voltage Tester' (Penguji Tegangan Tanpa Kontak)?", "id": "Sensor Tegangan Tanpa Sentuhan Kabel." },
    { "en": "Apa Itu 'Phase Rotation Meter' (Meter Rotasi Fase)?", "id": "Alat Cek Urutan Fase Listrik." },
    { "en": "Apa Itu 'Rogowski Coil' (Kumparan Rogowski)?", "id": "Sensor Arus Bolak-Balik Fleksibel Presisi." },
    { "en": "Apa Itu 'Fluxgate Sensor' (Sensor Fluxgate)?", "id": "Sensor Medan Magnet Sangat Sensitif." },
    { "en": "Apa Itu 'Eddy Current Sensor' (Sensor Arus Eddy)?", "id": "Sensor Pendeteksi Jarak Logam Induktif." },
    { "en": "Apa Itu 'Capacitive Displacement Sensor' (Sensor Pergeseran Kapasitif)?", "id": "Sensor Pengukur Posisi Sangat Akurat." },
    { "en": "Apa Itu 'Inductive Displacement Sensor' (Sensor Pergeseran Induktif)?", "id": "Sensor Pengukur Jarak Berbasis Magnet." },
    { "en": "Apa Itu 'Laser Doppler Vibrometer' (Vibrometer Doppler Laser)?", "id": "Alat Ukur Getaran Tanpa Kontak." },
    { "en": "Apa Itu 'Pyrometer' (Pirometer Optik)?", "id": "Alat Ukur Suhu Radiasi Panas." },
    { "en": "Apa Itu 'Spectrophotometer' (Spektrofotometer)?", "id": "Alat Ukur Intensitas Spektrum Cahaya." },
    { "en": "Apa Itu 'Oscilloscope Probe' (Probe Osiloskop)?", "id": "Kabel Penghubung Sinyal Ke Osiloskop." },
    { "en": "Apa Itu 'Differential Probe' (Probe Diferensial)?", "id": "Probe Pengukur Selisih Dua Titik." },
    { "en": "Apa Itu 'Current Probe' (Probe Arus)?", "id": "Probe Pengubah Arus Menjadi Tegangan." },
    { "en": "Apa Itu 'High Voltage Probe' (Probe Tegangan Tinggi)?", "id": "Probe Khusus Untuk Tegangan Ribuan." },
    { "en": "Apa Itu 'Data Logger' (Pencatat Data)?", "id": "Alat Perekam Data Parameter Elektrik." },
    { "en": "Apa Itu 'Remote I/O' (Input Output Jarak Jauh)?", "id": "Modul Antarmuka Data Lokasi Berbeda." },
    { "en": "Apa Itu 'Fieldbus' (Bus Lapangan)?", "id": "Jaringan Komunikasi Perangkat Industri Lapangan." },
    { "en": "Apa Itu 'DeviceNet' (Jaringan Perangkat)?", "id": "Protokol Komunikasi Industri Berbasis CAN." },
    { "en": "Apa Itu 'ControlNet' (Jaringan Kontrol)?", "id": "Protokol Komunikasi Kontrol Industri Cepat." },
    { "en": "Apa Itu 'HART' (Highway Addressable Remote Transducer)?", "id": "Protokol Komunikasi Sensor Cerdas Industri." },
    { "en": "Apa Itu 'WirelessHART' (HART Nirkabel)?", "id": "Protokol Komunikasi Sensor Industri Nirkabel." },
    { "en": "Apa Itu 'ISA100.11a' (Standar Jaringan Nirkabel Industri)?", "id": "Standar Komunikasi Nirkabel Otomasi Industri." },
    { "en": "Apa Itu 'Z-Wave' (Gelombang Z)?", "id": "Protokol Nirkabel Untuk Otomasi Rumah." },
    { "en": "Apa Itu 'Thread' (Protokol Jaringan Thread)?", "id": "Protokol Jaringan Nirkabel IP Rendah." },
    { "en": "Apa Itu 'Matter' (Standar Konektivitas Matter)?", "id": "Standar Universal Perangkat Rumah Pintar." },
    { "en": "Apa Itu 'AMQP' (Advanced Message Queuing Protocol)?", "id": "Protokol Pertukaran Pesan Antar Sistem." },
    { "en": "Apa Itu 'WebSocket' (Protokol Komunikasi Dua Arah)?", "id": "Saluran Komunikasi Data Interaktif Nyata." },
    { "en": "Apa Itu 'REST' (Representational State Transfer)?", "id": "Arsitektur Layanan Web Berbasis HTTP." },
    { "en": "Apa Itu 'SOAP' (Simple Object Access Protocol)?", "id": "Protokol Pertukaran Informasi Berbasis XML." },
    { "en": "Apa Itu 'JSON' (JavaScript Object Notation)?", "id": "Format Pertukaran Data Ringan Teks." },
    { "en": "Apa Itu 'XML' (Extensible Markup Language)?", "id": "Bahasa Penanda Untuk Struktur Data." },
    { "en": "Apa Itu 'Base64' (Pengkodean Base64)?", "id": "Sistem Pengkodean Data Biner Teks." },
    { "en": "Apa Itu 'IEEE 754' (Standar Bilangan Titik Mengambang)?", "id": "Standar Format Bilangan Komputer Digital." },
    { "en": "Apa Itu 'Fixed-Point' (Titik Tetap)?", "id": "Format Representasi Bilangan Pecahan Tetap." },
    { "en": "Apa Itu 'Floating-Point' (Titik Mengambang)?", "id": "Format Representasi Bilangan Pecahan Dinamis." },
    { "en": "Apa Itu 'ALU' (Arithmetic Logic Unit)?", "id": "Unit Pemroses Operasi Matematika Prosesor." },
    { "en": "Apa Itu 'Pipeline' (Jalur Pipa)?", "id": "Teknik Pemrosesan Instruksi Secara Simultan." },
    { "en": "Apa Itu 'L1 Cache' (Memori Cache Level 1)?", "id": "Memori Internal Prosesor Paling Cepat." },
    { "en": "Apa Itu 'L2 Cache' (Memori Cache Level 2)?", "id": "Memori Penyangga Prosesor Kapasitas Menengah." },
    { "en": "Apa Itu 'L3 Cache' (Memori Cache Level 3)?", "id": "Memori Penyangga Bersama Antar Inti." },
    { "en": "Apa Itu 'TLB' (Translation Lookside Buffer)?", "id": "Cache Untuk Percepat Translasi Memori." },
    { "en": "Apa Itu 'MMU' (Memory Management Unit)?", "id": "Unit Pengelola Alamat Memori Fisik." },
    { "en": "Apa Itu 'Paging' (Sistem Halaman)?", "id": "Metode Manajemen Memori Virtual Komputer." },
    { "en": "Apa Itu 'Segmentation' (Segmentasi Memori)?", "id": "Pembagian Memori Menjadi Bagian Logis." },
    { "en": "Apa Itu 'BIOS' (Basic Input Output System)?", "id": "Program Awal Pengaktif Perangkat Keras." },
    { "en": "Apa Itu 'UEFI' (Unified Extensible Firmware Interface)?", "id": "Antarmuka Firmware Modern Pengganti BIOS." },
    { "en": "Apa Itu 'POST' (Power-On Self-Test)?", "id": "Proses Pengecekan Mandiri Saat Dinyalakan." },
    { "en": "Apa Itu 'Kernel' (Inti Sistem)?", "id": "Program Utama Penghubung Software Hardware." },
    { "en": "Apa Itu 'Mutex' (Mutual Exclusion)?", "id": "Mekanisme Sinkronisasi Akses Data Bersama." },
    { "en": "Apa Itu 'Semaphore' (Semafor)?", "id": "Variabel Kendali Akses Sumber Daya." },
    { "en": "Apa Itu 'Deadlock' (Kebuntuan Sistem)?", "id": "Kondisi Proses Saling Menunggu Selamanya." },
    { "en": "Apa Itu 'Race Condition' (Kondisi Balapan)?", "id": "Kesalahan Hasil Akibat Urutan Eksekusi." },
    { "en": "Apa Itu 'Critical Section' (Bagian Kritis)?", "id": "Bagian Kode Pengakses Data Bersama." },
    { "en": "Apa Itu 'Overclocking' (Peningkatan Kecepatan)?", "id": "Meningkatkan Kecepatan Detak Di Atas." },
    { "en": "Apa Itu 'Thermal Throttling' (Pembatasan Panas)?", "id": "Penurunan Kecepatan Untuk Kurangi Panas." },
    { "en": "Apa Itu 'Safe Mode' (Mode Aman)?", "id": "Mode Operasi Minimal Untuk Diagnosa." },
    { "en": "Apa Itu 'Kernel Panic' (Panik Inti)?", "id": "Kesalahan Fatal Pada Sistem Operasi." },
    { "en": "Apa Itu 'Blue Screen Of Death' (Layar Biru Kematian)?", "id": "Tampilan Kesalahan Sistem Kritis Windows." },
    { "en": "Apa Itu 'Rooting' (Akses Akar)?", "id": "Mendapatkan Akses Kendali Sistem Tertinggi." },
    { "en": "Apa Itu 'Jailbreaking' (Pembobolan Penjara)?", "id": "Menghilangkan Pembatasan Perangkat Lunak Produsen." },
    { "en": "Apa Itu 'Sideloading' (Pemuatan Samping)?", "id": "Memasang Aplikasi Di Luar Toko." },
    { "en": "Apa Itu 'Malware' (Perangkat Lunak Berbahaya)?", "id": "Program Perusak Atau Pencuri Data." },
    { "en": "Apa Itu 'Ransomware' (Perangkat Lunak Tebusan)?", "id": "Program Pengunci Data Meminta Tebusan." },
    { "en": "Apa Itu 'Spyware' (Perangkat Lunak Mata-Mata)?", "id": "Program Pemantau Aktivitas Pengguna Diam-Diam." },
    { "en": "Apa Itu 'Phishing' (Pengelabuan)?", "id": "Pencurian Identitas Melalui Komunikasi Palsu." },
    { "en": "Apa Itu 'Trojan Horse' (Kuda Troya)?", "id": "Program Berbahaya Menyamar Sebagai Legal." },
    { "en": "Apa Itu 'Backdoor' (Pintu Belakang)?", "id": "Akses Rahasia Lewati Keamanan Sistem." },
    { "en": "Apa Itu 'SQL Injection' (Injeksi SQL)?", "id": "Serangan Manipulasi Database Melalui Input." },
    { "en": "Apa Itu 'Cross-Site Scripting' (Skrip Antar Situs)?", "id": "Injeksi Skrip Berbahaya Ke Halaman." },
    { "en": "Apa Itu 'DDoS' (Distributed Denial Of Service)?", "id": "Serangan Melumpuhkan Server Dengan Trafik." },
    { "en": "Apa Itu 'Brute Force Attack' (Serangan Paksa Brute)?", "id": "Menebak Kata Sandi Secara Berulang." },
    { "en": "Apa Itu 'Salt' (Garam Sandi)?", "id": "Data Acak Tambahan Untuk Enkripsi." },
    { "en": "Apa Itu 'Hash Function' (Fungsi Hash)?", "id": "Algoritma Pengubah Data Menjadi Unik." },
    { "en": "Apa Itu 'Two-Factor Authentication' (Autentikasi Dua Faktor)?", "id": "Verifikasi Identitas Melalui Dua Cara." },
    { "en": "Apa Itu 'Digital Certificate' (Sertifikat Digital)?", "id": "Dokumen Elektronik Pembuktian Identitas Online." },
    { "en": "Apa Itu 'Certificate Authority' (Otoritas Sertifikat)?", "id": "Lembaga Penerbit Sertifikat Keamanan Digital." },
    { "en": "Apa Itu 'Private Key' (Kunci Pribadi)?", "id": "Kunci Rahasia Untuk Mendekripsi Data." },
    { "en": "Apa Itu 'Public Key' (Kunci Publik)?", "id": "Kunci Terbuka Untuk Mengenkripsi Data." },
    { "en": "Apa Itu 'Zero-Day Vulnerability' (Kerentanan Hari Nol)?", "id": "Celah Keamanan Belum Diketahui Vendor." }



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
