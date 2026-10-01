<!DOCTYPE html>
<html lang="hi">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>डिजिटल सेवा - लाइव कंट्रोल एवं नागरिक सुविधा पोर्टल</title>
    <!-- FontAwesome Icons -->
    <link href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css" rel="stylesheet">
    <!-- Google Fonts -->
    <link href="https://fonts.googleapis.com/css2?family=Poppins:wght@400;500;600;700&display=swap" rel="stylesheet">
    
    <!-- Firebase SDKs -->
    <script src="https://www.gstatic.com/firebasejs/8.10.1/firebase-app.js"></script>
    <script src="https://www.gstatic.com/firebasejs/8.10.1/firebase-database.js"></script>

    <style>
        :root {
            --primary: #0d6efd;
            --primary-dark: #0b5ed7;
            --secondary: #002b5b;
            --accent: #ff7b00;
            --success: #198754;
            --danger: #dc3545;
            --bg-light: #f4f6f9;
            --text-dark: #212529;
            --shadow: 0 5px 20px rgba(0,0,0,0.08);
        }

        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
            font-family: 'Poppins', sans-serif;
        }

        body {
            background-color: var(--bg-light);
            color: var(--text-dark);
        }

        /* Top Header */
        header {
            background: linear-gradient(135deg, var(--secondary), var(--primary));
            color: white;
            padding: 15px 5%;
            display: flex;
            justify-content: space-between;
            align-items: center;
            box-shadow: 0 4px 12px rgba(0,0,0,0.15);
            position: sticky;
            top: 0;
            z-index: 1000;
        }

        .logo-area {
            display: flex;
            align-items: center;
            gap: 12px;
        }

        .logo-area i {
            font-size: 28px;
            color: var(--accent);
        }

        .logo-area h1 {
            font-size: 22px;
            font-weight: 700;
        }

        .status-badge {
            background: #22c55e;
            color: white;
            padding: 6px 16px;
            border-radius: 20px;
            font-size: 13px;
            font-weight: 600;
            display: flex;
            align-items: center;
            gap: 8px;
        }

        /* Main Container */
        .main-container {
            display: grid;
            grid-template-columns: 340px 1fr;
            gap: 25px;
            padding: 30px 5%;
            max-width: 1600px;
            margin: auto;
        }

        /* Admin Panel Styles */
        .admin-sidebar {
            background: white;
            padding: 25px;
            border-radius: 16px;
            box-shadow: var(--shadow);
            border-top: 5px solid var(--danger);
            height: fit-content;
        }

        .admin-sidebar h3 {
            color: var(--danger);
            font-size: 18px;
            margin-bottom: 5px;
            display: flex;
            align-items: center;
            gap: 10px;
        }

        .admin-subtext {
            font-size: 12px;
            color: #6c757d;
            margin-bottom: 20px;
            border-bottom: 1px dashed #ddd;
            padding-bottom: 10px;
        }

        .toggle-item {
            display: flex;
            justify-content: space-between;
            align-items: center;
            padding: 12px 0;
            border-bottom: 1px solid #f0f0f0;
            font-size: 14px;
            font-weight: 500;
        }

        /* Toggle Switches */
        .switch {
            position: relative;
            display: inline-block;
            width: 46px;
            height: 24px;
        }

        .switch input { opacity: 0; width: 0; height: 0; }

        .slider {
            position: absolute;
            cursor: pointer;
            top: 0; left: 0; right: 0; bottom: 0;
            background-color: #cbd5e1;
            transition: .3s;
            border-radius: 24px;
        }

        .slider:before {
            position: absolute;
            content: "";
            height: 18px;
            width: 18px;
            left: 3px;
            bottom: 3px;
            background-color: white;
            transition: .3s;
            border-radius: 50%;
        }

        input:checked + .slider { background-color: var(--success); }
        input:checked + .slider:before { transform: translateX(22px); }

        /* Client Portal View */
        .client-section {
            background: white;
            padding: 30px;
            border-radius: 16px;
            box-shadow: var(--shadow);
            border-top: 5px solid var(--primary);
        }

        /* Search & Controls Header */
        .portal-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            flex-wrap: wrap;
            gap: 15px;
            margin-bottom: 25px;
        }

        .search-box {
            position: relative;
            width: 100%;
            max-width: 380px;
        }

        .search-box input {
            width: 100%;
            padding: 12px 20px 12px 45px;
            border: 2px solid #e2e8f0;
            border-radius: 30px;
            outline: none;
            font-size: 14px;
            transition: all 0.3s;
        }

        .search-box input:focus {
            border-color: var(--primary);
            box-shadow: 0 0 0 3px rgba(13, 110, 253, 0.15);
        }

        .search-box i {
            position: absolute;
            left: 16px;
            top: 50%;
            transform: translateY(-50%);
            color: #94a3b8;
        }

        /* Filter Tabs */
        .filter-tabs {
            display: flex;
            gap: 10px;
            flex-wrap: wrap;
            margin-bottom: 25px;
        }

        .tab-btn {
            padding: 8px 18px;
            border-radius: 20px;
            border: none;
            background: #f1f5f9;
            color: #475569;
            font-weight: 500;
            font-size: 13px;
            cursor: pointer;
            transition: all 0.3s;
        }

        .tab-btn.active, .tab-btn:hover {
            background: var(--primary);
            color: white;
        }

        /* Services Grid */
        .services-grid {
            display: grid;
            grid-template-columns: repeat(auto-fill, minmax(230px, 1fr));
            gap: 20px;
        }

        .service-card {
            background: #ffffff;
            border: 1px solid #e2e8f0;
            border-radius: 12px;
            padding: 22px 18px;
            text-align: center;
            transition: all 0.3s ease;
            position: relative;
            overflow: hidden;
            cursor: pointer;
        }

        .service-card:hover {
            transform: translateY(-5px);
            box-shadow: 0 10px 25px rgba(0,0,0,0.1);
            border-color: var(--primary);
        }

        .service-card i {
            font-size: 38px;
            color: var(--primary);
            margin-bottom: 12px;
        }

        .service-card h4 {
            font-size: 16px;
            margin-bottom: 6px;
            color: var(--text-dark);
        }

        .service-card p {
            font-size: 12px;
            color: #64748b;
            line-height: 1.5;
        }

        .card-tag {
            position: absolute;
            top: 10px;
            right: 10px;
            background: #e0f2fe;
            color: #0369a1;
            font-size: 10px;
            padding: 3px 8px;
            border-radius: 10px;
            font-weight: 600;
        }

        .hidden { display: none !important; }

        /* Responsive Design */
        @media (max-width: 992px) {
            .main-container {
                grid-template-columns: 1fr;
            }
        }
    </style>
