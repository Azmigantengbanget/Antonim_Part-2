<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Kuis Pengetahuan Arduino Dasar</title>
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
        <h1>Pengetahuan Arduino Dasar</h1>
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


  { "en": "Prospek?", "id": "Retrospek." },
  { "en": "Protagonis?", "id": "Antagonis." },
  { "en": "Proteksi?", "id": "Serangan." },
  { "en": "Protes?", "id": "Setuju." },
  { "en": "Protokol?", "id": "Informal." },
  { "en": "Provinsi?", "id": "Pusat." },
  { "en": "Provisional?", "id": "Permanen." },
  { "en": "Provokasi?", "id": "Damai." },
  { "en": "Proyeksi?", "id": "Realisasi." },
  { "en": "Prudensial?", "id": "Spekulatif." },
  { "en": "Puan?", "id": "Tuan." },
  { "en": "Puas?", "id": "Kecewa." },
  { "en": "Puber?", "id": "Dewasa." },
  { "en": "Publik?", "id": "Privat." },
  { "en": "Pucat?", "id": "Merona." },
  { "en": "Pudar?", "id": "Cerah." },
  { "en": "Pudur?", "id": "Muncul." },
  { "en": "Pugak?", "id": "Rendah." },
  { "en": "Pugar?", "id": "Rusak." },
  { "en": "Puisi?", "id": "Prosa." },
  { "en": "Puji?", "id": "Caci." },
  { "en": "Pujuk?", "id": "Ancam." },
  { "en": "Pukal?", "id": "Eceran." },
  { "en": "Pukat?", "id": "Lepas." },
  { "en": "Pukul?", "id": "Elus." },
  { "en": "Pulang?", "id": "Pergi." },
  { "en": "Pulas?", "id": "Terjaga." },
  { "en": "Pulau?", "id": "Benua." },
  { "en": "Pulih?", "id": "Sakit." },
  { "en": "Pulun?", "id": "Urai." },
  { "en": "Punah?", "id": "Lestari." },
  { "en": "Puncak?", "id": "Dasar." },
  { "en": "Punggal?", "id": "Utuh." },
  { "en": "Punggung?", "id": "Depan." },
  { "en": "Pungkas?", "id": "Mulai." },
  { "en": "Pungut?", "id": "Buang." },
  { "en": "Punjul?", "id": "Kurang." },
  { "en": "Puntung?", "id": "Utuh." },
  { "en": "Pupus?", "id": "Tumbuh." },
  { "en": "Pura?", "id": "Nyata." },
  { "en": "Purbawisesa?", "id": "Terbatas." },
  { "en": "Puri?", "id": "Gubuk." },
  { "en": "Purna?", "id": "Awal." },
  { "en": "Purnabakti?", "id": "Aktif." },
  { "en": "Purnama?", "id": "Sabit." },
  { "en": "Puruk?", "id": "Muncul." },
  { "en": "Purun?", "id": "Ogah." },
  { "en": "Pusaka?", "id": "Baru." },
  { "en": "Pusar?", "id": "Tepi." },
  { "en": "Pusing?", "id": "Tenang." },
  { "en": "Pustaka?", "id": "Lisan." },
  { "en": "Pusus?", "id": "Banyak." },
  { "en": "Putar?", "id": "Lurus." },
  { "en": "Putih?", "id": "Hitam." },
  { "en": "Putra?", "id": "Putri." },
  { "en": "Putri?", "id": "Putra." },
  { "en": "Putus?", "id": "Sambung." },
  { "en": "Qadim?", "id": "Baru." },
  { "en": "Qariah?", "id": "Qari." },
  { "en": "Raba?", "id": "Lihat." },
  { "en": "Rabak?", "id": "Rapi." },
  { "en": "Rabat?", "id": "Tambah." },
  { "en": "Rabik?", "id": "Utuh." },
  { "en": "Rabuk?", "id": "Subur." },
  { "en": "Rabun?", "id": "Jelas." },
  { "en": "Racik?", "id": "Bongkar." },
  { "en": "Racun?", "id": "Obat." },
  { "en": "Radang?", "id": "Dingin." },
  { "en": "Radikal?", "id": "Konservatif." },
  { "en": "Raga?", "id": "Jiwa." },
  { "en": "Ragam?", "id": "Seragam." },
  { "en": "Ragi?", "id": "Polos." },
  { "en": "Ragu?", "id": "Yakin." },
  { "en": "Rahasia?", "id": "Umum." },
  { "en": "Rahayu?", "id": "Celaka." },
  { "en": "Rahib?", "id": "Awam." },
  { "en": "Rahmat?", "id": "Laknat." },
  { "en": "Raib?", "id": "Muncul." },
  { "en": "Raih?", "id": "Lepas." },
  { "en": "Raja?", "id": "Rakyat." },
  { "en": "Rajin?", "id": "Malas." },
  { "en": "Rakit?", "id": "Bongkar." },
  { "en": "Raksa?", "id": "Logam." },
  { "en": "Raksasa?", "id": "Kecil." },
  { "en": "Rakus?", "id": "Puas." },
  { "en": "Rakyat?", "id": "Penguasa." },
  { "en": "Ramai?", "id": "Sepi." },
  { "en": "Ramal?", "id": "Fakta." },
  { "en": "Rambah?", "id": "Biarkan." },
  { "en": "Rambat?", "id": "Cepat." },
  { "en": "Ramping?", "id": "Gemuk." },
  { "en": "Rampung?", "id": "Mulai." },
  { "en": "Rampus?", "id": "Sopan." },
  { "en": "Rana?", "id": "Damai." },
  { "en": "Rancu?", "id": "Jelas." },
  { "en": "Randah?", "id": "Tinggi." },
  { "en": "Random?", "id": "Teratur." },
  { "en": "Rangah?", "id": "Rendah." },
  { "en": "Rangka?", "id": "Isi." },
  { "en": "Rangkai?", "id": "Pisah." },
  { "en": "Rangkak?", "id": "Lari." },
  { "en": "Rangkul?", "id": "Lepas." },
  { "en": "Rangkus?", "id": "Sisih." },
  { "en": "Ranjau?", "id": "Aman." },
  { "en": "Ransum?", "id": "Boros." },
  { "en": "Rantau?", "id": "Kampung." },
  { "en": "Ranum?", "id": "Muda." },
  { "en": "Rapal?", "id": "Diam." },
  { "en": "Rapat?", "id": "Renggang." },
  { "en": "Rapel?", "id": "Utang." },
  { "en": "Rapi?", "id": "Berantakan." },
  { "en": "Rapuh?", "id": "Kuat." },
  { "en": "Rasa?", "id": "Hambar." },
  { "en": "Rasai?", "id": "Nikmati." },
  { "en": "Rasi?", "id": "Apes." },
  { "en": "Rasional?", "id": "Irasional." },
  { "en": "Rata?", "id": "Miring." },
  { "en": "Ratap?", "id": "Gembira." },
  { "en": "Ratifisir?", "id": "Tolak." },
  { "en": "Ratu?", "id": "Raja." },
  { "en": "Ratus?", "id": "Satuan." },
  { "en": "Raup?", "id": "Sebar." },
  { "en": "Rawan?", "id": "Aman." },
  { "en": "Rawat?", "id": "Abaikan." },
  { "en": "Raya?", "id": "Kecil." },
  { "en": "Rayah?", "id": "Beri." },
  { "en": "Reaksi?", "id": "Aksi." },
  { "en": "Reaksioner?", "id": "Progresif." },
  { "en": "Realistis?", "id": "Fantastis." },
  { "en": "Rebah?", "id": "Tegak." },
  { "en": "Rebut?", "id": "Serah." },
  { "en": "Reces?", "id": "Aktif." },
  { "en": "Reda?", "id": "Mengamuk." },
  { "en": "Redam?", "id": "Kobarkan." },
  { "en": "Reduksi?", "id": "Penambahan." },
  { "en": "Redum?", "id": "Jelas." },
  { "en": "Redup?", "id": "Terang." },
  { "en": "Reeksi?", "id": "Impor." },
  { "en": "Refleksi?", "id": "Aksi." },
  { "en": "Refluks?", "id": "Aliran." },
  { "en": "Reformasi?", "id": "Status." },
  { "en": "Refrein?", "id": "Solo." },
  { "en": "Regang?", "id": "Rapat." },
  { "en": "Regresi?", "id": "Progresi." },
  { "en": "Reguler?", "id": "Ireguler." },
  { "en": "Rehab?", "id": "Rusak." },
  { "en": "Rehat?", "id": "Kerja." },
  { "en": "Rejeksi?", "id": "Penerimaan." },
  { "en": "Reka?", "id": "Nyata." },
  { "en": "Rekah?", "id": "Kuncup." },
  { "en": "Rekapitulasi?", "id": "Detail." },
  { "en": "Rekonsiliasi?", "id": "Konflik." },
  { "en": "Relaks?", "id": "Tegang." },
  { "en": "Relasi?", "id": "Isolasi." },
  { "en": "Relatif?", "id": "Absolut." },
  { "en": "Relevan?", "id": "Tidak." },
  { "en": "Reliabel?", "id": "Diragukan." },
  { "en": "Relief?", "id": "Datar." },
  { "en": "Religi?", "id": "Sekuler." },
  { "en": "Remang?", "id": "Terang." },
  { "en": "Remedi?", "id": "Pengayaan." },
  { "en": "Remis?", "id": "Buka." },
  { "en": "Remisi?", "id": "Hukuman." },
  { "en": "Remuk?", "id": "Utuh." },
  { "en": "Rendah?", "id": "Tinggi." },
  { "en": "Rengat?", "id": "Rapat." },
  { "en": "Renggang?", "id": "Rapat." },
  { "en": "Renggut?", "id": "Beri." },
  { "en": "Reni?", "id": "Jarang." },
  { "en": "Renjana?", "id": "Benci." },
  { "en": "Renovasi?", "id": "Pembiaran." },
  { "en": "Rentak?", "id": "Lamban." },
  { "en": "Rentan?", "id": "Kebal." },
  { "en": "Reparasi?", "id": "Kerusakan." },
  { "en": "Repatriasi?", "id": "Deportasi." },
  { "en": "Repel?", "id": "Tarik." },
  { "en": "Represif?", "id": "Permisif." },
  { "en": "Reput?", "id": "Segar." },
  { "en": "Resah?", "id": "Tenang." },
  { "en": "Resap?", "id": "Keluar." },
  { "en": "Resepsi?", "id": "Tolak." },
  { "en": "Reses?", "id": "Sidang." },
  { "en": "Resesif?", "id": "Dominan." },
  { "en": "Resi?", "id": "Pengirim." },
  { "en": "Residu?", "id": "Murni." },
  { "en": "Resik?", "id": "Kotor." },
  { "en": "Resistensi?", "id": "Kepatuhan." },
  { "en": "Resmi?", "id": "Tidak." },
  { "en": "Respek?", "id": "Hina." },
  { "en": "Respirasi?", "id": "Apnea." },
  { "en": "Respon?", "id": "Abaikan." },
  { "en": "Restitusi?", "id": "Denda." },
  { "en": "Restriksi?", "id": "Kebebasan." },
  { "en": "Retak?", "id": "Utuh." },
  { "en": "Retardasi?", "id": "Akselerasi." },
  { "en": "Retensi?", "id": "Pelepasan." },
  { "en": "Retorika?", "id": "Praktik." },
  { "en": "Retro?", "id": "Modern." },
  { "en": "Retrogresi?", "id": "Progresi." },
  { "en": "Revolusi?", "id": "Evolusi." },
  { "en": "Rewel?", "id": "Tenang." },
  { "en": "Ria?", "id": "Duka." },
  { "en": "Riang?", "id": "Sedih." },
  { "en": "Riak?", "id": "Tenang." },
  { "en": "Ribut?", "id": "Tenang." },
  { "en": "Ricuh?", "id": "Tertib." },
  { "en": "Ridip?", "id": "Lurus." },
  { "en": "Rigid?", "id": "Fleksibel." },
  { "en": "Rihlah?", "id": "Mukim." },
  { "en": "Rimba?", "id": "Kota." },
  { "en": "Rimbun?", "id": "Gundul." },
  { "en": "Rinai?", "id": "Deras." },
  { "en": "Rindu?", "id": "Biasa." },
  { "en": "Ringan?", "id": "Berat." },
  { "en": "Ringkas?", "id": "Panjang." },
  { "en": "Ringsek?", "id": "Utuh." },
  { "en": "Rintang?", "id": "Bantu." },
  { "en": "Rintis?", "id": "Warisi." },
  { "en": "Risau?", "id": "Tenang." },
  { "en": "Risiko?", "id": "Aman." },
  { "en": "Ritual?", "id": "Spontan." },
  { "en": "Rival?", "id": "Kawan." },
  { "en": "Riwayat?", "id": "Fiksi." },
  { "en": "Robek?", "id": "Jahit." },
  { "en": "Roboh?", "id": "Berdiri." },
  { "en": "Roda?", "id": "Statis." },
  { "en": "Roh?", "id": "Jasad." },
  { "en": "Rohani?", "id": "Jasmani." },
  { "en": "Romantis?", "id": "Realistis." },
  { "en": "Rombak?", "id": "Pertahankan." },
  { "en": "Rompok?", "id": "Mewah." },
  { "en": "Rongga?", "id": "Padat." },
  { "en": "Rontok?", "id": "Tumbuh." },
  { "en": "Rosot?", "id": "Naik." },
  { "en": "Rotasi?", "id": "Tetap." },
  { "en": "Royal?", "id": "Hemat." },
  { "en": "Rua?", "id": "Sempit." },
  { "en": "Ruah?", "id": "Sedikit." },
  { "en": "Ruam?", "id": "Mulus." },
  { "en": "Ruang?", "id": "Padat." },
  { "en": "Rubeh?", "id": "Tegak." },
  { "en": "Rubuh?", "id": "Bangun." },
  { "en": "Rucah?", "id": "Sopan." },
  { "en": "Rudapaksa?", "id": "Persetujuan." },
  { "en": "Rugi?", "id": "Untung." },
  { "en": "Rujuk?", "id": "Cerai." },
  { "en": "Rukun?", "id": "Bertengkar." },
  { "en": "Rumah?", "id": "Luar." },
  { "en": "Rumit?", "id": "Sederhana." },
  { "en": "Rumpang?", "id": "Lengkap." },
  { "en": "Runcing?", "id": "Tumpul." },
  { "en": "Runduk?", "id": "Tegak." },
  { "en": "Rungut?", "id": "Puji." },
  { "en": "Runtuh?", "id": "Kokoh." },
  { "en": "Rupa?", "id": "Abstrak." },
  { "en": "Rupawan?", "id": "Buruk." },
  { "en": "Rupiah?", "id": "Dolar." },
  { "en": "Rural?", "id": "Urban." },
  { "en": "Rusak?", "id": "Baik." },
  { "en": "Rusuh?", "id": "Damai." },
  { "en": "Rutin?", "id": "Jarang." },
  { "en": "Sa?", "id": "Tidak." },
  { "en": "Saba?", "id": "Pergi." },
  { "en": "Sabar?", "id": "Marah." },
  { "en": "Sabas?", "id": "Celaka." },
  { "en": "Sabda?", "id": "Diam." },
  { "en": "Sabil?", "id": "Tersesat." },
  { "en": "Sabit?", "id": "Purnama." },
  { "en": "Sadai?", "id": "Tegak." },
  { "en": "Sadap?", "id": "Tutup." },
  { "en": "Sadar?", "id": "Pingsan." },
  { "en": "Sadik?", "id": "Curang." },
  { "en": "Sadis?", "id": "Lembut." },
  { "en": "Sadu?", "id": "Hina." },
  { "en": "Saf?", "id": "Berantakan." },
  { "en": "Safa?", "id": "Keruh." },
  { "en": "Safi?", "id": "Bodoh." },
  { "en": "Sagang?", "id": "Lepas." },
  { "en": "Sagar?", "id": "Tawar." },
  { "en": "Sah?", "id": "Batal." },
  { "en": "Sahabat?", "id": "Musuh." },
  { "en": "Sahaja?", "id": "Rumit." },
  { "en": "Sahara?", "id": "Kutub." },
  { "en": "Sahaya?", "id": "Tuan." },
  { "en": "Sahib?", "id": "Hamba." },
  { "en": "Sahih?", "id": "Palsu." },
  { "en": "Sais?", "id": "Penumpang." },
  { "en": "Saja?", "id": "Bersama." },
  { "en": "Sajak?", "id": "Prosa." },
  { "en": "Saji?", "id": "Ambil." },
  { "en": "Saka?", "id": "Baru." },
  { "en": "Sakal?", "id": "Lancip." },
  { "en": "Sakang?", "id": "Sempit." },
  { "en": "Sakat?", "id": "Surut." },
  { "en": "Sakit?", "id": "Sehat." },
  { "en": "Sakral?", "id": "Profan." },
  { "en": "Saksi?", "id": "Pelaku." },
  { "en": "Sakti?", "id": "Lemah." },
  { "en": "Saku?", "id": "Luar." },
  { "en": "Salah?", "id": "Benar." },
  { "en": "Salam?", "id": "Perpisahan." },
  { "en": "Saldo?", "id": "Utang." },
  { "en": "Saleh?", "id": "Jahat." },
  { "en": "Salib?", "id": "Bulan." },
  { "en": "Salin?", "id": "Asli." },
  { "en": "Salju?", "id": "Api." },
  { "en": "Salut?", "id": "Cemooh." },
  { "en": "Sama?", "id": "Beda." },
  { "en": "Samar?", "id": "Jelas." },
  { "en": "Sambang?", "id": "Abaikan." },
  { "en": "Sambar?", "id": "Lepas." },
  { "en": "Sambung?", "id": "Putus." },
  { "en": "Sambut?", "id": "Usir." },
  { "en": "Sampai?", "id": "Dari." },
  { "en": "Samping?", "id": "Tengah." },
  { "en": "Samun?", "id": "Aman." },
  { "en": "Samudra?", "id": "Danau." },
  { "en": "Sanat?", "id": "Baru." },
  { "en": "Sandal?", "id": "Sepatu." },
  { "en": "Sandang?", "id": "Pangan." },
  { "en": "Sandar?", "id": "Tegak." },
  { "en": "Sanding?", "id": "Jauh." },
  { "en": "Sandiwara?", "id": "Realitas." },
  { "en": "Sangar?", "id": "Lembut." },
  { "en": "Sanggah?", "id": "Setuju." },
  { "en": "Sanggama?", "id": "Puasa." },
  { "en": "Sangkal?", "id": "Aku." },
  { "en": "Sangkut?", "id": "Lepas." },
  { "en": "Sanitasi?", "id": "Kotor." },
  { "en": "Sanjung?", "id": "Cerca." },
  { "en": "Santa?", "id": "Iblis." },
  { "en": "Santai?", "id": "Tegang." },
  { "en": "Santap?", "id": "Lapar." },
  { "en": "Santir?", "id": "Serius." },
  { "en": "Santun?", "id": "Kasar." },
  { "en": "Sapih?", "id": "Susui." },
  { "en": "Sapu?", "id": "Kotori." },
  { "en": "Sara?", "id": "Netral." },
  { "en": "Saran?", "id": "Perintah." },
  { "en": "Sarana?", "id": "Hambatan." },
  { "en": "Sarat?", "id": "Kosong." },
  { "en": "Sari?", "id": "Ampas." },
  { "en": "Saring?", "id": "Campur." },
  { "en": "Sarju?", "id": "Sedih." },
  { "en": "Sarkasme?", "id": "Pujian." },
  { "en": "Sarsar?", "id": "Teratur." },
  { "en": "Sarwa?", "id": "Sebagian." },
  { "en": "Saru?", "id": "Sopan." },
  { "en": "Sarung?", "id": "Buka." },
  { "en": "Sasana?", "id": "Luar." },
  { "en": "Sasar?", "id": "Meleset." },
  { "en": "Sastrawan?", "id": "Pembaca." },
  { "en": "Satah?", "id": "Dalam." },
  { "en": "Satai?", "id": "Rebus." },
  { "en": "Satelit?", "id": "Planet." },
  { "en": "Satir?", "id": "Pujian." },
  { "en": "Sato?", "id": "Tumbuhan." },
  { "en": "Satu?", "id": "Banyak." },
  { "en": "Satwa?", "id": "Flora." },
  { "en": "Saudagar?", "id": "Pembeli." },
  { "en": "Sauh?", "id": "Layar." },
  { "en": "Saujana?", "id": "Dekat." },
  { "en": "Sawab?", "id": "Dosa." },
  { "en": "Sawang?", "id": "Padat." },
  { "en": "Sayang?", "id": "Benci." },
  { "en": "Sayembara?", "id": "Penunjukan." },
  { "en": "Sayu?", "id": "Ceria." },
  { "en": "Sebab?", "id": "Akibat." },
  { "en": "Sebal?", "id": "Suka." },
  { "en": "Sebar?", "id": "Kumpul." },
  { "en": "Sebat?", "id": "Elus." },
  { "en": "Sebel?", "id": "Senang." },
  { "en": "Sebentar?", "id": "Lama." },
  { "en": "Seberang?", "id": "Sisi." },
  { "en": "Sederhana?", "id": "Rumit." },
  { "en": "Sedia?", "id": "Habis." },
  { "en": "Sedih?", "id": "Senang." },
  { "en": "Sedikit?", "id": "Banyak." },
  { "en": "Sedo?", "id": "Nyala." },
  { "en": "Segak?", "id": "Lembut." },
  { "en": "Segan?", "id": "Berani." },
  { "en": "Segar?", "id": "Layu." },
  { "en": "Sehat?", "id": "Sakit." },
  { "en": "Sejahtera?", "id": "Sengsara." },
  { "en": "Sejak?", "id": "Hingga." },
  { "en": "Sejati?", "id": "Palsu." },
  { "en": "Sejuk?", "id": "Panas." },
  { "en": "Sekarang?", "id": "Dulu." },
  { "en": "Sekat?", "id": "Buka." },
  { "en": "Sekop?", "id": "Tanam." },
  { "en": "Sekuler?", "id": "Religius." },
  { "en": "Selalu?", "id": "Jarang." },
  { "en": "Selam?", "id": "Timbul." },
  { "en": "Selamat?", "id": "Celaka." },
  { "en": "Selang?", "id": "Rapat." },
  { "en": "Selatan?", "id": "Utara." },
  { "en": "Selesai?", "id": "Mulai." },
  { "en": "Selidik?", "id": "Abaikan." },
  { "en": "Selimpat?", "id": "Lurus." },
  { "en": "Selingkuh?", "id": "Setia." },
  { "en": "Selip?", "id": "Muncul." },
  { "en": "Selisih?", "id": "Sama." },
  { "en": "Sukar?", "id": "Mudah." },
  { "en": "Suka?", "id": "Duka." },
  { "en": "Sumbang?", "id": "Benar." },
  { "en": "Surga?", "id": "Neraka." },
  { "en": "Susah?", "id": "Senang." },
  { "en": "Syahdu?", "id": "Kasar." },
  { "en": "Swasta?", "id": "Negeri." },
  { "en": "Swatantra?", "id": "Tergantung." },
  { "en": "Tabah?", "id": "Rapuh." },
  { "en": "Tabiat?", "id": "Labil." },
  { "en": "Tabik?", "id": "Hina." },
  { "en": "Tabir?", "id": "Terbuka." },
  { "en": "Tabu?", "id": "Boleh." },
  { "en": "Tadah?", "id": "Buang." },
  { "en": "Tadi?", "id": "Nanti." },
  { "en": "Tafakur?", "id": "Lalai." },
  { "en": "Tagak?", "id": "Roboh." },
  { "en": "Tagih?", "id": "Bayar." },
  { "en": "Tahan?", "id": "Lepas." },
  { "en": "Tahbis?", "id": "Profan." },
  { "en": "Tahniah?", "id": "Dukacita." },
  { "en": "Tahir?", "id": "Najis." },
  { "en": "Tai?", "id": "Wangi." },
  { "en": "Taiga?", "id": "Gurun." },
  { "en": "Tajam?", "id": "Tumpul." },
  { "en": "Tajar?", "id": "Baru." },
  { "en": "Takabur?", "id": "Tawadu." },
  { "en": "Takar?", "id": "Acak." },
  { "en": "Takdir?", "id": "Usaha." },
  { "en": "Takhayul?", "id": "Rasional." },
  { "en": "Takhta?", "id": "Lantai." },
  { "en": "Takjub?", "id": "Biasa." },
  { "en": "Taklid?", "id": "Kritis." },
  { "en": "Takluk?", "id": "Merdeka." },
  { "en": "Taksir?", "id": "Pasti." },
  { "en": "Takut?", "id": "Berani." },
  { "en": "Takzim?", "id": "Hina." },
  { "en": "Talak?", "id": "Rujuk." },
  { "en": "Talam?", "id": "Cekung." },
  { "en": "Talang?", "id": "Salur." },
  { "en": "Tamasya?", "id": "Kerja." },
  { "en": "Tambah?", "id": "Kurang." },
  { "en": "Tambak?", "id": "Laut." },
  { "en": "Tambat?", "id": "Lepas." },
  { "en": "Tambun?", "id": "Kurus." },
  { "en": "Tampak?", "id": "Sembunyi." },
  { "en": "Tampan?", "id": "Jelek." },
  { "en": "Tampar?", "id": "Elus." },
  { "en": "Tampil?", "id": "Mundur." },
  { "en": "Tampung?", "id": "Buang." },
  { "en": "Tamsil?", "id": "Nyata." },
  { "en": "Tancap?", "id": "Cabut." },
  { "en": "Tanda?", "id": "Samar." },
  { "en": "Tandang?", "id": "Kandang." },
  { "en": "Tanding?", "id": "Kalah." },
  { "en": "Tanduk?", "id": "Polos." },
  { "en": "Tandus?", "id": "Subur." },
  { "en": "Tangan?", "id": "Kaki." },
  { "en": "Tangas?", "id": "Dingin." },
  { "en": "Tangga?", "id": "Lantai." },
  { "en": "Tanggal?", "id": "Pasang." },
  { "en": "Tangguh?", "id": "Rapuh." },
  { "en": "Tangguk?", "id": "Buang." },
  { "en": "Tanggung?", "id": "Lepas." },
  { "en": "Tangkai?", "id": "Dasar." },
  { "en": "Tangkal?", "id": "Sebab." },
  { "en": "Tangkap?", "id": "Lepas." },
  { "en": "Tangkas?", "id": "Lamban." },
  { "en": "Tangis?", "id": "Tawa." },
  { "en": "Tanjak?", "id": "Turun." },
  { "en": "Tanjul?", "id": "Lepas." },
  { "en": "Tanjung?", "id": "Teluk." },
  { "en": "Tanpa?", "id": "Dengan." },
  { "en": "Tantang?", "id": "Terima." },
  { "en": "Tanya?", "id": "Jawab." },
  { "en": "Tapa?", "id": "Duniawi." },
  { "en": "Tapai?", "id": "Segar." },
  { "en": "Tapal?", "id": "Lepas." },
  { "en": "Tapih?", "id": "Celana." },
  { "en": "Tara?", "id": "Dasar." },
  { "en": "Taraf?", "id": "Rendah." },
  { "en": "Tarawih?", "id": "Wajib." },
  { "en": "Target?", "id": "Meleset." },
  { "en": "Tari?", "id": "Diam." },
  { "en": "Tarik?", "id": "Dorong." },
  { "en": "Tarif?", "id": "Gratis." },
  { "en": "Taruh?", "id": "Ambil." },
  { "en": "Taruk?", "id": "Gugur." },
  { "en": "Tarung?", "id": "Damai." },
  { "en": "Tasalsul?", "id": "Putus." },
  { "en": "Tasawuf?", "id": "Material." },
  { "en": "Tasbih?", "id": "Maki." },
  { "en": "Tasdik?", "id": "Ingkar." },
  { "en": "Tata?", "id": "Kacau." },
  { "en": "Tatap?", "id": "Paling." },
  { "en": "Tatih?", "id": "Lancar." },
  { "en": "Tatkala?", "id": "Sebelum." },
  { "en": "Tauliah?", "id": "Cabut." },
  { "en": "Taun?", "id": "Sehat." },
  { "en": "Tawa?", "id": "Tangis." },
  { "en": "Tawadu?", "id": "Sombong." },
  { "en": "Tawakal?", "id": "Khawatir." },
  { "en": "Tawan?", "id": "Bebas." },
  { "en": "Tawar?", "id": "Manis." },
  { "en": "Tawon?", "id": "Madu." },
  { "en": "Tayang?", "id": "Simpan." },
  { "en": "Tayib?", "id": "Buruk." },
  { "en": "Teater?", "id": "Kenyataan." },
  { "en": "Teba?", "id": "Rata." },
  { "en": "Tebak?", "id": "Pasti." },
  { "en": "Tebal?", "id": "Tipis." },
  { "en": "Tebang?", "id": "Tanam." },
  { "en": "Tebar?", "id": "Kumpul." },
  { "en": "Tebas?", "id": "Rawat." },
  { "en": "Tebat?", "id": "Alirkan." },
  { "en": "Tebing?", "id": "Lembah." },
  { "en": "Tebus?", "id": "Gadai." },
  { "en": "Tedas?", "id": "Samar." },
  { "en": "Tedeng?", "id": "Terbuka." },
  { "en": "Teduh?", "id": "Panas." },
  { "en": "Tegah?", "id": "Izinkan." },
  { "en": "Tegak?", "id": "Miring." },
  { "en": "Tegar?", "id": "Rapuh." },
  { "en": "Tegas?", "id": "Ragu." },
  { "en": "Teguh?", "id": "Goyah." },
  { "en": "Teguk?", "id": "Sembur." },
  { "en": "Tegun?", "id": "Lanjut." },
  { "en": "Tegur?", "id": "Puji." },
  { "en": "Tekad?", "id": "Ragu." },
  { "en": "Tekan?", "id": "Lepas." },
  { "en": "Tekap?", "id": "Buka." },
  { "en": "Tekebur?", "id": "Rendah." },
  { "en": "Tekor?", "id": "Untung." },
  { "en": "Tekuk?", "id": "Luruskan." },
  { "en": "Tekun?", "id": "Malas." },
  { "en": "Telaah?", "id": "Abaikan." },
  { "en": "Telak?", "id": "Meleset." },
  { "en": "Telan?", "id": "Muntah." },
  { "en": "Telanjang?", "id": "Berpakaian." },
  { "en": "Telanjur?", "id": "Awal." },
  { "en": "Telantar?", "id": "Terurus." },
  { "en": "Telat?", "id": "Awal." },
  { "en": "Telempap?", "id": "Utuh." },
  { "en": "Telinga?", "id": "Mata." },
  { "en": "Teliti?", "id": "Ceroboh." },
  { "en": "Teluk?", "id": "Tanjung." },
  { "en": "Telungkup?", "id": "Telentang." },
  { "en": "Telur?", "id": "Ayam." },
  { "en": "Temaram?", "id": "Terang." },
  { "en": "Temas?", "id": "Dingin." },
  { "en": "Tembak?", "id": "Hindar." },
  { "en": "Tembelang?", "id": "Jujur." },
  { "en": "Tembem?", "id": "Tirus." },
  { "en": "Tembok?", "id": "Pintu." },
  { "en": "Tembus?", "id": "Buntu." },
  { "en": "Tempa?", "id": "Asli." },
  { "en": "Tempat?", "id": "Pindah." },
  { "en": "Tempel?", "id": "Lepas." },
  { "en": "Tempuh?", "id": "Hindar." },
  { "en": "Tempur?", "id": "Damai." },
  { "en": "Temu?", "id": "Pisah." },
  { "en": "Tenaga?", "id": "Lemah." },
  { "en": "Tenang?", "id": "Gelisah." },
  { "en": "Tenda?", "id": "Langit." },
  { "en": "Tendang?", "id": "Pegang." },
  { "en": "Tengadah?", "id": "Tunduk." },
  { "en": "Tengah?", "id": "Pinggir." },
  { "en": "Tenggak?", "id": "Sembur." },
  { "en": "Tenggan?", "id": "Cepat." },
  { "en": "Tenggara?", "id": "Barat." },
  { "en": "Tenggelam?", "id": "Terapung." },
  { "en": "Tengik?", "id": "Harum." },
  { "en": "Tengkar?", "id": "Akur." },
  { "en": "Tengok?", "id": "Abaikan." },
  { "en": "Tengkuk?", "id": "Wajah." },
  { "en": "Tengkurap?", "id": "Terlentang." },
  { "en": "Tentang?", "id": "Dukung." },
  { "en": "Tentara?", "id": "Sipil." },
  { "en": "Tenteram?", "id": "Kacau." },
  { "en": "Tentu?", "id": "Ragu." },
  { "en": "Tenun?", "id": "Urai." },
  { "en": "Tepat?", "id": "Meleset." },
  { "en": "Tepi?", "id": "Tengah." },
  { "en": "Tepok?", "id": "Pukul." },
  { "en": "Tepuk?", "id": "Pukul." },
  { "en": "Terang?", "id": "Gelap." },
  { "en": "Terap?", "id": "Dasar." },
  { "en": "Teratur?", "id": "Kacau." },
  { "en": "Terbang?", "id": "Mendarat." },
  { "en": "Terbit?", "id": "Tenggelam." },
  { "en": "Terjal?", "id": "Landai." },
  { "en": "Terjang?", "id": "Hindar." },
  { "en": "Terka?", "id": "Pasti." },
  { "en": "Terkenal?", "id": "Tidak." },
  { "en": "Terakhir?", "id": "Pertama." },
  { "en": "Terlentang?", "id": "Tengkurap." },
  { "en": "Terima?", "id": "Tolak." },
  { "en": "Termal?", "id": "Dingin." },
  { "en": "Termasuk?", "id": "Kecuali." },
  { "en": "Ternak?", "id": "Liar." },
  { "en": "Terpa?", "id": "Hindar." },
  { "en": "Tertawa?", "id": "Menangis." },
  { "en": "Tertib?", "id": "Kacau." },
  { "en": "Teruk?", "id": "Ringan." },
  { "en": "Terus?", "id": "Berhenti." },
  { "en": "Terwelu?", "id": "Kura-kura." },
  { "en": "Tetap?", "id": "Berubah." },
  { "en": "Tetas?", "id": "Tutup." },
  { "en": "Tewas?", "id": "Hidup." },
  { "en": "Tiada?", "id": "Ada." },
  { "en": "Tiarap?", "id": "Telentang." },
  { "en": "Tiba?", "id": "Pergi." },
  { "en": "Tidak?", "id": "Ya." },
  { "en": "Tidur?", "id": "Bangun." },
  { "en": "Tifus?", "id": "Sehat." },
  { "en": "Tikai?", "id": "Rukun." },
  { "en": "Tikam?", "id": "Obati." },
  { "en": "Tikar?", "id": "Kasur." },
  { "en": "Tikus?", "id": "Kucing." },
  { "en": "Timbul?", "id": "Tenggelam." },
  { "en": "Timbun?", "id": "Gali." },
  { "en": "Timpa?", "id": "Angkat." },
  { "en": "Timpal?", "id": "Asli." },
  { "en": "Timpang?", "id": "Seimbang." },
  { "en": "Timur?", "id": "Barat." },
  { "en": "Tindak?", "id": "Diam." },
  { "en": "Tindas?", "id": "Bantu." },
  { "en": "Tinggal?", "id": "Pergi." },
  { "en": "Tinggi?", "id": "Rendah." },
  { "en": "Tingkah?", "id": "Tenang." },
  { "en": "Tinja?", "id": "Bersih." },
  { "en": "Tinjau?", "id": "Abaikan." },
  { "en": "Tinta?", "id": "Hapus." },
  { "en": "Tipis?", "id": "Tebal." },
  { "en": "Tipu?", "id": "Jujur." },
  { "en": "Tirai?", "id": "Panggung." },
  { "en": "Tiris?", "id": "Rapat." },
  { "en": "Tiru?", "id": "Asli." },
  { "en": "Tirus?", "id": "Tembem." },
  { "en": "Titan?", "id": "Kerdil." },
  { "en": "Titik?", "id": "Garis." },
  { "en": "Titip?", "id": "Ambil." },
  { "en": "Tiup?", "id": "Isap." },
  { "en": "Toga?", "id": "Kasual." },
  { "en": "Togok?", "id": "Cabang." },
  { "en": "Tohok?", "id": "Tangkis." },
  { "en": "Tokcer?", "id": "Gagal." },
  { "en": "Toko?", "id": "Rumah." },
  { "en": "Tokoh?", "id": "Figuran." },
  { "en": "Tolak?", "id": "Terima." },
  { "en": "Toleh?", "id": "Diam." },
  { "en": "Toleran?", "id": "Intoleran." },
  { "en": "Tolok?", "id": "Berbeda." },
  { "en": "Tolong?", "id": "Hambat." },
  { "en": "Tomboi?", "id": "Feminin." },
  { "en": "Ton?", "id": "Kilogram." },
  { "en": "Tonggak?", "id": "Dasar." },
  { "en": "Tonggos?", "id": "Rata." },
  { "en": "Tonjol?", "id": "Lekuk." },
  { "en": "Topan?", "id": "Tenang." },
  { "en": "Topang?", "id": "Jatuhkan." },
  { "en": "Total?", "id": "Parsial." },
  { "en": "Totaliter?", "id": "Demokratis." },
  { "en": "Tradisi?", "id": "Modern." },
  { "en": "Tragedi?", "id": "Komedi." },
  { "en": "Transgresi?", "id": "Kepatuhan." },
  { "en": "Transisi?", "id": "Tetap." },
  { "en": "Translusen?", "id": "Opak." },
  { "en": "Transparan?", "id": "Buram." },
  { "en": "Trap?", "id": "Halus." },
  { "en": "Trauma?", "id": "Pulih." },
  { "en": "Trek?", "id": "Luar." },
  { "en": "Tremor?", "id": "Stabil." },
  { "en": "Trendi?", "id": "Kuno." },
  { "en": "Tribunal?", "id": "Damai." },
  { "en": "Trompet?", "id": "Sunyi." },
  { "en": "Tropis?", "id": "Kutub." },
  { "en": "Truf?", "id": "Biasa." },
  { "en": "Tua?", "id": "Muda." },
  { "en": "Tuah?", "id": "Sial." },
  { "en": "Tuai?", "id": "Tanam." },
  { "en": "Tuak?", "id": "Air." },
  { "en": "Tualang?", "id": "Menetap." },
  { "en": "Tuan?", "id": "Hamba." },
  { "en": "Tuang?", "id": "Kosongkan." },
  { "en": "Tuba?", "id": "Madu." },
  { "en": "Tubir?", "id": "Puncak." },
  { "en": "Tubruk?", "id": "Hindar." },
  { "en": "Tubuh?", "id": "Jiwa." },
  { "en": "Tuding?", "id": "Puji." },
  { "en": "Tudung?", "id": "Buka." },
  { "en": "Tufah?", "id": "Dasar." },
  { "en": "Tugal?", "id": "Tanam." },
  { "en": "Tegang?", "id": "Rileks." },
  { "en": "Tugi?", "id": "Beruntung." },
  { "en": "Tuhan?", "id": "Hamba." },
  { "en": "Tujah?", "id": "Cabut." },
  { "en": "Tuju?", "id": "Menyimpang." },
  { "en": "Tukak?", "id": "Mulus." },
  { "en": "Tukar?", "id": "Simpan." },
  { "en": "Tukik?", "id": "Naik." },
  { "en": "Tukmis?", "id": "Cukur." },
  { "en": "Tula?", "id": "Puji." },
  { "en": "Tulah?", "id": "Berkah." },
  { "en": "Tular?", "id": "Sembuh." },
  { "en": "Tulen?", "id": "Campuran." },
  { "en": "Tuli?", "id": "Dengar." },
  { "en": "Tulis?", "id": "Lisan." },
  { "en": "Tulus?", "id": "Licik." },
  { "en": "Tumbang?", "id": "Berdiri." },
  { "en": "Tumbuh?", "id": "Mati." },
  { "en": "Tumbuk?", "id": "Halus." },
  { "en": "Tumit?", "id": "Jari." },
  { "en": "Tumor?", "id": "Sehat." },
  { "en": "Tumpah?", "id": "Wadah." },
  { "en": "Tumpak?", "id": "Dasar." },
  { "en": "Tumpas?", "id": "Pelihara." },
  { "en": "Tumpul?", "id": "Tajam." },
  { "en": "Tumpu?", "id": "Gantung." },
  { "en": "Tunai?", "id": "Kredit." },
  { "en": "Tunam?", "id": "Subur." },
  { "en": "Tunanetra?", "id": "Melihat." },
  { "en": "Tunarungu?", "id": "Mendengar." },
  { "en": "Tunas?", "id": "Pohon." },
  { "en": "Tunda?", "id": "Lanjutkan." },
  { "en": "Tunduk?", "id": "Lawan." },
  { "en": "Tungau?", "id": "Bersih." },
  { "en": "Tunggal?", "id": "Jamak." },
  { "en": "Tunggang?", "id": "Turun." },
  { "en": "Tunggu?", "id": "Pergi." },
  { "en": "Tungku?", "id": "Dingin." },
  { "en": "Tuntas?", "id": "Belum." },
  { "en": "Tuntun?", "id": "Lepas." },
  { "en": "Tupai?", "id": "Ular." },
  { "en": "Turba?", "id": "Kota." },
  { "en": "Turis?", "id": "Lokal." },
  { "en": "Turun?", "id": "Naik." },
  { "en": "Turut?", "id": "Bantah." },
  { "en": "Tusuk?", "id": "Cabut." },
  { "en": "Tutuh?", "id": "Sambung." },
  { "en": "Tutup?", "id": "Buka." },
  { "en": "Tutur?", "id": "Diam." },
  { "en": "Uak?", "id": "Muda." },
  { "en": "Uap?", "id": "Cair." },
  { "en": "Ubah?", "id": "Tetap." },
  { "en": "Uban?", "id": "Hitam." },
  { "en": "Uber?", "id": "Lepas." },
  { "en": "Ubin?", "id": "Atap." },
  { "en": "Ubudiah?", "id": "Duniawi." },
  { "en": "Ucap?", "id": "Diam." },
  { "en": "Ucul?", "id": "Ikat." },
  { "en": "Udak?", "id": "Diamkan." },
  { "en": "Udam?", "id": "Cerah." },
  { "en": "Udang?", "id": "Ikan." },
  { "en": "Udara?", "id": "Tanah." },
  { "en": "Udik?", "id": "Hilir." },
  { "en": "Ufti?", "id": "Hadiah." },
  { "en": "Ugahari?", "id": "Mewah." },
  { "en": "Ugem?", "id": "Lepas." },
  { "en": "Ujar?", "id": "Diam." },
  { "en": "Uji?", "id": "Terima." },
  { "en": "Ujung?", "id": "Pangkal." },
  { "en": "Ukir?", "id": "Polos." },
  { "en": "Ukur?", "id": "Acak." },
  { "en": "Ulam?", "id": "Bumbu." },
  { "en": "Ulan?", "id": "Satuan." },
  { "en": "Ulang?", "id": "Sekali." },
  { "en": "Ular?", "id": "Elang." },
  { "en": "Ulas?", "id": "Singkat." },
  { "en": "Ulat?", "id": "Kupu." },
  { "en": "Ulet?", "id": "Rapuh." },
  { "en": "Ulik?", "id": "Abaikan." },
  { "en": "Ulir?", "id": "Lurus." },
  { "en": "Ultimatum?", "id": "Tawaran." },
  { "en": "Ultramodern?", "id": "Kuno." },
  { "en": "Ulung?", "id": "Amatir." },
  { "en": "Umara?", "id": "Rakyat." },
  { "en": "Umat?", "id": "Imam." },
  { "en": "Umbai?", "id": "Pokok." },
  { "en": "Umban?", "id": "Pegang." },
  { "en": "Umbar?", "id": "Tahan." },
  { "en": "Umbi?", "id": "Daun." },
  { "en": "Umbra?", "id": "Penumbra." },
  { "en": "Umpak?", "id": "Tiang." },
  { "en": "Umpama?", "id": "Fakta." },
  { "en": "Umpan?", "id": "Mangsa." },
  { "en": "Umpat?", "id": "Puji." },
  { "en": "Umum?", "id": "Khusus." },
  { "en": "Uncang?", "id": "Isi." },
  { "en": "Undak?", "id": "Datar." },
  { "en": "Undang?", "id": "Usir." },
  { "en": "Undur?", "id": "Maju." },
  { "en": "Unggah?", "id": "Unduh." },
  { "en": "Unggas?", "id": "Mamalia." },
  { "en": "Unggul?", "id": "Kalah." },
  { "en": "Unggun?", "id": "Padam." },
  { "en": "Ungka?", "id": "Manusia." },
  { "en": "Ungkap?", "id": "Sembunyi." },
  { "en": "Ungsi?", "id": "Kembali." },
  { "en": "Unik?", "id": "Biasa." },
  { "en": "Unilateral?", "id": "Multilateral." },
  { "en": "Universal?", "id": "Lokal." },
  { "en": "Unjuk?", "id": "Sembunyi." },
  { "en": "Untai?", "id": "Lepas." },
  { "en": "Untung?", "id": "Rugi." },
  { "en": "Upah?", "id": "Denda." },
  { "en": "Upaya?", "id": "Pasrah." },
  { "en": "Upeti?", "id": "Hadiah." },
  { "en": "Urai?", "id": "Ikat." },
  { "en": "Urban?", "id": "Rural." },
  { "en": "Uri?", "id": "Pusat." },
  { "en": "Urung?", "id": "Jadi." },
  { "en": "Urus?", "id": "Abaikan." },
  { "en": "Usah?", "id": "Larang." },
  { "en": "Usai?", "id": "Mulai." },
  { "en": "Usang?", "id": "Baru." },
  { "en": "Usap?", "id": "Gores." },
  { "en": "Usia?", "id": "Muda." },
  { "en": "Usik?", "id": "Diamkan." },
  { "en": "Usir?", "id": "Terima." },
  { "en": "Uskup?", "id": "Awam." },
  { "en": "Ustaz?", "id": "Murid." },
  { "en": "Usul?", "id": "Tolak." },
  { "en": "Usung?", "id": "Letak." },
  { "en": "Usur?", "id": "Baru." },
  { "en": "Utama?", "id": "Sekunder." },
  { "en": "Utang?", "id": "Piutang." },
  { "en": "Utara?", "id": "Selatan." },
  { "en": "Utas?", "id": "Lepas." },
  { "en": "Utuh?", "id": "Pecah." },
  { "en": "Vademekum?", "id": "Kosong." },
  { "en": "Vagina?", "id": "Penis." },
  { "en": "Vakum?", "id": "Penuh." },
  { "en": "Valas?", "id": "Rupiah." },
  { "en": "Valid?", "id": "Tidak." },
  { "en": "Vali?", "id": "Lemah." },
  { "en": "Valor?", "id": "Pengecut." },
  { "en": "Vandal?", "id": "Pelindung." },
  { "en": "Vang?", "id": "Kosong." },
  { "en": "Variabel?", "id": "Konstan." },
  { "en": "Variasi?", "id": "Seragam." },
  { "en": "Vas?", "id": "Bunga." },
  { "en": "Vegetarian?", "id": "Karnivora." },
  { "en": "Veil?", "id": "Terbuka." },
  { "en": "Velar?", "id": "Dental." },
  { "en": "Velodrom?", "id": "Stadion." },
  { "en": "Ventilasi?", "id": "Tertutup." },
  { "en": "Venus?", "id": "Mars." },
  { "en": "Verbal?", "id": "Nonverbal." },
  { "en": "Verifikasi?", "id": "Falsifikasi." },
  { "en": "Vernakular?", "id": "Resmi." },
  { "en": "Versatil?", "id": "Terbatas." },
  { "en": "Versi?", "id": "Asli." },
  { "en": "Vertikal?", "id": "Horizontal." },
  { "en": "Veteran?", "id": "Pemula." },
  { "en": "Veto?", "id": "Setuju." },
  { "en": "Via?", "id": "Langsung." },
  { "en": "Video?", "id": "Audio." },
  { "en": "Vila?", "id": "Gubuk." },
  { "en": "Virtual?", "id": "Nyata." },
  { "en": "Virus?", "id": "Antivirus." },
  { "en": "Visa?", "id": "Ditolak." },
  { "en": "Visible?", "id": "Invisible." },
  { "en": "Visi?", "id": "Buta." },
  { "en": "Visual?", "id": "Audio." },
  { "en": "Vital?", "id": "Tidak." },
  { "en": "Vivid?", "id": "Pudar." },
  { "en": "Vokal?", "id": "Konsonan." },
  { "en": "Volatil?", "id": "Stabil." },
  { "en": "Volum?", "id": "Kosong." },
  { "en": "Volunter?", "id": "Paksaan." },
  { "en": "Vonis?", "id": "Bebas." },
  { "en": "Vot?", "id": "Abstain." },
  { "en": "Vulgar?", "id": "Sopan." },
  { "en": "Wabah?", "id": "Sehat." },
  { "en": "Wacana?", "id": "Diam." },
  { "en": "Wadat?", "id": "Nikah." },
  { "en": "Wadah?", "id": "Isi." },
  { "en": "Wafat?", "id": "Lahir." },
  { "en": "Wahah?", "id": "Gurun." },
  { "en": "Wahana?", "id": "Diam." },
  { "en": "Wahib?", "id": "Penerima." },
  { "en": "Wahid?", "id": "Jamak." },
  { "en": "Wajah?", "id": "Belakang." },
  { "en": "Wajar?", "id": "Aneh." },
  { "en": "Wajib?", "id": "Sunah." },
  { "en": "Wakaf?", "id": "Milik." },
  { "en": "Wakil?", "id": "Atasan." },
  { "en": "Waktu?", "id": "Ruang." },
  { "en": "Walakin?", "id": "Namun." },
  { "en": "Wali?", "id": "Murid." },
  { "en": "Wanita?", "id": "Pria." },
  { "en": "Wanti?", "id": "Izinkan." },
  { "en": "Wara?", "id": "Umum." },
  { "en": "Waranggana?", "id": "Pria." },
  { "en": "Waras?", "id": "Gila." },
  { "en": "Warga?", "id": "Asing." },
  { "en": "Waringin?", "id": "Semak." },
  { "en": "Waris?", "id": "Pewaris." },
  { "en": "Warna?", "id": "Hitamputih." },
  { "en": "Warsa?", "id": "Hari." },
  { "en": "Warta?", "id": "Rahasia." },
  { "en": "Warung?", "id": "Restoran." },
  { "en": "Wasiat?", "id": "Lalai." },
  { "en": "Waskita?", "id": "Lengah." },
  { "en": "Waspada?", "id": "Lalai." },
  { "en": "Waswas?", "id": "Yakin." },
  { "en": "Watak?", "id": "Sementara." },
  { "en": "Watan?", "id": "Asing." },
  { "en": "Wawan?", "id": "Tunggal." },
  { "en": "Wawancara?", "id": "Monolog." },
  { "en": "Wawas?", "id": "Sempit." },
  { "en": "Wayang?", "id": "Nyata." },
  { "en": "Wreda?", "id": "Muda." },
  { "en": "Wibawa?", "id": "Remeh." },
  { "en": "Widuri?", "id": "Biasa." },
  { "en": "Widyaiswara?", "id": "Siswa." },
  { "en": "Widyawisata?", "id": "Belajar." },
  { "en": "Wihara?", "id": "Duniawi." },
  { "en": "Wijaya?", "id": "Kalah." },
  { "en": "Wujud?", "id": "Gaib." },
  { "en": "Wukuf?", "id": "Tawaf." },
  { "en": "Wulan?", "id": "Surya." },
  { "en": "Wungkal?", "id": "Halus." },
  { "en": "Xenofobia?", "id": "Xenofilia." },
  { "en": "Xerofit?", "id": "Hidrofit." },
  { "en": "Ya?", "id": "Tidak." },
  { "en": "Yahud?", "id": "Buruk." },
  { "en": "Yakin?", "id": "Ragu." },
  { "en": "Yatim?", "id": "Berorangtua." },
  { "en": "Yayi?", "id": "Kakang." },
  { "en": "Yekun?", "id": "Berubah." },
  { "en": "Yuris?", "id": "Awam." },
  { "en": "Yustisi?", "id": "Ketidakadilan." },
  { "en": "Yuwana?", "id": "Tua." },
  { "en": "Zabaniah?", "id": "Malaikat." },
  { "en": "Zahir?", "id": "Batin." },
  { "en": "Zaitun?", "id": "Gelap." },
  { "en": "Zakat?", "id": "Riba." },
  { "en": "Zalim?", "id": "Adil." },
  { "en": "Zaman?", "id": "Abadi." },
  { "en": "Zamrud?", "id": "Merah." },
  { "en": "Zarah?", "id": "Besar." },
  { "en": "Zebra?", "id": "Polos." },
  { "en": "Zenit?", "id": "Nadir." },
  { "en": "Zero?", "id": "Maksimum." },
  { "en": "Zigzag?", "id": "Lurus." },
  { "en": "Zina?", "id": "Suci." },
  { "en": "Zionisme?", "id": "Antizionisme." },
  { "en": "Zodiak?", "id": "Nyata." },
  { "en": "Zuhud?", "id": "Duniawi." },
  { "en": "Absurd?", "id": "Logis." },
  { "en": "Adhesi?", "id": "Kohesi." },
  { "en": "Aditorium?", "id": "Panggung." },
  { "en": "Aklamasi?", "id": "Protes." },
  { "en": "Akselerasi?", "id": "Perlambatan." },
  { "en": "Analog?", "id": "Digital." },
  { "en": "Andragogi?", "id": "Pedagogi." },
  { "en": "Anom?", "id": "Tua." },
  { "en": "Antisipasi?", "id": "Reaksi." },
  { "en": "Antologi?", "id": "Monograf." },
  { "en": "Apatisme?", "id": "Antusiasme." },
  { "en": "Argumen?", "id": "Kesimpulan." },
  { "en": "Artikulasi?", "id": "Diam." },
  { "en": "Asimilasi?", "id": "Pembedaan." },
  { "en": "Asketis?", "id": "Hedonis." },
  { "en": "Atletis?", "id": "Lemah." },
  { "en": "Atrofi?", "id": "Hipertrofi." },
  { "en": "Audiens?", "id": "Pembicara." },
  { "en": "Autarki?", "id": "Ketergantungan." },
  { "en": "Balada?", "id": "Liris." },
  { "en": "Banal?", "id": "Orisinal." },
  { "en": "Barometer?", "id": "Variabel." },
  { "en": "Benchmark?", "id": "Substandar." },
  { "en": "Benefisit?", "id": "Malefisit." },
  { "en": "Bibliografi?", "id": "Lisan." },
  { "en": "Bikameral?", "id": "Unikameral." },
  { "en": "Bilingual?", "id": "Monolingual." },
  { "en": "Biografi?", "id": "Fiksi." },
  { "en": "Bipolar?", "id": "Unipolar." },
  { "en": "Blending?", "id": "Pemisahan." },
  { "en": "Blokade?", "id": "Terbuka." },
  { "en": "Bodor?", "id": "Serius." },
  { "en": "Bogel?", "id": "Berpakaian." },
  { "en": "Brahmana?", "id": "Sudra." },
  { "en": "Brevet?", "id": "Tidak." },
  { "en": "Bujuk?", "id": "Usir." },
  { "en": "Busana?", "id": "Telanjang." },
  { "en": "Cakra?", "id": "Luar." },
  { "en": "Cakrawala?", "id": "Bumi." },
  { "en": "Candra?", "id": "Surya." },
  { "en": "Catur?", "id": "Bebas." },
  { "en": "Cegah?", "id": "Biarkan." },
  { "en": "Cekatan?", "id": "Lamban." },
  { "en": "Cekung?", "id": "Cembung." },
  { "en": "Celaka?", "id": "Selamat." },
  { "en": "Cemooh?", "id": "Sanjung." },
  { "en": "Cengang?", "id": "Biasa." },
  { "en": "Centang?", "id": "Hapus." },
  { "en": "Ceruk?", "id": "Tonjolan." },
  { "en": "Cespleng?", "id": "Gagal." },
  { "en": "Cicil?", "id": "Kontan." },
  { "en": "Citra?", "id": "Buruk." },
  { "en": "Cola?", "id": "Utuh." },
  { "en": "Dabih?", "id": "Lembut." },
  { "en": "Dacin?", "id": "Ringan." },
  { "en": "Dadar?", "id": "Bulat." },
  { "en": "Dading?", "id": "Muda." },
  { "en": "Dae?", "id": "Kecil." },
  { "en": "Dafnah?", "id": "Busuk." },
  { "en": "Dagang?", "id": "Beli." },
  { "en": "Dagas?", "id": "Lembut." },
  { "en": "Dahiat?", "id": "Pintar." },
  { "en": "Dahriah?", "id": "Spiritual." },
  { "en": "Daksa?", "id": "Kiri." },
  { "en": "Dalal?", "id": "Benar." },
  { "en": "Dalih?", "id": "Jujur." },
  { "en": "Dam?", "id": "Lepas." },
  { "en": "Damar?", "id": "Gelap." },
  { "en": "Damba?", "id": "Tolak." },
  { "en": "Dambin?", "id": "Ramping." },
  { "en": "Dames?", "id": "Kasar." },
  { "en": "Dampit?", "id": "Jauh." },
  { "en": "Danawa?", "id": "Manusia." },
  { "en": "Dandang?", "id": "Kecil." },
  { "en": "Dangkal?", "id": "Dalam." },
  { "en": "Dangsa?", "id": "Modern." },
  { "en": "Danta?", "id": "Hitam." },
  { "en": "Danuh?", "id": "Kering." },
  { "en": "Darab?", "id": "Bagi." },
  { "en": "Darul?", "id": "Fana." },
  { "en": "Das?", "id": "Halus." },
  { "en": "Dasa?", "id": "Eka." },
  { "en": "Daur?", "id": "Statis." },
  { "en": "Dawai?", "id": "Diam." },
  { "en": "Dayaka?", "id": "Penerima." },
  { "en": "Debah?", "id": "Naik." },
  { "en": "Debar?", "id": "Tenang." },
  { "en": "Debik?", "id": "Puji." },
  { "en": "Dedai?", "id": "Cepat." },
  { "en": "Dedal?", "id": "Kosong." },
  { "en": "Dedau?", "id": "Puji." },
  { "en": "Degan?", "id": "Tua." },
  { "en": "Degap?", "id": "Lemah." },
  { "en": "Deh?", "id": "Jangan." },
  { "en": "Dehan?", "id": "Pergi." },
  { "en": "Dekih?", "id": "Untung." },
  { "en": "Dekik?", "id": "Timbul." },
  { "en": "Delat?", "id": "Cepat." },
  { "en": "Delik?", "id": "Umum." },
  { "en": "Demang?", "id": "Rakyat." },
  { "en": "Demes?", "id": "Benci." },
  { "en": "Dempet?", "id": "Renggang." },
  { "en": "Dempok?", "id": "Hindar." },
  { "en": "Dencang?", "id": "Pelan." },
  { "en": "Dencing?", "id": "Redam." },
  { "en": "Dengkang?", "id": "Lurus." },
  { "en": "Denok?", "id": "Jelek." },
  { "en": "Depa?", "id": "Pendek." },
  { "en": "Depun?", "id": "Kosong." },
  { "en": "Derai?", "id": "Kumpul." },
  { "en": "Deruk?", "id": "Halus." },
  { "en": "Desir?", "id": "Diam." },
  { "en": "Desit?", "id": "Keras." },
  { "en": "Destar?", "id": "Polos." },
  { "en": "Dharma?", "id": "Adharma." },
  { "en": "Di?", "id": "Luar." },
  { "en": "Diagnostik?", "id": "Prognostik." },
  { "en": "Diameter?", "id": "Keliling." },
  { "en": "Dinding?", "id": "Pintu." },
  { "en": "Diris?", "id": "Tebal." },
  { "en": "Disinsentif?", "id": "Insentif." },
  { "en": "Diskontinu?", "id": "Kontinu." },
  { "en": "Disorganisasi?", "id": "Organisasi." },
  { "en": "Dispersi?", "id": "Konsentrasi." },
  { "en": "Diva?", "id": "Figuran." },
  { "en": "Dondang?", "id": "Henti." },
  { "en": "Dongkol?", "id": "Senang." },
  { "en": "Donatur?", "id": "Penerima." },
  { "en": "Dosis?", "id": "Kurang." },
  { "en": "Doyan?", "id": "Benci." },
  { "en": "Driji?", "id": "Kaki." },
  { "en": "Dualisme?", "id": "Monisme." },
  { "en": "Dubius?", "id": "Pasti." },
  { "en": "Dugdag?", "id": "Tenang." },
  { "en": "Dukana?", "id": "Sukacita." },
  { "en": "Dukuh?", "id": "Kota." },
  { "en": "Dumi?", "id": "Asli." },
  { "en": "Dur?", "id": "Dekat." },
  { "en": "Durhaka?", "id": "Patuh." },
  { "en": "Durnois?", "id": "Baik." },
  { "en": "Dwi?", "id": "Eka." },
  { "en": "Eces?", "id": "Sulit." },
  { "en": "Edema?", "id": "Kempis." },
  { "en": "Eforia?", "id": "Disforia." },
  { "en": "Ejakulasi?", "id": "Retensi." },
  { "en": "Eksentrik?", "id": "Konsentris." },
  { "en": "Eksitasi?", "id": "Inhibisi." },
  { "en": "Ekskavasi?", "id": "Penimbunan." },
  { "en": "Ekstasi?", "id": "Sedih." },
  { "en": "Ekuilibrium?", "id": "Disekuilibrium." },
  { "en": "Emas?", "id": "Besi." },
  { "en": "Embrio?", "id": "Dewasa." },
  { "en": "Emiten?", "id": "Investor." },
  { "en": "Empiris?", "id": "Rasional." },
  { "en": "Enigma?", "id": "Jelas." },
  { "en": "Enjat?", "id": "Diam." },
  { "en": "Entas?", "id": "Tenggelamkan." },
  { "en": "Epidemi?", "id": "Endemi." },
  { "en": "Epilepsi?", "id": "Normal." },
  { "en": "Eradikasi?", "id": "Pelestarian." },
  { "en": "Eskapisme?", "id": "Realisme." },
  { "en": "Esok?", "id": "Kemarin." },
  { "en": "Estetika?", "id": "Buruk." },
  { "en": "Etnis?", "id": "Nasional." },
  { "en": "Eufemisme?", "id": "Disfemisme." },
  { "en": "Fadil?", "id": "Hina." },
  { "en": "Fakih?", "id": "Awam." },
  { "en": "Falak?", "id": "Bumi." },
  { "en": "Fals?", "id": "Benar." },
  { "en": "Fantasi?", "id": "Realitas." },
  { "en": "Fardu?", "id": "Sunah." },
  { "en": "Farsakh?", "id": "Dekat." },
  { "en": "Fasakh?", "id": "Sah." },
  { "en": "Fatanah?", "id": "Baladah." },
  { "en": "Fatihah?", "id": "Khatimah." },
  { "en": "Fatir?", "id": "Beragi." },
  { "en": "Fatur?", "id": "Kuat." },
  { "en": "Fauna?", "id": "Flora." },
  { "en": "Fedayin?", "id": "Musuh." },
  { "en": "Federal?", "id": "Sentral." },
  { "en": "Femto?", "id": "Tera." },
  { "en": "Fenomena?", "id": "Noumena." },
  { "en": "Fermentasi?", "id": "Sterilisasi." },
  { "en": "Fertilitas?", "id": "Infertilitas." },
  { "en": "Fetor?", "id": "Aroma." },
  { "en": "Fiasko?", "id": "Sukses." },
  { "en": "Fidusi?", "id": "Tunai." },
  { "en": "Fikih?", "id": "Mistis." },
  { "en": "Filantrop?", "id": "Misantrop." },
  { "en": "Filial?", "id": "Induk." },
  { "en": "Filibuster?", "id": "Dukungan." },
  { "en": "Filologi?", "id": "Butaaksara." },
  { "en": "Filtrasi?", "id": "Pencampuran." },
  { "en": "Final?", "id": "Awal." },
  { "en": "Fiord?", "id": "Dataran." },
  { "en": "Firma?", "id": "Perorangan." },
  { "en": "Fisi?", "id": "Fusi." },
  { "en": "Fisiognomi?", "id": "Abstrak." },
  { "en": "Fisura?", "id": "Menyatu." },
  { "en": "Fit?", "id": "Tidaksehat." },
  { "en": "Flamboyan?", "id": "Sederhana." },
  { "en": "Flat?", "id": "Miring." },
  { "en": "Flegma?", "id": "Emosi." },
  { "en": "Fleksi?", "id": "Ekstensi." },
  { "en": "Fluktuasi?", "id": "Kestabilan." },
  { "en": "Fluoresens?", "id": "Fosforesens." },
  { "en": "Fobia?", "id": "Mania." },
  { "en": "Fokus?", "id": "Buyar." },
  { "en": "Fonem?", "id": "Grafem." },
  { "en": "Fonografi?", "id": "Lisan." },
  { "en": "Formasi?", "id": "Kekacauan." },
  { "en": "Formula?", "id": "Acak." },
  { "en": "Forward?", "id": "Backward." },
  { "en": "Fosil?", "id": "Baru." },
  { "en": "Fragan?", "id": "Bau." },
  { "en": "Fraksi?", "id": "Kesatuan." },
  { "en": "Fraktur?", "id": "Menyatu." },
  { "en": "Frekuen?", "id": "Jarang." },
  { "en": "Friksi?", "id": "Harmoni." },
  { "en": "Frivol?", "id": "Serius." },
  { "en": "Fruktosa?", "id": "Glukosa." },
  { "en": "Frustrasi?", "id": "Kepuasan." },
  { "en": "Fugasitas?", "id": "Kestabilan." },
  { "en": "Fulminan?", "id": "Lambat." },
  { "en": "Fulus?", "id": "Tidakpunya." },
  { "en": "Fundamen?", "id": "Permukaan." },
  { "en": "Fungibel?", "id": "Unik." },
  { "en": "Fungi?", "id": "Bakteri." },
  { "en": "Fungsional?", "id": "Disfungsi." },
  { "en": "Fusi?", "id": "Fisi." },
  { "en": "Futur?", "id": "Lalu." },
  { "en": "Gabas?", "id": "Lembut." },
  { "en": "Gabor?", "id": "Tenang." },
  { "en": "Gaduk?", "id": "Asli." },
  { "en": "Gafara?", "id": "Dosa." },
  { "en": "Gaharu?", "id": "Biasa." },
  { "en": "Gaib?", "id": "Nyata." },
  { "en": "Gajak?", "id": "Jujur." },
  { "en": "Galang?", "id": "Robohkan." },
  { "en": "Galat?", "id": "Benar." },
  { "en": "Gali?", "id": "Timbun." },
  { "en": "Galvanis?", "id": "Karatan." },
  { "en": "Gamat?", "id": "Cepat." },
  { "en": "Gamblang?", "id": "Rumit." },
  { "en": "Gambut?", "id": "Kering." },
  { "en": "Gamis?", "id": "Kasual." },
  { "en": "Gamit?", "id": "Lepas." },
  { "en": "Gana?", "id": "Kurang." },
  { "en": "Ganco?", "id": "Dorong." },
  { "en": "Gandam?", "id": "Sadar." },
  { "en": "Gandar?", "id": "Pusat." },
  { "en": "Gandeng?", "id": "Pisah." },
  { "en": "Gandes?", "id": "Kaku." },
  { "en": "Gandrung?", "id": "Benci." },
  { "en": "Gangsir?", "id": "Padat." },
  { "en": "Gani?", "id": "Fakir." },
  { "en": "Ganjal?", "id": "Mulus." },
  { "en": "Ganjar?", "id": "Hukum." },
  { "en": "Gantel?", "id": "Longgar." },
  { "en": "Gapah?", "id": "Lamban." },
  { "en": "Gapil?", "id": "Sopan." },
  { "en": "Garib?", "id": "Biasa." },
  { "en": "Garis?", "id": "Titik." },
  { "en": "Garwa?", "id": "Suami." },
  { "en": "Gasab?", "id": "Kembalikan." },
  { "en": "Gastro?", "id": "Luar." },
  { "en": "Gaung?", "id": "Senyap." },
  { "en": "Gawai?", "id": "Istirahat." },
  { "en": "Gawal?", "id": "Mudah." },
  { "en": "Gayam?", "id": "Nyata." },
  { "en": "Gayal?", "id": "Cepat." },
  { "en": "Gayeng?", "id": "Sepi." },
  { "en": "Gayuh?", "id": "Lepas." },
  { "en": "Gazal?", "id": "Prosa." },
  { "en": "Gebang?", "id": "Teduh." },
  { "en": "Gebar?", "id": "Halus." },
  { "en": "Geben?", "id": "Tipis." },
  { "en": "Geblek?", "id": "Pintar." },
  { "en": "Gebyur?", "id": "Kering." },
  { "en": "Gecar?", "id": "Padat." },
  { "en": "Gedik?", "id": "Stabil." },
  { "en": "Gegala?", "id": "Halus." },
  { "en": "Gegetun?", "id": "Gembira." },
  { "en": "Geladir?", "id": "Kering." },
  { "en": "Gelagar?", "id": "Senyap." },
  { "en": "Gelatak?", "id": "Lamban." },
  { "en": "Gelayar?", "id": "Lamban." },
  { "en": "Gelebah?", "id": "Tenang." },
  { "en": "Geleca?", "id": "Jujur." },
  { "en": "Gelecik?", "id": "Stabil." },
  { "en": "Gelecoh?", "id": "Jujur." },
  { "en": "Geletek?", "id": "Diam." },
  { "en": "Geliang?", "id": "Kaku." },
  { "en": "Gelimir?", "id": "Tebal." },
  { "en": "Gelintang?", "id": "Teratur." },
  { "en": "Gelitar?", "id": "Redup." },
  { "en": "Gelodar?", "id": "Rapi." },
  { "en": "Gelogok?", "id": "Pelan." },
  { "en": "Gelumat?", "id": "Kasar." },
  { "en": "Gemang?", "id": "Berani." },
  { "en": "Gemawan?", "id": "Bumi." },
  { "en": "Gembil?", "id": "Kurus." },
  { "en": "Gemblak?", "id": "Asli." },
  { "en": "Gemul?", "id": "Terpisah." },
  { "en": "Genahar?", "id": "Dingin." },
  { "en": "Gencar?", "id": "Jarang." },
  { "en": "Gencat?", "id": "Lanjutkan." },
  { "en": "Gending?", "id": "Prosa." },
  { "en": "Genealogi?", "id": "Acak." },
  { "en": "General?", "id": "Spesifik." },
  { "en": "Generasi?", "id": "Individu." },
  { "en": "Genesis?", "id": "Akhir." },
  { "en": "Genetika?", "id": "Lingkungan." },
  { "en": "Gentari?", "id": "Lambat." },
  { "en": "Geodesi?", "id": "Sembarang." },
  { "en": "Geofisika?", "id": "Metafisika." },
  { "en": "Geologi?", "id": "Astrologi." },
  { "en": "Geometri?", "id": "Aljabar." },
  { "en": "Gerages?", "id": "Rapi." },
  { "en": "Geramang?", "id": "Jinak." },
  { "en": "Geramsut?", "id": "Halus." },
  { "en": "Gerantak?", "id": "Teratur." },
  { "en": "Gerbas?", "id": "Baru." },
  { "en": "Gerbera?", "id": "Layu." },
  { "en": "Gerecak?", "id": "Tenang." },
  { "en": "Gereget?", "id": "Lemah." },
  { "en": "Geremet?", "id": "Cepat." },
  { "en": "Gerencang?", "id": "Senyap." },
  { "en": "Gerentam?", "id": "Reda." },
  { "en": "Gerha?", "id": "Luar." },
  { "en": "Geriatri?", "id": "Pediatri." },
  { "en": "Geridip?", "id": "Nyala." },
  { "en": "Geridit?", "id": "Besar." },
  { "en": "Gerilya?", "id": "Terbuka." },
  { "en": "Gerimis?", "id": "Deras." },
  { "en": "Gerinyau?", "id": "Diam." },
  { "en": "Gernat?", "id": "Peluru." },
  { "en": "Gerontokrasi?", "id": "Meritokrasi." },
  { "en": "Gerpol?", "id": "Damai." },
  { "en": "Gersik?", "id": "Subur." },
  { "en": "Gertak?", "id": "Bujuk." },
  { "en": "Gerugut?", "id": "Halus." },
  { "en": "Geruh?", "id": "Untung." },
  { "en": "Geruit?", "id": "Diam." },
  { "en": "Gesek?", "id": "Diam." },
  { "en": "Gestur?", "id": "Diam." },
  { "en": "Getun?", "id": "Senang." },
  { "en": "Gibah?", "id": "Puji." },
  { "en": "Gigabytes?", "id": "Kilobytes." },
  { "en": "Gilap?", "id": "Kusam." },
  { "en": "Gim?", "id": "Nyata." },
  { "en": "Gimbal?", "id": "Rapi." },
  { "en": "Gips?", "id": "Buka." },
  { "en": "Gir?", "id": "Datar." },
  { "en": "Giral?", "id": "Tunai." },
  { "en": "Giras?", "id": "Lamban." },
  { "en": "Girik?", "id": "Surat." },
  { "en": "Gisar?", "id": "Tetap." },
  { "en": "Gita?", "id": "Prosa." },
  { "en": "Giuk?", "id": "Tenang." },
  { "en": "Gladi?", "id": "Final." },
  { "en": "Glanir?", "id": "Kasar." },
  { "en": "Glasial?", "id": "Tropis." },
  { "en": "Glaukoma?", "id": "Normal." },
  { "en": "Glek?", "id": "Sembur." },
  { "en": "Gelintir?", "id": "Banyak." },
  { "en": "Globulin?", "id": "Albumin." },
  { "en": "Glomus?", "id": "Rata." },
  { "en": "Glotal?", "id": "Non." },
  { "en": "Glukagon?", "id": "Insulin." },
  { "en": "Godek?", "id": "Muda." },
  { "en": "Godok?", "id": "Mentah." },
  { "en": "Gogok?", "id": "Serius." },
  { "en": "Gombak?", "id": "Botak." },
  { "en": "Gompal?", "id": "Utuh." },
  { "en": "Gonad?", "id": "Tubuh." },
  { "en": "Gondok?", "id": "Normal." },
  { "en": "Gong?", "id": "Sunyi." },
  { "en": "Gongseng?", "id": "Rebus." },
  { "en": "Goni?", "id": "Halus." },
  { "en": "Gono?", "id": "Bersama." },
  { "en": "Gontok?", "id": "Akur." },
  { "en": "Gonyak?", "id": "Tenang." },
  { "en": "Gorek?", "id": "Halus." },
  { "en": "Gotes?", "id": "Utuh." },
  { "en": "Gotri?", "id": "Besar." },
  { "en": "Goyah?", "id": "Mantap." },
  { "en": "Gradasi?", "id": "Kontras." },
  { "en": "Graf?", "id": "Acak." },
  { "en": "Graha?", "id": "Luar." },
  { "en": "Gram?", "id": "Banyak." },
  { "en": "Grasiil?", "id": "Kasar." },
  { "en": "Gratifikasi?", "id": "Hukuman." },
  { "en": "Gravitasi?", "id": "Mengambang." },
  { "en": "Grecok?", "id": "Tenang." },
  { "en": "Gregarius?", "id": "Soliter." },
  { "en": "Grehon?", "id": "Jujur." },
  { "en": "Gria?", "id": "Duniawi." },
  { "en": "Grif?", "id": "Kosong." },
  { "en": "Gros?", "id": "Eceran." },
  { "en": "Gua?", "id": "Terbuka." },
  { "en": "Gubah?", "id": "Asli." },
  { "en": "Gubit?", "id": "Lepas." },
  { "en": "Guci?", "id": "Datar." },
  { "en": "Gudam?", "id": "Nyala." },
  { "en": "Gudang?", "id": "Pamer." },
  { "en": "Gugat?", "id": "Dukung." },
  { "en": "Guguk?", "id": "Datar." },
  { "en": "Gula?", "id": "Garam." },
  { "en": "Gulai?", "id": "Hambar." },
  { "en": "Gulat?", "id": "Damai." },
  { "en": "Gulden?", "id": "Rupiah." },
  { "en": "Gulir?", "id": "Henti." },
  { "en": "Gulud?", "id": "Lembah." },
  { "en": "Gum?", "id": "Nyata." },
  { "en": "Gumam?", "id": "Jelas." },
  { "en": "Gumbuk?", "id": "Paksa." },
  { "en": "Gumpil?", "id": "Kokoh." },
  { "en": "Gumul?", "id": "Pisah." },
  { "en": "Gun?", "id": "Rugi." },
  { "en": "Gunawan?", "id": "Jahadia." },
  { "en": "Guncang?", "id": "Stabil." },
  { "en": "Gundik?", "id": "Permaisuri." },
  { "en": "Gung?", "id": "Rendah." },
  { "en": "Guni?", "id": "Modern." },
  { "en": "Gunjai?", "id": "Rapi." },
  { "en": "Guntai?", "id": "Kencang." },
  { "en": "Guntur?", "id": "Senyap." },
  { "en": "Gunung?", "id": "Lembah." },
  { "en": "Gup?", "id": "Nyala." },
  { "en": "Gurab?", "id": "Modern." },
  { "en": "Gurah?", "id": "Isi." },
  { "en": "Gurami?", "id": "Darat." },
  { "en": "Gurdi?", "id": "Tutup." },
  { "en": "Guri?", "id": "Hambar." },
  { "en": "Gurita?", "id": "Ikan." },
  { "en": "Gurnadur?", "id": "Daerah." },
  { "en": "Guru?", "id": "Murid." },
  { "en": "Gurub?", "id": "Terbit." },
  { "en": "Gurun?", "id": "Oasis." },
  { "en": "Gusar?", "id": "Tenang." },
  { "en": "Gusul?", "id": "Kering." },
  { "en": "Gusur?", "id": "Bangun." },
  { "en": "Gutik?", "id": "Pukul." },
  { "en": "Habis?", "id": "Mulai." },
  { "en": "Habitat?", "id": "Asing." },
  { "en": "Hablur?", "id": "Amorf." },
  { "en": "Hadang?", "id": "Lewati." },
  { "en": "Hadap?", "id": "Belakangi." },
  { "en": "Hades?", "id": "Surga." },
  { "en": "Hadiah?", "id": "Hukuman." },
  { "en": "Hadis?", "id": "Quran." },
  { "en": "Hafiz?", "id": "Lupa." },
  { "en": "Haiking?", "id": "Diam." },
  { "en": "Hail?", "id": "Gagal." },
  { "en": "Hajar?", "id": "Sayang." },
  { "en": "Hajat?", "id": "Tidak." },
  { "en": "Haji?", "id": "Umrah." },
  { "en": "Hakiki?", "id": "Majasi." },
  { "en": "Hal?", "id": "Khusus." },
  { "en": "Halau?", "id": "Panggil." },
  { "en": "Halazon?", "id": "Polusi." },
  { "en": "Halba?", "id": "Manis." },
  { "en": "Halimun?", "id": "Cerah." },
  { "en": "Halkah?", "id": "Terbuka." },
  { "en": "Halma?", "id": "Sendiri." },
  { "en": "Halsduk?", "id": "Lepas." },
  { "en": "Haluan?", "id": "Buritan." },
  { "en": "Hama?", "id": "Subur." },
  { "en": "Hamba?", "id": "Tuan." },
  { "en": "Hambalang?", "id": "Rapi." },
  { "en": "Hambar?", "id": "Gurih." },
  { "en": "Hambur?", "id": "Kumpul." },
  { "en": "Hamen?", "id": "Publik." },
  { "en": "Hamil?", "id": "Tidak." },
  { "en": "Hampir?", "id": "Jauh." },
  { "en": "Hamud?", "id": "Basa." },
  { "en": "Hancing?", "id": "Wangi." },
  { "en": "Handai?", "id": "Musuh." },
  { "en": "Handal?", "id": "Gagal." },
  { "en": "Handuk?", "id": "Basah." },
  { "en": "Hanggar?", "id": "Lapang." },
  { "en": "Hangit?", "id": "Harum." },
  { "en": "Hanif?", "id": "Musyrik." },
  { "en": "Hanjakal?", "id": "Syukur." },
  { "en": "Hantam?", "id": "Elus." },
  { "en": "Hantar?", "id": "Ambil." },
  { "en": "Hantir?", "id": "Dekat." },
  { "en": "Hantu?", "id": "Manusia." },
  { "en": "Hanya?", "id": "Semua." },
  { "en": "Hanyut?", "id": "Menepi." },
  { "en": "Hapetan?", "id": "Bebas." },
  { "en": "Haplologi?", "id": "Pengulangan." },
  { "en": "Haplus?", "id": "Kasar." },
  { "en": "Hara?", "id": "Racun." },
  { "en": "Hardik?", "id": "Puji." },
  { "en": "Harem?", "id": "Publik." },
  { "en": "Harfiah?", "id": "Kiasan." },
  { "en": "Harmonium?", "id": "Perkusi." },
  { "en": "Harta?", "id": "Utang." },
  { "en": "Hartal?", "id": "Kerja." },
  { "en": "Haru?", "id": "Tegar." },
  { "en": "Harung?", "id": "Hindar." },
  { "en": "Hasta?", "id": "Kaki." },
  { "en": "Hasud?", "id": "Dermawan." },
  { "en": "Hasyah?", "id": "Pujian." },
  { "en": "Hasyis?", "id": "Sadar." },
  { "en": "Hatta?", "id": "Sebelum." },
  { "en": "Haur?", "id": "Kuat." },
  { "en": "Hawa?", "id": "Dingin." },
  { "en": "Hawar?", "id": "Sehat." },
  { "en": "Heboh?", "id": "Tenang." },
  { "en": "Hedonis?", "id": "Asketis." },
  { "en": "Hegemoni?", "id": "Subordinasi." },
  { "en": "Helah?", "id": "Jujur." },
  { "en": "Helai?", "id": "Lembar." },
  { "en": "Helat?", "id": "Biasa." },
  { "en": "Helikopter?", "id": "Pesawat." },
  { "en": "Helium?", "id": "Berat." },
  { "en": "Helm?", "id": "Terbuka." },
  { "en": "Hemapoiesis?", "id": "Hemolisis." },
  { "en": "Hemi?", "id": "Seluruh." },
  { "en": "Hempap?", "id": "Angkat." },
  { "en": "Hempas?", "id": "Pegang." },
  { "en": "Hendak?", "id": "Enggan." },
  { "en": "Hening?", "id": "Ribut." },
  { "en": "Hentar?", "id": "Pelan." },
  { "en": "Hepatitis?", "id": "Sehat." },
  { "en": "Herba?", "id": "Kimia." },
  { "en": "Herbik?", "id": "Subur." },
  { "en": "Herder?", "id": "Pengecut." },
  { "en": "Hereditas?", "id": "Lingkungan." },
  { "en": "Heroin?", "id": "Obat." },
  { "en": "Heroisme?", "id": "Kepengecutan." },
  { "en": "Hetero?", "id": "Homo." },
  { "en": "Heteronim?", "id": "Sinonim." },
  { "en": "Heurisitik?", "id": "Algoritmik." },
  { "en": "Hewani?", "id": "Nabati." },
  { "en": "Heksagon?", "id": "Lingkaran." },
  { "en": "Hialin?", "id": "Buram." },
  { "en": "Hian?", "id": "Setia." },
  { "en": "Hias?", "id": "Polos." },
  { "en": "Hibat?", "id": "Kecil." },
  { "en": "Hibrida?", "id": "Murni." },
  { "en": "Hidraulis?", "id": "Mekanis." },
  { "en": "Hidro?", "id": "Piro." },
  { "en": "Hidrofit?", "id": "Xerofit." },
  { "en": "Hidrofobia?", "id": "Hidrofilia." },
  { "en": "Hierarki?", "id": "Setara." },
  { "en": "Higienis?", "id": "Kotor." },
  { "en": "Hijab?", "id": "Terbuka." },
  { "en": "Hijau?", "id": "Merah." },
  { "en": "Hijrah?", "id": "Menetap." },
  { "en": "Hikayat?", "id": "Fakta." },
  { "en": "Hilal?", "id": "Purnama." },
  { "en": "Himpit?", "id": "Longgar." },
  { "en": "Himpun?", "id": "Sebar." },
  { "en": "Hingarbingar?", "id": "Senyap." },
  { "en": "Hinggap?", "id": "Terbang." },
  { "en": "Hiper?", "id": "Hipo." },
  { "en": "Hiperbol?", "id": "Litotes." },
  { "en": "Hiperemia?", "id": "Anemia." },
  { "en": "Hipnosis?", "id": "Sadar." },
  { "en": "Hipokonder?", "id": "Optimis." },
  { "en": "Hipokotil?", "id": "Epikotil." },
  { "en": "Hipotesis?", "id": "Fakta." },
  { "en": "Hipsometer?", "id": "Barometer." },
  { "en": "Hirap?", "id": "Muncul." },
  { "en": "Hirau?", "id": "Peduli." },
  { "en": "Hirudin?", "id": "Pembeku." },
  { "en": "Hisab?", "id": "Rukyat." },
  { "en": "Histologi?", "id": "Makroskopis." },
  { "en": "Histeresis?", "id": "Reversibel." },
  { "en": "Hitung?", "id": "Abaikan." },
  { "en": "Hobi?", "id": "Pekerjaan." },
  { "en": "Homofon?", "id": "Heterofon." },
  { "en": "Homogenitas?", "id": "Heterogenitas." },
  { "en": "Homograf?", "id": "Heterograf." },
  { "en": "Homologi?", "id": "Analogi." },
  { "en": "Homonim?", "id": "Sinonim." },
  { "en": "Honor?", "id": "Denda." },
  { "en": "Honorer?", "id": "Tetap." },
  { "en": "Horison?", "id": "Zenit." },
  { "en": "Hormon?", "id": "Enzim." },
  { "en": "Horor?", "id": "Komedi." },
  { "en": "Hortikultura?", "id": "Agronomi." },
  { "en": "Hospital?", "id": "Musuhan." },
  { "en": "Hosti?", "id": "Duniawi." },
  { "en": "Hot?", "id": "Dingin." },
  { "en": "Hujaj?", "id": "Penentang." },
  { "en": "Hujah?", "id": "Pujian." },
  { "en": "Hukama?", "id": "Awam." },
  { "en": "Hukum?", "id": "Anarki." },
  { "en": "Huler?", "id": "Tarik." },
  { "en": "Human?", "id": "Nonhuman." },
  { "en": "Humaniora?", "id": "Sains." },
  { "en": "Humor?", "id": "Serius." },
  { "en": "Humus?", "id": "Gersang." },
  { "en": "Huni?", "id": "Kosongkan." },
  { "en": "Huriah?", "id": "Perbudakan." },
  { "en": "Hus?", "id": "Bising." },
  { "en": "Hutan?", "id": "Kota." },
  { "en": "Ibadah?", "id": "Maksiat." },
  { "en": "Ibunda?", "id": "Ananda." },
  { "en": "Ibrani?", "id": "Arab." },
  { "en": "Idafi?", "id": "Nyata." },
  { "en": "Ideal?", "id": "Praktis." },
  { "en": "Ideologi?", "id": "Pragmatisme." },
  { "en": "Idem?", "id": "Berbeda." },
  { "en": "Identifikasi?", "id": "Samarkan." },
  { "en": "Idiom?", "id": "Harfiah." },
  { "en": "Idrak?", "id": "Lalai." },
  { "en": "Idulfitri?", "id": "Iduladha." },
  { "en": "Ifrit?", "id": "Malaikat." },
  { "en": "Iga?", "id": "Daging." },
  { "en": "Igas?", "id": "Sambung." },
  { "en": "Ihwal?", "id": "Tidakpenting." },
  { "en": "Ijazah?", "id": "Tidaklulus." },
  { "en": "Ijtihad?", "id": "Taklid." },
  { "en": "Ikamat?", "id": "Azan." },
  { "en": "Ikon?", "id": "Tidakdikenal." },
  { "en": "Ikterus?", "id": "Normal." },
  { "en": "Ikhtiar?", "id": "Pasrah." },
  { "en": "Ikhtiari?", "id": "Wajib." },
  { "en": "Ikhtilaf?", "id": "Sepakat." },
  { "en": "Ikhtisar?", "id": "Detail." },
  { "en": "Ilah?", "id": "Makhluk." },
  { "en": "Ilat?", "id": "Normal." },
  { "en": "Ileum?", "id": "Lambung." },
  { "en": "Ilham?", "id": "Buntu." },
  { "en": "Iluminasi?", "id": "Kegelapan." },
  { "en": "Imago?", "id": "Larva." },
  { "en": "Imamat?", "id": "Awam." },
  { "en": "Imanensi?", "id": "Transendensi." },
  { "en": "Imaterial?", "id": "Material." },
  { "en": "Imatur?", "id": "Matur." },
  { "en": "Imbau?", "id": "Abaikan." },
  { "en": "Imbibisi?", "id": "Ekskresi." },
  { "en": "Imigrasi?", "id": "Emigrasi." },
  { "en": "Iming?", "id": "Jauhkan." },
  { "en": "Imkan?", "id": "Mustahil." },
  { "en": "Imla?", "id": "Lisan." },
  { "en": "Imobil?", "id": "Mobil." },
  { "en": "Impak?", "id": "Sebab." },
  { "en": "Impedansi?", "id": "Konduktansi." },
  { "en": "Imperatif?", "id": "Opsional." },
  { "en": "Imperfek?", "id": "Perfek." },
  { "en": "Imperial?", "id": "Republik." },
  { "en": "Impersonal?", "id": "Personal." },
  { "en": "Implikasi?", "id": "Eksplikasi." },
  { "en": "Implisit?", "id": "Terang." },
  { "en": "Imponderabilia?", "id": "Terukur." },
  { "en": "Impresario?", "id": "Penonton." },
  { "en": "Impresif?", "id": "Biasa." },
  { "en": "Imprimatur?", "id": "Sensor." },
  { "en": "Improvisasi?", "id": "Terencana." },
  { "en": "Impulsif?", "id": "Terencana." },
  { "en": "Imun?", "id": "Rentan." },
  { "en": "Inai?", "id": "Hapus." },
  { "en": "Inang?", "id": "Parasit." },
  { "en": "Inaugurasi?", "id": "Akhir." },
  { "en": "Inayat?", "id": "Lalai." },
  { "en": "Indah?", "id": "Buruk." },
  { "en": "Indeks?", "id": "Acak." },
  { "en": "Independen?", "id": "Dependen." },
  { "en": "Indigen?", "id": "Asing." },
  { "en": "Indikasi?", "id": "Kontraindikasi." },
  { "en": "Indigo?", "id": "Merah." },
  { "en": "Indolen?", "id": "Rajin." },
  { "en": "Indukta?", "id": "Dedukta." },
  { "en": "Induktif?", "id": "Deduktif." },
  { "en": "Indulgensia?", "id": "Hukuman." },
  { "en": "Industria?", "id": "Agraria." },
  { "en": "Inefisiensi?", "id": "Efisiensi." },
  { "en": "Inersia?", "id": "Aktif." },
  { "en": "Infantil?", "id": "Dewasa." },
  { "en": "Infak?", "id": "Ambil." },
  { "en": "Infiltrasi?", "id": "Eksfiltrasi." },
  { "en": "Infinit?", "id": "Finit." },
  { "en": "Infinitesimal?", "id": "Besar." },
  { "en": "Inflamasi?", "id": "Penenangan." },
  { "en": "Infleksi?", "id": "Lurus." },
  { "en": "Influensa?", "id": "Sehat." },
  { "en": "Infra?", "id": "Supra." },
  { "en": "Infrasonik?", "id": "Ultrasonik." },
  { "en": "Ingar?", "id": "Tenang." },
  { "en": "Ingat?", "id": "Lupa." },
  { "en": "Ingenuitas?", "id": "Kebodohan." },
  { "en": "Ingresif?", "id": "Egresif." },
  { "en": "Inhalasi?", "id": "Ekshalasi." },
  { "en": "Inheren?", "id": "Eksternal." },
  { "en": "Inisial?", "id": "Final." }


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
            }, 4000);
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
