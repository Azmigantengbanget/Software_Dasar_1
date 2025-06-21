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


  { "en": "Instruksi untuk komputer?", "id": "Perangkat lunak (Software)." },
  { "en": "Bagian fisik komputer?", "id": "Perangkat keras (Hardware)." },
  { "en": "Dua jenis utama software?", "id": "Sistem dan Aplikasi." },
  { "en": "Software yang mengelola hardware?", "id": "Perangkat Lunak Sistem." },
  { "en": "Software untuk tugas spesifik pengguna?", "id": "Perangkat Lunak Aplikasi." },
  { "en": "Contoh perangkat lunak sistem?", "id": "Sistem Operasi (OS)." },
  { "en": "Inti dari sebuah sistem operasi?", "id": "Kernel." },
  { "en": "Fungsi utama sistem operasi?", "id": "Manajemen sumber daya komputer." },
  { "en": "Contoh sistem operasi desktop?", "id": "Windows, macOS, Linux." },
  { "en": "Contoh sistem operasi mobile?", "id": "Android, iOS." },
  { "en": "Software penghubung hardware dan OS?", "id": "Device Driver." },
  { "en": "Software untuk pemeliharaan sistem?", "id": "Utility Software." },
  { "en": "Contoh utility software?", "id": "Antivirus, disk defragmenter." },
  { "en": "Software untuk membuat dokumen teks?", "id": "Word Processor." },
  { "en": "Contoh word processor?", "id": "Microsoft Word, Google Docs." },
  { "en": "Software untuk mengolah angka/tabel?", "id": "Spreadsheet." },
  { "en": "Contoh spreadsheet?", "id": "Microsoft Excel, Google Sheets." },
  { "en": "Software untuk presentasi?", "id": "Presentation Software." },
  { "en": "Contoh software presentasi?", "id": "PowerPoint, Google Slides." },
  { "en": "Software untuk menjelajah internet?", "id": "Web Browser." },
  { "en": "Contoh web browser?", "id": "Chrome, Firefox, Safari." },
  { "en": "Software untuk mengelola database?", "id": "DBMS." },
  { "en": "Kepanjangan DBMS?", "id": "Database Management System." },
  { "en": "Software untuk mengedit foto?", "id": "Photo Editor." },
  { "en": "Contoh photo editor?", "id": "Adobe Photoshop, GIMP." },
  { "en": "Software untuk bermain?", "id": "Game." },
  { "en": "Bahasa yang digunakan membuat software?", "id": "Bahasa Pemrograman." },
  { "en": "Contoh bahasa pemrograman?", "id": "Python, Java, C++." },
  { "en": "Kode yang ditulis oleh manusia?", "id": "Source Code (Kode Sumber)." },
  { "en": "Kode yang dimengerti oleh mesin?", "id": "Machine Code (Kode Mesin)." },
  { "en": "Penerjemah source code ke machine code?", "id": "Compiler atau Interpreter." },
  { "en": "Penerjemah seluruh kode sekaligus?", "id": "Compiler." },
  { "en": "Penerjemah kode baris per baris?", "id": "Interpreter." },
  { "en": "Langkah-langkah logis untuk menyelesaikan masalah?", "id": "Algoritma." },
  { "en": "Representasi visual dari algoritma?", "id": "Flowchart (Diagram Alir)." },
  { "en": "Kesalahan dalam sebuah program?", "id": "Bug." },
  { "en": "Proses mencari dan memperbaiki bug?", "id": "Debugging." },
  { "en": "Software yang kode sumbernya terbuka?", "id": "Open Source." },
  { "en": "Contoh software open source?", "id": "Linux, Firefox, VLC Player." },
  { "en": "Software yang kode sumbernya tertutup?", "id": "Proprietary / Closed Source." },
  { "en": "Contoh software proprietary?", "id": "Windows, Microsoft Office." },
  { "en": "Software gratis tanpa batasan?", "id": "Freeware." },
  { "en": "Contoh freeware?", "id": "Google Chrome, Adobe Reader." },
  { "en": "Software gratis untuk masa percobaan?", "id": "Shareware." },
  { "en": "Versi percobaan dengan fitur terbatas?", "id": "Trial version." },
  { "en": "Antarmuka pengguna grafis?", "id": "GUI (Graphical User Interface)." },
  { "en": "Antarmuka pengguna berbasis teks?", "id": "CLI (Command-Line Interface)." },
  { "en": "Pengalaman pengguna saat memakai software?", "id": "UX (User Experience)." },
  { "en": "Tampilan visual dari sebuah software?", "id": "UI (User Interface)." },
  { "en": "Sekumpulan data yang disimpan?", "id": "File." },
  { "en": "Wadah untuk menyimpan file?", "id": "Folder atau Direktori." },
  { "en": "Penanda jenis file?", "id": "Ekstensi file (.txt, .jpg)." },
  { "en": "Proses pemasangan software ke komputer?", "id": "Instalasi (Installation)." },
  { "en": "Proses pelepasan software dari komputer?", "id": "Uninstallation." },
  { "en": "Versi software ditandai dengan?", "id": "Nomor versi (misal: v1.2)." },
  { "en": "Pembaruan kecil untuk perbaiki bug?", "id": "Patch." },
  { "en": "Pembaruan besar dengan fitur baru?", "id": "Update atau Upgrade." },
  { "en": "Perjanjian lisensi pengguna akhir?", "id": "EULA." },
  { "en": "Kepanjangan EULA?", "id": "End-User License Agreement." },
  { "en": "Software yang berjalan di web browser?", "id": "Aplikasi Web (Web App)." },
  { "en": "Software yang diinstal di perangkat mobile?", "id": "Aplikasi Mobile (Mobile App)." },
  { "en": "Toko aplikasi untuk Android?", "id": "Google Play Store." },
  { "en": "Toko aplikasi untuk iOS?", "id": "Apple App Store." },
  { "en": "Software yang dirancang khusus?", "id": "Custom software." },
  { "en": "Software siap pakai?", "id": "Off-the-shelf software." },
  { "en": "Kerangka kerja untuk membuat software?", "id": "Framework." },
  { "en": "Kumpulan kode siap pakai?", "id": "Library (Pustaka)." },
  { "en": "Antarmuka penghubung antar software?", "id": "API (Application Programming Interface)." },
  { "en": "Siklus hidup pengembangan software?", "id": "SDLC." },
  { "en": "Kepanjangan SDLC?", "id": "Software Development Life Cycle." },
  { "en": "Tahap pertama SDLC?", "id": "Perencanaan (Planning)." },
  { "en": "Tahap setelah perencanaan SDLC?", "id": "Analisis kebutuhan." },
  { "en": "Tahap perancangan arsitektur software?", "id": "Desain (Design)." },
  { "en": "Tahap penulisan kode?", "id": "Implementasi (Implementation/Coding)." },
  { "en": "Tahap pengujian software?", "id": "Testing." },
  { "en": "Tahap perilisan software ke pengguna?", "id": "Deployment." },
  { "en": "Tahap setelah perilisan software?", "id": "Pemeliharaan (Maintenance)." },
  { "en": "Model SDLC air terjun?", "id": "Waterfall Model." },
  { "en": "Model SDLC yang fleksibel dan iteratif?", "id": "Agile Model." },
  { "en": "Pengguna yang mencoba software sebelum rilis?", "id": "Beta Tester." },
  { "en": "Versi awal software untuk testing internal?", "id": "Alpha version." },
  { "en": "Versi software untuk testing publik?", "id": "Beta version." },
  { "en": "Software yang tertanam di hardware?", "id": "Firmware." },
  { "en": "Contoh firmware?", "id": "BIOS atau UEFI." },
  { "en": "Software yang mengelola langganan?", "id": "Subscription model." },
  { "en": "Software yang dibeli sekali pakai?", "id": "Perpetual license." },
  { "en": "Pembajakan perangkat lunak?", "id": "Software piracy." },
  { "en": "Software berbahaya?", "id": "Malware." },
  { "en": "Software pelindung dari malware?", "id": "Antivirus." },
  { "en": "Software untuk kolaborasi tim?", "id": "Collaboration software." },
  { "en": "Contoh collaboration software?", "id": "Slack, Microsoft Teams." },
  { "en": "Software yang berjalan di cloud?", "id": "Cloud software / SaaS." },
  { "en": "Kepanjangan SaaS?", "id": "Software as a Service." },
  { "en": "Kumpulan software dalam satu paket?", "id": "Software suite." },
  { "en": "Contoh software suite?", "id": "Microsoft Office, Adobe Creative Suite." },
  { "en": "Tampilan awal sebuah program?", "id": "Splash screen." },
  { "en": "Paradigma pemrograman berbasis prosedur?", "id": "Pemrograman Prosedural." },
  { "en": "Paradigma pemrograman berbasis objek?", "id": "Object-Oriented Programming (OOP)." },
  { "en": "Konsep 'blueprint' dalam OOP?", "id": "Class." },
  { "en": "Instans nyata dari sebuah class?", "id": "Object." },
  { "en": "Konsep pewarisan sifat dalam OOP?", "id": "Inheritance." },
  { "en": "Class yang mewarisi sifat class lain?", "id": "Child Class / Subclass." },
  { "en": "Class yang diwarisi sifatnya?", "id": "Parent Class / Superclass." },
  { "en": "Konsep 'banyak bentuk' dalam OOP?", "id": "Polymorphism." },
  { "en": "Konsep pembungkusan data dan metode?", "id": "Encapsulation." },
  { "en": "Hak akses dalam enkapsulasi?", "id": "Public, Private, Protected." },
  { "en": "Arsitektur software dalam satu blok besar?", "id": "Monolithic." },
  { "en": "Arsitektur software dari layanan-layanan kecil?", "id": "Microservices." },
  { "en": "Arsitektur dengan penyedia dan peminta layanan?", "id": "Client-Server." },
  { "en": "Pihak yang meminta layanan/data?", "id": "Client." },
  { "en": "Pihak yang menyediakan layanan/data?", "id": "Server." },
  { "en": "Arsitektur tiga lapis?", "id": "Three-tier architecture." },
  { "en": "Tiga lapisan dalam three-tier architecture?", "id": "Presentation, Logic, Data." },
  { "en": "Lapisan antarmuka pengguna?", "id": "Presentation Layer." },
  { "en": "Lapisan pemrosesan bisnis?", "id": "Logic Layer." },
  { "en": "Lapisan penyimpanan data?", "id": "Data Layer." },
  { "en": "Arsitektur tanpa server pusat?", "id": "Peer-to-Peer (P2P)." },
  { "en": "Bagian aplikasi web yang dilihat pengguna?", "id": "Front-End." },
  { "en": "Bagian aplikasi web di sisi server?", "id": "Back-End." },
  { "en": "Bahasa untuk struktur halaman web?", "id": "HTML." },
  { "en": "Bahasa untuk mengatur tampilan web?", "id": "CSS." },
  { "en": "Bahasa untuk interaktivitas web?", "id": "JavaScript." },
  { "en": "Database berbasis tabel?", "id": "SQL Database." },
  { "en": "Database tidak berbasis tabel?", "id": "NoSQL Database." },
  { "en": "Permintaan data dari klien ke server?", "id": "HTTP Request." },
  { "en": "Jawaban data dari server ke klien?", "id": "HTTP Response." },
  { "en": "Tempat penyimpanan nilai dalam program?", "id": "Variabel." },
  { "en": "Jenis data seperti angka, teks?", "id": "Tipe Data." },
  { "en": "Tipe data untuk bilangan bulat?", "id": "Integer." },
  { "en": "Tipe data untuk teks?", "id": "String." },
  { "en": "Tipe data untuk nilai benar/salah?", "id": "Boolean." },
  { "en": "Cara program mengorganisir data?", "id": "Struktur Data." },
  { "en": "Kumpulan data dalam urutan?", "id": "Array atau List." },
  { "en": "Struktur data 'tumpukan' (terakhir masuk, pertama keluar)?", "id": "Stack (LIFO)." },
  { "en": "Struktur data 'antrian' (pertama masuk, pertama keluar)?", "id": "Queue (FIFO)." },
  { "en": "Operasi memasukkan data ke stack?", "id": "Push." },
  { "en": "Operasi mengambil data dari stack?", "id": "Pop." },
  { "en": "Sistem pelacak perubahan kode?", "id": "Version Control System (VCS)." },
  { "en": "VCS paling populer saat ini?", "id": "Git." },
  { "en": "Platform hosting untuk repositori Git?", "id": "GitHub, GitLab, Bitbucket." },
  { "en": "Database proyek dalam Git?", "id": "Repository." },
  { "en": "Proses menyimpan perubahan kode?", "id": "Commit." },
  { "en": "Cabang pengembangan dalam Git?", "id": "Branch." },
  { "en": "Proses menggabungkan dua branch?", "id": "Merge." },
  { "en": "Duplikasi repositori ke lokal?", "id": "Clone." },
  { "en": "Mengirim perubahan lokal ke remote?", "id": "Push." },
  { "en": "Mengambil perubahan dari remote ke lokal?", "id": "Pull." },
  { "en": "Proses pengujian kualitas software?", "id": "Software Testing." },
  { "en": "Tes untuk satu unit/fungsi kecil?", "id": "Unit Testing." },
  { "en": "Tes gabungan beberapa unit/modul?", "id": "Integration Testing." },
  { "en": "Tes seluruh sistem secara menyeluruh?", "id": "System Testing." },
  { "en": "Tes yang dilakukan oleh pengguna akhir?", "id": "User Acceptance Testing (UAT)." },
  { "en": "Skenario pengujian spesifik?", "id": "Test Case." },
  { "en": "Proses peninjauan kode oleh tim?", "id": "Code Review." },
  { "en": "Pengujian otomatis?", "id": "Automated Testing." },
  { "en": "Pengujian manual?", "id": "Manual Testing." },
  { "en": "Format pertukaran data ringan?", "id": "JSON." },
  { "en": "Kepanjangan JSON?", "id": "JavaScript Object Notation." },
  { "en": "Format pertukaran data berbasis tag?", "id": "XML." },
  { "en": "Kepanjangan XML?", "id": "eXtensible Markup Language." },
  { "en": "Gaya arsitektur API populer?", "id": "REST (Representational State Transfer)." },
  { "en": "API yang menggunakan gaya REST?", "id": "RESTful API." },
  { "en": "Lingkungan untuk menjalankan kode?", "id": "Runtime Environment." },
  { "en": "Contoh runtime environment?", "id": "Node.js, Java Virtual Machine." },
  { "en": "Kode yang bisa dijalankan di banyak platform?", "id": "Cross-platform." },
  { "en": "Aplikasi yang dibuat untuk satu platform?", "id": "Aplikasi native." },
  { "en": "Dokumentasi dalam kode?", "id": "Komentar (Comments)." },
  { "en": "Membuat kode lebih mudah dibaca?", "id": "Readability." },
  { "en": "Mengubah struktur kode tanpa mengubah fungsi?", "id": "Refactoring." },
  { "en": "Pola desain yang umum dalam software?", "id": "Design Patterns." },
  { "en": "Software yang mengelola paket/library?", "id": "Package Manager." },
  { "en": "Contoh package manager?", "id": "NPM (JavaScript), Pip (Python)." },
  { "en": "Ketergantungan software pada library lain?", "id": "Dependency." },
  { "en": "Software yang menjalankan software lain?", "id": "Virtual Machine." },
  { "en": "Software untuk membuat virtual machine?", "id": "Hypervisor." },
  { "en": "Teknologi 'wadah' aplikasi ringan?", "id": "Containerization." },
  { "en": "Contoh platform container?", "id": "Docker." },
  { "en": "Proses menjalankan kode di server?", "id": "Server-side scripting." },
  { "en": "Proses menjalankan kode di browser?", "id": "Client-side scripting." },
  { "en": "Menyembunyikan kompleksitas internal?", "id": "Abstraction." },
  { "en": "Software yang mengotomatiskan proses build?", "id": "Build Automation Tool." },
  { "en": "Contoh build tool?", "id": "Maven, Gradle, Webpack." },
  { "en": "Integrasi dan pengiriman berkelanjutan?", "id": "CI/CD." },
  { "en": "Kepanjangan CI?", "id": "Continuous Integration." },
  { "en": "Kepanjangan CD?", "id": "Continuous Delivery/Deployment." },
  { "en": "Praktik menulis tes sebelum kode?", "id": "Test-Driven Development (TDD)." },
  { "en": "Versi stabil dari sebuah software?", "id": "Stable release." },
  { "en": "Unit eksekusi terkecil dalam OS?", "id": "Thread." },
  { "en": "Program yang sedang berjalan di memori?", "id": "Proses (Process)." },
  { "en": "Satu proses bisa punya banyak?", "id": "Thread." },
  { "en": "Menjalankan banyak tugas seolah bersamaan?", "id": "Concurrency." },
  { "en": "Menjalankan banyak tugas benar-benar bersamaan?", "id": "Parallelism." },
  { "en": "Bagian OS yang mengatur jadwal eksekusi?", "id": "Scheduler." },
  { "en": "Memori ilusi yang lebih besar dari RAM?", "id": "Virtual Memory." },
  { "en": "Teknik membagi memori menjadi blok-blok?", "id": "Paging." },
  { "en": "Blok data pada memori virtual?", "id": "Page." },
  { "en": "Blok data pada memori fisik?", "id": "Frame." },
  { "en": "Error saat data tidak ada di RAM?", "id": "Page Fault." },
  { "en": "Jembatan antara aplikasi dan kernel OS?", "id": "System Call (Syscall)." },
  { "en": "Komunikasi antar proses?", "id": "IPC (Inter-Process Communication)." },
  { "en": "Contoh IPC?", "id": "Pipes, Sockets, Shared Memory." },
  { "en": "Prinsip desain 'jangan ulangi dirimu'?", "id": "DRY (Don't Repeat Yourself)." },
  { "en": "Prinsip desain 'buat tetap sederhana'?", "id": "KISS (Keep It Simple, Stupid)." },
  { "en": "Lima prinsip desain OOP?", "id": "SOLID." },
  { "en": "Biaya implisit dari pengerjaan ulang kode?", "id": "Technical Debt." },
  { "en": "Metodologi Agile dengan iterasi waktu tetap?", "id": "Scrum." },
  { "en": "Satu iterasi dalam Scrum disebut?", "id": "Sprint." },
  { "en": "Papan visual untuk manajemen tugas Agile?", "id": "Kanban Board." },
  { "en": "Rapat harian singkat dalam Scrum?", "id": "Daily Stand-up." },
  { "en": "Dokumentasi untuk pengguna akhir?", "id": "User Manual." },
  { "en": "Dokumentasi untuk pengembang lain?", "id": "API Documentation." },
  { "en": "Alat pemeriksa gaya penulisan kode?", "id": "Linter." },
  { "en": "Kualitas kode mudah dimodifikasi?", "id": "Maintainability." },
  { "en": "Kualitas kode menangani beban meningkat?", "id": "Scalability." },
  { "en": "Perintah untuk meminta data dari database?", "id": "Query." },
  { "en": "Bahasa standar untuk query database relasional?", "id": "SQL." },
  { "en": "Perintah SQL untuk mengambil data?", "id": "SELECT." },
  { "en": "Perintah SQL untuk menambah data?", "id": "INSERT." },
  { "en": "Perintah SQL untuk mengubah data?", "id": "UPDATE." },
  { "en": "Perintah SQL untuk menghapus data?", "id": "DELETE." },
  { "en": "Kunci unik untuk setiap baris tabel?", "id": "Primary Key." },
  { "en": "Kunci yang merujuk ke tabel lain?", "id": "Foreign Key." },
  { "en": "Struktur data untuk mempercepat pencarian database?", "id": "Index." },
  { "en": "Satu unit kerja utuh dalam database?", "id": "Transaction." },
  { "en": "Sifat-sifat transaksi database?", "id": "ACID." },
  { "en": "ACID: A singkatan dari?", "id": "Atomicity." },
  { "en": "ACID: C singkatan dari?", "id": "Consistency." },
  { "en": "ACID: I singkatan dari?", "id": "Isolation." },
  { "en": "ACID: D singkatan dari?", "id": "Durability." },
  { "en": "Endpoint komunikasi jaringan?", "id": "Socket." },
  { "en": "Protokol koneksi andal, berurutan?", "id": "TCP." },
  { "en": "Protokol koneksi cepat, tanpa jaminan?", "id": "UDP." },
  { "en": "Nomor identifikasi aplikasi di jaringan?", "id": "Port number." },
  { "en": "Alamat IP untuk mesin sendiri?", "id": "localhost atau 127.0.0.1." },
  { "en": "Proses verifikasi 'siapa kamu'?", "id": "Authentication (Otentikasi)." },
  { "en": "Proses verifikasi 'apa yang boleh kamu lakukan'?", "id": "Authorization (Otorisasi)." },
  { "en": "Menyimpan password dalam bentuk acak?", "id": "Password Hashing." },
  { "en": "Data acak ditambahkan sebelum hashing?", "id": "Salt." },
  { "en": "Tujuan salting pada password?", "id": "Mencegah serangan rainbow table." },
  { "en": "Membersihkan input pengguna untuk keamanan?", "id": "Input Sanitization." },
  { "en": "Tujuan input sanitization?", "id": "Mencegah serangan injection." },
  { "en": "Prinsip keamanan hak akses minimum?", "id": "Principle of Least Privilege." },
  { "en": "Daftar kerentanan keamanan web teratas?", "id": "OWASP Top 10." },
  { "en": "Menyediakan infrastruktur IT sebagai layanan?", "id": "IaaS (Infrastructure as a Service)." },
  { "en": "Menyediakan platform development sebagai layanan?", "id": "PaaS (Platform as a Service)." },
  { "en": "Menyediakan software sebagai layanan?", "id": "SaaS (Software as a Service)." },
  { "en": "Contoh IaaS?", "id": "Amazon EC2, Google Compute Engine." },
  { "en": "Contoh PaaS?", "id": "Heroku, Google App Engine." },
  { "en": "Contoh SaaS?", "id": "Gmail, Dropbox, Salesforce." },
  { "en": "Model komputasi tanpa mengelola server?", "id": "Serverless Computing." },
  { "en": "Nama lain serverless computing?", "id": "Functions as a Service (FaaS)." },
  { "en": "Platform pengembangan dengan sedikit koding?", "id": "Low-code platform." },
  { "en": "Platform pengembangan tanpa koding?", "id": "No-code platform." },
  { "en": "File konfigurasi dependensi JavaScript?", "id": "package.json." },
  { "en": "File konfigurasi dependensi Python?", "id": "requirements.txt." },
  { "en": "Software yang bisa menangani banyak pengguna?", "id": "Multi-tenant software." },
  { "en": "Software untuk satu pengguna/organisasi?", "id": "Single-tenant software." },
  { "en": "Proses membuat software bisa banyak bahasa?", "id": "Internationalization (i18n)." },
  { "en": "Proses adaptasi software ke bahasa lokal?", "id": "Localization (l10n)." },
  { "en": "Kemudahan software diakses penyandang disabilitas?", "id": "Accessibility (a11y)." },
  { "en": "Teks alternatif untuk gambar di web?", "id": "Alt text." },
  { "en": "Standar aksesibilitas konten web?", "id": "WCAG." },
  { "en": "Lisensi yang memperbolehkan modifikasi & distribusi?", "id": "Lisensi open source." },
  { "en": "Contoh lisensi open source?", "id": "MIT, GPL, Apache." },
  { "en": "Hak cipta eksklusif untuk pembuat?", "id": "Copyright." },
  { "en": "Karya yang bebas digunakan publik?", "id": "Public Domain." },
  { "en": "Program kecil untuk tugas spesifik?", "id": "Script." },
  { "en": "Bahasa yang sering digunakan untuk scripting?", "id": "Python, Bash, PowerShell." },
  { "en": "Menjalankan script dari baris perintah?", "id": "Command-line scripting." },
  { "en": "Otomatisasi tugas-tugas berulang?", "id": "Automation." },
  { "en": "Memori cache di level software?", "id": "Software caching." },
  { "en": "Tujuan software caching?", "id": "Mempercepat akses data." },
  { "en": "Menyimpan hasil query database di cache?", "id": "Database query caching." },
  { "en": "Representasi teks dari sebuah objek?", "id": "Serialization." },
  { "en": "Contoh format serialisasi?", "id": "JSON, XML, Pickle." },
  { "en": "Proses kebalikan dari serialisasi?", "id": "Deserialization." },
  { "en": "Bahasa markup ringan?", "id": "Markdown." },
  { "en": "Perangkat lunak gratis dan open source?", "id": "FOSS (Free and Open Source Software)." },
  { "en": "Lingkungan pengembangan terintegrasi?", "id": "IDE (Integrated Development Environment)." },
  { "en": "Contoh IDE populer?", "id": "Visual Studio Code, IntelliJ." },
  { "en": "Editor teks untuk programmer?", "id": "Code Editor." },
  { "en": "Fitur IDE yang melengkapi kode otomatis?", "id": "Code completion / IntelliSense." },
  { "en": "Alat untuk menjalankan debugger?", "id": "Debugger." },
  { "en": "Titik henti dalam proses debugging?", "id": "Breakpoint." },
  { "en": "Metode HTTP untuk membuat sumber daya baru?", "id": "POST." },
  { "en": "Metode HTTP untuk memperbarui sumber daya?", "id": "PUT atau PATCH." },
  { "en": "Perbedaan PUT dan POST?", "id": "PUT bersifat idempoten." },
  { "en": "Metode HTTP untuk menghapus sumber daya?", "id": "DELETE." },
  { "en": "Keluarga kode status HTTP sukses?", "id": "2xx (misal: 200 OK)." },
  { "en": "Keluarga kode status HTTP pengalihan?", "id": "3xx (misal: 301, 302)." },
  { "en": "Keluarga kode status HTTP eror klien?", "id": "4xx (misal: 404 Not Found)." },
  { "en": "Keluarga kode status HTTP eror server?", "id": "5xx (misal: 500 Internal Server Error)." },
  { "en": "Penyimpanan data kecil di sisi klien?", "id": "Cookie." },
  { "en": "Penyimpanan data sesi di sisi server?", "id": "Session." },
  { "en": "Penyimpanan data lebih besar di browser?", "id": "Local Storage." },
  { "en": "Protokol komunikasi dua arah real-time?", "id": "WebSocket." },
  { "en": "Sistem nama domain untuk internet?", "id": "DNS (Domain Name System)." },
  { "en": "Jaringan pengiriman konten terdistribusi?", "id": "CDN (Content Delivery Network)." },
  { "en": "Tujuan utama CDN?", "id": "Mempercepat pengiriman konten global." },
  { "en": "Filosofi kolaborasi tim development dan operations?", "id": "DevOps." },
  { "en": "Alur kerja otomatis CI/CD?", "id": "CI/CD Pipeline." },
  { "en": "Tahap build dalam pipeline?", "id": "Kompilasi kode menjadi artefak." },
  { "en": "Tahap deploy dalam pipeline?", "id": "Merilis aplikasi ke server." },
  { "en": "Mengelola infrastruktur menggunakan kode?", "id": "Infrastructure as Code (IaC)." },
  { "en": "Contoh alat IaC?", "id": "Terraform, Ansible." },
  { "en": "Alat orkestrasi container paling populer?", "id": "Kubernetes (K8s)." },
  { "en": "Unit terkecil yang bisa di-deploy di Kubernetes?", "id": "Pod." },
  { "en": "Satu pod bisa berisi?", "id": "Satu atau lebih container." },
  { "en": "Cara mengekspos aplikasi di Kubernetes?", "id": "Service." },
  { "en": "Gerbang tunggal untuk semua permintaan API?", "id": "API Gateway." },
  { "en": "Lapisan infrastruktur untuk komunikasi microservice?", "id": "Service Mesh." },
  { "en": "Konsep kemampuan sistem untuk dipantau?", "id": "Observability." },
  { "en": "Tiga pilar observability?", "id": "Logs, Metrics, Traces." },
  { "en": "Catatan kejadian dari aplikasi?", "id": "Logs." },
  { "en": "Data numerik yang diukur dari waktu ke waktu?", "id": "Metrics." },
  { "en": "Jejak permintaan melewati berbagai layanan?", "id": "Traces." },
  { "en": "Gudang data untuk analisis bisnis?", "id": "Data Warehouse." },
  { "en": "Penyimpanan data mentah dalam skala besar?", "id": "Data Lake." },
  { "en": "Proses memindahkan data ke data warehouse?", "id": "ETL." },
  { "en": "Kepanjangan ETL?", "id": "Extract, Transform, Load." },
  { "en": "Teknik memetakan objek ke tabel database?", "id": "ORM (Object-Relational Mapping)." },
  { "en": "Keuntungan menggunakan ORM?", "id": "Menulis query dalam kode." },
  { "en": "Strategi cache 'tulis tembus'?", "id": "Write-through cache." },
  { "en": "Strategi cache 'tulis kembali'?", "id": "Write-back cache." },
  { "en": "Menambah sumber daya pada server yang ada?", "id": "Scaling Vertikal (Scale up)." },
  { "en": "Menambah jumlah server?", "id": "Scaling Horizontal (Scale out)." },
  { "en": "Perangkat lunak pendistribusi trafik?", "id": "Load Balancer." },
  { "en": "Waktu tunda dalam pemrosesan?", "id": "Latency." },
  { "en": "Jumlah pekerjaan yang diselesaikan per waktu?", "id": "Throughput." },
  { "en": "Aplikasi yang tidak menyimpan data klien?", "id": "Aplikasi stateless." },
  { "en": "Keuntungan aplikasi stateless?", "id": "Mudah di-scaling secara horizontal." },
  { "en": "Pemrograman yang tidak memblokir eksekusi?", "id": "Pemrograman Asinkron (Asynchronous)." },
  { "en": "Objek representasi hasil operasi asinkron?", "id": "Promise." },
  { "en": "Sintaksis untuk menyederhanakan promise?", "id": "async/await." },
  { "en": "Fungsi yang dilewatkan sebagai argumen?", "id": "Callback function." },
  { "en": "Kondisi saat urutan eksekusi tak terduga?", "id": "Race Condition." },
  { "en": "Mekanisme pengunci untuk mencegah race condition?", "id": "Mutex atau Lock." },
  { "en": "Pola desain 'hanya satu instans'?", "id": "Singleton Pattern." },
  { "en": "Pola desain 'membuat objek tanpa spesifikasi class'?", "id": "Factory Pattern." },
  { "en": "Pola desain 'notifikasi perubahan state'?", "id": "Observer Pattern." },
  { "en": "Kegagalan membebaskan memori yang tak terpakai?", "id": "Memory Leak." },
  { "en": "Proses pembersihan memori otomatis?", "id": "Garbage Collection." },
  { "en": "Bahasa dengan garbage collection?", "id": "Java, Python, C#." },
  { "en": "Bahasa tanpa garbage collection?", "id": "C, C++." },
  { "en": "Pointer yang tidak menunjuk ke alamat valid?", "id": "Dangling pointer." },
  { "en": "Proses menerjemahkan alamat jaringan?", "id": "NAT (Network Address Translation)." },
  { "en": "Mekanisme server mengirim update ke klien?", "id": "Push notification." },
  { "en": "Permintaan data periodik dari klien?", "id": "Polling." },
  { "en": "Polling dengan koneksi yang lama terbuka?", "id": "Long polling." },
  { "en": "Tanda tangan digital untuk kode?", "id": "Code Signing." },
  { "en": "Tujuan code signing?", "id": "Memverifikasi pembuat dan integritas." },
  { "en": "Proses mengubah kode agar sulit dibaca?", "id": "Obfuscation." },
  { "en": "Proses memperkecil ukuran file kode?", "id": "Minification." },
  { "en": "Menggabungkan beberapa file menjadi satu?", "id": "Bundling." },
  { "en": "Alat untuk bundling JavaScript?", "id": "Webpack, Parcel." },
  { "en": "Server web paling populer?", "id": "Apache, Nginx." },
  { "en": "Penyimpanan data sementara di memori?", "id": "In-memory cache." },
  { "en": "Contoh in-memory cache?", "id": "Redis, Memcached." },
  { "en": "File teks berisi instruksi dependensi?", "id": "Manifest file." },
  { "en": "Kontrak yang mendefinisikan sebuah API?", "id": "API Specification." },
  { "en": "Standar spesifikasi untuk REST API?", "id": "OpenAPI (Swagger)." },
  { "en": "Gaya arsitektur API berbasis query?", "id": "GraphQL." },
  { "en": "Siapa pengembang GraphQL?", "id": "Facebook." },
  { "en": "Kemampuan sistem untuk pulih dari kegagalan?", "id": "Fault Tolerance." },
  { "en": "Memiliki komponen cadangan?", "id": "Redundancy." },
  { "en": "Kemampuan sistem untuk tetap berjalan?", "id": "High Availability." },
  { "en": "Ukuran ketersediaan sistem?", "id": "Uptime (e.g., 99.9%)." },
  { "en": "Proses kembali ke versi sebelumnya?", "id": "Rollback." },
  { "en": "Pengujian A/B?", "id": "Membandingkan dua versi halaman." },
  { "en": "Lingkungan yang identik dengan produksi?", "id": "Staging environment." },
  { "en": "Lingkungan untuk pengembang?", "id": "Development environment." },
  { "en": "Lingkungan untuk pengguna akhir?", "id": "Production environment." },
  { "en": "Seni dan ilmu membuat software?", "id": "Rekayasa Perangkat Lunak." },
  { "en": "Karakteristik metode Agile?", "id": "Inkremental dan iteratif." },
  { "en": "Karakteristik metode Waterfall?", "id": "Linear dan sekuensial." },
  { "en": "Pengujian 'kotak hitam'?", "id": "Black-box testing." },
  { "en": "Pengujian 'kotak putih'?", "id": "White-box testing." },
  { "en": "Black-box testing menguji?", "id": "Fungsionalitas tanpa lihat kode." },
  { "en": "White-box testing menguji?", "id": "Struktur internal kode." },
  { "en": "Pengujian regresi?", "id": "Memastikan fitur lama tidak rusak." },
  { "en": "Analisis efisiensi algoritma?", "id": "Analisis Kompleksitas (Big O)." },
  { "en": "Kompleksitas waktu konstan?", "id": "O(1)." },
  { "en": "Kompleksitas waktu linear?", "id": "O(n)." },
  { "en": "Kompleksitas waktu logaritmik?", "id": "O(log n)." },
  { "en": "Kompleksitas waktu kuadratik?", "id": "O(n^2)." },
  { "en": "Algoritma pencarian paling dasar?", "id": "Linear Search." },
  { "en": "Algoritma pencarian pada data terurut?", "id": "Binary Search." },
  { "en": "Algoritma pengurutan paling sederhana?", "id": "Bubble Sort." },
  { "en": "Algoritma pengurutan 'divide and conquer'?", "id": "Merge Sort, Quick Sort." },
  { "en": "Struktur data untuk pencarian cepat?", "id": "Hash Table." },
  { "en": "Masalah apakah program akan berhenti?", "id": "Halting Problem." },
  { "en": "Pola untuk pencocokan teks?", "id": "Regular Expression (Regex)." },
  { "en": "Tiga pilar keamanan informasi?", "id": "CIA Triad." },
  { "en": "CIA: 'Confidentiality' artinya?", "id": "Kerahasiaan data." },
  { "en": "CIA: 'Integrity' artinya?", "id": "Keaslian data." },
  { "en": "CIA: 'Availability' artinya?", "id": "Ketersediaan data/layanan." },
  { "en": "Proses identifikasi ancaman pada desain?", "id": "Threat Modeling." },
  { "en": "Simulasi serangan untuk mencari kerentanan?", "id": "Penetration Testing (Pentest)." },
  { "en": "Analisis keamanan kode sumber?", "id": "SAST (Static Application Security Testing)." },
  { "en": "Analisis keamanan aplikasi saat berjalan?", "id": "DAST (Dynamic Application Security Testing)." },
  { "en": "Memindai pustaka/library untuk kerentanan?", "id": "Dependency Scanning." },
  { "en": "Manajemen kredensial sensitif?", "id": "Secrets Management." },
  { "en": "Contoh 'secret' dalam software?", "id": "API key, password database." },
  { "en": "Perbedaan utama OAuth dan OpenID Connect?", "id": "OpenID Connect untuk otentikasi." },
  { "en": "OAuth digunakan untuk?", "id": "Otorisasi (izin akses)." },
  { "en": "Strategi keamanan berlapis-lapis?", "id": "Defense in Depth." },
  { "en": "Bagian depan compiler?", "id": "Front-end." },
  { "en": "Bagian belakang compiler?", "id": "Back-end." },
  { "en": "Tugas compiler front-end?", "id": "Analisis kode sumber (parsing)." },
  { "en": "Tugas compiler back-end?", "id": "Optimasi dan generasi kode mesin." },
  { "en": "Tahap pemecahan kode menjadi token?", "id": "Lexical Analysis (Lexing)." },
  { "en": "Tahap analisis struktur gramatikal kode?", "id": "Parsing." },
  { "en": "Representasi struktur pohon dari kode?", "id": "AST (Abstract Syntax Tree)." },
  { "en": "Representasi kode perantara?", "id": "Intermediate Representation (IR)." },
  { "en": "Kompilasi saat program dijalankan?", "id": "Just-In-Time (JIT) Compilation." },
  { "en": "Dua programmer bekerja di satu komputer?", "id": "Pair Programming." },
  { "en": "Satu tim bekerja di satu komputer?", "id": "Mob Programming." },
  { "en": "Analisis insiden tanpa menyalahkan?", "id": "Blameless Postmortem." },
  { "en": "Dokumen rancangan teknis software?", "id": "Technical Specification / Design Doc." },
  { "en": "Konsep kepemilikan kode dalam tim?", "id": "Code Ownership." },
  { "en": "Pengkodean karakter 7-bit standar?", "id": "ASCII." },
  { "en": "Pengkodean karakter yang mendukung semua bahasa?", "id": "Unicode (misal: UTF-8)." },
  { "en": "Format serialisasi data biner dari Google?", "id": "Protocol Buffers (Protobuf)." },
  { "en": "Keuntungan format biner?", "id": "Lebih kecil dan cepat." },
  { "en": "Definisi struktur data?", "id": "Schema." },
  { "en": "Algoritma garbage collection dasar?", "id": "Mark-and-Sweep." },
  { "en": "Konsep mencegah eror terkait memori?", "id": "Memory Safety." },
  { "en": "Operasi API yang aman diulang?", "id": "Idempotent." },
  { "en": "Contoh metode HTTP idempotent?", "id": "GET, PUT, DELETE." },
  { "en": "Teorema sistem terdistribusi (CAP)?", "id": "Consistency, Availability, Partition Tolerance." },
  { "en": "Sebuah sistem hanya bisa memilih dua dari?", "id": "Tiga properti CAP." },
  { "en": "Perangkat lunak untuk menjalankan OS lain?", "id": "Virtualization software." },
  { "en": "OS yang berjalan di atas hypervisor?", "id": "Guest OS." },
  { "en": "OS yang menjadi host hypervisor?", "id": "Host OS." },
  { "en": "Hypervisor yang berjalan langsung di hardware?", "id": "Type 1 Hypervisor." },
  { "en": "Hypervisor yang berjalan di atas OS?", "id": "Type 2 Hypervisor." },
  { "en": "Isolasi proses di level kernel?", "id": "Container." },
  { "en": "Keuntungan container dibanding VM?", "id": "Lebih ringan dan cepat." },
  { "en": "Kode yang tidak memiliki efek samping?", "id": "Pure function." },
  { "en": "Paradigma pemrograman berbasis fungsi murni?", "id": "Pemrograman Fungsional." },
  { "en": "Kondisi saat proses saling menunggu?", "id": "Deadlock." },
  { "en": "Kondisi saat proses kekurangan sumber daya?", "id": "Starvation." },
  { "en": "Proses sinkronisasi thread?", "id": "Thread synchronization." },
  { "en": "Mekanisme pengunci untuk thread?", "id": "Mutex atau Semaphore." },
  { "en": "Memori yang di-cache di CPU?", "id": "CPU Cache." },
  { "en": "Masalah konsistensi data di cache?", "id": "Cache coherence problem." },
  { "en": "Eksekusi perintah di luar urutan?", "id": "Out-of-order execution." },
  { "en": "Prediksi cabang program oleh CPU?", "id": "Branch prediction." },
  { "en": "Pengujian performa software?", "id": "Performance testing." },
  { "en": "Pengujian dengan beban tinggi?", "id": "Load testing." },
  { "en": "Pengujian ketahanan sistem?", "id": "Stress testing." },
  { "en": "Perangkat lunak 'perantara'?", "id": "Middleware." },
  { "en": "Contoh middleware?", "id": "Message queue, application server." },
  { "en": "Antrian pesan antar aplikasi?", "id": "Message Queue." },
  { "en": "Pola komunikasi 'publish-subscribe'?", "id": "Pub/Sub." },
  { "en": "Server yang menangani logika aplikasi?", "id": "Application Server." },
  { "en": "Server yang hanya menyajikan file statis?", "id": "Web Server." },
  { "en": "Software yang mengelola sumber daya cluster?", "id": "Cluster manager." },
  { "en": "Contoh cluster manager?", "id": "Kubernetes, Apache Mesos." },
  { "en": "Membuat software andal dan skalabel?", "id": "Site Reliability Engineering (SRE)." },
  { "en": "Pola desain untuk membuat objek?", "id": "Creational patterns." },
  { "en": "Pola desain untuk komposisi class/objek?", "id": "Structural patterns." },
  { "en": "Pola desain untuk interaksi objek?", "id": "Behavioral patterns." },
  { "en": "Pola Singleton termasuk jenis apa?", "id": "Creational pattern." },
  { "en": "Pola Observer termasuk jenis apa?", "id": "Behavioral pattern." },
  { "en": "Antarmuka yang menyederhanakan sistem kompleks?", "id": "Facade Pattern." },
  { "en": "Memisahkan algoritma dari objek?", "id": "Strategy Pattern." },
  { "en": "Pola untuk iterasi koleksi data?", "id": "Iterator Pattern." },
  { "en": "Membuat antarmuka yang tidak kompatibel bekerja sama?", "id": "Adapter Pattern." },
  { "en": "Mengurangi ketergantungan dengan inversi kontrol?", "id": "Dependency Injection." },
  { "en": "File yang berisi log perubahan?", "id": "Changelog." },
  { "en": "Penomoran versi software (Mayor.Minor.Patch)?", "id": "Semantic Versioning." },
  { "en": "Perubahan 'Patch' pada semantic versioning?", "id": "Perbaikan bug." },
  { "en": "Perubahan 'Minor' pada semantic versioning?", "id": "Penambahan fitur (backward-compatible)." },
  { "en": "Perubahan 'Mayor' pada semantic versioning?", "id": "Perubahan yang merusak kompatibilitas." },
  { "en": "Software yang sudah tidak didukung?", "id": "End-of-life (EOL) software." },
  { "en": "Tujuan utama sebuah OS?", "id": "Menjembatani pengguna dan hardware." },
  { "en": "Compiler menghasilkan apa?", "id": "File eksekusi (executable)." },
  { "en": "Interpreter menerjemahkan kapan?", "id": "Saat program dijalankan." },
  { "en": "Front-end berjalan di mana?", "id": "Browser pengguna (client-side)." },
  { "en": "Back-end berjalan di mana?", "id": "Server (server-side)." },
  { "en": "Logika penyelesaian masalah?", "id": "Algoritma." },
  { "en": "Wadah penyimpanan dan pengorganisasian data?", "id": "Struktur Data." },
  { "en": "Perbedaan utama algoritma dan struktur data?", "id": "Langkah vs wadah." },
  { "en": "Siapa programmer komputer pertama di dunia?", "id": "Ada Lovelace." },
  { "en": "Siapa yang mempopulerkan istilah 'bug'?", "id": "Grace Hopper." },
  { "en": "Pencipta kernel Linux?", "id": "Linus Torvalds." },
  { "en": "Pendiri gerakan 'free software'?", "id": "Richard Stallman." },
  { "en": "Bahasa pemrograman tingkat tinggi pertama?", "id": "Fortran." },
  { "en": "Jaringan cikal bakal internet?", "id": "ARPANET." },
  { "en": "Siapa yang bertanggung jawab membangun produk?", "id": "Software Engineer." },
  { "en": "Siapa yang bertanggung jawab atas kualitas produk?", "id": "QA Engineer." },
  { "en": "Siapa yang mengelola alur rilis (CI/CD)?", "id": "DevOps Engineer." },
  { "en": "Siapa yang mendefinisikan visi dan fitur produk?", "id": "Product Manager." },
  { "en": "Siapa yang merancang tampilan dan nuansa produk?", "id": "UI/UX Designer." },
  { "en": "Siapa yang memfasilitasi proses Scrum?", "id": "Scrum Master." },
  { "en": "Metodologi Agile dengan praktik-praktik ekstrim?", "id": "Extreme Programming (XP)." },
  { "en": "Prinsip XP dalam pemrograman?", "id": "Pair programming, TDD." },
  { "en": "Metodologi software yang fokus pada eliminasi pemborosan?", "id": "Lean Software Development." },
  { "en": "Empat nilai utama dalam Agile Manifesto?", "id": "Individu, software, kolaborasi, respons." },
  { "en": "Fakta mentah yang belum diolah?", "id": "Data." },
  { "en": "Data yang sudah diolah dan bermakna?", "id": "Informasi." },
  { "en": "Data tentang data?", "id": "Metadata." },
  { "en": "Contoh metadata file gambar?", "id": "Lokasi, tanggal, jenis kamera." },
  { "en": "Format file ditentukan oleh?", "id": "Struktur dan ekstensinya." },
  { "en": "Urutan byte dalam memori?", "id": "Endianness." },
  { "en": "Menyembunyikan detail kompleks?", "id": "Abstraksi." },
  { "en": "Memecah program menjadi bagian independen?", "id": "Modularitas." },
  { "en": "Keuntungan modularitas?", "id": "Mudah dipelihara dan diuji." },
  { "en": "API sebagai 'perjanjian' antar software?", "id": "Kontrak API." },
  { "en": "Kode yang mudah dipahami?", "id": "Clean Code." },
  { "en": "Sistem operasi untuk sistem tertanam?", "id": "RTOS (Real-Time Operating System)." },
  { "en": "Karakteristik utama RTOS?", "id": "Respons waktu yang dapat diprediksi." },
  { "en": "Software yang dibuat untuk hardware spesifik?", "id": "Embedded Software." },
  { "en": "Contoh embedded software?", "id": "Software di mobil, TV, AC." },
  { "en": "Variabel yang tersedia di seluruh sistem?", "id": "Environment Variable." },
  { "en": "Input yang diberikan saat program dijalankan?", "id": "Command-line arguments." },
  { "en": "Aplikasi tanpa antarmuka grafis?", "id": "Headless application." },
  { "en": "Sistem lama yang masih digunakan?", "id": "Legacy system." },
  { "en": "Kode yang sangat berantakan dan sulit dipahami?", "id": "Spaghetti code." },
  { "en": "Proses verifikasi apakah software sudah benar?", "id": "Validation." },
  { "en": "Proses verifikasi apakah software dibuat benar?", "id": "Verification." },
  { "en": "Lisensi yang mengizinkan penggunaan, modifikasi, distribusi?", "id": "Lisensi Open Source." },
  { "en": "Lisensi yang membatasi penggunaan?", "id": "Lisensi Proprietary." },
  { "en": "Software gratis disebut?", "id": "Freeware." },
  { "en": "Software bayar setelah masa coba?", "id": "Shareware." },
  { "en": "Apa itu 'software bloat'?", "id": "Software terlalu besar dan lambat." },
  { "en": "Pola arsitektur MVC?", "id": "Model-View-Controller." },
  { "en": "MVC: 'Model' adalah?", "id": "Data dan logika bisnis." },
  { "en": "MVC: 'View' adalah?", "id": "Tampilan antarmuka pengguna (UI)." },
  { "en": "MVC: 'Controller' adalah?", "id": "Penghubung Model dan View." },
  { "en": "Kerangka kerja Front-End populer?", "id": "React, Angular, Vue." },
  { "en": "Kerangka kerja Back-End populer?", "id": "Express.js, Django, Laravel." },
  { "en": "Lingkungan runtime untuk JavaScript di server?", "id": "Node.js." },
  { "en": "Tipe data yang tidak bisa diubah?", "id": "Immutable." },
  { "en": "Keuntungan tipe data immutable?", "id": "Lebih aman untuk concurrency." },
  { "en": "Fungsi yang tidak mengubah state luar?", "id": "Pure function." },
  { "en": "Program yang ditulis dalam satu file?", "id": "Script." },
  { "en": "Perbedaan script dan aplikasi?", "id": "Skala dan kompleksitas." },
  { "en": "Tingkat hak akses pengguna?", "id": "User permissions." },
  { "en": "Pengguna dengan hak akses tertinggi?", "id": "Administrator, root, superuser." },
  { "en": "Menjalankan program dengan hak akses admin?", "id": "Run as administrator." },
  { "en": "Penyimpanan data sementara?", "id": "Cache." },
  { "en": "Menghapus cache berarti?", "id": "Menghapus data sementara." },
  { "en": "Kompatibilitas mundur?", "id": "Backward compatibility." },
  { "en": "Software baru bisa membaca file lama?", "id": "Contoh backward compatibility." },
  { "en": "Proses 'downgrade' software?", "id": "Kembali ke versi sebelumnya." },
  { "en": "Standar pengkodean karakter paling dasar?", "id": "ASCII." },
  { "en": "Standar pengkodean karakter modern?", "id": "Unicode (UTF-8)." },
  { "en": "Dua jenis utama lisensi open source?", "id": "Permissive dan Copyleft." },
  { "en": "Lisensi 'permissive' seperti?", "id": "MIT, Apache." },
  { "en": "Lisensi 'copyleft' seperti?", "id": "GPL." },
  { "en": "Syarat utama lisensi GPL?", "id": "Karya turunan harus GPL juga." },
  { "en": "Hak eksklusif pencipta karya?", "id": "Copyright." },
  { "en": "Melepaskan semua hak cipta?", "id": "Public Domain." },
  { "en": "Pengujian pada lingkungan pengguna nyata?", "id": "Beta testing." },
  { "en": "Pembaruan otomatis di latar belakang?", "id": "Silent update." },
  { "en": "File teks petunjuk instalasi/penggunaan?", "id": "README file." },
  { "en": "Perangkat lunak sebagai hobi?", "id": "Hobby project." },
  { "en": "Software yang dikelola oleh komunitas?", "id": "Community-driven software." },
  { "en": "Pengembangan software berbasis komponen?", "id": "Component-Based Development." },
  { "en": "Bagian software yang dapat digunakan kembali?", "id": "Reusable component." },
  { "en": "Dampak perubahan pada sistem?", "id": "Impact analysis." },
  { "en": "Analisis akar penyebab masalah?", "id": "Root Cause Analysis (RCA)." },
  { "en": "Seni menyembunyikan informasi?", "id": "Steganography." },
  { "en": "Seni memecahkan kode rahasia?", "id": "Cryptography." },
  { "en": "Nomor identifikasi unik global?", "id": "UUID / GUID." },
  { "en": "Apa itu 'software stack'?", "id": "Kumpulan teknologi software." },
  { "en": "Contoh 'software stack'?", "id": "LAMP, MEAN, MERN." },
  { "en": "Stack LAMP adalah?", "id": "Linux, Apache, MySQL, PHP." },
  { "en": "Proses pensiun sebuah software?", "id": "Deprecation." },
  { "en": "Fitur yang ditandai 'deprecated'?", "id": "Akan dihapus di masa depan." }




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