</head>
<body>

<!-- Header -->
<header>
    <div class="logo-area">
        <i class="fa-solid fa-layer-group"></i>
        <div>
            <h1>डिजिटल सेवा पोर्टल</h1>
            <p style="font-size: 11px; opacity: 0.8;">नागरिक एवं बैंकिंग सुविधा केंद्र</p>
        </div>
    </div>
    <div class="status-badge">
        <i class="fa-solid fa-bolt"></i> एडमिन सिंक एक्टिव
    </div>
</header>

<div class="main-container">
    
    <!-- Admin Control Sidebar -->
    <div class="admin-sidebar">
        <h3><i class="fa-solid fa-sliders"></i> एडमिन कंट्रोल पैनल</h3>
        <p class="admin-subtext">यहाँ से ऑन/ऑफ करें, यूजर स्क्रीन पर वही सेवा दिखाई देगी:</p>

        <div class="toggle-item">
            <span>आधार सेवाएँ</span>
            <label class="switch">
                <input type="checkbox" id="chk-aadhaar" checked onchange="updateFirebase('aadhaar', this.checked)">
                <span class="slider"></span>
            </label>
        </div>

        <div class="toggle-item">
            <span>पैन कार्ड सेवाएँ</span>
            <label class="switch">
                <input type="checkbox" id="chk-pan" checked onchange="updateFirebase('pan', this.checked)">
                <span class="slider"></span>
            </label>
        </div>

        <div class="toggle-item">
            <span>समग्र पोर्टल (MP)</span>
            <label class="switch">
                <input type="checkbox" id="chk-samagra" checked onchange="updateFirebase('samagra', this.checked)">
                <span class="slider"></span>
            </label>
        </div>

        <div class="toggle-item">
            <span>आयुष्मान भारत</span>
            <label class="switch">
                <input type="checkbox" id="chk-ayushman" checked onchange="updateFirebase('ayushman', this.checked)">
                <span class="slider"></span>
            </label>
        </div>

        <div class="toggle-item">
            <span>ई-श्रम व संबल योजना</span>
            <label class="switch">
                <input type="checkbox" id="chk-eshram" checked onchange="updateFirebase('eshram', this.checked)">
                <span class="slider"></span>
            </label>
        </div>

        <div class="toggle-item">
            <span>MPOnline / लोक सेवा</span>
            <label class="switch">
                <input type="checkbox" id="chk-mponline" checked onchange="updateFirebase('mponline', this.checked)">
                <span class="slider"></span>
            </label>
        </div>

        <div class="toggle-item">
            <span>ऑल बैंकिंग पोर्टल</span>
            <label class="switch">
                <input type="checkbox" id="chk-banking" checked onchange="updateFirebase('banking', this.checked)">
                <span class="slider"></span>
            </label>
        </div>
    </div>

    <!-- User View Section -->
    <div class="client-section">
        <div class="portal-header">
            <div>
                <h2 style="font-size: 20px;"><i class="fa-solid fa-grid-2"></i> उपलब्ध नागरिक सुविधाएँ</h2>
                <p style="font-size: 13px; color: #64748b;">अपनी सेवा चुनें अथवा सर्च बार में खोजें</p>
            </div>
            
            <!-- Live Search -->
            <div class="search-box">
                <i class="fa-solid fa-magnifying-glass"></i>
                <input type="text" id="searchInput" placeholder="सेवा खोजें (उदा. आधार, पैन, बैंकिंग)..." onkeyup="filterServices()">
            </div>
        </div>

        <!-- Filter Tabs -->
        <div class="filter-tabs">
            <button class="tab-btn active" onclick="filterCategory('all')">सभी (All)</button>
            <button class="tab-btn" onclick="filterCategory('govt')">शासकीय सेवाएँ</button>
            <button class="tab-btn" onclick="filterCategory('scheme')">योजनाएँ</button>
            <button class="tab-btn" onclick="filterCategory('banking')">बैंकिंग</button>
        </div>

        <!-- Services Grid -->
        <div class="services-grid" id="servicesGrid">
            
            <!-- Aadhaar Card -->
            <div class="service-card" id="box-aadhaar" data-category="govt" data-title="आधार सेवाएँ aadhaar ekyc card">
                <span class="card-tag">Govt</span>
                <i class="fa-solid fa-id-card"></i>
                <h4>आधार सेवाएँ</h4>
                <p>डाउनलोड, अपडेट, ई-केवाईसी व स्टेटस</p>
            </div>

            <!-- PAN Card -->
            <div class="service-card" id="box-pan" data-category="govt" data-title="पैन कार्ड pan card apply update">
                <span class="card-tag">Govt</span>
                <i class="fa-solid fa-address-card"></i>
                <h4>पैन कार्ड</h4>
                <p>नया पैन, सुधार व आधार लिंक</p>
            </div>

            <!-- Samagra Portal -->
            <div class="service-card" id="box-samagra" data-category="govt" data-title="समग्र आईडी samagra ekyc mp">
                <span class="card-tag">MP Govt</span>
                <i class="fa-solid fa-users"></i>
                <h4>समग्र पोर्टल</h4>
                <p>परिवार/सदस्य आईडी व eKYC</p>
            </div>

            <!-- Ayushman Card -->
            <div class="service-card" id="box-ayushman" data-category="scheme" data-title="आयुष्मान भारत ayushman card health">
                <span class="card-tag">Scheme</span>
                <i class="fa-solid fa-notes-medical" style="color: #e11d48;"></i>
                <h4>आयुष्मान भारत</h4>
                <p>कार्ड डाउनलोड व पात्रता जांच</p>
            </div>

            <!-- E-Shram & Sambal -->
            <div class="service-card" id="box-eshram" data-category="scheme" data-title="ई-श्रम संबल eshram sambal card">
                <span class="card-tag">Scheme</span>
                <i class="fa-solid fa-hands-holding-child" style="color: #d97706;"></i>
                <h4>ई-श्रम व संबल</h4>
                <p>श्रमिक पंजीयन व योजना आवेदन</p>
            </div>

            <!-- MPOnline & Lok Sewa -->
            <div class="service-card" id="box-mponline" data-category="govt" data-title="mponline mp online लोक सेवा प्रमाण पत्र">
                <span class="card-tag">Portal</span>
                <i class="fa-solid fa-landmark"></i>
                <h4>MPOnline / लोक सेवा</h4>
                <p>आय, जाति, मूल निवासी प्रमाण पत्र</p>
            </div>

            <!-- Banking Services -->
            <div class="service-card" id="box-banking" data-category="banking" data-title="बैंकिंग banking netbanking balance enquiry">
                <span class="card-tag">Banking</span>
                <i class="fa-solid fa-building-columns" style="color: #059669;"></i>
                <h4>ऑल बैंकिंग सेवाएँ</h4>
                <p>नेट बैंकिंग, पासबुक व बैलेंस जांच</p>
            </div>

        </div>
    </div>
