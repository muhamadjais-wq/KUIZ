# KUIZ
<!DOCTYPE html>
<html lang="ms">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Akses Kuiz Geganti Perlindungan</title>
    <!-- Muat turun Tailwind CSS -->
    <script src="https://cdn.tailwindcss.com"></script>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;600;700&display=swap" rel="stylesheet">
    <style>
        /* Tetapkan fon Inter dan latar belakang yang kemas */
        body {
            font-family: 'Inter', sans-serif;
            background-color: #f0f4f8;
            min-height: 100vh;
            display: flex;
            align-items: center;
            justify-content: center;
            padding: 20px;
        }
        /* Style untuk butang utama */
        .btn-primary {
            transition: all 0.2s;
            box-shadow: 0 4px 6px -1px rgba(0, 0, 0, 0.1), 0 2px 4px -2px rgba(0, 0, 0, 0.06);
        }
        .btn-primary:hover {
            transform: translateY(-2px);
            box-shadow: 0 10px 15px -3px rgba(49, 130, 206, 0.5), 0 4px 6px -4px rgba(49, 130, 206, 0.2);
        }
        .container-card {
            max-width: 500px;
            width: 100%;
        }
        /* Style untuk kotak mesej (modal) */
        .message-box {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: rgba(0, 0, 0, 0.6);
            display: none; /* Disembunyikan secara lalai */
            align-items: center;
            justify-content: center;
            z-index: 1000;
        }
    </style>
