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


  { "en": "Dasar negara Indonesia?", "id": "Pancasila." },
  { "en": "Ideologi Kepolisian Negara Republik Indonesia?", "id": "Pancasila." },
  { "en": "Sila pertama Pancasila?", "id": "Ketuhanan Yang Maha Esa." },
  { "en": "Lambang sila pertama?", "id": "Bintang emas." },
  { "en": "Makna sila pertama bagi Polri?", "id": "Beriman dan bertakwa kepada Tuhan." },
  { "en": "Sila kedua Pancasila?", "id": "Kemanusiaan yang adil dan beradab." },
  { "en": "Lambang sila kedua?", "id": "Rantai emas." },
  { "en": "Makna sila kedua bagi Polri?", "id": "Menjunjung tinggi hak asasi manusia." },
  { "en": "Sila ketiga Pancasila?", "id": "Persatuan Indonesia." },
  { "en": "Lambang sila ketiga?", "id": "Pohon beringin." },
  { "en": "Makna sila ketiga bagi Polri?", "id": "Menjaga keutuhan NKRI." },
  { "en": "Sila keempat Pancasila?", "id": "Kerakyatan yang dipimpin oleh hikmat." },
  { "en": "Lambang sila keempat?", "id": "Kepala banteng." },
  { "en": "Makna sila keempat bagi Polri?", "id": "Mengutamakan musyawarah untuk mufakat." },
  { "en": "Sila kelima Pancasila?", "id": "Keadilan sosial bagi seluruh rakyat." },
  { "en": "Lambang sila kelima?", "id": "Padi dan kapas." },
  { "en": "Makna sila kelima bagi Polri?", "id": "Berlaku adil tanpa diskriminasi." },
  { "en": "Pedoman hidup Polri?", "id": "Tri Brata." },
  { "en": "Berapa isi Tri Brata?", "id": "Tiga butir janji." },
  { "en": "Inti Tri Brata pertama?", "id": "Berbakti pada nusa dan bangsa." },
  { "en": "Inti Tri Brata kedua?", "id": "Menjunjung tinggi kebenaran dan keadilan." },
  { "en": "Inti Tri Brata ketiga?", "id": "Melindungi, mengayomi, melayani masyarakat." },
  { "en": "Tujuan Tri Brata?", "id": "Membentuk Polisi berakhlak mulia." },
  { "en": "Pedoman kerja Polri?", "id": "Catur Prasetya." },
  { "en": "Berapa isi Catur Prasetya?", "id": "Empat butir janji." },
  { "en": "Inti Catur Prasetya pertama?", "id": "Setia pada Pancasila dan UUD." },
  { "en": "Inti Catur Prasetya kedua?", "id": "Menaati segala peraturan perundangan." },
  { "en": "Inti Catur Prasetya ketiga?", "id": "Menjaga keselamatan jiwa raga harta." },
  { "en": "Inti Catur Prasetya keempat?", "id": "Menjunjung tinggi panji-panji Tribrata." },
  { "en": "Dasar hukum tertinggi negara?", "id": "UUD Negara Republik Indonesia 1945." },
  { "en": "Landasan konstitusional Polri?", "id": "Pasal 30 UUD 1945." },
  { "en": "UU tentang Kepolisian RI?", "id": "UU Nomor 2 Tahun 2002." },
  { "en": "Tugas pokok Polri?", "id": "Memelihara kamtibmas, menegakkan hukum." },
  { "en": "Tugas pokok Polri lainnya?", "id": "Memberi perlindungan, pengayoman, pelayanan." },
  { "en": "Siapa pimpinan tertinggi Polri?", "id": "Kepala Kepolisian Negara Republik Indonesia." },
  { "en": "Polri bertanggung jawab kepada?", "id": "Presiden." },
  { "en": "Semboyan Polri?", "id": "Rastra Sewakottama." },
  { "en": "Arti Rastra Sewakottama?", "id": "Abdi utama nusa dan bangsa." },
  { "en": "Sikap dasar insan Bhayangkara?", "id": "Jujur, adil, dan manusiawi." },
  { "en": "Sikap yang harus dihindari?", "id": "Arogan, sewenang-wenang, dan koruptif." },
  { "en": "Apa itu integritas Polri?", "id": "Kesatuan kata dengan perbuatan." },
  { "en": "Mengapa integritas penting?", "id": "Membangun kepercayaan publik." },
  { "en": "Apa itu profesionalisme Polri?", "id": "Bekerja sesuai kompetensi dan aturan." },
  { "en": "Ciri Polri profesional?", "id": "Cerdas, terampil, dan beretika." },
  { "en": "Apa itu netralitas Polri?", "id": "Tidak berpihak dalam politik praktis." },
  { "en": "Kapan netralitas Polri diuji?", "id": "Saat pemilihan umum (Pemilu)." },
  { "en": "Konsekuensi tidak netral?", "id": "Sanksi disiplin dan pidana." },
  { "en": "Wujud pelayanan prima Polri?", "id": "Cepat, tepat, ramah, tidak diskriminatif." },
  { "en": "Contoh pelayanan prima?", "id": "Proses laporan yang mudah." },
  { "en": "Hubungan Polri dan masyarakat?", "id": "Kemitraan yang setara." },
  { "en": "Program kemitraan Polri-masyarakat?", "id": "Pemolisian Masyarakat (Polmas)." },
  { "en": "Tujuan Polmas?", "id": "Memecahkan masalah sosial bersama." },
  { "en": "Ancaman ideologi bagi Polri?", "id": "Radikalisme dan intoleransi." },
  { "en": "Sikap Polri terhadap radikalisme?", "id": "Tegas menolak dan memberantas." },
  { "en": "Cara Polri cegah radikalisme?", "id": "Deradikalisasi dan kontra-radikalisasi." },
  { "en": "Fungsi intelijen dalam ideologi?", "id": "Deteksi dini ancaman ideologi." },
  { "en": "Apa itu disiplin Polri?", "id": "Kepatuhan pada aturan dan perintah." },
  { "en": "Manfaat disiplin bagi Polri?", "id": "Menjaga soliditas dan kinerja." },
  { "en": "Dasar hukum disiplin Polri?", "id": "PP Nomor 2 Tahun 2003." },
  { "en": "Kode Etik Profesi Polri?", "id": "Perkap Nomor 14 Tahun 2011." },
  { "en": "Tujuan Kode Etik Polri?", "id": "Menjaga kehormatan dan martabat Polri." },
  { "en": "Siapa yang mengawasi etika Polri?", "id": "Komisi Kode Etik Polri." },
  { "en": "Sanksi pelanggaran kode etik?", "id": "Demosi, tunda pangkat, hingga PTDH." },
  { "en": "Singkatan PTDH?", "id": "Pemberhentian Tidak Dengan Hormat." },
  { "en": "Program Polri saat ini?", "id": "PRESISI." },
  { "en": "Kepanjangan PRESISI?", "id": "Prediktif, Responsibilitas, Transparansi Berkeadilan." },
  { "en": "Arti Prediktif dalam PRESISI?", "id": "Mencegah kejahatan dengan analisis data." },
  { "en": "Arti Responsibilitas dalam PRESISI?", "id": "Rasa tanggung jawab dalam tugas." },
  { "en": "Arti Transparansi Berkeadilan?", "id": "Terbuka, akuntabel, dan berkeadilan." },
  { "en": "Fondasi mental anggota Polri?", "id": "Takwa, setia, disiplin, tanggung jawab." },
  { "en": "Makna lambang Polri secara umum?", "id": "Peran dan tugas mulia Polri." },
  { "en": "Bentuk perisai lambang Polri?", "id": "Bermakna pelindung rakyat dan negara." },
  { "en": "Arti tiga bintang di lambang?", "id": "Melambangkan Tri Brata." },
  { "en": "Warna hitam pada lambang?", "id": "Ketenangan, kebijaksanaan, dan kemantapan." },
  { "en": "Warna kuning emas pada lambang?", "id": "Keagungan dan keluhuran." },
  { "en": "Hierarki di Polri disebut?", "id": "Tata urutan kepangkatan." },
  { "en": "Tujuan hierarki di Polri?", "id": "Menjamin garis komando yang jelas." },
  { "en": "Sikap terhadap atasan?", "id": "Hormat dan taat perintah dinas." },
  { "en": "Sikap terhadap bawahan?", "id": "Membimbing, mengayomi, dan melindungi." },
  { "en": "Sikap terhadap sesama pangkat?", "id": "Menjaga jiwa korsa dan soliditas." },
  { "en": "Apa itu jiwa korsa?", "id": "Rasa senasib sepenanggungan yang positif." },
  { "en": "Bahaya jiwa korsa negatif?", "id": "Menutupi kesalahan rekan kerja." },
  { "en": "Wujud pengamalan Pancasila sehari-hari?", "id": "Menghormati perbedaan suku dan agama." },
  { "en": "Sikap humanis Polri?", "id": "Memanusiakan manusia dalam setiap tindakan." },
  { "en": "Contoh sikap humanis?", "id": "Memberi pertolongan tanpa pamrih." },
  { "en": "Wawasan kebangsaan bagi Polri?", "id": "Memahami nilai-nilai luhur bangsa." },
  { "en": "Empat pilar kebangsaan?", "id": "Pancasila, UUD 1945, NKRI, Bhinneka." },
  { "en": "Peran Polri dalam Bhinneka Tunggal Ika?", "id": "Merawat keragaman dan mencegah konflik." },
  { "en": "Mengapa Polri harus bebas KKN?", "id": "Agar dipercaya oleh masyarakat." },
  { "en": "Singkatan KKN?", "id": "Korupsi, Kolusi, dan Nepotisme." },
  { "en": "Sikap terhadap gratifikasi?", "id": "Tegas menolak dan melaporkan." },
  { "en": "Pembina fungsi mental ideologi?", "id": "Divisi Profesi dan Pengamanan (Propam)." },
  { "en": "Tantangan ideologi di era digital?", "id": "Penyebaran hoaks dan ujaran kebencian." },
  { "en": "Upaya Polri hadapi tantangan digital?", "id": "Patroli siber dan literasi digital." },
  { "en": "Landasan spiritual anggota Polri?", "id": "Keimanan kepada Tuhan Yang Maha Esa." },
  { "en": "Landasan operasional anggota Polri?", "id": "Catur Prasetya." },
  { "en": "Pentingnya kesehatan mental Polri?", "id": "Menunjang kinerja dan pengambilan keputusan." },
  { "en": "Wujud kesiapsiagaan anggota Polri?", "id": "Selalu siap sedia melayani masyarakat." },
  { "en": "Etika utama dalam bernegara?", "id": "Setia kepada NKRI dan pemerintah." },
  { "en": "Etika dalam hubungan kelembagaan?", "id": "Saling menghormati dan bekerja sama." },
  { "en": "Etika dalam kehidupan bermasyarakat?", "id": "Menjadi teladan dan panutan." },
  { "en": "Etika pribadi anggota Polri?", "id": "Berakhlak mulia dan bertanggung jawab." },
  { "en": "Perilaku yang dilarang di publik?", "id": "Memamerkan gaya hidup mewah (hedonisme)." },
  { "en": "Mengapa hedonisme harus dihindari?", "id": "Mencederai rasa keadilan masyarakat." },
  { "en": "Sikap Polri saat tidak bertugas?", "id": "Tetap menjaga kehormatan institusi." },
  { "en": "Cara menyikapi kritik masyarakat?", "id": "Menerima sebagai masukan yang membangun." },
  { "en": "Prinsip dasar Polri terhadap HAM?", "id": "Melindungi, menghormati, dan menjunjung tinggi." },
  { "en": "Dasar hukum HAM di Indonesia?", "id": "UU Nomor 39 Tahun 1999." },
  { "en": "Tiga prinsip penggunaan kekuatan?", "id": "Legalitas, nesesitas, dan proporsionalitas." },
  { "en": "Apa arti prinsip legalitas?", "id": "Tindakan harus berdasarkan hukum." },
  { "en": "Apa arti prinsip nesesitas?", "id": "Tindakan hanya jika sangat diperlukan." },
  { "en": "Apa arti prinsip proporsionalitas?", "id": "Kekuatan seimbang dengan tingkat ancaman." },
  { "en": "Tindakan dilarang dalam pemeriksaan?", "id": "Kekerasan, ancaman, dan penyiksaan." },
  { "en": "Peran Polri dalam pemilu?", "id": "Mengamankan seluruh tahapan proses pemilu." },
  { "en": "Darimana sumber wewenang Polri?", "id": "Berasal dari Undang-Undang." },
  { "en": "Sifat kewenangan yang dimiliki Polri?", "id": "Bersifat memaksa demi kepentingan umum." },
  { "en": "Apa itu diskresi kepolisian?", "id": "Kewenangan bertindak menurut penilaian sendiri." },
  { "en": "Syarat utama menggunakan diskresi?", "id": "Tidak bertentangan dengan hukum." },
  { "en": "Tujuan utama penggunaan wewenang?", "id": "Mewujudkan keamanan dan ketertiban." },
  { "en": "Batas penggunaan wewenang Polri?", "id": "Tidak melampaui kewenangan yang sah." },
  { "en": "Penyalahgunaan wewenang disebut?", "id": "Abuse of power." },
  { "en": "Pentingnya kesamaptaan jasmani?", "id": "Menunjang pelaksanaan tugas di lapangan." },
  { "en": "Hubungan mental sehat dan ideologi?", "id": "Mental sehat perkuat pemahaman ideologi." },
  { "en": "Cara Polri menjaga kesehatan mental?", "id": "Konseling dan pembinaan rohani teratur." },
  { "en": "Arti 'sikap tampan' Polri?", "id": "Penampilan rapi, bersih, dan berwibawa." },
  { "en": "Tujuan dari 'sikap tampan'?", "id": "Meningkatkan kewibawaan dan kepercayaan publik." },
  { "en": "Kapan Hari Bhayangkara diperingati?", "id": "Setiap tanggal 1 Juli." },
  { "en": "Siapa Bapak Kepolisian RI?", "id": "Jenderal Polisi (Purn.) R.S. Soekanto." },
  { "en": "Makna salam hormat Polri?", "id": "Penghormatan kepada institusi dan hierarki." },
  { "en": "Fungsi utama seragam dinas?", "id": "Sebagai identitas, wibawa, dan pelindung." },
  { "en": "Warna baret fungsi Sabhara?", "id": "Cokelat tua." },
  { "en": "Warna baret satuan Brimob?", "id": "Biru tua." },
  { "en": "Peran Polri dalam memberantas terorisme?", "id": "Melakukan penindakan dan pencegahan teror." },
  { "en": "Satuan khusus anti-teror Polri?", "id": "Detasemen Khusus 88 Anti Teror." },
  { "en": "Peran Polri di kancah internasional?", "id": "Menjadi bagian dari pasukan perdamaian PBB." },
  { "en": "Nama pasukan Polri untuk PBB?", "id": "Formed Police Unit (FPU) Garuda." },
  { "en": "Manfaat misi PBB bagi Polri?", "id": "Meningkatkan profesionalisme berstandar internasional." },
  { "en": "Ancaman globalisasi bagi keamanan?", "id": "Kejahatan transnasional atau lintas negara." },
  { "en": "Contoh kejahatan transnasional?", "id": "Perdagangan narkoba dan perdagangan manusia." },
  { "en": "Sikap Polri terhadap berita hoaks?", "id": "Klarifikasi dan penegakan hukum." },
  { "en": "Kewajiban Polri terkait rahasia jabatan?", "id": "Wajib menyimpan rahasia negara/jabatan." },
  { "en": "Konsekuensi membocorkan rahasia jabatan?", "id": "Sanksi pidana dan pemberhentian." },
  { "en": "Pentingnya soliditas TNI-Polri?", "id": "Menjaga pilar utama pertahanan keamanan." },
  { "en": "Wujud nyata soliditas TNI-Polri?", "id": "Patroli gabungan dan apel bersama." },
  { "en": "Mentalitas 'melayani' berarti?", "id": "Mendahulukan kepentingan masyarakat luas." },
  { "en": "Mentalitas 'mengayomi' berarti?", "id": "Memberikan rasa aman dan tentram." },
  { "en": "Mentalitas 'melindungi' berarti?", "id": "Mencegah warga dari ancaman bahaya." },
  { "en": "Penyimpangan ideologi paling berbahaya?", "id": "Terpapar paham radikal dan terorisme." },
  { "en": "Wujud loyalitas anggota Polri?", "id": "Tegak lurus pada negara." },
  { "en": "Dasar loyalitas anggota Polri?", "id": "Pancasila, UUD 1945, dan Tribrata." },
  { "en": "Peran keluarga bagi anggota Polri?", "id": "Memberi dukungan moril dan semangat." },
  { "en": "Pengaruh lingkungan kerja yang baik?", "id": "Meningkatkan semangat dan kinerja positif." },
  { "en": "Apa itu budaya anti korupsi?", "id": "Sikap menolak segala bentuk korupsi." },
  { "en": "Cara membangun budaya anti korupsi?", "id": "Dimulai dari diri sendiri." },
  { "en": "Peran pimpinan dalam ideologi?", "id": "Menjadi teladan bagi seluruh anggota." },
  { "en": "Tantangan ideologi dari dalam?", "id": "Sikap individualisme dan arogansi." },
  { "en": "Cara mengatasi tantangan dari dalam?", "id": "Penguatan mental dan pengawasan internal." },
  { "en": "Fungsi pengawasan internal Polri?", "id": "Itwasum dan Propam." },
  { "en": "Tujuan pengawasan internal?", "id": "Mencegah dan menindak penyimpangan." },
  { "en": "Harapan masyarakat terhadap Polri?", "id": "Polisi yang humanis dan profesional." },
  { "en": "Polri dalam sistem peradilan pidana?", "id": "Sebagai penyidik utama." },
  { "en": "Hubungan Polri dengan Kejaksaan?", "id": "Hubungan koordinasi dan fungsional." },
  { "en": "Sikap Polri terhadap kearifan lokal?", "id": "Menghormati dan mendukung selama positif." },
  { "en": "Contoh peran jaga kearifan lokal?", "id": "Mengamankan upacara adat masyarakat." },
  { "en": "Pentingnya teknologi bagi Polri?", "id": "Mendukung tugas menjadi lebih efektif." },
  { "en": "Contoh pemanfaatan teknologi Polri?", "id": "ETLE atau tilang elektronik." },
  { "en": "Komitmen Polri terhadap lingkungan hidup?", "id": "Menindak tegas perusak lingkungan." },
  { "en": "Contoh kejahatan lingkungan?", "id": "Pemabalakan liar dan pembakaran hutan." },
  { "en": "Pola hidup sederhana artinya?", "id": "Tidak berlebihan dan sesuai kepantasan." },
  { "en": "Mengapa harus hidup sederhana?", "id": "Menghindari kecemburuan sosial." },
  { "en": "Mentalitas yang harus dibangun?", "id": "Mental pejuang dan bukan pegawai." },
  { "en": "Arti mental pejuang?", "id": "Rela berkorban demi bangsa negara." },
  { "en": "Pengetahuan yang wajib dimiliki?", "id": "Peraturan perundang-undangan yang berlaku." },
  { "en": "Keterampilan yang wajib dimiliki?", "id": "Beladiri dan menembak." },
  { "en": "Pentingnya latihan rutin?", "id": "Menjaga kemampuan dan profesionalisme." },
  { "en": "Sikap dalam berkomunikasi dengan media?", "id": "Terbuka, transparan, dan profesional." },
  { "en": "Jargon motivasi Polri?", "id": "Sekali melangkah pantang menyerah." },
  { "en": "Mental ideologi sebagai apa?", "id": "Benteng dari segala penyimpangan." },
  { "en": "Apa itu 'transformasi menuju Presisi'?", "id": "Perubahan menuju Polri yang modern." },
  { "en": "Tujuan utama transformasi Polri?", "id": "Meningkatkan kepercayaan publik." },
  { "en": "Sikap saat hadapi unjuk rasa?", "id": "Persuasif, humanis, dan melindungi." },
  { "en": "Prinsip dalam penanganan massa?", "id": "Sebagai upaya terakhir menggunakan kekuatan." },
  { "en": "Fungsi Bhabinkamtibmas?", "id": "Membina keamanan di tingkat desa." },
  { "en": "Bhabinkamtibmas adalah ujung tombak?", "id": "Ujung tombak pemolisian masyarakat." },
  { "en": "Sikap kepada tersangka kejahatan?", "id": "Tetap menjunjung asas praduga takbersalah." },
  { "en": "Asas praduga tak bersalah?", "id": "Dianggap tidak bersalah sebelum vonis." },
  { "en": "Sikap kepada korban kejahatan?", "id": "Memberi perlindungan dan rasa aman." },
  { "en": "Wujud negara hadir?", "id": "Kehadiran polisi di tengah masyarakat." },
  { "en": "Pentingnya laporan harta kekayaan?", "id": "Bentuk transparansi dan cegah korupsi." },
  { "en": "LHKPN dilaporkan kepada siapa?", "id": "Komisi Pemberantasan Korupsi (KPK)." },
  { "en": "Semangat pengabdian Polri?", "id": "Mengabdi tanpa batas." },
  { "en": "Warisan terbaik seorang polisi?", "id": "Nama baik dan kehormatan." },
  { "en": "Kunci keberhasilan tugas Polri?", "id": "Dukungan dan kepercayaan masyarakat." },
  { "en": "Lembaga pendidikan perwira Polri?", "id": "Akademi Kepolisian (Akpol)." },
  { "en": "Lembaga pendidikan bintara Polri?", "id": "Sekolah Polisi Negara (SPN)." },
  { "en": "Tujuan utama pendidikan pembentukan?", "id": "Mencetak insan Bhayangkara sejati." },
  { "en": "Tiga aspek utama dalam pendidikan?", "id": "Mental, akademik, dan kesamaptaan jasmani." },
  { "en": "Apa itu pembinaan karier?", "id": "Proses pengembangan kompetensi setiap anggota." },
  { "en": "Dasar sistem pembinaan karier?", "id": "Prestasi, dedikasi, loyalitas, dan moralitas." },
  { "en": "Sistem penilaian kinerja Polri?", "id": "Sistem Manajemen Kinerja (SMK)." },
  { "en": "Prinsip dasar dalam pembinaan personel?", "id": "Reward and Punishment." },
  { "en": "Tujuan dari 'reward and punishment'?", "id": "Mendorong prestasi, menekan pelanggaran." },
  { "en": "Lembaga pengawas internal utama Polri?", "id": "Propam dan Itwasum." },
  { "en": "Kepanjangan dari Propam?", "id": "Divisi Profesi dan Pengamanan." },
  { "en": "Tugas utama dari Propam?", "id": "Membina dan menegakkan disiplin/etik." },
  { "en": "Kepanjangan dari Itwasum?", "id": "Inspektorat Pengawasan Umum." },
  { "en": "Tugas utama dari Itwasum?", "id": "Melakukan pengawasan dan pemeriksaan internal." },
  { "en": "Lembaga pengawas eksternal Polri?", "id": "Komisi Kepolisian Nasional (Kompolnas)." },
  { "en": "Lembaga negara pengawas pelayanan publik?", "id": "Ombudsman Republik Indonesia." },
  { "en": "Tujuan utama pengawasan eksternal?", "id": "Menjamin akuntabilitas dan transparansi Polri." },
  { "en": "Ideologi Polantas di jalan raya?", "id": "Melayani, melindungi, tertib berlalu lintas." },
  { "en": "Sikap Polantas saat menindak?", "id": "Tegas, humanis, dan tidak transaksional." },
  { "en": "Ideologi bagi fungsi Reserse?", "id": "Mengungkap kebenaran materiil secara profesional." },
  { "en": "Prinsip utama seorang penyidik?", "id": "Profesional, proporsional, prosedural, transparan." },
  { "en": "Ideologi bagi fungsi Intelkam?", "id": "Deteksi dini, cegah potensi gangguan." },
  { "en": "Sifat utama kerja Intelijen?", "id": "Cepat, akurat, rahasia, dan aman." },
  { "en": "Ideologi fungsi Sabhara/Samapta?", "id": "Kesiapsiagaan cegah niat dan kesempatan." },
  { "en": "Tugas pokok fungsi Sabhara?", "id": "Turjawali (pengaturan, penjagaan, pengawalan, patroli)." },
  { "en": "Ideologi satuan Brimob?", "id": "Pasukan pamungkas penindak gangguan kamtibmas." },
  { "en": "Kemampuan utama satuan Brimob?", "id": "PHH, SAR, Jibom, dan antiteror." },
  { "en": "Ideologi fungsi Binmas?", "id": "Membangun kemitraan setara dengan masyarakat." },
  { "en": "Fungsi yang membawahi Bhabinkamtibmas?", "id": "Satuan Pembinaan Masyarakat (Sat Binmas)." },
  { "en": "Hubungan kesejahteraan dan integritas?", "id": "Kesejahteraan cukup menekan potensi korupsi." },
  { "en": "Bentuk kesejahteraan dari negara?", "id": "Gaji, tunjangan, perumahan, dan kesehatan." },
  { "en": "Pentingnya tes psikologi bagi Polri?", "id": "Menjaring calon anggota bermental stabil." },
  { "en": "Kapan tes psikologi rutin dilakukan?", "id": "Saat seleksi, pendidikan, dan promosi." },
  { "en": "Unit yang menangani psikologi Polri?", "id": "Biro Psikologi Staf Sumber Daya Manusia." },
  { "en": "Gangguan psikologis yang rentan dialami?", "id": "Stres, trauma pasca tugas, depresi." },
  { "en": "Solusi untuk gangguan psikologis?", "id": "Pendampingan dan konseling oleh psikolog." },
  { "en": "Peran terpenting seorang pimpinan?", "id": "Menjadi suri teladan bagi anggotanya." },
  { "en": "Prinsip kepemimpinan dari Ki Hajar?", "id": "Ing ngarso sung tulodo." },
  { "en": "Arti dari 'ing ngarso sung tulodo'?", "id": "Di depan memberi contoh/teladan." },
  { "en": "Arahan kebijakan pimpinan tertinggi disebut?", "id": "Commander Wish atau atensi pimpinan." },
  { "en": "Program prioritas terukur Kapolri?", "id": "Program Quick Wins Presisi." },
  { "en": "Contoh program Quick Wins?", "id": "Peningkatan kualitas pelayanan publik." },
  { "en": "Sikap Polri terhadap penyalahguna narkoba?", "id": "Penegakan hukum dan mendukung rehabilitasi." },
  { "en": "Sikap anggota Polri terhadap narkoba?", "id": "Haram dan sanksinya PTDH." },
  { "en": "Sanksi anggota Polri terlibat narkoba?", "id": "Pidana dan Pemberhentian Tidak Hormat." },
  { "en": "Pentingnya menjaga aset negara?", "id": "Wujud tanggung jawab pada negara." },
  { "en": "Contoh aset dinas Polri?", "id": "Gedung kantor, kendaraan, dan alutsista." },
  { "en": "Kapan senjata api boleh digunakan?", "id": "Dalam keadaan terpaksa membela diri." },
  { "en": "Dasar aturan penggunaan senjata api?", "id": "Perkap Nomor 1 Tahun 2009." },
  { "en": "Manfaat data dalam tugas Polri?", "id": "Dasar pengambilan keputusan yang akurat." },
  { "en": "Sistem pengaduan online masyarakat?", "id": "Aplikasi Dumas Presisi atau Call 110." },
  { "en": "Tujuan dari pengaduan online?", "id": "Mempermudah akses masyarakat pada Polri." },
  { "en": "Sikap Polri terhadap kekerasan domestik?", "id": "Melindungi korban dan menindak pelaku." },
  { "en": "Unit yang tangani perempuan/anak?", "id": "Unit PPA (Perlindungan Perempuan Anak)." },
  { "en": "Tantangan ideologi dari media sosial?", "id": "Informasi salah (disinformasi) dan fitnah." },
  { "en": "Cara Polri hadapi serangan siber?", "id": "Dengan tim patroli siber (cyber patrol)." },
  { "en": "Pentingnya kerja sama antar fungsi?", "id": "Menyelesaikan tugas secara komprehensif." },
  { "en": "Sikap terhadap tahanan di rutan?", "id": "Manusiawi dan sesuai aturan." },
  { "en": "Hak-hak seorang tahanan?", "id": "Hak kesehatan, ibadah, dan bantuan hukum." },
  { "en": "Apa itu 'restorative justice'?", "id": "Penyelesaian perkara di luar pengadilan." },
  { "en": "Tujuan utama 'restorative justice'?", "id": "Memulihkan keadilan, bukan pembalasan." },
  { "en": "Contoh kasus 'restorative justice'?", "id": "Perkara ringan dengan perdamaian." },
  { "en": "Sikap Polri terhadap kelompok minoritas?", "id": "Melindungi dan menjamin hak-haknya." },
  { "en": "Arti Polri sebagai 'problem solver'?", "id": "Mampu memecahkan masalah sosial." },
  { "en": "Pentingnya laporan yang akurat?", "id": "Dasar untuk tindakan selanjutnya." },
  { "en": "Sikap saat menerima laporan warga?", "id": "Mendengar, mencatat, dan menindaklanjuti." },
  { "en": "Slogan pelayanan Sentra Pelayanan Kepolisian?", "id": "Senyum, Sapa, Salam (3S)." },
  { "en": "Sikap saat menemukan barang bukti?", "id": "Mengamankan TKP dan barang bukti." },
  { "en": "Mengapa TKP harus steril?", "id": "Agar barang bukti tidak rusak." },
  { "en": "Kepanjangan dari TKP?", "id": "Tempat Kejadian Perkara." },
  { "en": "Apa itu loyalitas tegak lurus?", "id": "Setia pada institusi, bukan perorangan." },
  { "en": "Sikap saat diperiksa Propam?", "id": "Kooperatif dan memberikan keterangan jujur." },
  { "en": "Makna 'abdi utama nusa bangsa'?", "id": "Pelayan terbaik bagi negara." },
  { "en": "Komitmen moral seorang Bhayangkara?", "id": "Berpegang teguh pada Tribrata." },
  { "en": "Komitmen operasional seorang Bhayangkara?", "id": "Berpegang teguh pada Catur Prasetya." },
  { "en": "Sikap terhadap wartawan di lapangan?", "id": "Memberi informasi benar dan proporsional." },
  { "en": "Fungsi Humas Polri?", "id": "Juru bicara resmi institusi Polri." },
  { "en": "Sikap dalam menjaga kebugaran fisik?", "id": "Rutin berolahraga dan pola hidup sehat." },
  { "en": "Tujuan pembinaan rohani dan mental?", "id": "Membentuk pribadi beriman dan berakhlak." },
  { "en": "Sikap terhadap perbedaan pendapat internal?", "id": "Diselesaikan melalui mekanisme organisasi." },
  { "en": "Pentingnya apel pagi/apel jam pimpinan?", "id": "Media penyampaian arahan dan informasi." },
  { "en": "Arti Bhayangkara bagi negara?", "id": "Penjaga dan pengawal negara." },
  { "en": "Bagaimana cara menjadi polisi modern?", "id": "Menguasai teknologi dan pengetahuan." },
  { "en": "Sikap terhadap tugas di daerah terpencil?", "id": "Menjalankan dengan ikhlas dan semangat." },
  { "en": "Makna Polri sebagai penegak hukum?", "id": "Menjamin kepastian hukum bagi semua." },
  { "en": "Apa itu 'cooling system'?", "id": "Upaya mendinginkan situasi kamtibmas." },
  { "en": "Penerapan 'cooling system' saat pemilu?", "id": "Dialog dengan tokoh masyarakat/agama." },
  { "en": "Sikap terhadap anggota yang berprestasi?", "id": "Memberikan penghargaan dan apresiasi." },
  { "en": "Sikap terhadap anggota yang melanggar?", "id": "Menegakkan aturan tanpa pandang bulu." },
  { "en": "Mentalitas dalam menghadapi perubahan zaman?", "id": "Adaptif, inovatif, dan terbuka." },
  { "en": "Ideologi sebagai filter apa?", "id": "Filter pengaruh negatif dari luar." },
  { "en": "Harapan akhir dari penguatan ideologi?", "id": "Terwujudnya Polri yang dicintai masyarakat." },
  { "en": "Sikap dalam menjaga nama baik Polri?", "id": "Berperilaku baik kapanpun dimanapun." },
  { "en": "Pentingnya sinergi dengan pemerintah daerah?", "id": "Mendukung kelancaran pembangunan daerah." },
  { "en": "Peran Polri dalam penanggulangan bencana?", "id": "Melakukan evakuasi, pertolongan, dan pengamanan." },
  { "en": "Sikap saat off-duty melihat kejahatan?", "id": "Wajib melakukan tindakan kepolisian." },
  { "en": "Kewajiban moral seorang polisi?", "id": "Menolong sesama tanpa diminta." },
  { "en": "Prinsip utama komunikasi publik Polri?", "id": "Transparan, akuntabel, dan humanis." },
  { "en": "Tujuan utama dari manajemen media?", "id": "Membangun citra positif dan kepercayaan." },
  { "en": "Cara Polri menyikapi berita negatif?", "id": "Klarifikasi dengan data dan fakta." },
  { "en": "Bolehkah penyidik merilis detail perkara?", "id": "Hanya informasi yang tak ganggu penyidikan." },
  { "en": "Siapa yang menjadi juru bicara institusi?", "id": "Divisi Hubungan Masyarakat (Humas) Polri." },
  { "en": "Pentingnya literasi digital bagi anggota?", "id": "Agar tidak mudah termakan hoaks." },
  { "en": "Hubungan keamanan dan pembangunan nasional?", "id": "Keamanan stabil, pembangunan berjalan lancar." },
  { "en": "Peran Polri dalam proyek strategis?", "id": "Mengamankan Objek Vital Nasional (Obvitnas)." },
  { "en": "Contoh dari Objek Vital Nasional?", "id": "Pelabuhan, bandara, dan kilang minyak." },
  { "en": "Bagaimana Polri mendukung sektor pariwisata?", "id": "Menciptakan rasa aman bagi wisatawan." },
  { "en": "Satuan khusus untuk area wisata?", "id": "Satuan Polisi Pariwisata." },
  { "en": "Peran Polri dalam konflik sosial?", "id": "Sebagai mediator dan penegak hukum." },
  { "en": "Tujuan utama manajemen konflik sosial?", "id": "Mencegah konflik meluas jadi anarkis." },
  { "en": "Sikap Polri terhadap adat istiadat?", "id": "Menghormati selama tidak melanggar hukum." },
  { "en": "Mengapa wawasan multikultural penting?", "id": "Agar bisa bertugas di semua daerah." },
  { "en": "Cara membangun kedekatan dengan warga?", "id": "Aktif dalam kegiatan sosial kemasyarakatan." },
  { "en": "Implementasi Bhinneka Tunggal Ika Polri?", "id": "Tidak membedakan pelayanan berdasarkan SARA." },
  { "en": "Unit yang tangani kejahatan internet?", "id": "Direktorat Tindak Pidana Siber (Dittipidsiber)." },
  { "en": "Tantangan utama dalam kejahatan siber?", "id": "Pelaku anonim dan lintas negara." },
  { "en": "Ideologi dalam menangani data digital?", "id": "Menjaga privasi sesuai aturan hukum." },
  { "en": "Apa yang dimaksud bukti digital?", "id": "Informasi elektronik sebagai alat bukti." },
  { "en": "Pentingnya ilmu forensik digital?", "id": "Untuk mengungkap kejahatan berbasis teknologi." },
  { "en": "Dasar hukum 'Restorative Justice' Polri?", "id": "Perpol Nomor 8 Tahun 2021." },
  { "en": "Dasar hukum Pam Swakarsa?", "id": "Perpol Nomor 4 Tahun 2020." },
  { "en": "Siapa saja bentuk Pam Swakarsa?", "id": "Satpam dan Satuan Keamanan Lingkungan." },
  { "en": "Peran Polri terhadap Pam Swakarsa?", "id": "Sebagai pembina teknis dan pengawas." },
  { "en": "Aturan tentang seragam dinas Polri?", "id": "Diatur dalam Peraturan Kapolri (Perkap)." },
  { "en": "Tujuan penyeragaman pakaian dinas?", "id": "Menunjukkan disiplin, keseragaman, dan identitas." },
  { "en": "Cara memperkuat ketahanan ideologi pribadi?", "id": "Ibadah, belajar, dan pergaulan positif." },
  { "en": "Apa itu kepemimpinan yang melayani?", "id": "Pemimpin yang mengutamakan kebutuhan anggota." },
  { "en": "Apa itu kepemimpinan transformasional?", "id": "Pemimpin yang mampu menginspirasi perubahan." },
  { "en": "Sikap pemimpin pada anggota bermasalah?", "id": "Menindak tegas namun tetap membina." },
  { "en": "Pentingnya evaluasi kinerja secara rutin?", "id": "Mengukur pencapaian dan bahan perbaikan." },
  { "en": "Contoh pembinaan mental di satuan?", "id": "Jam pimpinan dan bimbingan rohani." },
  { "en": "Ancaman terbesar bagi mental ideologi?", "id": "Pragmatisme dan sikap hidup transaksional." },
  { "en": "Cara melawan sikap pragmatis?", "id": "Menanamkan kebanggaan dan kehormatan profesi." },
  { "en": "Peran Polisi Perairan dan Udara?", "id": "Jaga keamanan wilayah perairan udara." },
  { "en": "Singkatan dari Polairud?", "id": "Kepolisian Perairan dan Udara." },
  { "en": "Fungsi Laboratorium Forensik (Labfor)?", "id": "Membantu pembuktian ilmiah tindak pidana." },
  { "en": "Fungsi utama dari Inafis?", "id": "Identifikasi pelaku/korban lewat sidik jari." },
  { "en": "Kepanjangan dari Inafis?", "id": "Indonesia Automatic Fingerprint Identification System." },
  { "en": "Peran Dokkes dalam tugas kepolisian?", "id": "Dukungan kesehatan, DVI, dan otopsi." },
  { "en": "Kepanjangan dari Dokkes Polri?", "id": "Kedokteran dan Kesehatan Polri." },
  { "en": "Kepanjangan dari DVI?", "id": "Disaster Victim Identification." },
  { "en": "Sikap utama terhadap saksi/korban?", "id": "Memberikan perlindungan fisik dan psikis." },
  { "en": "Lembaga negara yang lindungi saksi?", "id": "Lembaga Perlindungan Saksi dan Korban." },
  { "en": "Singkatan dari lembaga tersebut?", "id": "LPSK." },
  { "en": "Bentuk sinergi Polri dengan LPSK?", "id": "Koordinasi dalam perlindungan saksi/korban." },
  { "en": "Mentalitas saat menghadapi era globalisasi?", "id": "Adaptif, inovatif dan berwawasan global." },
  { "en": "Sikap dalam penggunaan media sosial pribadi?", "id": "Bijak, santun, dan tidak provokatif." },
  { "en": "Larangan bagi anggota di medsos?", "id": "Mengunggah foto/video bergaya hidup mewah." },
  { "en": "Tugas utama Pusat Keuangan Polri?", "id": "Mengelola anggaran negara secara akuntabel." },
  { "en": "Tujuan pengelolaan anggaran yang akuntabel?", "id": "Mencegah penyelewengan dan korupsi." },
  { "en": "Peran Divisi Hukum (Divkum) Polri?", "id": "Memberi nasehat dan bantuan hukum." },
  { "en": "Fungsi Divisi Hubinter Polri?", "id": "Mengurus kerja sama internasional." },
  { "en": "Kepanjangan dari Hubinter?", "id": "Hubungan Internasional." },
  { "en": "Contoh kerja sama Hubinter?", "id": "Pengejaran buronan di luar negeri." },
  { "en": "Apa itu 'red notice' Interpol?", "id": "Permintaan pencarian dan penangkapan buronan." },
  { "en": "Filosofi borgol dalam kepolisian?", "id": "Alat pembatas gerak demi keamanan." },
  { "en": "Filosofi tongkat polisi?", "id": "Lambang kewibawaan dan alat pertahanan." },
  { "en": "Filosofi peluit polisi?", "id": "Alat komunikasi untuk menarik perhatian." },
  { "en": "Kewajiban menjaga kebersihan markas?", "id": "Cerminan dari disiplin dan keteraturan." },
  { "en": "Sikap terhadap keluarga tersangka?", "id": "Tetap humanis dan memberikan informasi." },
  { "en": "Pentingnya menjaga postur tubuh ideal?", "id": "Menunjang penampilan dan kesiapsiagaan fisik." },
  { "en": "Sikap saat ada anggota gugur?", "id": "Memberi penghormatan tertinggi." },
  { "en": "Penghargaan bagi anggota yang gugur?", "id": "Kenaikan Pangkat Luar Biasa Anumerta." },
  { "en": "Sikap terhadap purnawirawan Polri?", "id": "Menghormati sebagai senior dan sesepuh." },
  { "en": "Organisasi para purnawirawan Polri?", "id": "Persatuan Purnawirawan (PP) Polri." },
  { "en": "Sikap dalam tim kerja?", "id": "Solid, kooperatif, dan saling mendukung." },
  { "en": "Jika terjadi salah tangkap?", "id": "Segera membebaskan dan meminta maaf." },
  { "en": "Prinsip dalam penggeledahan rumah?", "id": "Disaksikan oleh ketua lingkungan setempat." },
  { "en": "Tujuan surat perintah dalam tugas?", "id": "Sebagai dasar legalitas pelaksanaan tugas." },
  { "en": "Bolehkah menolak perintah atasan?", "id": "Tidak boleh, kecuali perintah ilegal." },
  { "en": "Apa yang dimaksud perintah ilegal?", "id": "Perintah yang melanggar hukum." },
  { "en": "Sikap saat menerima perintah ilegal?", "id": "Menolak dengan santun dan melaporkan." },
  { "en": "Pentingnya apel serah terima tugas?", "id": "Memastikan kelanjutan tugas berjalan lancar." },
  { "en": "Apa itu 'status quo' TKP?", "id": "Menjaga TKP dalam keadaan utuh." },
  { "en": "Peran istri/suami anggota Polri?", "id": "Sebagai pendukung utama dalam keluarga." },
  { "en": "Organisasi istri anggota Polri?", "id": "Bhayangkari." },
  { "en": "Peran utama Bhayangkari?", "id": "Mendukung tugas suami/istri dan sosial." },
  { "en": "Sikap terhadap pengacara/penasihat hukum?", "id": "Menghormati sebagai mitra penegak hukum." },
  { "en": "Hak tersangka didampingi pengacara?", "id": "Hak mutlak yang wajib dipenuhi." },
  { "en": "Mentalitas dalam menjaga perbatasan negara?", "id": "Patriotisme dan cinta tanah air." },
  { "en": "Sinergi di perbatasan negara?", "id": "Dengan TNI dan petugas imigrasi." },
  { "en": "Penyakit masyarakat yang harus diberantas?", "id": "Judi, miras, dan premanisme." },
  { "en": "Kepanjangan dari Pekat?", "id": "Penyakit Masyarakat." },
  { "en": "Ideologi sebagai pondasi apa?", "id": "Pondasi karakter dan jati diri." },
  { "en": "Sikap terhadap kemajuan zaman?", "id": "Terbuka namun tetap waspada." },
  { "en": "Pentingnya kemampuan berbahasa asing?", "id": "Mendukung tugas di era global." },
  { "en": "Sikap saat menjadi saksi di pengadilan?", "id": "Memberi keterangan jujur dan objektif." },
  { "en": "Bagaimana menjaga motivasi kerja?", "id": "Mengingat sumpah dan janji profesi." },
  { "en": "Pentingnya doa sebelum bertugas?", "id": "Memohon perlindungan dan kelancaran tugas." },
  { "en": "Sikap setelah selesai bertugas?", "id": "Bersyukur dan melakukan evaluasi diri." },
  { "en": "Warisan ideologi untuk generasi penerus?", "id": "Semangat pengabdian yang tidak lekang." },
  { "en": "Mentalitas menghadapi kegagalan tugas?", "id": "Belajar dari kesalahan, tidak putus asa." },
  { "en": "Kunci utama polisi dicintai rakyat?", "id": "Bekerja tulus ikhlas sepenuh hati." },
  { "en": "Ideologi sebagai perisai mental?", "id": "Memberi kekuatan saat hadapi tekanan." },
  { "en": "Pentingnya 'debriefing' pasca insiden kritis?", "id": "Melepas beban trauma pasca tugas." },
  { "en": "Siapa yang memimpin sesi 'debriefing'?", "id": "Psikolog atau konselor yang terlatih." },
  { "en": "Apa itu kecerdasan emosional?", "id": "Kemampuan mengelola emosi diri/orang lain." },
  { "en": "Manfaat kecerdasan emosional bagi Polri?", "id": "Mencegah tindakan reaktif dan berlebihan." },
  { "en": "Apa itu kecerdasan sosial?", "id": "Kemampuan berinteraksi efektif dengan masyarakat." },
  { "en": "Contoh penerapan kecerdasan sosial?", "id": "Memahami budaya lokal, membangun jaringan." },
  { "en": "Dasar pengambilan keputusan di lapangan?", "id": "Aturan hukum, diskresi, dan nurani." },
  { "en": "Apa itu 'tunnel vision'?", "id": "Terlalu fokus pada satu hal." },
  { "en": "Bahaya dari 'tunnel vision'?", "id": "Potensi kesalahan fatal dalam bertindak." },
  { "en": "Cara Polri mendorong lahirnya inovasi?", "id": "Melalui kompetisi dan wadah gagasan." },
  { "en": "Contoh inovasi dalam pelayanan publik?", "id": "Aplikasi SKCK Online dan SIGNAL." },
  { "en": "Sikap terhadap perkembangan teknologi?", "id": "Harus adaptif dan terus belajar." },
  { "en": "Pendekatan lunak (soft approach) deradikalisasi?", "id": "Dialog, pembinaan, dan pemberdayaan ekonomi." },
  { "en": "Pendekatan keras (hard approach) deradikalisasi?", "id": "Penegakan hukum terhadap kelompok radikal." },
  { "en": "Peran Bhabinkamtibmas dalam kontra-radikalisme?", "id": "Deteksi dini penyebaran paham radikal." },
  { "en": "Siapa sasaran utama paham radikal?", "id": "Individu yang terisolasi dan rentan." },
  { "en": "Ideologi tandingan paling ampuh?", "id": "Pancasila dan Wawasan Kebangsaan." },
  { "en": "Sikap terhadap kendaraan dinas?", "id": "Merawatnya dengan penuh tanggung jawab." },
  { "en": "Tanggung jawab atas senpi dinas?", "id": "Menjaga agar tidak pernah disalahgunakan." },
  { "en": "Pentingnya olahraga teratur bagi anggota?", "id": "Menjaga kebugaran fisik dan mental." },
  { "en": "Hubungan pola makan dan kinerja?", "id": "Asupan gizi seimbang dukung energi." },
  { "en": "Filosofi di balik pola hidup sederhana?", "id": "Fokus pada pengabdian bukan kemewahan." },
  { "en": "Sikap saat menangani kasus anak?", "id": "Mengutamakan kepentingan terbaik bagi anak." },
  { "en": "Prinsip penanganan korban perempuan?", "id": "Memberi rasa aman, hindari viktimisasi." },
  { "en": "Apa itu 'viktimisasi sekunder'?", "id": "Korban disalahkan atas kejadian menimpanya." },
  { "en": "Cara Polri melayani penyandang disabilitas?", "id": "Menyediakan akses dan pendampingan khusus." },
  { "en": "Sikap terhadap lansia yang melapor?", "id": "Sabar, hormat, dan mendengarkan seksama." },
  { "en": "Mengapa 'melayani' adalah suatu kehormatan?", "id": "Dipercaya negara untuk bantu rakyat." },
  { "en": "Perbedaan mental 'dilayani' dan 'melayani'?", "id": "Egois versus mendahulukan orang lain." },
  { "en": "Kapan Polri resmi terpisah dari ABRI?", "id": "1 April 1999." },
  { "en": "Tujuan utama pemisahan Polri-ABRI?", "id": "Mewujudkan kepolisian sipil yang mandiri." },
  { "en": "Apa itu reformasi kultural Polri?", "id": "Mengubah mentalitas militeristik ke sipil." },
  { "en": "Fokus utama dari reformasi kultural?", "id": "Pelayanan prima dan pendekatan humanis." },
  { "en": "Peran Satuan Musik (Satsik) Polri?", "id": "Pengiring upacara dan diplomasi budaya." },
  { "en": "Peran Satuan K-9 dalam tugas?", "id": "Pelacakan, deteksi narkoba, dan SAR." },
  { "en": "Perlengkapan dasar seorang polisi patroli?", "id": "Borgol, tongkat, radio HT, peluit." },
  { "en": "Arti 'body system' saat bertugas?", "id": "Saling melindungi antar rekan kerja." },
  { "en": "Pentingnya bahasa tubuh (body language)?", "id": "Menunjukkan sikap percaya diri/terbuka." },
  { "en": "Contoh bahasa tubuh yang salah?", "id": "Melipat tangan atau menghindari kontak mata." },
  { "en": "Tindakan pertama saat ada laporan hilang?", "id": "Menenangkan keluarga, mengumpulkan data." },
  { "en": "Pentingnya Latihan Pra-Operasi (Latpraops)?", "id": "Menyamakan persepsi dan cara bertindak." },
  { "en": "Apa tujuan Analisis dan Evaluasi (Anev)?", "id": "Mengkaji hasil tugas untuk perbaikan." },
  { "en": "Sikap Polri terhadap profesi lain?", "id": "Menghargai dan siap untuk bekerja sama." },
  { "en": "Kewajiban lapor jika pindah domisili?", "id": "Bagian dari ketertiban administrasi personel." },
  { "en": "Tindakan pertama di TKP laka lantas?", "id": "Mengamankan lokasi dan menolong korban." },
  { "en": "Tugas utama dari DVI Polri?", "id": "Identifikasi korban bencana massal." },
  { "en": "Kapan tim DVI diterjunkan?", "id": "Saat terjadi bencana dengan banyak korban." },
  { "en": "Tugas Pusat Sejarah (Pusjarah) Polri?", "id": "Merawat dan mendokumentasikan sejarah Polri." },
  { "en": "Di mana letak Museum Polri?", "id": "Jakarta Selatan." },
  { "en": "Pentingnya mempelajari sejarah Polri?", "id": "Menghargai perjuangan dan nilai luhur." },
  { "en": "Sikap saat bertugas di wilayah konflik?", "id": "Netral, deeskalasi, dan lindungi warga." },
  { "en": "Prinsip utama dalam negosiasi?", "id": "Mencari solusi menang-menang (win-win solution)." },
  { "en": "Peran negosiator dalam penyanderaan?", "id": "Mengulur waktu dan menyelamatkan sandera." },
  { "en": "Sikap terhadap mantan narapidana terorisme?", "id": "Mengawasi sambil membantu reintegrasi sosial." },
  { "en": "Apa itu 'police hazard'?", "id": "Bahaya yang melekat pada profesi." },
  { "en": "Contoh dari 'police hazard'?", "id": "Ancaman fisik, stres, dan kelelahan." },
  { "en": "Cara memitigasi 'police hazard'?", "id": "Latihan, prosedur, dan perlengkapan aman." },
  { "en": "Sikap terhadap keluarga besar Polri?", "id": "Menjaga silaturahmi dan jiwa korsa." },
  { "en": "Tujuan dari 'coffee morning' pimpinan?", "id": "Komunikasi informal dan menyerap aspirasi." },
  { "en": "Pentingnya menjaga kerahasiaan pelapor?", "id": "Melindungi pelapor dari potensi ancaman." },
  { "en": "Sikap terhadap terdakwa di pengadilan?", "id": "Tetap objektif dan berdasarkan fakta." },
  { "en": "Bagaimana cara menjadi pendengar aktif?", "id": "Fokus, tidak memotong, memberi umpan balik." },
  { "en": "Pentingnya kemampuan menulis laporan?", "id": "Agar kronologis jelas dan akurat." },
  { "en": "Struktur dasar Laporan Polisi (LP)?", "id": "Waktu, tempat, kejadian, identitas pelapor." },
  { "en": "Apa itu 'community policing'?", "id": "Pemolisian berbasis komunitas atau Polmas." },
  { "en": "Filosofi 'community policing'?", "id": "Polisi dan masyarakat adalah mitra." },
  { "en": "Sikap dalam rapat koordinasi eksternal?", "id": "Kooperatif dan mewakili nama institusi." },
  { "en": "Cara menjaga semangat pengabdian?", "id": "Mengingat kembali sumpah jabatan." },
  { "en": "Prinsip dalam menyampaikan kabar buruk?", "id": "Jujur, empatik, dan didampingi." },
  { "en": "Makna 'the thin blue line'?", "id": "Polisi sebagai pemisah ketertiban/kekacauan." },
  { "en": "Ideologi dalam penggunaan anggaran penyidikan?", "id": "Efisien, efektif, dan dapat dipertanggungjawabkan." },
  { "en": "Peran serta dalam upacara bendera?", "id": "Wujud cinta tanah air/nasionalisme." },
  { "en": "Sikap terhadap temuan Komnas HAM?", "id": "Menghormati dan menindaklanjuti sesuai prosedur." },
  { "en": "Sikap terhadap temuan Ombudsman?", "id": "Menjadikannya sebagai bahan perbaikan layanan." },
  { "en": "Pentingnya kemampuan pertolongan pertama?", "id": "Menyelamatkan nyawa di saat darurat." },
  { "en": "Prinsip dasar P3K?", "id": "Jangan membuat korban lebih cedera." },
  { "en": "Sikap saat jam kerja selesai?", "id": "Menyelesaikan tanggung jawab sebelum pulang." },
  { "en": "Tujuan pemeriksaan kesehatan berkala?", "id": "Deteksi dini penyakit, jaga kesehatan." },
  { "en": "Apa itu kesetiaan ganda?", "id": "Loyal pada hal yang salah." },
  { "en": "Bahaya dari kesetiaan ganda?", "id": "Merusak integritas dan soliditas institusi." },
  { "en": "Loyalitas tertinggi seorang anggota Polri?", "id": "Kepada Negara Kesatuan Republik Indonesia." },
  { "en": "Sikap menghadapi provokasi massa?", "id": "Tetap tenang, tidak terpancing emosi." },
  { "en": "Pentingnya 'map of the heart'?", "id": "Memetakan hati untuk tahu kelemahan diri." },
  { "en": "Cara membangun 'trust' atau kepercayaan?", "id": "Konsisten antara perkataan dan perbuatan." },
  { "en": "Apa itu 'presumption of innocence'?", "id": "Asas praduga tak bersalah." },
  { "en": "Sikap terhadap pelaku yang menyerahkan diri?", "id": "Menghargai dan memperlakukan dengan baik." },
  { "en": "Tujuan utama dari penegakan hukum?", "id": "Menciptakan keadilan dan kepastian hukum." },
  { "en": "Kewajiban moral setelah menerima gaji?", "id": "Bekerja lebih baik dan bertanggungjawab." },
  { "en": "Sikap dalam menjaga lingkungan Mako?", "id": "Ikut serta menjaga kebersihan/keindahan." },
  { "en": "Contoh tindakan proaktif polisi?", "id": "Patroli dialogis di daerah rawan." },
  { "en": "Arti 'problem oriented policing'?", "id": "Pemolisian berorientasi pada pemecahan masalah." },
  { "en": "Sikap jika menemukan anggota lain melanggar?", "id": "Mengingatkan dan melaporkan sesuai prosedur." },
  { "en": "Ideologi sebagai kompas moral?", "id": "Penunjuk arah dalam setiap tindakan." },
  { "en": "Prinsip utama dalam analisis intelijen?", "id": "Objektif, akurat, dan tidak bias." },
  { "en": "Tujuan akhir dari produk intelijen?", "id": "Mendukung pengambilan keputusan pimpinan." },
  { "en": "Sikap 'intellectual humility' seorang analis?", "id": "Sadar akan keterbatasan pengetahuan diri." },
  { "en": "Mengapa fungsi intelijen harus netral?", "id": "Agar tidak menjadi alat politik." },
  { "en": "Fungsi 'early warning' dari intelijen?", "id": "Memberi peringatan dini potensi ancaman." },
  { "en": "Dasar penggunaan anggaran satuan kerja?", "id": "Daftar Isian Pelaksanaan Anggaran (DIPA)." },
  { "en": "Ideologi dalam menggunakan dana DIPA?", "id": "Sesuai aturan, akuntabel, tanpa korupsi." },
  { "en": "Apa itu Perwabkeu?", "id": "Pertanggungjawaban Keuangan." },
  { "en": "Pentingnya membuat Perwabkeu yang benar?", "id": "Bukti transparansi penggunaan uang negara." },
  { "en": "Hubungan kinerja dengan tunjangan?", "id": "Tunjangan kinerja berdasarkan penilaian SMK." },
  { "en": "Mentalitas produktif bagi anggota Polri?", "id": "Bekerja efisien untuk hasil maksimal." },
  { "en": "Kunci keberhasilan negosiasi kepolisian?", "id": "Membangun rapport dan rasa percaya." },
  { "en": "Sikap dalam negosiasi unjuk rasa?", "id": "Mendengar aspirasi, tawarkan solusi damai." },
  { "en": "Pentingnya 'public speaking' bagi perwira?", "id": "Agar dapat menyampaikan arahan jelas." },
  { "en": "Bahasa yang harus dihindari polisi?", "id": "Bahasa kasar, arogan, dan merendahkan." },
  { "en": "Tujuan utama dari komunikasi efektif?", "id": "Pesan tersampaikan, hindari salah paham." },
  { "en": "Etika dalam penggunaan 'big data'?", "id": "Untuk keamanan, bukan mengontrol masyarakat." },
  { "en": "Risiko dari 'predictive policing'?", "id": "Potensi terjadinya bias dan diskriminasi." },
  { "en": "Cara mitigasi risiko 'predictive policing'?", "id": "Dengan audit algoritma dan pengawasan." },
  { "en": "Sistem informasi kepegawaian Polri?", "id": "SIPP (Sistem Informasi Personel Polri)." },
  { "en": "Manfaat integrasi data bagi Polri?", "id": "Mempercepat proses lidik dan sidik." },
  { "en": "Peran keluarga sebagai benteng ideologi?", "id": "Memberi dukungan moral, ingatkan kebaikan." },
  { "en": "Dampak masalah rumah tangga pada dinas?", "id": "Menurunkan konsentrasi dan kinerja." },
  { "en": "Peran Bhayangkari bagi ketahanan keluarga?", "id": "Membina keharmonisan rumah tangga anggota." },
  { "en": "Sikap terhadap lingkungan tempat tinggal?", "id": "Menjadi contoh warga negara baik." },
  { "en": "Kewajiban sosial seorang anggota Polri?", "id": "Peka dan peduli kondisi lingkungan." },
  { "en": "Filosofi pasukan Dalmas (Pengendalian Massa)?", "id": "Melindungi pengunjuk rasa dan fasilitas umum." },
  { "en": "Urutan penggunaan kekuatan pasukan Dalmas?", "id": "Dalmas awal, Dalmas lanjut, penindak." },
  { "en": "Prinsip utama pasukan anti huru-hara?", "id": "Kendali diri, solid, tidak terprovokasi." },
  { "en": "Kapan gas air mata digunakan?", "id": "Opsi terakhir untuk membubarkan massa." },
  { "en": "Tindakan pasca pengamanan unjuk rasa?", "id": "Melakukan konsolidasi pasukan dan anev." },
  { "en": "Apa itu 'whistleblowing system'?", "id": "Sistem pelaporan pelanggaran secara internal." },
  { "en": "Perlindungan bagi 'whistleblower' internal?", "id": "Wajib dijamin kerahasiaan dan keamanannya." },
  { "en": "Pentingnya riset bagi kepolisian?", "id": "Mengembangkan taktik berbasis bukti ilmiah." },
  { "en": "Lembaga pendidikan tertinggi di Polri?", "id": "Sespimti (Sekolah Staf Pimpinan Tinggi)." },
  { "en": "Tujuan dari pendidikan Sespimti?", "id": "Mempersiapkan calon pemimpin tingkat nasional." },
  { "en": "Mentalitas seorang 'peacekeeper' di PBB?", "id": "Netral, menghormati budaya lokal, disiplin." },
  { "en": "Tantangan tugas sebagai pasukan perdamaian?", "id": "Perbedaan bahasa, budaya, dan medan." },
  { "en": "Sikap saat melakukan razia kendaraan?", "id": "Humanis, sopan, dan sesuai prosedur." },
  { "en": "Dokumen yang wajib ditunjukkan pengemudi?", "id": "SIM dan STNK yang masih berlaku." },
  { "en": "Sikap terhadap pelanggar lalu lintas?", "id": "Memberi edukasi, bukan hanya menindak." },
  { "en": "Filosofi dari lampu rotator biru?", "id": "Tanda kehadiran polisi demi keamanan." },
  { "en": "Kapan sirene dan rotator digunakan?", "id": "Hanya untuk kepentingan dinas darurat." },
  { "en": "Sikap saat menemukan hewan terlantar?", "id": "Menghubungi dinas terkait atau komunitas." },
  { "en": "Peran Polri dalam siskamling?", "id": "Sebagai pembina dan motivator warga." },
  { "en": "Sikap terhadap profesi jurnalis/pers?", "id": "Sebagai mitra strategis penyebar informasi." },
  { "en": "Hak tolak seorang jurnalis?", "id": "Hak untuk tidak ungkapkan identitas narasumber." },
  { "en": "Pentingnya menjaga arsip sebuah perkara?", "id": "Untuk kepentingan hukum di masa depan." },
  { "en": "Ideologi dalam merawat ruang tahanan?", "id": "Menjaga kebersihan dan kelayakan." },
  { "en": "Hak tahanan yang tidak dapat dikurangi?", "id": "Hak untuk hidup & tidak disiksa." },
  { "en": "Pentingnya soliditas antar angkatan/letting?", "id": "Menjaga jiwa korsa dan kekompakan." },
  { "en": "Contoh solidaritas positif antar letting?", "id": "Saling bantu dalam hal kebaikan." },
  { "en": "Sikap terhadap junior di kepolisian?", "id": "Membina, membimbing, dan melindungi." },
  { "en": "Sikap terhadap senior di kepolisian?", "id": "Hormat, sopan, dan meminta arahan." },
  { "en": "Mentalitas dalam mengikuti apel?", "id": "Disiplin waktu dan siap terima arahan." },
  { "en": "Pentingnya kerapian dalam berseragam?", "id": "Cerminan dari kepribadian dan disiplin." },
  { "en": "Bagaimana jika seragam rusak/sobek?", "id": "Segera diperbaiki atau diganti baru." },
  { "en": "Sikap saat melihat bendera merah putih?", "id": "Memberikan penghormatan jika memungkinkan." },
  { "en": "Pentingnya menyanyikan lagu Indonesia Raya?", "id": "Meningkatkan rasa cinta tanah air." },
  { "en": "Mentalitas 'problem solving' bagi polisi?", "id": "Fokus pada solusi, bukan masalah." },
  { "en": "Pentingnya kemampuan 'de-escalation'?", "id": "Meredakan situasi tegang tanpa kekerasan." },
  { "en": "Contoh teknik de-eskalasi?", "id": "Berbicara tenang, menjaga jarak aman." },
  { "en": "Sikap terhadap pengemudi ojek online?", "id": "Sebagai mitra dalam menjaga kamtibmas." },
  { "en": "Peran serta dalam program pemerintah?", "id": "Mendukung dan mengamankan kebijakan pemerintah." },
  { "en": "Contoh dukungan pada program pemerintah?", "id": "Pengamanan distribusi bantuan sosial (Bansos)." },
  { "en": "Ideologi dalam menghadapi bencana alam?", "id": "Totalitas menolong korban tanpa pamrih." },
  { "en": "Tugas pertama saat tiba di lokasi bencana?", "id": "Melakukan pencarian dan penyelamatan korban." },
  { "en": "Pentingnya menjaga stamina saat tugas bencana?", "id": "Agar bisa bekerja optimal berkelanjutan." },
  { "en": "Sikap terhadap relawan di lokasi bencana?", "id": "Bekerja sama dan saling mendukung." },
  { "en": "Mentalitas dalam menjaga perbatasan?", "id": "NKRI harga mati." },
  { "en": "Ancaman di wilayah perbatasan negara?", "id": "Penyelundupan, narkoba, dan pergeseran patok." },
  { "en": "Pentingnya sinergi dengan tentara (TNI)?", "id": "Fondasi utama pertahanan dan keamanan." },
  { "en": "Contoh sinergi TNI-Polri di lapangan?", "id": "Patroli bersama, olahraga bersama." },
  { "en": "Salam khas sinergi TNI-Polri?", "id": "Salam Komando atau Salam Presisi." },
  { "en": "Sikap jika ada gesekan dengan TNI?", "id": "Segera melapor dan selesaikan damai." },
  { "en": "Pentingnya menjaga netralitas dalam Pilkada?", "id": "Agar tidak memihak salah satu calon." },
  { "en": "Larangan bagi Polri selama Pilkada?", "id": "Foto bersama calon, ikut kampanye." },
  { "en": "Sikap terhadap ASN (Aparatur Sipil Negara)?", "id": "Sebagai mitra kerja dalam pemerintahan." },
  { "en": "Filosofi ' Promoter'?", "id": "Profesional, Modern, dan Terpercaya." },
  { "en": "Program 'Promoter' dicetuskan oleh siapa?", "id": "Jenderal Polisi (Purn.) Tito Karnavian." },
  { "en": "Makna Polri sebagai pelindung?", "id": "Memberi rasa aman dari segala ancaman." },
  { "en": "Makna Polri sebagai pengayom?", "id": "Menjadi tempat berlindung dan berkeluh kesah." },
  { "en": "Makna Polri sebagai pelayan?", "id": "Siap membantu kebutuhan masyarakat." },
  { "en": "Sikap terhadap barang temuan masyarakat?", "id": "Mengamankan dan mencari pemiliknya." },
  { "en": "Ideologi dalam menjaga kesehatan pribadi?", "id": "Tubuh sehat menunjang pengabdian maksimal." },
  { "en": "Pentingnya istirahat yang cukup?", "id": "Memulihkan energi dan kejernihan pikiran." },
  { "en": "Sikap dalam mengelola keuangan pribadi?", "id": "Tidak besar pasak daripada tiang." },
  { "en": "Bahaya dari terlilit utang?", "id": "Dapat memicu tindakan penyimpangan/korupsi." },
  { "en": "Sikap terhadap tawaran 'beking'?", "id": "Tegas menolak karena melanggar hukum." },
  { "en": "Arti 'beking' dalam konteks negatif?", "id": "Melindungi kejahatan dengan kekuasaan." },
  { "en": "Pentingnya kemampuan beradaptasi (adaptability)?", "id": "Agar tidak tertinggal oleh perubahan." },
  { "en": "Mentalitas dalam belajar hal baru?", "id": "Terbuka, rendah hati, dan antusias." },
  { "en": "Sumber belajar seorang anggota Polri?", "id": "Pendidikan, pelatihan, dan pengalaman." },
  { "en": "Ideologi sebagai sumber motivasi?", "id": "Memberi alasan mulia untuk mengabdi." },
  { "en": "Cara termudah mengimplementasikan Tribrata?", "id": "Bekerja dengan tulus dan ikhlas." },
  { "en": "Apa itu 'evidence-based policing'?", "id": "Tindakan kepolisian berbasis bukti ilmiah." },
  { "en": "Bahaya dari stereotip dalam bertugas?", "id": "Menimbulkan prasangka dan tindakan diskriminatif." },
  { "en": "Dasar keyakinan seorang penyidik?", "id": "Minimal dua alat bukti sah." },
  { "en": "Bagaimana ideologi memerangi prasangka?", "id": "Mengajarkan untuk melihat semua setara." },
  { "en": "Apa itu keadilan prosedural?", "id": "Keadilan yang didapat melalui prosedur." },
  { "en": "Apa itu keadilan substantif?", "id": "Keadilan yang dirasakan secara nyata." },
  { "en": "Prioritas Polri terkait keadilan?", "id": "Menyeimbangkan keadilan prosedural dan substantif." },
  { "en": "Warisan terpenting seorang anggota Polri?", "id": "Nama baik dan kehormatan profesi." },
  { "en": "Cara membangun 'legacy' yang baik?", "id": "Bekerja tulus, jujur, dan bermanfaat." },
  { "en": "Sikap 'menjadi garam dan terang'?", "id": "Memberi pengaruh baik di manapun." },
  { "en": "Mengapa keteladanan pimpinan sangat penting?", "id": "Anggota cenderung meniru perilaku pimpinannya." },
  { "en": "Sikap pimpinan yang tidak patut?", "id": "Arogan, koruptif, dan tidak peduli." },
  { "en": "Apa itu manajemen risiko operasional?", "id": "Mengidentifikasi dan mitigasi bahaya tugas." },
  { "en": "Risiko terbesar bagi citra Polri?", "id": "Pelanggaran oleh anggota yang viral." },
  { "en": "Fungsi Litbang (Penelitian dan Pengembangan)?", "id": "Mengkaji dan mengembangkan ilmu kepolisian." },
  { "en": "Etika dalam pengembangan taktik baru?", "id": "Harus sesuai HAM dan perundangan." },
  { "en": "Tugas utama dari Satgas Pangan?", "id": "Menjaga stabilitas harga dan pasokan." },
  { "en": "Ideologi di balik Satgas Pangan?", "id": "Menjamin kebutuhan pokok masyarakat aman." },
  { "en": "Peran Polri dalam sengketa agraria?", "id": "Mengamankan proses, tidak memihak." },
  { "en": "Program mendekatkan polisi ke lingkungan?", "id": "Program Polisi RW." },
  { "en": "Tujuan utama dari Polisi RW?", "id": "Deteksi dini dan pemecahan masalah." },
  { "en": "Bolehkah anggota Polri memiliki bisnis?", "id": "Boleh, asal tidak ganggu dinas." },
  { "en": "Jenis bisnis yang dilarang bagi Polri?", "id": "Terkait perjudian, miras, pengamanan ilegal." },
  { "en": "Sikap saat ada masalah dengan tetangga?", "id": "Menyelesaikan secara kekeluargaan dan damai." },
  { "en": "Apa itu kepemimpinan situasional?", "id": "Gaya memimpin sesuai kondisi anggota." },
  { "en": "Pentingnya mawas diri bagi polisi?", "id": "Untuk mengoreksi kesalahan dan kekurangan." },
  { "en": "Kapan waktu terbaik untuk introspeksi?", "id": "Setiap selesai melaksanakan suatu tugas." },
  { "en": "Filosofi di balik penggunaan borgol?", "id": "Bukan menyakiti, tapi membatasi perlawanan." },
  { "en": "Filosofi di balik tongkat polisi?", "id": "Alat pertahanan diri paling dasar." },
  { "en": "Pentingnya senyum tulus saat melayani?", "id": "Mencairkan suasana, menunjukkan keramahan." },
  { "en": "Sikap menghadapi adanya laporan palsu?", "id": "Tetap tenang, dalami, bisa dipidana." },
  { "en": "Apa itu 'contempt of court'?", "id": "Perbuatan menghina atau melawan pengadilan." },
  { "en": "Sikap Polri terhadap perintah pengadilan?", "id": "Wajib melaksanakan dengan penuh tanggungjawab." },
  { "en": "Pentingnya menjaga kesehatan mental keluarga?", "id": "Keluarga tenang, anggota dinas fokus." },
  { "en": "Dampak positif dari olahraga bersama?", "id": "Membangun kekompakan tim dan kesehatan." },
  { "en": "Sikap dalam tim investigasi gabungan?", "id": "Hilangkan ego sektoral, fokus tujuan." },
  { "en": "Contoh lembaga investigasi gabungan?", "id": "Dengan KPK, BNN, atau PPATK." },
  { "en": "Kepanjangan dari PPATK?", "id": "Pusat Pelaporan Analisis Transaksi Keuangan." },
  { "en": "Peran Polri cegah pencucian uang?", "id": "Menyelidiki tindak pidana asalnya." },
  { "en": "Mentalitas saat pertama kali dinas?", "id": "Cepat beradaptasi dan terus belajar." },
  { "en": "Sikap terhadap tokoh agama/adat?", "id": "Menghormati dan menjalin komunikasi baik." },
  { "en": "Manfaat silaturahmi dengan para tokoh?", "id": "Mempermudah 'cooling system' situasi kamtibmas." },
  { "en": "Ideologi dalam mengelola barang bukti?", "id": "Menjaga keutuhan untuk kepentingan pembuktian." },
  { "en": "Bahaya dari barang bukti yang rusak?", "id": "Dapat melemahkan proses penuntutan." },
  { "en": "Tujuan pelabelan pada barang bukti?", "id": "Agar tidak tertukar dan jelas." },
  { "en": "Sikap jika keluarga terlibat pidana?", "id": "Serahkan proses pada unit berwenang." },
  { "en": "Prinsip 'nemo judex in causa sua'?", "id": "Tidak boleh jadi hakim kasusnya sendiri." },
  { "en": "Mentalitas 'knowledge is power'?", "id": "Pengetahuan membuat tindakan lebih berkualitas." },
  { "en": "Cara terus memperbarui pengetahuan hukum?", "id": "Membaca peraturan baru dan yurisprudensi." },
  { "en": "Sikap saat menangani kasus sensitif SARA?", "id": "Sangat hati-hati dan tidak berpihak." },
  { "en": "Tujuan utama gelar perkara?", "id": "Menentukan status perkara dan langkah lanjut." },
  { "en": "Siapa saja yang ikut gelar perkara?", "id": "Penyidik, pengawas, dan fungsi terkait." },
  { "en": "Sikap saat pendapat berbeda dalam gelar?", "id": "Menghargai, beradu argumen dengan data." },
  { "en": "Apa itu 'scientific crime investigation'?", "id": "Penyidikan kejahatan secara ilmiah." },
  { "en": "Manfaat 'scientific crime investigation'?", "id": "Mengurangi potensi kesalahan dalam penyidikan." },
  { "en": "Contoh penerapan 'scientific crime investigation'?", "id": "Tes DNA, analisis balistik, forensik." },
  { "en": "Sikap terhadap perbedaan angkatan/generasi?", "id": "Saling menghormati dan belajar." },
  { "en": "Pesan untuk junior yang baru masuk?", "id": "Cepat belajar dan jangan sombong." },
  { "en": "Sikap saat dikoreksi oleh junior?", "id": "Terbuka dan berterima kasih." },
  { "en": "Pentingnya menjaga data pribadi masyarakat?", "id": "Amanah yang tidak boleh disalahgunakan." },
  { "en": "Konsekuensi membocorkan data pribadi?", "id": "Sanksi pidana dan pelanggaran etik." },
  { "en": "Sikap dalam menjaga kata sandi sistem?", "id": "Rahasia pribadi yang tidak dibagikan." },
  { "en": "Mentalitas menghadapi kritik di medsos?", "id": "Filter informasi, ambil yang membangun." },
  { "en": "Sikap jika difitnah di medsos?", "id": "Laporkan melalui jalur hukum yang ada." },
  { "en": "Pentingnya kemampuan manajemen waktu?", "id": "Agar semua tugas selesai tepat waktu." },
  { "en": "Teknik manajemen waktu yang baik?", "id": "Membuat skala prioritas pekerjaan." },
  { "en": "Pekerjaan prioritas tertinggi adalah?", "id": "Pekerjaan penting dan mendesak." },
  { "en": "Mentalitas saat mengikuti pendidikan pengembangan?", "id": "Serius, fokus, dan menyerap ilmu." },
  { "en": "Tujuan dari pendidikan pengembangan?", "id": "Meningkatkan kompetensi dan jenjang karier." },
  { "en": "Contoh pendidikan pengembangan (Dikbang)?", "id": "Sespimma, Sespimmen, dan Sespimti." },
  { "en": "Ideologi dalam penggunaan fasilitas kantor?", "id": "Untuk kepentingan dinas, bukan pribadi." },
  { "en": "Sikap saat listrik kantor padam?", "id": "Tetap bekerja dengan sarana alternatif." },
  { "en": "Pentingnya program 'Jumat Curhat'?", "id": "Menyerap aspirasi masyarakat secara langsung." },
  { "en": "Sikap polisi saat 'Jumat Curhat'?", "id": "Mendengar aktif dan memberi solusi." },
  { "en": "Mentalitas dalam menjaga pertemanan?", "id": "Setia kawan dalam hal kebaikan." },
  { "en": "Sikap jika teman melakukan pelanggaran?", "id": "Mengingatkan dengan baik dan benar." },
  { "en": "Makna 'integritas adalah mata uang'?", "id": "Integritas adalah hal paling berharga." },
  { "en": "Cara termudah menjaga integritas?", "id": "Menolak hal kecil yang tidak benar." },
  { "en": "Sikap terhadap korban pinjaman online?", "id": "Memberi edukasi dan membantu pelaporan." },
  { "en": "Ancaman dari pinjaman online ilegal?", "id": "Bunga tinggi dan cara penagihan." },
  { "en": "Sikap terhadap pelaku 'flexing'?", "id": "Tidak iri dan fokus pada pengabdian." },
  { "en": "Apa itu 'flexing'?", "id": "Memamerkan kekayaan atau kemewahan." },
  { "en": "Mentalitas dalam menghadapi godaan suap?", "id": "Iman, integritas, dan ingat keluarga." },
  { "en": "Apa beda suap dan gratifikasi?", "id": "Suap ada kesepakatan, gratifikasi tidak." },
  { "en": "Kewajiban jika menerima gratifikasi?", "id": "Wajib melaporkan ke KPK atau UPG." },
  { "en": "Kepanjangan dari UPG?", "id": "Unit Pengendali Gratifikasi." },
  { "en": "Pentingnya kesabaran dalam bertugas?", "id": "Mencegah tindakan gegabah dan emosional." },
  { "en": "Cara melatih kesabaran?", "id": "Latihan pernapasan dan berpikir jernih." },
  { "en": "Mentalitas dalam menghadapi tugas rutin?", "id": "Tidak bosan dan tetap waspada." },
  { "en": "Bahaya dari menganggap remeh tugas?", "id": "Menyebabkan kelalaian yang berakibat fatal." },
  { "en": "Pentingnya bersyukur sebagai anggota Polri?", "id": "Meningkatkan keikhlasan dalam mengabdi." },
  { "en": "Cara bersyukur yang paling mudah?", "id": "Menjalankan tugas dengan sebaik-baiknya." },
  { "en": "Ideologi sebagai sumber kekuatan tak terlihat?", "id": "Memberi semangat saat lelah/putus asa." },
  { "en": "Tujuan akhir mental ideologi kuat?", "id": "Polri yang dicintai dan dipercaya." },
  { "en": "Apa itu 'green criminology'?", "id": "Kajian tentang kejahatan terhadap lingkungan." },
  { "en": "Sikap Polri pada kejahatan lingkungan?", "id": "Menindak tegas sebagai kejahatan serius." },
  { "en": "Ideologi dalam konsep 'smart policing'?", "id": "Teknologi untuk efisiensi, bukan dehumanisasi." },
  { "en": "Peran kearifan lokal bagi kamtibmas?", "id": "Mekanisme penyelesaian masalah di masyarakat." },
  { "en": "Contoh sinergi dengan kearifan lokal?", "id": "Musyawarah adat untuk selesaikan sengketa." },
  { "en": "Sikap jika kearifan lokal lawan hukum?", "id": "Edukasi dan utamakan hukum positif." },
  { "en": "Prinsip saat hadapi krisis komunikasi?", "id": "Cepat, akurat, transparan, dan empatik." },
  { "en": "Bahaya sikap 'no comment' saat krisis?", "id": "Menimbulkan spekulasi dan ketidakpercayaan publik." },
  { "en": "Siapa yang boleh bicara saat krisis?", "id": "Juru bicara resmi yang ditunjuk." },
  { "en": "Tujuan 'press conference' saat krisis?", "id": "Memberi informasi utuh, meredam rumor." },
  { "en": "Pandangan ideologi terhadap massa aksi?", "id": "Warga negara yang menyampaikan aspirasi." },
  { "en": "Faktor pemicu anarkisme massa?", "id": "Provokasi, rasa ketidakadilan, dan hoaks." },
  { "en": "Etika penggunaan teknologi 'facial recognition'?", "id": "Harus memiliki dasar hukum jelas." },
  { "en": "Tantangan penggunaan AI dalam kepolisian?", "id": "Potensi bias algoritma dan akuntabilitas." },
  { "en": "Ideologi sebagai panduan etika AI?", "id": "AI harus adil dan non-diskriminatif." },
  { "en": "Inti dari filosofi 'servant leadership'?", "id": "Pemimpin yang melayani kebutuhan timnya." },
  { "en": "Cara menerapkan 'servant leadership'?", "id": "Mendengar, empati, dan mengembangkan anggota." },
  { "en": "Apa itu resiliensi institusional?", "id": "Kemampuan lembaga bangkit dari krisis." },
  { "en": "Cara ideologi membangun resiliensi pribadi?", "id": "Memberi makna dan tujuan pengabdian." },
  { "en": "Sikap saat menghadapi kegagalan besar?", "id": "Evaluasi, akui kesalahan, dan perbaiki." },
  { "en": "Sumber utama legitimasi seorang polisi?", "id": "Kepercayaan dan penerimaan dari masyarakat." },
  { "en": "Bagaimana cara mendapatkan legitimasi publik?", "id": "Melalui tindakan adil dan profesional." },
  { "en": "Dampak dari hilangnya legitimasi?", "id": "Masyarakat tidak patuh, menolak kerjasama." },
  { "en": "Faktor utama perusak kepercayaan publik?", "id": "Arogansi, korupsi, kekerasan berlebihan." },
  { "en": "Cara membangun kembali kepercayaan publik?", "id": "Perbaikan nyata dan komunikasi terbuka." },
  { "en": "Sikap terhadap 'debt collector' kasar?", "id": "Menindak tegas karena meresahkan masyarakat." },
  { "en": "Nama operasi pengamanan Idul Fitri?", "id": "Operasi Ketupat." },
  { "en": "Ideologi di balik Operasi Ketupat?", "id": "Negara hadir menjamin keamanan hari raya." },
  { "en": "Nama operasi pengamanan Natal/Tahun Baru?", "id": "Operasi Lilin." },
  { "en": "Sikap saat mengamankan rumah ibadah?", "id": "Hormat, sopan, tidak masuk sembarangan." },
  { "en": "Mentalitas dalam operasi kemanusiaan?", "id": "Menolong sesama adalah panggilan jiwa." },
  { "en": "Sikap terhadap WNA langgar hukum?", "id": "Diproses sesuai hukum yang berlaku." },
  { "en": "Sinergi dengan Ditjen Imigrasi?", "id": "Pengawasan dan penindakan orang asing." },
  { "en": "Ideologi dalam menjaga data pribadi anggota?", "id": "Melindungi privasi personel dan keluarga." },
  { "en": "Pentingnya 'tour of duty/area'?", "id": "Menambah pengalaman dan wawasan kebangsaan." },
  { "en": "Mentalitas saat mendapat perintah mutasi?", "id": "Siap ditempatkan di seluruh Indonesia." },
  { "en": "Apa itu 'Standar Operasional Prosedur'?", "id": "Panduan kerja agar seragam/terukur." },
  { "en": "Mengapa harus selalu patuh pada SOP?", "id": "Menghindari kesalahan dan melindungi diri." },
  { "en": "Bolehkah menyimpang dari SOP?", "id": "Boleh, jika demi keselamatan (diskresi)." },
  { "en": "Sikap saat menjadi komandan upacara?", "id": "Tegas, suara lantang, dan berwibawa." },
  { "en": "Pentingnya latihan baris-berbaris (PBB)?", "id": "Melatih disiplin, kekompakan, dan kepatuhan." },
  { "en": "Filosofi di balik gerakan PBB?", "id": "Satu komando, satu gerakan serentak." },
  { "en": "Sikap dalam rapat dengan instansi lain?", "id": "Menjaga nama baik, kolaboratif, profesional." },
  { "en": "Mentalitas menghadapi 'deadlock' dalam rapat?", "id": "Mencari titik temu, bukan menang sendiri." },
  { "en": "Pentingnya membawa buku catatan saku?", "id": "Mencatat hal penting, jadi bukti." },
  { "en": "Sikap jika menemukan dompet di jalan?", "id": "Amankan, cari identitas, hubungi pemilik." },
  { "en": "Tujuan dari gelar pasukan?", "id": "Mengecek kesiapan personel dan alutsista." },
  { "en": "Mentalitas saat mengikuti gelar pasukan?", "id": "Bangga, siaga, dan penuh semangat." },
  { "en": "Sikap terhadap pemuda dan mahasiswa?", "id": "Sebagai mitra kritis dan calon pemimpin." },
  { "en": "Program Polri untuk kaum milenial?", "id": "Police Goes to Campus/School." },
  { "en": "Ideologi dalam menjaga aset sitaan?", "id": "Menjaga amanah pengadilan hingga putusan." },
  { "en": "Tempat penyimpanan barang bukti/sitaan?", "id": "Rumah Penyimpanan Benda Sitaan Negara." },
  { "en": "Kepanjangan dari Rupbasan?", "id": "Rumah Penyimpanan Benda Sitaan Negara." },
  { "en": "Sikap terhadap anggota yang sakit?", "id": "Menjenguk dan memberikan dukungan moril." },
  { "en": "Pentingnya menjaga kesehatan gigi?", "id": "Mendukung penampilan dan kepercayaan diri." },
  { "en": "Prinsip 'zero tolerance' untuk apa?", "id": "Untuk narkoba dan pelanggaran berat." },
  { "en": "Pentingnya kemampuan investigasi berbasis siber?", "id": "Mengikuti perkembangan modus kejahatan digital." },
  { "en": "Apa itu 'digital footprint'?", "id": "Jejak digital yang ditinggalkan online." },
  { "en": "Pentingnya menjaga 'digital footprint'?", "id": "Jejak digital cerminkan kepribadian." },
  { "en": "Sikap saat patroli di malam hari?", "id": "Waspada, teliti, dan tidak lengah." },
  { "en": "Tujuan utama patroli malam hari?", "id": "Mencegah kejahatan saat jam rawan." },
  { "en": "Sikap terhadap warga ronda malam?", "id": "Apresiasi dan berikan motivasi." },
  { "en": "Pentingnya kemampuan interogasi yang baik?", "id": "Menggali keterangan tanpa paksaan/kekerasan." },
  { "en": "Seni dalam bertanya saat interogasi?", "id": "Terbuka, terarah, dan tidak menghakimi." },
  { "en": "Mentalitas dalam menjaga ruang kerja?", "id": "Bersih, rapi, dan terorganisir." },
  { "en": "Manfaat ruang kerja yang rapi?", "id": "Meningkatkan mood dan produktivitas kerja." },
  { "en": "Sikap saat menerima telepon pengaduan?", "id": "Ramah, catat detail, berikan kepastian." },
  { "en": "Ideologi dalam mengelola media sosial institusi?", "id": "Informatif, edukatif, dan humanis." },
  { "en": "Konten yang harus dihindari di medsos?", "id": "Konten pamer kuasa atau sensitif." },
  { "en": "Pentingnya 'sense of crisis'?", "id": "Kepekaan terhadap potensi masalah besar." },
  { "en": "Pentingnya 'sense of responsibility'?", "id": "Rasa tanggung jawab atas setiap tugas." },
  { "en": "Sikap saat melihat rekan kelelahan?", "id": "Menawarkan bantuan, saling menjaga." },
  { "en": "Pentingnya memiliki mentor dalam dinas?", "id": "Untuk bimbingan karier dan nasihat." },
  { "en": "Sikap sebagai seorang mentor?", "id": "Ikhlas berbagi ilmu dan pengalaman." },
  { "en": "Tujuan dari program 'commander wish'?", "id": "Menerjemahkan kebijakan pimpinan secara cepat." },
  { "en": "Sikap terhadap kebijakan baru pimpinan?", "id": "Mendukung dan melaksanakan dengan baik." },
  { "en": "Mentalitas dalam belajar sejarah bangsa?", "id": "Menghargai jasa pahlawan, perkuat nasionalisme." },
  { "en": "Pentingnya ziarah ke makam pahlawan?", "id": "Mengenang jasa dan meneladani semangatnya." },
  { "en": "Ideologi dalam menghadapi berita duka?", "id": "Tabah, saling menguatkan, bantu keluarga." },
  { "en": "Sikap saat menjadi perwakilan institusi?", "id": "Jaga sikap, tutur kata, penampilan." },
  { "en": "Pentingnya kemampuan negosiasi lintas budaya?", "id": "Agar efektif saat berhadapan dengan WNA." },
  { "en": "Mentalitas 'continuous improvement'?", "id": "Selalu berusaha menjadi lebih baik." },
  { "en": "Cara menerapkan 'continuous improvement'?", "id": "Evaluasi diri, terima masukan, belajar." },
  { "en": "Sikap saat mengikuti sertifikasi keahlian?", "id": "Serius untuk meningkatkan kompetensi diri." },
  { "en": "Manfaat sertifikasi keahlian?", "id": "Pengakuan kompetensi dan menunjang karier." },
  { "en": "Pentingnya menjaga hubungan dengan pensiunan?", "id": "Menimba pengalaman dan menjaga silaturahmi." },
  { "en": "Sikap saat hari libur nasional?", "id": "Tetap siaga jika dibutuhkan dinas." },
  { "en": "Mentalitas 'on call' 24 jam?", "id": "Siap sedia setiap saat negara memanggil." },
  { "en": "Pentingnya manajemen stres yang baik?", "id": "Agar tidak berdampak buruk pada keluarga." },
  { "en": "Kegiatan positif untuk lepas stres?", "id": "Olahraga, hobi, dan berkumpul keluarga." },
  { "en": "Ideologi sebagai sumber kebahagiaan batin?", "id": "Bahagia karena bisa mengabdi/bermanfaat." },
  { "en": "Kunci utama menjadi polisi hebat?", "id": "Bermental ideologi kuat dan tulus." },
  { "en": "Tantangan Polri di era metaverse?", "id": "Kejahatan virtual, penipuan, dan pelecehan." },
  { "en": "Ideologi hadapi kejahatan berbasis AI?", "id": "Adaptif, terus belajar, dorong regulasi." },
  { "en": "Prinsip 'human in the loop'?", "id": "Keputusan akhir tetap di tangan manusia." },
  { "en": "Tujuan utama modernisasi alutsista Polri?", "id": "Tingkatkan efektivitas dan lindungi anggota." },
  { "en": "Sikap terhadap penggunaan drone pengawas?", "id": "Efektif, namun tetap hormati privasi." },
  { "en": "Dasar hukum penindakan kekerasan hewan?", "id": "KUHP dan UU Kesejahteraan Hewan." },
  { "en": "Ideologi di balik perlindungan hewan?", "id": "Kemanusiaan mencakup perlindungan semua makhluk." },
  { "en": "Peran unit satwa (K-9)?", "id": "Sebagai mitra kerja yang dirawat." },
  { "en": "Sikap Polri terhadap perburuan liar?", "id": "Menindak tegas sebagai perusak ekosistem." },
  { "en": "Apa itu manajemen talenta di Polri?", "id": "Mencari, mengembangkan, mempertahankan bibit unggul." },
  { "en": "Ideologi dalam sistem manajemen talenta?", "id": "Meritokrasi berdasarkan prestasi dan integritas." },
  { "en": "Tujuan dari 'assessment center' Polri?", "id": "Memetakan kompetensi untuk jabatan tepat." },
  { "en": "Pentingnya regenerasi kepemimpinan di Polri?", "id": "Menjamin keberlanjutan organisasi yang sehat." },
  { "en": "Apa itu 'confirmation bias'?", "id": "Cari bukti yang dukung keyakinan awal." },
  { "en": "Cara ideologi melawan 'confirmation bias'?", "id": "Mengajarkan objektivitas dan pikiran terbuka." },
  { "en": "Apa itu 'duty to intervene'?", "id": "Kewajiban intervensi rekan yang melanggar." },
  { "en": "Tujuan dari kewajiban 'duty to intervene'?", "id": "Mencegah pelanggaran, jaga nama institusi." },
  { "en": "Apa prinsip 'sanctity of life'?", "id": "Menghargai kesucian hidup setiap manusia." },
  { "en": "Bagaimana prinsip ini memandu penggunaan senpi?", "id": "Senjata api adalah alternatif terakhir." },
  { "en": "Makna historis dari 'Bhayangkara Negara'?", "id": "Pasukan elit pengawal Kerajaan Majapahit." },
  { "en": "Relevansi 'Bhayangkara Negara' untuk Polri?", "id": "Polri sebagai pengawal negara Pancasila." },
  { "en": "Siapa Mahapatih di balik Bhayangkara?", "id": "Mahapatih Gajah Mada." },
  { "en": "Sumpah Gajah Mada yang terkenal?", "id": "Sumpah Palapa." },
  { "en": "Nilai dari Sumpah Palapa bagi Polri?", "id": "Semangat persatuan dan keutuhan wilayah." },
  { "en": "Mentalitas saat menjaga objek demonstrasi?", "id": "Lindungi aset dan orangnya sekaligus." },
  { "en": "Sikap hadapi hoaks tentang institusi?", "id": "Tidak emosi, klarifikasi melalui Humas." },
  { "en": "Pentingnya pemisahan berkas (splitsing)?", "id": "Efektivitas pembuktian peran masing-masing tersangka." },
  { "en": "Ideologi dalam melakukan olah TKP?", "id": "Teliti, sabar, dan tidak merusak bukti." },
  { "en": "Pentingnya memakai sarung tangan di TKP?", "id": "Mencegah kontaminasi sidik jari/DNA." },
  { "en": "Sikap saat menemukan korban bunuh diri?", "id": "Tetap lakukan penyelidikan sesuai prosedur." },
  { "en": "Pentingnya menjaga hubungan baik dengan kampus?", "id": "Sumber kajian ilmiah calon anggota." },
  { "en": "Sikap terhadap kritik dari akademisi?", "id": "Menjadi bahan masukan berbasis riset." },
  { "en": "Peran Sentra Pelayanan Kepolisian Terpadu?", "id": "Gerbang utama pelayanan publik Polri." },
  { "en": "Kepanjangan dari SPKT?", "id": "Sentra Pelayanan Kepolisian Terpadu." },
  { "en": "Mentalitas utama petugas di SPKT?", "id": "Sabar, solutif, dan penuh empati." },
  { "en": "Ideologi dalam manajemen lalu lintas?", "id": "Kelancaran dan keselamatan adalah prioritas." },
  { "en": "Tujuan dari rekayasa lalu lintas?", "id": "Mengurangi kemacetan dan angka kecelakaan." },
  { "en": "Contoh dari rekayasa lalu lintas?", "id": "Sistem ganjil-genap atau contraflow." },
  { "en": "Sikap saat berdebat dengan penasihat hukum?", "id": "Profesional, adu data, bukan emosi." },
  { "en": "Pentingnya sinergi dengan Damkar?", "id": "Kerja sama penanganan kebakaran/bencana." },
  { "en": "Pentingnya sinergi dengan Basarnas?", "id": "Kerja sama operasi pencarian/penyelamatan." },
  { "en": "Kepanjangan dari Basarnas?", "id": "Badan Nasional Pencarian dan Pertolongan." },
  { "en": "Mentalitas dalam menghadapi sidang disiplin?", "id": "Jujur, ksatria, dan siap bertanggungjawab." },
  { "en": "Tujuan dari sidang disiplin/etik?", "id": "Menjaga kehormatan dan martabat profesi." },
  { "en": "Sikap setelah menjalani hukuman disiplin?", "id": "Memperbaiki diri dan tidak mengulangi." },
  { "en": "Ideologi dalam menjaga keharmonisan beragama?", "id": "Menjamin kebebasan beribadah setiap warga." },
  { "en": "Peran Polri dalam hari besar keagamaan?", "id": "Mengamankan agar ibadah berjalan khidmat." },
  { "en": "Sikap terhadap simbol-simbol agama?", "id": "Menghormati dan tidak melecehkan." },
  { "en": "Pentingnya memahami 'peran polisi sipil'?", "id": "Bukan penguasa, tapi pelayan masyarakat." },
  { "en": "Ciri khas dari polisi sipil?", "id": "Humanis, transparan, dan akuntabel." },
  { "en": "Mentalitas dalam menjaga perbatasan laut?", "id": "Menjaga setiap jengkal kedaulatan maritim." },
  { "en": "Ancaman di wilayah perbatasan laut?", "id": "Illegal fishing dan penyelundupan." },
  { "en": "Sikap terhadap nelayan negara tetangga?", "id": "Ditindak sesuai hukum jika langgar batas." },
  { "en": "Pentingnya menguasai bahasa daerah setempat?", "id": "Mempermudah komunikasi dan kedekatan." },
  { "en": "Sikap jika tidak paham bahasa daerah?", "id": "Belajar atau meminta bantuan penerjemah." },
  { "en": "Mentalitas saat menjadi ajudan pimpinan?", "id": "Loyal, cekatan, dan menjaga rahasia." },
  { "en": "Tugas utama seorang ajudan?", "id": "Mendukung kelancaran tugas dan agenda." },
  { "en": "Sikap saat tidak sependapat dengan pimpinan?", "id": "Sampaikan masukan di waktu tepat." },
  { "en": "Ideologi dalam menghadapi kemajuan fashion?", "id": "Berpakaian sopan dan sesuai norma." },
  { "en": "Batasan anggota Polri dalam berpenampilan?", "id": "Tidak bertato dan bertindik (laki-laki)." },
  { "en": "Pentingnya kepekaan sosial (social sensitivity)?", "id": "Mampu merasakan kondisi lingkungan sekitar." },
  { "en": "Cara mengasah kepekaan sosial?", "id": "Terlibat aktif dalam kegiatan masyarakat." },
  { "en": "Mentalitas dalam mengelola keuangan keluarga?", "id": "Cermat, hemat, dan hindari utang." },
  { "en": "Bahaya dari gaya hidup konsumtif?", "id": "Mendorong perilaku koruptif dan penyimpangan." },
  { "en": "Sikap saat ditawari kartu kredit gratis?", "id": "Waspada dan ukur kemampuan finansial." },
  { "en": "Ideologi dalam menafsirkan sebuah aturan?", "id": "Demi kepastian hukum dan keadilan." },
  { "en": "Jika ada keraguan dalam tafsir aturan?", "id": "Berkonsultasi dengan fungsi hukum." },
  { "en": "Pentingnya 'Standard Minimum Pelayanan'?", "id": "Menjamin kualitas pelayanan yang sama." },
  { "en": "Sikap jika tidak bisa penuhi standar?", "id": "Evaluasi dan cari solusi perbaikan." },
  { "en": "Mentalitas dalam mengikuti tes kesamaptaan?", "id": "Jujur, sportif, dan semaksimal mungkin." },
  { "en": "Tujuan dari tes kesamaptaan jasmani?", "id": "Mengukur dan menjaga kemampuan fisik." },
  { "en": "Konsekuensi tidak lulus tes kesamaptaan?", "id": "Pembinaan khusus dan mempengaruhi karier." },
  { "en": "Ideologi dalam menjaga kebugaran rohani?", "id": "Mendekatkan diri kepada Tuhan." },
  { "en": "Manfaat dari kebugaran rohani?", "id": "Memberi ketenangan batin dan kendali diri." },
  { "en": "Contoh kegiatan kebugaran rohani?", "id": "Doa bersama, pengajian, atau kebaktian." },
  { "en": "Sikap terhadap keberhasilan rekan kerja?", "id": "Ikut senang dan memberi selamat." },
  { "en": "Bahaya dari sifat iri dan dengki?", "id": "Merusak soliditas dan kerjasama tim." },
  { "en": "Mentalitas dalam kompetisi yang sehat?", "id": "Bersaing dalam prestasi dan kebaikan." },
  { "en": "Ideologi dalam menjaga data rahasia?", "id": "Amanah negara yang harus dijaga." },
  { "en": "Ancaman bagi keamanan data internal?", "id": "Serangan siber dan kelalaian personel." },
  { "en": "Pentingnya 'password' yang kuat?", "id": "Mencegah akses tidak sah." },
  { "en": "Sikap jika ada upaya peretasan?", "id": "Segera laporkan kepada tim siber." },
  { "en": "Mentalitas sebagai 'problem solver' masyarakat?", "id": "Hadir untuk memberi solusi, bukan masalah." },
  { "en": "Langkah pertama dalam 'problem solving'?", "id": "Identifikasi akar masalah secara akurat." },
  { "en": "Pentingnya 'follow-up' setelah pelayanan?", "id": "Memastikan kepuasan dan tuntasnya masalah." },
  { "en": "Sikap dalam menghadapi 'prank' laporan?", "id": "Tegas dan bisa dikenakan sanksi." },
  { "en": "Ideologi dalam mengamankan pemilu damai?", "id": "Netralitas adalah harga mati." },
  { "en": "Tantangan pengamanan pemilu di medsos?", "id": "Black campaign dan penyebaran hoaks." },
  { "en": "Peran patroli siber saat pemilu?", "id": "Memantau dan menindak konten provokatif." },
  { "en": "Mentalitas menghadapi hasil pemilu?", "id": "Mengamankan apapun keputusan MK." },
  { "en": "Sikap terhadap pihak yang tidak puas?", "id": "Mengarahkan untuk menempuh jalur konstitusional." },
  { "en": "Pentingnya menjaga stamina saat pengamanan?", "id": "Agar tetap fokus dan tidak emosional." },
  { "en": "Ideologi sebagai sumber energi tak terbatas?", "id": "Pengabdian tulus tidak mengenal lelah." },
  { "en": "Tanggung jawab ideologis seorang senior?", "id": "Mewariskan nilai luhur pada junior." },
  { "en": "Apa yang tidak boleh diwariskan senior?", "id": "Budaya arogansi dan kekerasan." },
  { "en": "Mentalitas dalam membina seorang junior?", "id": "Seperti adik, bimbing dan lindungi." },
  { "en": "Pentingnya 'transfer of knowledge'?", "id": "Agar pengalaman baik tidak hilang." },
  { "en": "Ciri utama seorang mentor yang baik?", "id": "Memberi teladan, mau mendengar, ikhlas." },
  { "en": "Ideologi dalam menjaga 'work-life balance'?", "id": "Keseimbangan hidup dukung kinerja prima." },
  { "en": "Mengapa keluarga sangat penting bagi dinas?", "id": "Sebagai sumber dukungan moril utama." },
  { "en": "Sikap terhadap masalah dinas di rumah?", "id": "Sebisa mungkin jangan dibawa pulang." },
  { "en": "Cara efektif mengelola stres kerja?", "id": "Olahraga, hobi, dan komunikasi keluarga." },
  { "en": "Pentingnya hak cuti bagi anggota?", "id": "Untuk memulihkan kondisi fisik/psikis." },
  { "en": "Makna seragam bagi seorang polisi?", "id": "Representasi kehadiran negara dan hukum." },
  { "en": "Tanggung jawab saat mengenakan seragam?", "id": "Menjaga setiap perkataan dan perbuatan." },
  { "en": "Filosofi di balik seragam yang rapi?", "id": "Cerminan dari disiplin pribadi/institusi." },
  { "en": "Perasaan yang harus muncul saat berseragam?", "id": "Rasa bangga dan tanggung jawab besar." },
  { "en": "Penyakit utama dalam kerjasama antarlembaga?", "id": "Ego sektoral yang berlebihan." },
  { "en": "Bagaimana Pancasila atasi ego sektoral?", "id": "Dengan semangat Persatuan Indonesia (Sila 3)." },
  { "en": "Sikap dalam rapat bersama TNI?", "id": "Sebagai mitra sejajar saling menghormati." },
  { "en": "Posisi Polri-Kejaksaan dalam peradilan?", "id": "Bagian 'integrated criminal justice system'." },
  { "en": "Bentuk sinergi dengan Pemda?", "id": "Dukungan regulasi dan anggaran kamtibmas." },
  { "en": "Analogi kepercayaan publik bagi Polri?", "id": "Sebagai nyawa atau urat nadi." },
  { "en": "Satu tindakan buruk dapat merusak?", "id": "Kepercayaan yang dibangun ribuan kebaikan." },
  { "en": "Sikap rendah hati (humility) berarti?", "id": "Sadar bahwa kekuatan adalah amanah." },
  { "en": "Mengapa harus bersyukur menjadi polisi?", "id": "Kesempatan mengabdi langsung pada negara." },
  { "en": "Cara paling nyata menunjukkan rasa syukur?", "id": "Bekerja tulus melayani secara maksimal." },
  { "en": "Mentalitas 'long-life learning' bagi Polri?", "id": "Belajar terus menerus sepanjang hayat." },
  { "en": "Sikap saat dihadapkan teknologi baru?", "id": "Mempelajari agar tidak tertinggal zaman." },
  { "en": "Pentingnya membaca buku non-hukum?", "id": "Untuk memperluas wawasan/cara pandang." },
  { "en": "Kapan seorang polisi boleh berhenti belajar?", "id": "Tidak akan pernah berhenti belajar." },
  { "en": "Pentingnya refleksi diri ('self-reflection') harian?", "id": "Mengoreksi diri, merencanakan perbaikan esok." },
  { "en": "Jika atasan memerintah langgar aturan?", "id": "Tolak dengan hormat, jelaskan risikonya." },
  { "en": "Sikap jika melihat atasan korupsi?", "id": "Wajib laporkan melalui sistem pengaduan." },
  { "en": "Jika teman dekat terlibat narkoba?", "id": "Tidak melindungi, dukung proses hukum." },
  { "en": "Sikap saat keluarga minta bantuan kasus?", "id": "Jelaskan prosedur, jangan pernah intervensi." },
  { "en": "Menemukan uang lebih di mesin ATM?", "id": "Amankan dan segera laporkan ke bank." },
  { "en": "Ditawari makan gratis saat patroli?", "id": "Ucapkan terima kasih dan tolak halus." },
  { "en": "Ideologi di balik menolak pemberian kecil?", "id": "Menjaga integritas sejak hal terkecil." },
  { "en": "Sikap mobil dinas diserempet warga?", "id": "Selesaikan dengan tenang dan prosedural." },
  { "en": "Jika anak sendiri langgar lalu lintas?", "id": "Tetap ditindak sesuai aturan berlaku." },
  { "en": "Prinsip 'equality before the law'?", "id": "Semua orang sama di hadapan hukum." },
  { "en": "Mentalitas saat menjaga tahanan politik?", "id": "Profesional, penuhi hak, tidak menghakimi." },
  { "en": "Sikap pada kelompok rentan di jalan?", "id": "Melindungi, berkoordinasi dengan dinas sosial." },
  { "en": "Ideologi dalam mengatur arus mudik?", "id": "Kelancaran, keamanan, dan keselamatan pemudik." },
  { "en": "Sikap menghadapi pemudik yang kelelahan?", "id": "Sarankan istirahat di 'rest area'." },
  { "en": "Filosofi 'public happiness' dalam pelayanan?", "id": "Kepuasan masyarakat adalah tujuan utama." },
  { "en": "Ukuran keberhasilan seorang Bhabinkamtibmas?", "id": "Diterima dan dipercaya oleh warganya." },
  { "en": "Mentalitas dalam menjaga obyek wisata?", "id": "Ramah pada turis, jaga nama bangsa." },
  { "en": "Sikap terhadap turis yang tidak sopan?", "id": "Tegur dengan simpatik dan edukatif." },
  { "en": "Ideologi dalam menjaga kelestarian cagar budaya?", "id": "Aset bangsa yang harus dilindungi." },
  { "en": "Pentingnya kemampuan 'public speaking'?", "id": "Mampu berkomunikasi efektif dengan publik." },
  { "en": "Cara mengatasi gugup saat berbicara?", "id": "Persiapan materi, latihan, dan pernapasan." },
  { "en": "Sikap saat menjadi narasumber seminar?", "id": "Sampaikan materi akurat, jaga wibawa." },
  { "en": "Mentalitas dalam menjaga rahasia penyidikan?", "id": "Komitmen profesional demi keberhasilan kasus." },
  { "en": "Bolehkah berbagi info kasus ke keluarga?", "id": "Sama sekali tidak diperbolehkan." },
  { "en": "Bahaya membocorkan informasi penyidikan?", "id": "Pelaku kabur, barang bukti hilang." },
  { "en": "Ideologi dalam menjaga kesehatan jangka panjang?", "id": "Investasi untuk masa tua yang sehat." },
  { "en": "Pentingnya deteksi dini (medical check-up)?", "id": "Mencegah penyakit menjadi lebih parah." },
  { "en": "Sikap terhadap makanan dan minuman?", "id": "Makan sehat, hindari alkohol berlebih." },
  { "en": "Mentalitas dalam mempersiapkan masa pensiun?", "id": "Merencanakan keuangan dan kegiatan positif." },
  { "en": "Peran purnawirawan di masyarakat?", "id": "Tetap menjadi teladan dan pengayom." },
  { "en": "Pentingnya ikatan dalam PP Polri?", "id": "Menjaga silaturahmi dan saling bantu." },
  { "en": "Sikap terhadap permintaan data oleh wartawan?", "id": "Berikan data rilis melalui Humas." },
  { "en": "Prinsip 'off the record' dengan wartawan?", "id": "Informasi latar belakang, bukan kutipan." },
  { "en": "Cara membangun hubungan baik dengan pers?", "id": "Profesional, transparan, dan responsif." },
  { "en": "Mentalitas dalam menghadapi era 'post-truth'?", "id": "Berpegang pada fakta dan data." },
  { "en": "Era 'post-truth' adalah?", "id": "Era di mana emosi lebih dipercaya." },
  { "en": "Cara melawan disinformasi di era 'post-truth'?", "id": "Literasi digital dan penegakan hukum." },
  { "en": "Ideologi dalam merawat alutsista/alsus?", "id": "Aset negara yang perpanjang usia pakai." },
  { "en": "Sikap jika menemukan kerusakan alutsista?", "id": "Segera laporkan untuk perbaikan." },
  { "en": "Pentingnya kalibrasi alat bukti (alkohol/narkoba)?", "id": "Menjamin akurasi dan legalitas hasil." },
  { "en": "Mentalitas dalam memberikan kesaksian ahli?", "id": "Objektif sesuai keilmuan, bukan pesanan." },
  { "en": "Tanggung jawab seorang saksi ahli?", "id": "Memberi terang suatu perkara pidana." },
  { "en": "Sikap saat kesaksiannya diragukan hakim?", "id": "Menjelaskan kembali dengan dasar ilmiah." },
  { "en": "Pentingnya belajar dari kasus besar?", "id": "Sebagai yurisprudensi dan pelajaran berharga." },
  { "en": "Sikap terhadap putusan bebas pengadilan?", "id": "Menghormati, lakukan evaluasi proses penyidikan." },
  { "en": "Tujuan utama dari gelar perkara khusus?", "id": "Menangani kasus yang menarik perhatian publik." },
  { "en": "Mentalitas dalam hadapi tekanan publik?", "id": "Tetap bekerja profesional sesuai aturan." },
  { "en": "Pentingnya 'cooling down' setelah tugas berat?", "id": "Agar emosi stabil kembali normal." },
  { "en": "Sikap saat mendapat promosi jabatan?", "id": "Bersyukur, amanah, dan tanggung jawab." },
  { "en": "Sikap saat tidak mendapat promosi?", "id": "Introspeksi diri dan tingkatkan kinerja." },
  { "en": "Bahaya dari ambisi jabatan berlebihan?", "id": "Dapat menghalalkan segala cara." },
  { "en": "Ideologi dalam berorganisasi (Bhayangkari/PP Polri)?", "id": "Gotong royong dan kekeluargaan." },
  { "en": "Manfaat mengikuti kegiatan organisasi?", "id": "Memperluas jaringan dan wawasan." },
  { "en": "Sikap saat menjadi bendahara organisasi?", "id": "Jujur, transparan, dan sangat teliti." },
  { "en": "Pentingnya kemampuan manajemen konflik personal?", "id": "Agar tidak ada masalah berkepanjangan." },
  { "en": "Langkah pertama selesaikan konflik personal?", "id": "Tabayun atau klarifikasi langsung." },
  { "en": "Sikap saat merasa dirugikan rekan?", "id": "Bicarakan baik-baik, jangan simpan dendam." },
  { "en": "Mentalitas 'zero mistake' dalam tugas tertentu?", "id": "Ketelitian maksimal, tidak boleh salah." },
  { "en": "Contoh tugas yang butuh 'zero mistake'?", "id": "Penjinakan bom atau pengawalan VVIP." },
  { "en": "Pentingnya kemampuan 'out of the box thinking'?", "id": "Menemukan solusi kreatif untuk masalah." },
  { "en": "Ideologi sebagai jangkar dalam berpikir?", "id": "Kreatif namun tidak melanggar nilai." },
  { "en": "Sikap terhadap tradisi korps/satuan?", "id": "Menghormati selama positif dan membangun." },
  { "en": "Tradisi yang harus ditinggalkan?", "id": "Tradisi kekerasan atau perpeloncoan." },
  { "en": "Pentingnya doa bersama sebelum operasi?", "id": "Memohon keselamatan dan kelancaran pada Tuhan." },
  { "en": "Keyakinan akhir seorang Bhayangkara sejati?", "id": "Pengabdian terbaik adalah ibadah." },
  { "en": "Tujuan tertinggi pengabdian seorang polisi?", "id": "Terwujudnya masyarakat aman, adil, makmur." },
  { "en": "Arti 'rela berkorban' yang sesungguhnya?", "id": "Siap korbankan waktu, tenaga, nyawa." },
  { "en": "Untuk siapa pengorbanan itu dipersembahkan?", "id": "Demi bangsa dan negara Indonesia." },
  { "en": "Apa itu 'panggilan jiwa' jadi polisi?", "id": "Dorongan batin untuk mengabdi/melayani." },
  { "en": "Ideologi sebagai jawaban atas pertanyaan 'mengapa'?", "id": "Memberi alasan mulia untuk bertindak." },
  { "en": "Ciri Bhayangkara sejati saat bertutur?", "id": "Santun, menenangkan, dan penuh wibawa." },
  { "en": "Ciri Bhayangkara sejati dalam bersikap?", "id": "Tegas, humanis, dan tidak arogan." },
  { "en": "Ciri Bhayangkara sejati saat berpikir?", "id": "Jernih, adil, dan berwawasan kebangsaan." },
  { "en": "Warisan ideologi Polri untuk Indonesia?", "id": "Rasa aman dan kepercayaan pada hukum." },
  { "en": "Harapan Polri untuk masa depan bangsa?", "id": "Bangsa yang tertib dan taat hukum." },
  { "en": "Pelajaran terpenting dari dinas panjang?", "id": "Kerendahan hati dan kesabaran tiada batas." },
  { "en": "Nasihat emas untuk polisi muda?", "id": "Jaga integritas, jangan pernah berhenti belajar." },
  { "en": "Cara memandang kekuasaan dan jabatan?", "id": "Sebagai amanah besar, bukan hak." },
  { "en": "Kunci menjaga kewarasan di tengah tekanan?", "id": "Ingat Tuhan, keluarga, tujuan awal." },
  { "en": "Definisi sukses sejati seorang polisi?", "id": "Lebih dicintai daripada ditakuti masyarakat." },
  { "en": "Cara paling ampuh menaklukkan masyarakat?", "id": "Bukan dengan kekuatan, tapi dengan hati." },
  { "en": "Apa itu 'winning hearts and minds'?", "id": "Merebut hati dan pikiran rakyat." },
  { "en": "Contoh tindakan 'winning hearts and minds'?", "id": "Menolong warga di luar jam dinas." },
  { "en": "Polisi sebagai 'teman' bagi masyarakat?", "id": "Bisa jadi tempat curhat berkeluh-kesah." },
  { "en": "Senjata paling ampuh yang dimiliki polisi?", "id": "Senyum tulus dan rasa empati." },
  { "en": "Bentuk tertinggi dari profesionalisme?", "id": "Bekerja sempurna tanpa perlu diawasi." },
  { "en": "Bentuk tertinggi dari perlindungan?", "id": "Mencegah kejahatan sebelum sempat terjadi." },
  { "en": "Bentuk tertinggi dari pengayoman?", "id": "Membuat masyarakat merasa aman dimanapun." },
  { "en": "Bentuk tertinggi dari pelayanan?", "id": "Memberikan lebih dari yang diharapkan." },
  { "en": "Bentuk tertinggi dari integritas?", "id": "Jujur bahkan saat tak ada saksi." },
  { "en": "Makna 'humanis' yang paling dalam?", "id": "Memanusiakan manusia dalam segala kondisi." },
  { "en": "'Netralitas' adalah jiwa dari apa?", "id": "Keadilan pemilu dan proses demokrasi." },
  { "en": "'Transparansi' adalah kunci utama dari?", "id": "Akuntabilitas dan kepercayaan publik." },
  { "en": "'Akuntabilitas' berarti siap untuk apa?", "id": "Menjelaskan dan mempertanggungjawabkan setiap tindakan." },
  { "en": "Musuh terbesar seorang anggota Polri?", "id": "Ego dan kesombongan diri sendiri." },
  { "en": "Kawan terbaik seorang anggota Polri?", "id": "Disiplin dan integritas diri." },
  { "en": "Dua janji yang harus selalu diingat?", "id": "Tri Brata dan Catur Prasetya." },
  { "en": "Kompas yang harus selalu dipegang erat?", "id": "Ideologi Pancasila dan UUD 1945." },
  { "en": "Pelabuhan terakhir seorang Bhayangkara sejati?", "id": "Kembali jadi masyarakat biasa terhormat." },
  { "en": "Sikap saat melepaskan seragam (pensiun)?", "id": "Tetap menjadi warga negara teladan." },
  { "en": "Doa yang selalu dipanjatkan anggota?", "id": "Selamat pergi tugas, selamat kembali." },
  { "en": "Apa yang membuat semua pengorbanan berharga?", "id": "Senyum dan terima kasih tulus masyarakat." },
  { "en": "Ideologi dalam tarikan napas pengabdian?", "id": "Setia mengabdi sampai akhir hayat." },
  { "en": "Frasa terakhir untuk seorang Bhayangkara?", "id": "Pengabdianku tulus untukmu, Indonesia." },
  { "en": "Pentingnya 'exit interview' sebelum pensiun?", "id": "Mewariskan saran dan masukan berharga." },
  { "en": "Mentalitas 'legacy building'?", "id": "Membangun warisan baik untuk generasi." },
  { "en": "Ideologi dalam menghadapi era VUCA?", "id": "Adaptif, visioner, dan tetap berprinsip." },
  { "en": "Kepanjangan dari VUCA?", "id": "Volatility, Uncertainty, Complexity, Ambiguity." },
  { "en": "Sikap terhadap kritik yang tidak adil?", "id": "Tetap tenang, biarkan kinerja menjawab." },
  { "en": "Pentingnya menjaga 'emotional sobriety'?", "id": "Tetap stabil secara emosi." },
  { "en": "Mentalitas 'kaizen' dalam kepolisian?", "id": "Perbaikan kecil terus-menerus setiap hari." },
  { "en": "Sikap saat melihat keberhasilan institusi lain?", "id": "Turut bangga dan mengambil pelajaran." },
  { "en": "Ideologi dalam menanggapi isu global?", "id": "Berpikir global, bertindak lokal." },
  { "en": "Pentingnya menguasai minimal satu keahlian?", "id": "Menjadi spesialis di bidang tertentu." },
  { "en": "Mentalitas 'never complain, never explain'?", "id": "Bekerja keras, biar hasil bicara." },
  { "en": "Sikap saat diminta menjadi saksi pernikahan?", "id": "Sebuah kehormatan dan kepercayaan." },
  { "en": "Pentingnya menjaga hubungan dengan almamater?", "id": "Sumber inspirasi dan jaringan." },
  { "en": "Ideologi dalam menyikapi kegagalan pribadi?", "id": "Pembelajaran berharga untuk bangkit lagi." },
  { "en": "Bahaya dari 'burnout' atau kelelahan kerja?", "id": "Menurunkan kinerja dan kesehatan mental." },
  { "en": "Cara mencegah 'burnout'?", "id": "Manajemen waktu, hobi, dan istirahat." },
  { "en": "Mentalitas dalam mengikuti rapat yang panjang?", "id": "Tetap fokus dan berkontribusi positif." },
  { "en": "Sikap saat menjadi panitia sebuah acara?", "id": "Bertanggung jawab atas tugas yang diberi." },
  { "en": "Pentingnya dokumentasi setiap kegiatan?", "id": "Sebagai laporan dan bukti pelaksanaan." },
  { "en": "Ideologi dalam mengelola ekspektasi?", "id": "Berharap terbaik, bersiap untuk terburuk." },
  { "en": "Bahaya dari ekspektasi terlalu tinggi?", "id": "Mudah kecewa dan stres." },
  { "en": "Mentalitas dalam mengelola perubahan mendadak?", "id": "Fleksibel dan cepat beradaptasi." },
  { "en": "Sikap terhadap aturan yang tidak populer?", "id": "Tetap laksanakan sebagai perintah dinas." },
  { "en": "Pentingnya memahami psikologi massa?", "id": "Memprediksi dan mengendalikan perilaku kerumunan." },
  { "en": "Ideologi dalam menghadapi era 'cancel culture'?", "id": "Berhati-hati dalam bersikap dan berucap." },
  { "en": "Bahaya 'cancel culture' bagi anggota?", "id": "Karier bisa hancur karena kesalahan." },
  { "en": "Mentalitas dalam menjaga keharmonisan tim?", "id": "Menghargai perbedaan, mencari persamaan." },
  { "en": "Sikap saat ada anggota tim terjerat masalah?", "id": "Beri dukungan moril, bukan intervensi." },
  { "en": "Pentingnya 'post-incident analysis'?", "id": "Mempelajari insiden untuk cegah terulang." },
  { "en": "Ideologi dalam menggunakan anggaran negara?", "id": "Setiap rupiah adalah uang rakyat." },
  { "en": "Sikap terhadap pemborosan fasilitas kantor?", "id": "Saling mengingatkan untuk berhemat." },
  { "en": "Mentalitas dalam menghadapi keterbatasan sarana?", "id": "Tetap kreatif dan bekerja maksimal." },
  { "en": "Sikap saat melihat ketidakadilan di depan mata?", "id": "Wajib bertindak sesuai kewenangan." },
  { "en": "Pentingnya memahami 'golden hour' dalam TKP?", "id": "Waktu krusial untuk kumpulkan bukti." },
  { "en": "Ideologi sebagai sumber energi positif?", "id": "Memberi semangat saat kondisi sulit." },
  { "en": "Bahaya dari sinisme dan apatisme?", "id": "Membunuh semangat pengabdian dari dalam." },
  { "en": "Cara melawan sinisme dan apatisme?", "id": "Fokus pada kebaikan, syukuri pengabdian." },
  { "en": "Mentalitas 'espirit de corps'?", "id": "Menjaga kehormatan dan kebanggaan korps." },
  { "en": "Sikap saat institusi Polri dipuji?", "id": "Rendah hati dan tidak cepat puas." },
  { "en": "Sikap saat institusi Polri dikritik?", "id": "Introspeksi dan lakukan perbaikan nyata." },
  { "en": "Pentingnya kemampuan 'cross-functional collaboration'?", "id": "Bekerja sama lintas fungsi/satuan." },
  { "en": "Ideologi sebagai fondasi karakter Bhayangkara?", "id": "Membentuk pribadi tangguh dan berintegritas." },
  { "en": "Bahaya jika karakter rapuh?", "id": "Mudah tergoda penyimpangan dan pelanggaran." },
  { "en": "Cara membangun karakter yang kuat?", "id": "Melalui latihan, teladan, dan pembiasaan." },
  { "en": "Mentalitas dalam menjaga kesehatan finansial?", "id": "Hidup sederhana, menabung, berinvestasi." },
  { "en": "Sikap terhadap investasi bodong?", "id": "Tidak mudah tergiur keuntungan instan." },
  { "en": "Pentingnya verifikasi sebelum berbagi informasi?", "id": "Saring sebelum 'sharing'." },
  { "en": "Ideologi sebagai benteng terakhir pertahanan diri?", "id": "Melindungi dari godaan dan ancaman." },
  { "en": "Warisan tak ternilai dari para pendahulu?", "id": "Semangat juang dan nilai-nilai Tribrata." },
  { "en": "Tanggung jawab terhadap generasi penerus Polri?", "id": "Mewariskan institusi yang lebih baik." },
  { "en": "Mentalitas dalam menyongsong masa depan Polri?", "id": "Optimis, adaptif, dan selalu Presisi." },
  { "en": "Sikap terakhir saat menutup mata?", "id": "Bangga pernah jadi abdi negara." },
  { "en": "Pesan abadi seorang Bhayangkara?", "id": "Jadilah polisi yang dirindukan masyarakat." },
  { "en": "Ideologi Pancasila untuk Polri?", "id": "Harga mati, tidak bisa ditawar." }


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