</div>

<script>
    // 1. Firebase Configuration (अपनी कीज़ यहाँ रखें)
    const firebaseConfig = {
        apiKey: "YOUR_API_KEY",
        authDomain: "your-app.firebaseapp.com",
        databaseURL: "https://your-app-default-rtdb.firebaseio.com",
        projectId: "your-app",
        storageBucket: "your-app.appspot.com",
        messagingSenderId: "123456789",
        appId: "1:123456789:web:abcdef"
    };

    // Firebase Initialize
    if (!firebase.apps.length) {
        firebase.initializeApp(firebaseConfig);
    }
    const db = firebase.database();

    // 2. Admin Action - Realtime Update to Firebase
    function updateFirebase(serviceKey, isChecked) {
        db.ref('services/' + serviceKey).set(isChecked);
    }

    // 3. Realtime Listener - Sync UI for Admin & Users
    db.ref('services').on('value', (snapshot) => {
        const data = snapshot.val();
        if (data) {
            for (const [serviceKey, isVisible] of Object.entries(data)) {
                // Update User Box
                const box = document.getElementById('box-' + serviceKey);
                if (box) {
                    if (isVisible) {
                        box.classList.remove('hidden');
                    } else {
                        box.classList.add('hidden');
                    }
                }
                // Update Admin Checkbox
                const chk = document.getElementById('chk-' + serviceKey);
                if (chk) {
                    chk.checked = isVisible;
                }
            }
        }
    });

    // 4. Search Filter Logic
    function filterServices() {
        const input = document.getElementById('searchInput').value.toLowerCase();
        const cards = document.querySelectorAll('.service-card');

        cards.forEach(card => {
            const title = card.getAttribute('data-title').toLowerCase();
            if (title.includes(input)) {
                card.style.display = "block";
            } else {
                card.style.display = "none";
            }
        });
    }

    // 5. Category Tab Filtering
    function filterCategory(category) {
        // Tab Active Style
        document.querySelectorAll('.tab-btn').forEach(btn => btn.classList.remove('active'));
        event.target.classList.add('active');

        // Filter Cards
        const cards = document.querySelectorAll('.service-card');
        cards.forEach(card => {
            if (category === 'all' || card.getAttribute('data-category') === category) {
                card.style.display = "block";
            } else {
                card.style.display = "none";
            }
        });
    }
</script>

</body>
</html>