</head>
<body>

    <!-- Container Utama -->
    <div id="appContainer" class="container-card bg-white p-8 md:p-10 rounded-xl shadow-2xl transition-all duration-500">
        
        <!-- Skrin Pengesahan (Login) -->
        <div id="loginScreen" class="space-y-6">
            <h1 class="text-3xl font-extrabold text-gray-900 text-center mb-2">Kuiz: GEGANTI PERLINDUNGAN</h1>
            <h2 class="text-xl font-semibold text-blue-600 text-center mb-6">Sistem Akses Berpintu</h2>
            <p class="text-center text-gray-600">Sila masukkan Nama Penuh dan Kad Pengenalan anda untuk pengesahan dan akses kuiz.</p>

            <!-- Input Nama (Placeholder dikemaskini) -->
            <div>
                <label for="nameInput" class="block text-sm font-medium text-gray-700 mb-1">Nama Penuh (Seperti Dalam Kad Pengenalan)</label>
                <input type="text" id="nameInput" placeholder="Contoh: MUHAMAD JAIS BIN JALUN" 
                       class="w-full px-4 py-2 border border-gray-300 rounded-lg focus:ring-blue-500 focus:border-blue-500 transition duration-150" required>
            </div>

            <!-- Input Kad Pengenalan (Placeholder dikemaskini) -->
            <div>
                <label for="icInput" class="block text-sm font-medium text-gray-700 mb-1">Nombor Kad Pengenalan (Contoh: 770707-12-7777)</label>
                <input type="text" id="icInput" placeholder="Contoh: 770707-12-7777" 
                       class="w-full px-4 py-2 border border-gray-300 rounded-lg focus:ring-blue-500 focus:border-blue-500 transition duration-150" required>
            </div>

            <!-- Mesej Ralat -->
            <div id="errorMessage" class="hidden text-sm p-3 bg-red-100 border-l-4 border-red-500 text-red-700 rounded-md">
                Maklumat yang dimasukkan salah. Sila semak Nama Penuh dan Nombor Kad Pengenalan anda dan cuba lagi.
            </div>
            
            <!-- Mesej sedang memuatkan -->
            <div id="loadingMessage" class="hidden text-sm p-3 bg-blue-100 border-l-4 border-blue-500 text-blue-700 rounded-md">
                Pengesahan berjaya. Sedang mengarahkan anda ke Kuiz...
            </div>


            <!-- Butang Log Masuk -->
            <button id="loginButton" onclick="handleLogin()"
                    class="btn-primary w-full bg-green-600 text-white py-3 rounded-xl font-semibold text-lg hover:bg-green-700">
                Akses Kuiz
            </button>
        </div>

    </div>

    <!-- Kotak Mesej (Modal) -->
    <div id="messageBox" class="message-box">
        <div class="bg-white p-6 rounded-xl shadow-2xl max-w-sm w-full space-y-4">
            <h3 id="messageTitle" class="text-xl font-bold text-gray-800">Tajuk Mesej</h3>
            <p id="messageText" class="text-gray-600">Ini adalah teks mesej.</p>
            <button onclick="closeMessageBox()"
                    class="w-full bg-blue-500 text-white py-2 rounded-lg font-semibold hover:bg-blue-600 transition duration-150">
                Tutup
            </button>
        </div>
    </div>

    <script>
        // Data Pengguna Sah (PENGGUNA BARU DITAMBAH)
        const validUsers = [
            { name: "ALVIN LIM", ic: "881226-12-5897" },
            { name: "AMIR@AMIRARIS BIN M BASIR", ic: "720708-12-5307" },
            { name: "ASRI BIN RUSLIH", ic: "900806-12-5253" },
            { name: "AWANG HAFIZ BIN AWANG TAHIR", ic: "850405-12-5743" },
            { name: "CHONG KETT CHUNG", ic: "780112-12-5925" },
            { name: "EUKI BIN SAWANG", ic: "750104-12-5549" },
            { name: "JOSLIE BIN GANANG", ic: "810504-12-5523" },
            { name: "JUBIN BIN SIDEK", ic: "880310-12-5609" },
            { name: "MOHD ALFIEZ RIZAMSYAH B MOHAMAD", ic: "940413-12-5929" },
            { name: "RIZWAN BIN ABD RAHMAN", ic: "881130-12-5681" },
            { name: "RUDDY SANDHU", ic: "791008-12-5611" },
            { name: "RYNDEL STEVIE BIN MORRIS", ic: "900423-12-5651" },
            { name: "AFWAN NAJIB BIN WALID", ic: "781114-12-5691" },
            { name: "MUHAMAD JAIS BIN JALUN", ic: "770707-12-7777" }, // Pengguna baru
        ];
        
        // Pautan Kuiz Google Form
        const quizUrl = "https://forms.gle/qD3LkEgGVTcoJNBe8";

        // --- Fungsi Modal Mesej ---

        /**
         * Memaparkan kotak mesej/modal tersuai.
         * @param {string} title - Tajuk mesej.
         * @param {string} text - Kandungan mesej.
         */
        function showMessageBox(title, text) {
            document.getElementById('messageTitle').textContent = title;
            document.getElementById('messageText').textContent = text;
            document.getElementById('messageBox').style.display = 'flex';
        }

        /**
         * Menutup kotak mesej/modal tersuai.
         */
        function closeMessageBox() {
            document.getElementById('messageBox').style.display = 'none';
        }

        // --- Fungsi Pengesahan Akses & Redirect ---

        /**
         * Mengendalikan proses log masuk, pengesahan, dan pengarahan (redirection) pengguna.
         */
        function handleLogin() {
            const nameInput = document.getElementById('nameInput');
            const icInput = document.getElementById('icInput');
            const errorMessageDiv = document.getElementById('errorMessage');
            const loadingMessageDiv = document.getElementById('loadingMessage');
            const loginButton = document.getElementById('loginButton');

            const nameValue = nameInput.value.trim();
            const icValue = icInput.value.trim();

            // Format input untuk perbandingan ketat
            const inputName = nameValue.toUpperCase().replace(/\s+/g, ' '); // Pangkas ruang berlebihan
            const cleanIC = icValue.replace(/-/g, '').trim(); // Buang sempang, pangkas ruang
            
            // Logik Pengesahan
            const foundUser = validUsers.find(user => {
                const storedName = user.name.toUpperCase().replace(/\s+/g, ' ');
                const storedIC = user.ic.replace(/-/g, '').trim();
                return storedName === inputName && storedIC === cleanIC;
            });

            if (foundUser) {
                // Pengesahan berjaya
                errorMessageDiv.classList.add('hidden');
                loadingMessageDiv.classList.remove('hidden');
                loginButton.disabled = true;
                loginButton.textContent = 'Memuatkan...';

                // Dapatkan 4 angka nombor kad pengenalan terakhir
                const icTail = cleanIC.slice(-4);
                
                // Bina URL redirect dengan parameter
                // Parameter 'ic_tail' boleh digunakan oleh Google Form (melalui Add-ons atau pre-fill link)
                const redirectUrl = `${quizUrl}?ic_tail=${icTail}`; 
                
                // Lakukan pengarahan selepas sedikit kelewatan untuk menunjukkan mesej loading
                setTimeout(() => {
                    window.location.href = redirectUrl;
                }, 1000); // Kelewatan 1 saat

            } else {
                // Pengesahan gagal
                loadingMessageDiv.classList.add('hidden');
                errorMessageDiv.classList.remove('hidden');
            }
        }

        // Tetapkan event listener untuk 'Enter' key pada skrin log masuk
        document.addEventListener('DOMContentLoaded', () => {
            const icInput = document.getElementById('icInput');
            if (icInput) {
                icInput.addEventListener('keypress', function(e) {
                    if (e.key === 'Enter') {
                        e.preventDefault(); // Elak form submit biasa
                        handleLogin();
                    }
                });
            }
        });
    </script>

</body>
</html>
