<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Paradise Hospital - Management System</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <script src="https://cdn.jsdelivr.net/npm/chart.js@4.4.0/dist/chart.umd.min.js"></script>
    <link href="https://fonts.googleapis.com/css2?family=Poppins:wght@300;400;500;600;700;800&display=swap" rel="stylesheet">
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <style>
        * { font-family: 'Poppins', sans-serif; box-sizing: border-box; }
        :root {
            --primary: #374151; --primary-dark: #1f2937; --secondary: #4b5563;
            --accent: #6b7280; --bg: #f3f4f6; --card: #ffffff; --text: #111827;
            --border: #e5e7eb; --success: #059669; --warning: #d97706; --danger: #dc2626;
        }
        .dark { --bg: #111827; --card: #1f2937; --text: #f9fafb; --border: #374151; }
        body { background: var(--bg); color: var(--text); min-height: 100vh; }
        .card { background: var(--card); border-radius: 12px; border: 1px solid var(--border); }
        .btn { padding: 0.5rem 1rem; border-radius: 8px; font-weight: 500; cursor: pointer; transition: all 0.2s; border: none; }
        .btn-primary { background: var(--primary); color: white; }
        .btn-primary:hover { background: var(--primary-dark); }
        .btn-success { background: var(--success); color: white; }
        .btn-danger { background: var(--danger); color: white; }
        .btn-secondary { background: var(--secondary); color: white; }
        input, select, textarea { width: 100%; padding: 0.625rem; border: 1px solid var(--border); border-radius: 8px; background: var(--card); color: var(--text); font-size: 16px; }
        input:focus, select:focus { outline: none; border-color: var(--primary); }
        .nav-tab { padding: 0.75rem 1rem; cursor: pointer; border-bottom: 3px solid transparent; transition: all 0.2s; white-space: nowrap; font-size: 0.875rem; }
        .nav-tab:hover { background: rgba(55,65,81,0.1); }
        .nav-tab.active { border-bottom-color: var(--primary); font-weight: 600; background: rgba(55,65,81,0.1); }
        .section { display: none; }
        .section.active { display: block; animation: fadeIn 0.3s; }
        @keyframes fadeIn { from { opacity: 0; } to { opacity: 1; } }
        .stat-card { background: linear-gradient(135deg, #374151, #4b5563); color: white; padding: 1.5rem; border-radius: 12px; }
        .modal { display: none; position: fixed; inset: 0; background: rgba(0,0,0,0.5); z-index: 1000; align-items: center; justify-content: center; }
        .modal.active { display: flex; }
        .modal-content { background: var(--card); padding: 1.5rem; border-radius: 12px; max-width: 500px; width: 90%; max-height: 85vh; overflow-y: auto; }
        table { width: 100%; border-collapse: collapse; }
        th { background: var(--primary); color: white; padding: 0.75rem; text-align: left; font-size: 0.875rem; }
        td { padding: 0.75rem; border-bottom: 1px solid var(--border); font-size: 0.875rem; }
        tr:hover { background: rgba(55,65,81,0.05); }
        .badge { padding: 0.25rem 0.5rem; border-radius: 6px; font-size: 0.75rem; font-weight: 500; }
        .badge-success { background: #d1fae5; color: #065f46; }
        .badge-warning { background: #fef3c7; color: #92400e; }
        .badge-danger { background: #fee2e2; color: #991b1b; }
        .badge-info { background: #dbeafe; color: #1e40af; }
        .dark .badge-success { background: #065f46; color: #d1fae5; }
        .dark .badge-warning { background: #92400e; color: #fef3c7; }
        .dark .badge-danger { background: #991b1b; color: #fee2e2; }
        .dark .badge-info { background: #1e40af; color: #dbeafe; }
        .login-container { min-height: 100vh; display: flex; align-items: center; justify-content: center; background: linear-gradient(135deg, #1f2937 0%, #374151 100%); }
        .login-card { background: white; padding: 2.5rem; border-radius: 16px; max-width: 400px; width: 90%; box-shadow: 0 20px 40px rgba(0,0,0,0.3); }
        .app-container { display: none; }
        .app-container.active { display: block; }
        ::-webkit-scrollbar { width: 8px; height: 8px; }
        ::-webkit-scrollbar-track { background: var(--border); }
        ::-webkit-scrollbar-thumb { background: var(--secondary); border-radius: 4px; }
    </style>
</head>
<body>
    <!-- Login Screen -->
    <div id="loginScreen" class="login-container">
        <div class="login-card">
            <div class="text-center mb-6">
                <div class="w-16 h-16 bg-gradient-to-br from-gray-700 to-gray-500 rounded-2xl flex items-center justify-center mx-auto mb-4">
                    <i class="fas fa-hospital text-white text-2xl"></i>
                </div>
                <h1 class="text-2xl font-bold text-gray-800">Paradise Hospital</h1>
                <p class="text-gray-500 text-sm">Management System</p>
            </div>
            <div id="loginError" class="bg-red-100 text-red-700 p-3 rounded-lg mb-4 hidden text-sm">
                <i class="fas fa-exclamation-circle"></i> Invalid credentials
            </div>
            <form onsubmit="handleLogin(event)">
                <div class="mb-4">
                    <label class="block text-sm font-medium text-gray-700 mb-1">Username</label>
                    <input type="text" id="loginUsername" required placeholder="Enter username">
                </div>
                <div class="mb-6">
                    <label class="block text-sm font-medium text-gray-700 mb-1">Password</label>
                    <input type="password" id="loginPassword" required placeholder="Enter password">
                </div>
                <button type="submit" class="btn btn-primary w-full py-3">
                    <i class="fas fa-sign-in-alt mr-2"></i>Login
                </button>
            </form>
            <p class="text-center text-xs text-gray-400 mt-6">Powered by <span class="font-semibold text-gray-600">Exalt Consulting</span></p>
        </div>
    </div>

    <!-- Main Application -->
    <div id="appContainer" class="app-container">
        <!-- Header -->
        <header class="bg-gradient-to-r from-gray-800 to-gray-700 text-white px-4 py-3">
            <div class="flex items-center justify-between">
                <div class="flex items-center gap-3">
                    <i class="fas fa-hospital text-2xl"></i>
                    <div>
                        <h1 class="text-lg font-bold">Paradise Hospital</h1>
                        <p class="text-xs text-gray-300">Management System</p>
                    </div>
                </div>
                <div class="flex items-center gap-4">
                    <span class="text-sm hidden sm:block"><i class="fas fa-user mr-1"></i> <span id="loggedUser">Tendai Manjeru</span></span>
                    <button onclick="toggleDark()" class="btn btn-secondary text-xs"><i class="fas fa-moon"></i></button>
                    <button onclick="handleLogout()" class="btn btn-danger text-xs"><i class="fas fa-sign-out-alt"></i></button>
                </div>
            </div>
        </header>

        <!-- Navigation -->
        <nav class="bg-white dark:bg-gray-800 border-b border-gray-200 dark:border-gray-700 overflow-x-auto">
            <div class="flex">
                <div class="nav-tab active" data-section="dashboard"><i class="fas fa-chart-pie mr-1"></i><span class="hidden sm:inline">Dashboard</span></div>
                <div class="nav-tab" data-section="inventory"><i class="fas fa-boxes mr-1"></i><span class="hidden sm:inline">Inventory</span></div>
                <div class="nav-tab" data-section="patients"><i class="fas fa-users mr-1"></i><span class="hidden sm:inline">Patients</span></div>
                <div class="nav-tab" data-section="admissions"><i class="fas fa-bed mr-1"></i><span class="hidden sm:inline">Admissions</span></div>
                <div class="nav-tab" data-section="opd"><i class="fas fa-clinic-medical mr-1"></i><span class="hidden sm:inline">OPD</span></div>
                <div class="nav-tab" data-section="pharmacy"><i class="fas fa-pills mr-1"></i><span class="hidden sm:inline">Pharmacy</span></div>
                <div class="nav-tab" data-section="childhealth"><i class="fas fa-baby mr-1"></i><span class="hidden sm:inline">Child Health</span></div>
                <div class="nav-tab" data-section="dentistry"><i class="fas fa-tooth mr-1"></i><span class="hidden sm:inline">Dentistry</span></div>
                <div class="nav-tab" data-section="transfers"><i class="fas fa-ambulance mr-1"></i><span class="hidden sm:inline">Transfers</span></div>
                <div class="nav-tab" data-section="employees"><i class="fas fa-user-md mr-1"></i><span class="hidden sm:inline">Staff</span></div>
            </div>
        </nav>

        <!-- Main Content -->
        <main class="p-4">
            <!-- Dashboard -->
            <div id="dashboard" class="section active">
                <div class="flex justify-between items-center mb-6">
                    <h2 class="text-2xl font-bold">Dashboard Overview</h2>
                    <button onclick="downloadAllData()" class="btn btn-success text-sm"><i class="fas fa-download mr-1"></i>Export All</button>
                </div>

                <!-- Stats Grid -->
                <div class="grid grid-cols-2 md:grid-cols-4 lg:grid-cols-6 gap-4 mb-6">
                    <div class="stat-card"><p class="text-xs opacity-80">Inventory Items</p><p class="text-2xl font-bold" id="stat-inv">0</p></div>
                    <div class="stat-card" style="background:linear-gradient(135deg,#4b5563,#6b7280)"><p class="text-xs opacity-80">Patients</p><p class="text-2xl font-bold" id="stat-pat">0</p></div>
                    <div class="stat-card" style="background:linear-gradient(135deg,#1f2937,#374151)"><p class="text-xs opacity-80">Admissions</p><p class="text-2xl font-bold" id="stat-adm">0</p></div>
                    <div class="stat-card" style="background:linear-gradient(135deg,#6b7280,#9ca3af)"><p class="text-xs opacity-80">OPD Visits</p><p class="text-2xl font-bold" id="stat-opd">0</p></div>
                    <div class="stat-card" style="background:linear-gradient(135deg,#374151,#4b5563)"><p class="text-xs opacity-80">Pharmacy</p><p class="text-2xl font-bold" id="stat-pharm">0</p></div>
                    <div class="stat-card" style="background:linear-gradient(135deg,#4b5563,#374151)"><p class="text-xs opacity-80">Staff</p><p class="text-2xl font-bold" id="stat-emp">0</p></div>
                </div>

                <!-- Charts -->
                <div class="grid grid-cols-1 lg:grid-cols-2 gap-4 mb-6">
                    <div class="card p-4"><h3 class="font-semibold mb-3">Inventory by Category</h3><canvas id="chartInventory"></canvas></div>
                    <div class="card p-4"><h3 class="font-semibold mb-3">Monthly Admissions</h3><canvas id="chartAdmissions"></canvas></div>
                </div>
                <div class="grid grid-cols-1 lg:grid-cols-3 gap-4">
                    <div class="card p-4"><h3 class="font-semibold mb-3">OPD by Department</h3><canvas id="chartOPD"></canvas></div>
                    <div class="card p-4"><h3 class="font-semibold mb-3">Staff Distribution</h3><canvas id="chartStaff"></canvas></div>
                    <div class="card p-4"><h3 class="font-semibold mb-3">Pharmacy Dispensed</h3><canvas id="chartPharmacy"></canvas></div>
                </div>
            </div>

            <!-- Inventory Section -->
            <div id="inventory" class="section">
                <div class="flex flex-wrap justify-between items-center gap-3 mb-4">
                    <h2 class="text-2xl font-bold">Inventory Management</h2>
                    <div class="flex gap-2">
                        <input type="text" id="searchInv" placeholder="Search..." class="w-48" onkeyup="renderTable('inventory')">
                        <button onclick="downloadCSV('inventory')" class="btn btn-success text-sm"><i class="fas fa-download"></i></button>
                        <button onclick="openModal('invModal')" class="btn btn-primary text-sm"><i class="fas fa-plus mr-1"></i>Add</button>
                    </div>
                </div>
                <div class="card overflow-x-auto">
                    <table><thead><tr><th>Item Code</th><th>Name</th><th>Category</th><th>Qty</th><th>Unit</th><th>Min Stock</th><th>Status</th><th>Actions</th></tr></thead><tbody id="invBody"></tbody></table>
                </div>
            </div>

            <!-- Patients Section -->
            <div id="patients" class="section">
                <div class="flex flex-wrap justify-between items-center gap-3 mb-4">
                    <h2 class="text-2xl font-bold">Patient Registration</h2>
                    <div class="flex gap-2">
                        <input type="text" id="searchPat" placeholder="Search..." class="w-48" onkeyup="renderTable('patients')">
                        <button onclick="downloadCSV('patients')" class="btn btn-success text-sm"><i class="fas fa-download"></i></button>
                        <button onclick="openModal('patModal')" class="btn btn-primary text-sm"><i class="fas fa-plus mr-1"></i>Register</button>
                    </div>
                </div>
                <div class="card overflow-x-auto">
                    <table><thead><tr><th>Patient ID</th><th>Name</th><th>DOB</th><th>Gender</th><th>Phone</th><th>Address</th><th>Blood Group</th><th>Actions</th></tr></thead><tbody id="patBody"></tbody></table>
                </div>
            </div>

            <!-- Admissions Section -->
            <div id="admissions" class="section">
                <div class="flex flex-wrap justify-between items-center gap-3 mb-4">
                    <h2 class="text-2xl font-bold">Admissions</h2>
                    <div class="flex gap-2">
                        <input type="text" id="searchAdm" placeholder="Search..." class="w-48" onkeyup="renderTable('admissions')">
                        <button onclick="downloadCSV('admissions')" class="btn btn-success text-sm"><i class="fas fa-download"></i></button>
                        <button onclick="openModal('admModal')" class="btn btn-primary text-sm"><i class="fas fa-plus mr-1"></i>Admit</button>
                    </div>
                </div>
                <div class="card overflow-x-auto">
                    <table><thead><tr><th>Admission ID</th><th>Patient</th><th>Ward</th><th>Bed</th><th>Doctor</th><th>Date</th><th>Diagnosis</th><th>Status</th><th>Actions</th></tr></thead><tbody id="admBody"></tbody></table>
                </div>
            </div>

            <!-- OPD Section -->
            <div id="opd" class="section">
                <div class="flex flex-wrap justify-between items-center gap-3 mb-4">
                    <h2 class="text-2xl font-bold">Outpatient Department (OPD)</h2>
                    <div class="flex gap-2">
                        <input type="text" id="searchOpd" placeholder="Search..." class="w-48" onkeyup="renderTable('opd')">
                        <button onclick="downloadCSV('opd')" class="btn btn-success text-sm"><i class="fas fa-download"></i></button>
                        <button onclick="openModal('opdModal')" class="btn btn-primary text-sm"><i class="fas fa-plus mr-1"></i>New Visit</button>
                    </div>
                </div>
                <div class="card overflow-x-auto">
                    <table><thead><tr><th>Visit ID</th><th>Patient</th><th>Department</th><th>Doctor</th><th>Date</th><th>Complaint</th><th>Diagnosis</th><th>Actions</th></tr></thead><tbody id="opdBody"></tbody></table>
                </div>
            </div>

            <!-- Pharmacy Section -->
            <div id="pharmacy" class="section">
                <div class="flex flex-wrap justify-between items-center gap-3 mb-4">
                    <h2 class="text-2xl font-bold">Pharmacy</h2>
                    <div class="flex gap-2">
                        <input type="text" id="searchPharm" placeholder="Search..." class="w-48" onkeyup="renderTable('pharmacy')">
                        <button onclick="downloadCSV('pharmacy')" class="btn btn-success text-sm"><i class="fas fa-download"></i></button>
                        <button onclick="openModal('pharmModal')" class="btn btn-primary text-sm"><i class="fas fa-plus mr-1"></i>Dispense</button>
                    </div>
                </div>
                <div class="card overflow-x-auto">
                    <table><thead><tr><th>Rx ID</th><th>Patient</th><th>Medication</th><th>Dosage</th><th>Qty</th><th>Prescribed By</th><th>Date</th><th>Actions</th></tr></thead><tbody id="pharmBody"></tbody></table>
                </div>
            </div>

            <!-- Child Health Section -->
            <div id="childhealth" class="section">
                <div class="flex flex-wrap justify-between items-center gap-3 mb-4">
                    <h2 class="text-2xl font-bold">Child Health Services</h2>
                    <div class="flex gap-2">
                        <input type="text" id="searchChild" placeholder="Search..." class="w-48" onkeyup="renderTable('childhealth')">
                        <button onclick="downloadCSV('childhealth')" class="btn btn-success text-sm"><i class="fas fa-download"></i></button>
                        <button onclick="openModal('childModal')" class="btn btn-primary text-sm"><i class="fas fa-plus mr-1"></i>Add Record</button>
                    </div>
                </div>
                <div class="card overflow-x-auto">
                    <table><thead><tr><th>Record ID</th><th>Child Name</th><th>DOB</th><th>Guardian</th><th>Service</th><th>Vaccine/Treatment</th><th>Date</th><th>Next Visit</th><th>Actions</th></tr></thead><tbody id="childBody"></tbody></table>
                </div>
            </div>

            <!-- Dentistry Section -->
            <div id="dentistry" class="section">
                <div class="flex flex-wrap justify-between items-center gap-3 mb-4">
                    <h2 class="text-2xl font-bold">Dentistry</h2>
                    <div class="flex gap-2">
                        <input type="text" id="searchDent" placeholder="Search..." class="w-48" onkeyup="renderTable('dentistry')">
                        <button onclick="downloadCSV('dentistry')" class="btn btn-success text-sm"><i class="fas fa-download"></i></button>
                        <button onclick="openModal('dentModal')" class="btn btn-primary text-sm"><i class="fas fa-plus mr-1"></i>Add Record</button>
                    </div>
                </div>
                <div class="card overflow-x-auto">
                    <table><thead><tr><th>Visit ID</th><th>Patient</th><th>Procedure</th><th>Dentist</th><th>Date</th><th>Teeth</th><th>Notes</th><th>Actions</th></tr></thead><tbody id="dentBody"></tbody></table>
                </div>
            </div>

            <!-- Transfers Section -->
            <div id="transfers" class="section">
                <div class="flex flex-wrap justify-between items-center gap-3 mb-4">
                    <h2 class="text-2xl font-bold">Patient Transfers</h2>
                    <div class="flex gap-2">
                        <input type="text" id="searchTrans" placeholder="Search..." class="w-48" onkeyup="renderTable('transfers')">
                        <button onclick="downloadCSV('transfers')" class="btn btn-success text-sm"><i class="fas fa-download"></i></button>
                        <button onclick="openModal('transModal')" class="btn btn-primary text-sm"><i class="fas fa-plus mr-1"></i>New Transfer</button>
                    </div>
                </div>
                <div class="card overflow-x-auto">
                    <table><thead><tr><th>Transfer ID</th><th>Patient</th><th>From</th><th>To</th><th>Reason</th><th>Date</th><th>Status</th><th>Actions</th></tr></thead><tbody id="transBody"></tbody></table>
                </div>
            </div>

            <!-- Employees Section -->
            <div id="employees" class="section">
                <div class="flex flex-wrap justify-between items-center gap-3 mb-4">
                    <h2 class="text-2xl font-bold">Staff Management</h2>
                    <div class="flex gap-2">
                        <input type="text" id="searchEmp" placeholder="Search..." class="w-48" onkeyup="renderTable('employees')">
                        <button onclick="downloadCSV('employees')" class="btn btn-success text-sm"><i class="fas fa-download"></i></button>
                        <button onclick="openModal('empModal')" class="btn btn-primary text-sm"><i class="fas fa-plus mr-1"></i>Add Staff</button>
                    </div>
                </div>
                <div class="card overflow-x-auto">
                    <table><thead><tr><th>Staff ID</th><th>Name</th><th>Role</th><th>Department</th><th>Phone</th><th>Email</th><th>Hire Date</th><th>Actions</th></tr></thead><tbody id="empBody"></tbody></table>
                </div>
            </div>
        </main>
    </div>

    <!-- Modals -->
    <!-- Inventory Modal -->
    <div id="invModal" class="modal">
        <div class="modal-content">
            <div class="flex justify-between items-center mb-4"><h3 class="text-lg font-bold">Inventory Item</h3><button onclick="closeModal('invModal')" class="text-xl">&times;</button></div>
            <form onsubmit="saveRecord(event,'inventory')">
                <input type="hidden" id="inv-id">
                <div class="grid grid-cols-2 gap-3 mb-3">
                    <div><label class="block text-sm mb-1">Item Code</label><input type="text" id="inv-code" required></div>
                    <div><label class="block text-sm mb-1">Item Name</label><input type="text" id="inv-name" required></div>
                </div>
                <div class="grid grid-cols-2 gap-3 mb-3">
                    <div><label class="block text-sm mb-1">Category</label>
                        <select id="inv-category" required>
                            <option value="">Select</option>
                            <option>Pharmaceuticals</option><option>Medical Equipment</option><option>Surgical Supplies</option>
                            <option>Laboratory Supplies</option><option>PPE</option><option>Consumables</option>
                            <option>Diagnostic Equipment</option><option>Furniture</option><option>Linens</option><option>Cleaning Supplies</option>
                        </select>
                    </div>
                    <div><label class="block text-sm mb-1">Unit</label>
                        <select id="inv-unit" required><option value="">Select</option><option>Pieces</option><option>Boxes</option><option>Packs</option><option>Bottles</option><option>Liters</option><option>Kg</option><option>Pairs</option><option>Sets</option></select>
                    </div>
                </div>
                <div class="grid grid-cols-2 gap-3 mb-3">
                    <div><label class="block text-sm mb-1">Quantity</label><input type="number" id="inv-qty" required min="0"></div>
                    <div><label class="block text-sm mb-1">Min Stock Level</label><input type="number" id="inv-min" required min="0"></div>
                </div>
                <div class="flex gap-2 mt-4"><button type="submit" class="btn btn-primary flex-1">Save</button><button type="button" onclick="closeModal('invModal')" class="btn btn-secondary">Cancel</button></div>
            </form>
        </div>
    </div>

    <!-- Patient Modal -->
    <div id="patModal" class="modal">
        <div class="modal-content">
            <div class="flex justify-between items-center mb-4"><h3 class="text-lg font-bold">Patient Registration</h3><button onclick="closeModal('patModal')" class="text-xl">&times;</button></div>
            <form onsubmit="saveRecord(event,'patients')">
                <input type="hidden" id="pat-id">
                <div class="mb-3"><label class="block text-sm mb-1">Full Name</label><input type="text" id="pat-name" required></div>
                <div class="grid grid-cols-2 gap-3 mb-3">
                    <div><label class="block text-sm mb-1">Date of Birth</label><input type="date" id="pat-dob" required></div>
                    <div><label class="block text-sm mb-1">Gender</label><select id="pat-gender" required><option value="">Select</option><option>Male</option><option>Female</option><option>Other</option></select></div>
                </div>
                <div class="grid grid-cols-2 gap-3 mb-3">
                    <div><label class="block text-sm mb-1">Phone</label><input type="tel" id="pat-phone" required></div>
                    <div><label class="block text-sm mb-1">Blood Group</label><select id="pat-blood"><option value="">Unknown</option><option>A+</option><option>A-</option><option>B+</option><option>B-</option><option>AB+</option><option>AB-</option><option>O+</option><option>O-</option></select></div>
                </div>
                <div class="mb-3"><label class="block text-sm mb-1">Address</label><textarea id="pat-address" rows="2"></textarea></div>
                <div class="flex gap-2 mt-4"><button type="submit" class="btn btn-primary flex-1">Save</button><button type="button" onclick="closeModal('patModal')" class="btn btn-secondary">Cancel</button></div>
            </form>
        </div>
    </div>

    <!-- Admissions Modal -->
    <div id="admModal" class="modal">
        <div class="modal-content">
            <div class="flex justify-between items-center mb-4"><h3 class="text-lg font-bold">Patient Admission</h3><button onclick="closeModal('admModal')" class="text-xl">&times;</button></div>
            <form onsubmit="saveRecord(event,'admissions')">
                <input type="hidden" id="adm-id">
                <div class="grid grid-cols-2 gap-3 mb-3">
                    <div><label class="block text-sm mb-1">Patient Name</label><input type="text" id="adm-patient" required></div>
                    <div><label class="block text-sm mb-1">Admission Date</label><input type="date" id="adm-date" required></div>
                </div>
                <div class="grid grid-cols-2 gap-3 mb-3">
                    <div><label class="block text-sm mb-1">Ward</label><select id="adm-ward" required><option value="">Select</option><option>General Ward</option><option>Private Ward</option><option>ICU</option><option>Maternity</option><option>Pediatric</option><option>Surgical</option><option>Emergency</option></select></div>
                    <div><label class="block text-sm mb-1">Bed Number</label><input type="text" id="adm-bed" required></div>
                </div>
                <div class="grid grid-cols-2 gap-3 mb-3">
                    <div><label class="block text-sm mb-1">Attending Doctor</label><input type="text" id="adm-doctor" required></div>
                    <div><label class="block text-sm mb-1">Status</label><select id="adm-status" required><option>Admitted</option><option>Discharged</option><option>Transferred</option><option>Critical</option></select></div>
                </div>
                <div class="mb-3"><label class="block text-sm mb-1">Diagnosis</label><textarea id="adm-diagnosis" rows="2" required></textarea></div>
                <div class="flex gap-2 mt-4"><button type="submit" class="btn btn-primary flex-1">Save</button><button type="button" onclick="closeModal('admModal')" class="btn btn-secondary">Cancel</button></div>
            </form>
        </div>
    </div>

    <!-- OPD Modal -->
    <div id="opdModal" class="modal">
        <div class="modal-content">
            <div class="flex justify-between items-center mb-4"><h3 class="text-lg font-bold">OPD Visit</h3><button onclick="closeModal('opdModal')" class="text-xl">&times;</button></div>
            <form onsubmit="saveRecord(event,'opd')">
                <input type="hidden" id="opd-id">
                <div class="grid grid-cols-2 gap-3 mb-3">
                    <div><label class="block text-sm mb-1">Patient Name</label><input type="text" id="opd-patient" required></div>
                    <div><label class="block text-sm mb-1">Visit Date</label><input type="date" id="opd-date" required></div>
                </div>
                <div class="grid grid-cols-2 gap-3 mb-3">
                    <div><label class="block text-sm mb-1">Department</label><select id="opd-dept" required><option value="">Select</option><option>General Medicine</option><option>Pediatrics</option><option>Gynecology</option><option>Orthopedics</option><option>ENT</option><option>Ophthalmology</option><option>Dermatology</option><option>Cardiology</option><option>Neurology</option></select></div>
                    <div><label class="block text-sm mb-1">Doctor</label><input type="text" id="opd-doctor" required></div>
                </div>
                <div class="mb-3"><label class="block text-sm mb-1">Chief Complaint</label><input type="text" id="opd-complaint" required></div>
                <div class="mb-3"><label class="block text-sm mb-1">Diagnosis</label><textarea id="opd-diagnosis" rows="2"></textarea></div>
                <div class="flex gap-2 mt-4"><button type="submit" class="btn btn-primary flex-1">Save</button><button type="button" onclick="closeModal('opdModal')" class="btn btn-secondary">Cancel</button></div>
            </form>
        </div>
    </div>

    <!-- Pharmacy Modal -->
    <div id="pharmModal" class="modal">
        <div class="modal-content">
            <div class="flex justify-between items-center mb-4"><h3 class="text-lg font-bold">Pharmacy Dispensing</h3><button onclick="closeModal('pharmModal')" class="text-xl">&times;</button></div>
            <form onsubmit="saveRecord(event,'pharmacy')">
                <input type="hidden" id="pharm-id">
                <div class="grid grid-cols-2 gap-3 mb-3">
                    <div><label class="block text-sm mb-1">Patient Name</label><input type="text" id="pharm-patient" required></div>
                    <div><label class="block text-sm mb-1">Date</label><input type="date" id="pharm-date" required></div>
                </div>
                <div class="mb-3"><label class="block text-sm mb-1">Medication</label><input type="text" id="pharm-med" required></div>
                <div class="grid grid-cols-2 gap-3 mb-3">
                    <div><label class="block text-sm mb-1">Dosage</label><input type="text" id="pharm-dosage" required placeholder="e.g., 500mg 3x daily"></div>
                    <div><label class="block text-sm mb-1">Quantity</label><input type="number" id="pharm-qty" required min="1"></div>
                </div>
                <div class="mb-3"><label class="block text-sm mb-1">Prescribed By</label><input type="text" id="pharm-doctor" required></div>
                <div class="flex gap-2 mt-4"><button type="submit" class="btn btn-primary flex-1">Save</button><button type="button" onclick="closeModal('pharmModal')" class="btn btn-secondary">Cancel</button></div>
            </form>
        </div>
    </div>

    <!-- Child Health Modal -->
    <div id="childModal" class="modal">
        <div class="modal-content">
            <div class="flex justify-between items-center mb-4"><h3 class="text-lg font-bold">Child Health Record</h3><button onclick="closeModal('childModal')" class="text-xl">&times;</button></div>
            <form onsubmit="saveRecord(event,'childhealth')">
                <input type="hidden" id="child-id">
                <div class="grid grid-cols-2 gap-3 mb-3">
                    <div><label class="block text-sm mb-1">Child Name</label><input type="text" id="child-name" required></div>
                    <div><label class="block text-sm mb-1">Date of Birth</label><input type="date" id="child-dob" required></div>
                </div>
                <div class="grid grid-cols-2 gap-3 mb-3">
                    <div><label class="block text-sm mb-1">Guardian Name</label><input type="text" id="child-guardian" required></div>
                    <div><label class="block text-sm mb-1">Service Type</label><select id="child-service" required><option value="">Select</option><option>Vaccination</option><option>Growth Monitoring</option><option>Nutrition</option><option>General Checkup</option><option>Immunization</option></select></div>
                </div>
                <div class="grid grid-cols-2 gap-3 mb-3">
                    <div><label class="block text-sm mb-1">Vaccine/Treatment</label><input type="text" id="child-treatment"></div>
                    <div><label class="block text-sm mb-1">Visit Date</label><input type="date" id="child-date" required></div>
                </div>
                <div class="mb-3"><label class="block text-sm mb-1">Next Visit Date</label><input type="date" id="child-next"></div>
                <div class="flex gap-2 mt-4"><button type="submit" class="btn btn-primary flex-1">Save</button><button type="button" onclick="closeModal('childModal')" class="btn btn-secondary">Cancel</button></div>
            </form>
        </div>
    </div>

    <!-- Dentistry Modal -->
    <div id="dentModal" class="modal">
        <div class="modal-content">
            <div class="flex justify-between items-center mb-4"><h3 class="text-lg font-bold">Dental Record</h3><button onclick="closeModal('dentModal')" class="text-xl">&times;</button></div>
            <form onsubmit="saveRecord(event,'dentistry')">
                <input type="hidden" id="dent-id">
                <div class="grid grid-cols-2 gap-3 mb-3">
                    <div><label class="block text-sm mb-1">Patient Name</label><input type="text" id="dent-patient" required></div>
                    <div><label class="block text-sm mb-1">Visit Date</label><input type="date" id="dent-date" required></div>
                </div>
                <div class="grid grid-cols-2 gap-3 mb-3">
                    <div><label class="block text-sm mb-1">Procedure</label><select id="dent-procedure" required><option value="">Select</option><option>Extraction</option><option>Filling</option><option>Root Canal</option><option>Cleaning</option><option>Crown</option><option>Braces</option><option>Checkup</option><option>X-Ray</option></select></div>
                    <div><label class="block text-sm mb-1">Dentist</label><input type="text" id="dent-dentist" required></div>
                </div>
                <div class="mb-3"><label class="block text-sm mb-1">Teeth Affected</label><input type="text" id="dent-teeth" placeholder="e.g., Upper right molar"></div>
                <div class="mb-3"><label class="block text-sm mb-1">Notes</label><textarea id="dent-notes" rows="2"></textarea></div>
                <div class="flex gap-2 mt-4"><button type="submit" class="btn btn-primary flex-1">Save</button><button type="button" onclick="closeModal('dentModal')" class="btn btn-secondary">Cancel</button></div>
            </form>
        </div>
    </div>

    <!-- Transfers Modal -->
    <div id="transModal" class="modal">
        <div class="modal-content">
            <div class="flex justify-between items-center mb-4"><h3 class="text-lg font-bold">Patient Transfer</h3><button onclick="closeModal('transModal')" class="text-xl">&times;</button></div>
            <form onsubmit="saveRecord(event,'transfers')">
                <input type="hidden" id="trans-id">
                <div class="grid grid-cols-2 gap-3 mb-3">
                    <div><label class="block text-sm mb-1">Patient Name</label><input type="text" id="trans-patient" required></div>
                    <div><label class="block text-sm mb-1">Transfer Date</label><input type="date" id="trans-date" required></div>
                </div>
                <div class="grid grid-cols-2 gap-3 mb-3">
                    <div><label class="block text-sm mb-1">From (Ward/Facility)</label><input type="text" id="trans-from" required></div>
                    <div><label class="block text-sm mb-1">To (Ward/Facility)</label><input type="text" id="trans-to" required></div>
                </div>
                <div class="grid grid-cols-2 gap-3 mb-3">
                    <div><label class="block text-sm mb-1">Reason</label><input type="text" id="trans-reason" required></div>
                    <div><label class="block text-sm mb-1">Status</label><select id="trans-status" required><option>Pending</option><option>In Transit</option><option>Completed</option><option>Cancelled</option></select></div>
                </div>
                <div class="flex gap-2 mt-4"><button type="submit" class="btn btn-primary flex-1">Save</button><button type="button" onclick="closeModal('transModal')" class="btn btn-secondary">Cancel</button></div>
            </form>
        </div>
    </div>

    <!-- Employees Modal -->
    <div id="empModal" class="modal">
        <div class="modal-content">
            <div class="flex justify-between items-center mb-4"><h3 class="text-lg font-bold">Staff Member</h3><button onclick="closeModal('empModal')" class="text-xl">&times;</button></div>
            <form onsubmit="saveRecord(event,'employees')">
                <input type="hidden" id="emp-id">
                <div class="mb-3"><label class="block text-sm mb-1">Full Name</label><input type="text" id="emp-name" required></div>
                <div class="grid grid-cols-2 gap-3 mb-3">
                    <div><label class="block text-sm mb-1">Role</label><select id="emp-role" required><option value="">Select</option><option>Doctor</option><option>Nurse</option><option>Pharmacist</option><option>Lab Technician</option><option>Radiologist</option><option>Dentist</option><option>Receptionist</option><option>Administrator</option><option>Cleaner</option><option>Security</option></select></div>
                    <div><label class="block text-sm mb-1">Department</label><select id="emp-dept" required><option value="">Select</option><option>Administration</option><option>Emergency</option><option>General Medicine</option><option>Surgery</option><option>Pediatrics</option><option>OPD</option><option>Pharmacy</option><option>Laboratory</option><option>Radiology</option><option>Dentistry</option></select></div>
                </div>
                <div class="grid grid-cols-2 gap-3 mb-3">
                    <div><label class="block text-sm mb-1">Phone</label><input type="tel" id="emp-phone" required></div>
                    <div><label class="block text-sm mb-1">Email</label><input type="email" id="emp-email"></div>
                </div>
                <div class="mb-3"><label class="block text-sm mb-1">Hire Date</label><input type="date" id="emp-hire" required></div>
                <div class="flex gap-2 mt-4"><button type="submit" class="btn btn-primary flex-1">Save</button><button type="button" onclick="closeModal('empModal')" class="btn btn-secondary">Cancel</button></div>
            </form>
        </div>
    </div>

    <script>
        // Authentication
        const credentials = { username: 'Tendai Manjeru', password: 'admin' };

        function handleLogin(e) {
            e.preventDefault();
            const u = document.getElementById('loginUsername').value;
            const p = document.getElementById('loginPassword').value;
            if (u === credentials.username && p === credentials.password) {
                document.getElementById('loginScreen').style.display = 'none';
                document.getElementById('appContainer').classList.add('active');
                document.getElementById('loggedUser').textContent = u;
                initApp();
            } else {
                document.getElementById('loginError').classList.remove('hidden');
                setTimeout(() => document.getElementById('loginError').classList.add('hidden'), 3000);
            }
        }

        function handleLogout() {
            showConfirm('Logout?', () => {
                document.getElementById('loginScreen').style.display = 'flex';
                document.getElementById('appContainer').classList.remove('active');
                document.getElementById('loginUsername').value = '';
                document.getElementById('loginPassword').value = '';
            });
        }

        // Data Storage
        let db = { inventory: [], patients: [], admissions: [], opd: [], pharmacy: [], childhealth: [], dentistry: [], transfers: [], employees: [] };
        let charts = {};

        function initApp() {
            loadSampleData();
            Object.keys(db).forEach(k => renderTable(k));
            updateDashboard();
        }

        function loadSampleData() {
            db.inventory = [
                { id: 'INV001', code: 'MED001', name: 'Paracetamol 500mg', category: 'Pharmaceuticals', qty: 5000, unit: 'Pieces', min: 500 },
                { id: 'INV002', code: 'MED002', name: 'Amoxicillin 250mg', category: 'Pharmaceuticals', qty: 3000, unit: 'Pieces', min: 300 },
                { id: 'INV003', code: 'EQP001', name: 'Digital Thermometer', category: 'Medical Equipment', qty: 50, unit: 'Pieces', min: 10 },
                { id: 'INV004', code: 'EQP002', name: 'Blood Pressure Monitor', category: 'Medical Equipment', qty: 25, unit: 'Pieces', min: 5 },
                { id: 'INV005', code: 'SUR001', name: 'Surgical Gloves (Sterile)', category: 'Surgical Supplies', qty: 2000, unit: 'Pairs', min: 200 },
                { id: 'INV006', code: 'SUR002', name: 'Suture Kit', category: 'Surgical Supplies', qty: 150, unit: 'Sets', min: 20 },
                { id: 'INV007', code: 'PPE001', name: 'N95 Masks', category: 'PPE', qty: 1000, unit: 'Pieces', min: 100 },
                { id: 'INV008', code: 'PPE002', name: 'Surgical Gowns', category: 'PPE', qty: 300, unit: 'Pieces', min: 50 },
                { id: 'INV009', code: 'LAB001', name: 'Blood Collection Tubes', category: 'Laboratory Supplies', qty: 5000, unit: 'Pieces', min: 500 },
                { id: 'INV010', code: 'LAB002', name: 'Urine Test Strips', category: 'Laboratory Supplies', qty: 2000, unit: 'Pieces', min: 200 },
                { id: 'INV011', code: 'CON001', name: 'Syringes 5ml', category: 'Consumables', qty: 10000, unit: 'Pieces', min: 1000 },
                { id: 'INV012', code: 'CON002', name: 'IV Cannula', category: 'Consumables', qty: 500, unit: 'Pieces', min: 50 },
                { id: 'INV013', code: 'CON003', name: 'Bandages (Rolled)', category: 'Consumables', qty: 800, unit: 'Pieces', min: 100 },
                { id: 'INV014', code: 'CON004', name: 'Cotton Wool 500g', category: 'Consumables', qty: 200, unit: 'Packs', min: 30 },
                { id: 'INV015', code: 'DIA001', name: 'Pulse Oximeter', category: 'Diagnostic Equipment', qty: 30, unit: 'Pieces', min: 5 },
                { id: 'INV016', code: 'LIN001', name: 'Hospital Bed Sheets', category: 'Linens', qty: 150, unit: 'Pieces', min: 30 },
                { id: 'INV017', code: 'CLN001', name: 'Hand Sanitizer 500ml', category: 'Cleaning Supplies', qty: 100, unit: 'Bottles', min: 20 },
                { id: 'INV018', code: 'MED003', name: 'Ibuprofen 400mg', category: 'Pharmaceuticals', qty: 4000, unit: 'Pieces', min: 400 }
            ];

            db.patients = [
                { id: 'PAT001', name: 'John Moyo', dob: '1985-03-15', gender: 'Male', phone: '0771234567', address: '12 Main St, Harare', blood: 'O+' },
                { id: 'PAT002', name: 'Mary Ndlovu', dob: '1990-07-22', gender: 'Female', phone: '0772345678', address: '45 Oak Ave, Bulawayo', blood: 'A+' },
                { id: 'PAT003', name: 'Peter Chigwedere', dob: '1978-11-08', gender: 'Male', phone: '0773456789', address: '78 River Rd, Mutare', blood: 'B+' },
                { id: 'PAT004', name: 'Grace Muzenda', dob: '1995-05-30', gender: 'Female', phone: '0774567890', address: '23 Hill St, Gweru', blood: 'AB+' }
            ];

            db.admissions = [
                { id: 'ADM001', patient: 'John Moyo', date: '2024-01-15', ward: 'General Ward', bed: 'A-12', doctor: 'Dr. Sibanda', diagnosis: 'Pneumonia', status: 'Admitted' },
                { id: 'ADM002', patient: 'Mary Ndlovu', date: '2024-01-16', ward: 'Maternity', bed: 'M-05', doctor: 'Dr. Chikwava', diagnosis: 'Labor & Delivery', status: 'Admitted' },
                { id: 'ADM003', patient: 'Peter Chigwedere', date: '2024-01-10', ward: 'ICU', bed: 'ICU-02', doctor: 'Dr. Mpofu', diagnosis: 'Cardiac Arrest Recovery', status: 'Critical' }
            ];

            db.opd = [
                { id: 'OPD001', patient: 'Grace Muzenda', date: '2024-01-18', dept: 'General Medicine', doctor: 'Dr. Ncube', complaint: 'Persistent headache', diagnosis: 'Migraine' },
                { id: 'OPD002', patient: 'John Moyo', date: '2024-01-12', dept: 'Cardiology', doctor: 'Dr. Mpofu', complaint: 'Chest pain', diagnosis: 'Angina' },
                { id: 'OPD003', patient: 'Mary Ndlovu', date: '2024-01-14', dept: 'Gynecology', doctor: 'Dr. Chikwava', complaint: 'Prenatal checkup', diagnosis: 'Normal pregnancy' }
            ];

            db.pharmacy = [
                { id: 'RX001', patient: 'John Moyo', date: '2024-01-15', med: 'Amoxicillin 500mg', dosage: '1 tablet 3x daily', qty: 21, doctor: 'Dr. Sibanda' },
                { id: 'RX002', patient: 'Grace Muzenda', date: '2024-01-18', med: 'Ibuprofen 400mg', dosage: '1 tablet when needed', qty: 10, doctor: 'Dr. Ncube' },
                { id: 'RX003', patient: 'Peter Chigwedere', date: '2024-01-10', med: 'Aspirin 100mg', dosage: '1 tablet daily', qty: 30, doctor: 'Dr. Mpofu' }
            ];

            db.childhealth = [
                { id: 'CH001', name: 'Baby Moyo', dob: '2023-06-15', guardian: 'Sarah Moyo', service: 'Vaccination', treatment: 'BCG Vaccine', date: '2024-01-10', next: '2024-02-10' },
                { id: 'CH002', name: 'Tinashe Ndlovu', dob: '2022-03-20', guardian: 'Mary Ndlovu', service: 'Growth Monitoring', treatment: 'Weight Check', date: '2024-01-15', next: '2024-02-15' }
            ];

            db.dentistry = [
                { id: 'DEN001', patient: 'Grace Muzenda', date: '2024-01-17', procedure: 'Filling', dentist: 'Dr. Zimba', teeth: 'Lower left molar', notes: 'Composite filling applied' },
                { id: 'DEN002', patient: 'John Moyo', date: '2024-01-11', procedure: 'Extraction', dentist: 'Dr. Zimba', teeth: 'Upper right wisdom tooth', notes: 'Impacted tooth removed' }
            ];

            db.transfers = [
                { id: 'TRF001', patient: 'Peter Chigwedere', date: '2024-01-12', from: 'Emergency', to: 'ICU', reason: 'Critical condition', status: 'Completed' },
                { id: 'TRF002', patient: 'John Moyo', date: '2024-01-16', from: 'ICU', to: 'General Ward', reason: 'Condition improved', status: 'Completed' }
            ];

            db.employees = [
                { id: 'EMP001', name: 'Dr. Sibanda', role: 'Doctor', dept: 'General Medicine', phone: '0781234567', email: 'sibanda@paradise.co.zw', hire: '2020-03-15' },
                { id: 'EMP002', name: 'Dr. Mpofu', role: 'Doctor', dept: 'Cardiology', phone: '0782345678', email: 'mpofu@paradise.co.zw', hire: '2019-06-20' },
                { id: 'EMP003', name: 'Nurse Takura', role: 'Nurse', dept: 'Emergency', phone: '0783456789', email: 'takura@paradise.co.zw', hire: '2021-01-10' },
                { id: 'EMP004', name: 'Dr. Zimba', role: 'Dentist', dept: 'Dentistry', phone: '0784567890', email: 'zimba@paradise.co.zw', hire: '2018-09-05' },
                { id: 'EMP005', name: 'Dr. Chikwava', role: 'Doctor', dept: 'Gynecology', phone: '0785678901', email: 'chikwava@paradise.co.zw', hire: '2017-04-12' }
            ];
        }

        // Navigation
        document.querySelectorAll('.nav-tab').forEach(tab => {
            tab.addEventListener('click', () => {
                document.querySelectorAll('.nav-tab').forEach(t => t.classList.remove('active'));
                document.querySelectorAll('.section').forEach(s => s.classList.remove('active'));
                tab.classList.add('active');
                document.getElementById(tab.dataset.section).classList.add('active');
            });
        });

        // Modal functions
        function openModal(id) { document.getElementById(id).classList.add('active'); }
        function closeModal(id) { document.getElementById(id).classList.remove('active'); document.querySelector(`#${id} form`).reset(); }

        // Generic save function
        function saveRecord(e, type) {
            e.preventDefault();
            const idField = document.getElementById(getPrefix(type) + '-id');
            const isEdit = idField.value !== '';
            const record = extractFormData(type);

            if (isEdit) {
                const idx = db[type].findIndex(r => r.id === idField.value);
                if (idx > -1) db[type][idx] = { ...db[type][idx], ...record };
            } else {
                record.id = generateId(type);
                db[type].push(record);
            }

            closeModal(getModalId(type));
            renderTable(type);
            updateDashboard();
        }

        function getPrefix(type) {
            const prefixes = { inventory: 'inv', patients: 'pat', admissions: 'adm', opd: 'opd', pharmacy: 'pharm', childhealth: 'child', dentistry: 'dent', transfers: 'trans', employees: 'emp' };
            return prefixes[type];
        }

        function getModalId(type) {
            const modals = { inventory: 'invModal', patients: 'patModal', admissions: 'admModal', opd: 'opdModal', pharmacy: 'pharmModal', childhealth: 'childModal', dentistry: 'dentModal', transfers: 'transModal', employees: 'empModal' };
            return modals[type];
        }

        function generateId(type) {
            const prefixes = { inventory: 'INV', patients: 'PAT', admissions: 'ADM', opd: 'OPD', pharmacy: 'RX', childhealth: 'CH', dentistry: 'DEN', transfers: 'TRF', employees: 'EMP' };
            return prefixes[type] + String(db[type].length + 1).padStart(3, '0');
        }

        function extractFormData(type) {
            const p = getPrefix(type);
            const data = {};
            switch(type) {
                case 'inventory': Object.assign(data, { code: gv(p+'-code'), name: gv(p+'-name'), category: gv(p+'-category'), qty: parseInt(gv(p+'-qty')), unit: gv(p+'-unit'), min: parseInt(gv(p+'-min')) }); break;
                case 'patients': Object.assign(data, { name: gv(p+'-name'), dob: gv(p+'-dob'), gender: gv(p+'-gender'), phone: gv(p+'-phone'), address: gv(p+'-address'), blood: gv(p+'-blood') }); break;
                case 'admissions': Object.assign(data, { patient: gv(p+'-patient'), date: gv(p+'-date'), ward: gv(p+'-ward'), bed: gv(p+'-bed'), doctor: gv(p+'-doctor'), diagnosis: gv(p+'-diagnosis'), status: gv(p+'-status') }); break;
                case 'opd': Object.assign(data, { patient: gv(p+'-patient'), date: gv(p+'-date'), dept: gv(p+'-dept'), doctor: gv(p+'-doctor'), complaint: gv(p+'-complaint'), diagnosis: gv(p+'-diagnosis') }); break;
                case 'pharmacy': Object.assign(data, { patient: gv(p+'-patient'), date: gv(p+'-date'), med: gv(p+'-med'), dosage: gv(p+'-dosage'), qty: parseInt(gv(p+'-qty')), doctor: gv(p+'-doctor') }); break;
                case 'childhealth': Object.assign(data, { name: gv(p+'-name'), dob: gv(p+'-dob'), guardian: gv(p+'-guardian'), service: gv(p+'-service'), treatment: gv(p+'-treatment'), date: gv(p+'-date'), next: gv(p+'-next') }); break;
                case 'dentistry': Object.assign(data, { patient: gv(p+'-patient'), date: gv(p+'-date'), procedure: gv(p+'-procedure'), dentist: gv(p+'-dentist'), teeth: gv(p+'-teeth'), notes: gv(p+'-notes') }); break;
                case 'transfers': Object.assign(data, { patient: gv(p+'-patient'), date: gv(p+'-date'), from: gv(p+'-from'), to: gv(p+'-to'), reason: gv(p+'-reason'), status: gv(p+'-status') }); break;
                case 'employees': Object.assign(data, { name: gv(p+'-name'), role: gv(p+'-role'), dept: gv(p+'-dept'), phone: gv(p+'-phone'), email: gv(p+'-email'), hire: gv(p+'-hire') }); break;
            }
            return data;
        }

        function gv(id) { return document.getElementById(id).value; }
        function sv(id, val) { document.getElementById(id).value = val; }

        function editRecord(type, id) {
            const record = db[type].find(r => r.id === id);
            if (!record) return;
            const p = getPrefix(type);
            sv(p + '-id', id);

            switch(type) {
                case 'inventory': sv(p+'-code', record.code); sv(p+'-name', record.name); sv(p+'-category', record.category); sv(p+'-qty', record.qty); sv(p+'-unit', record.unit); sv(p+'-min', record.min); break;
                case 'patients': sv(p+'-name', record.name); sv(p+'-dob', record.dob); sv(p+'-gender', record.gender); sv(p+'-phone', record.phone); sv(p+'-address', record.address); sv(p+'-blood', record.blood); break;
                case 'admissions': sv(p+'-patient', record.patient); sv(p+'-date', record.date); sv(p+'-ward', record.ward); sv(p+'-bed', record.bed); sv(p+'-doctor', record.doctor); sv(p+'-diagnosis', record.diagnosis); sv(p+'-status', record.status); break;
                case 'opd': sv(p+'-patient', record.patient); sv(p+'-date', record.date); sv(p+'-dept', record.dept); sv(p+'-doctor', record.doctor); sv(p+'-complaint', record.complaint); sv(p+'-diagnosis', record.diagnosis); break;
                case 'pharmacy': sv(p+'-patient', record.patient); sv(p+'-date', record.date); sv(p+'-med', record.med); sv(p+'-dosage', record.dosage); sv(p+'-qty', record.qty); sv(p+'-doctor', record.doctor); break;
                case 'childhealth': sv(p+'-name', record.name); sv(p+'-dob', record.dob); sv(p+'-guardian', record.guardian); sv(p+'-service', record.service); sv(p+'-treatment', record.treatment); sv(p+'-date', record.date); sv(p+'-next', record.next); break;
                case 'dentistry': sv(p+'-patient', record.patient); sv(p+'-date', record.date); sv(p+'-procedure', record.procedure); sv(p+'-dentist', record.dentist); sv(p+'-teeth', record.teeth); sv(p+'-notes', record.notes); break;
                case 'transfers': sv(p+'-patient', record.patient); sv(p+'-date', record.date); sv(p+'-from', record.from); sv(p+'-to', record.to); sv(p+'-reason', record.reason); sv(p+'-status', record.status); break;
                case 'employees': sv(p+'-name', record.name); sv(p+'-role', record.role); sv(p+'-dept', record.dept); sv(p+'-phone', record.phone); sv(p+'-email', record.email); sv(p+'-hire', record.hire); break;
            }
            openModal(getModalId(type));
        }

        function deleteRecord(type, id) {
            showConfirm('Delete this record?', () => {
                db[type] = db[type].filter(r => r.id !== id);
                renderTable(type);
                updateDashboard();
            });
        }

        // Render tables
        function renderTable(type) {
            const searchId = { inventory: 'searchInv', patients: 'searchPat', admissions: 'searchAdm', opd: 'searchOpd', pharmacy: 'searchPharm', childhealth: 'searchChild', dentistry: 'searchDent', transfers: 'searchTrans', employees: 'searchEmp' };
            const bodyId = { inventory: 'invBody', patients: 'patBody', admissions: 'admBody', opd: 'opdBody', pharmacy: 'pharmBody', childhealth: 'childBody', dentistry: 'dentBody', transfers: 'transBody', employees: 'empBody' };

            const search = document.getElementById(searchId[type])?.value.toLowerCase() || '';
            const body = document.getElementById(bodyId[type]);
            let filtered = db[type].filter(r => JSON.stringify(r).toLowerCase().includes(search));

            const actions = (t, id) => `<button onclick="editRecord('${t}','${id}')" class="btn btn-primary text-xs mr-1"><i class="fas fa-edit"></i></button><button onclick="deleteRecord('${t}','${id}')" class="btn btn-danger text-xs"><i class="fas fa-trash"></i></button>`;

            switch(type) {
                case 'inventory':
                    body.innerHTML = filtered.map(r => {
                        const status = r.qty <= r.min ? 'Low Stock' : r.qty <= r.min * 2 ? 'Medium' : 'In Stock';
                        const badge = r.qty <= r.min ? 'badge-danger' : r.qty <= r.min * 2 ? 'badge-warning' : 'badge-success';
                        return `<tr><td>${r.code}</td><td>${r.name}</td><td>${r.category}</td><td>${r.qty}</td><td>${r.unit}</td><td>${r.min}</td><td><span class="badge ${badge}">${status}</span></td><td>${actions(type, r.id)}</td></tr>`;
                    }).join('');
                    break;
                case 'patients':
                    body.innerHTML = filtered.map(r => `<tr><td>${r.id}</td><td>${r.name}</td><td>${r.dob}</td><td>${r.gender}</td><td>${r.phone}</td><td>${r.address}</td><td>${r.blood || '-'}</td><td>${actions(type, r.id)}</td></tr>`).join('');
                    break;
                case 'admissions':
                    body.innerHTML = filtered.map(r => {
                        const badge = r.status === 'Admitted' ? 'badge-info' : r.status === 'Discharged' ? 'badge-success' : r.status === 'Critical' ? 'badge-danger' : 'badge-warning';
                        return `<tr><td>${r.id}</td><td>${r.patient}</td><td>${r.ward}</td><td>${r.bed}</td><td>${r.doctor}</td><td>${r.date}</td><td>${r.diagnosis}</td><td><span class="badge ${badge}">${r.status}</span></td><td>${actions(type, r.id)}</td></tr>`;
                    }).join('');
                    break;
                case 'opd':
                    body.innerHTML = filtered.map(r => `<tr><td>${r.id}</td><td>${r.patient}</td><td>${r.dept}</td><td>${r.doctor}</td><td>${r.date}</td><td>${r.complaint}</td><td>${r.diagnosis || '-'}</td><td>${actions(type, r.id)}</td></tr>`).join('');
                    break;
                case 'pharmacy':
                    body.innerHTML = filtered.map(r => `<tr><td>${r.id}</td><td>${r.patient}</td><td>${r.med}</td><td>${r.dosage}</td><td>${r.qty}</td><td>${r.doctor}</td><td>${r.date}</td><td>${actions(type, r.id)}</td></tr>`).join('');
                    break;
                case 'childhealth':
                    body.innerHTML = filtered.map(r => `<tr><td>${r.id}</td><td>${r.name}</td><td>${r.dob}</td><td>${r.guardian}</td><td>${r.service}</td><td>${r.treatment || '-'}</td><td>${r.date}</td><td>${r.next || '-'}</td><td>${actions(type, r.id)}</td></tr>`).join('');
                    break;
                case 'dentistry':
                    body.innerHTML = filtered.map(r => `<tr><td>${r.id}</td><td>${r.patient}</td><td>${r.procedure}</td><td>${r.dentist}</td><td>${r.date}</td><td>${r.teeth || '-'}</td><td>${r.notes || '-'}</td><td>${actions(type, r.id)}</td></tr>`).join('');
                    break;
                case 'transfers':
                    body.innerHTML = filtered.map(r => {
                        const badge = r.status === 'Completed' ? 'badge-success' : r.status === 'In Transit' ? 'badge-info' : r.status === 'Cancelled' ? 'badge-danger' : 'badge-warning';
                        return `<tr><td>${r.id}</td><td>${r.patient}</td><td>${r.from}</td><td>${r.to}</td><td>${r.reason}</td><td>${r.date}</td><td><span class="badge ${badge}">${r.status}</span></td><td>${actions(type, r.id)}</td></tr>`;
                    }).join('');
                    break;
                case 'employees':
                    body.innerHTML = filtered.map(r => `<tr><td>${r.id}</td><td>${r.name}</td><td>${r.role}</td><td>${r.dept}</td><td>${r.phone}</td><td>${r.email || '-'}</td><td>${r.hire}</td><td>${actions(type, r.id)}</td></tr>`).join('');
                    break;
            }
        }

        // Dashboard
        function updateDashboard() {
            document.getElementById('stat-inv').textContent = db.inventory.length;
            document.getElementById('stat-pat').textContent = db.patients.length;
            document.getElementById('stat-adm').textContent = db.admissions.filter(a => a.status === 'Admitted' || a.status === 'Critical').length;
            document.getElementById('stat-opd').textContent = db.opd.length;
            document.getElementById('stat-pharm').textContent = db.pharmacy.length;
            document.getElementById('stat-emp').textContent = db.employees.length;
            updateCharts();
        }

        function updateCharts() {
            const colors = ['#374151', '#4b5563', '#6b7280', '#9ca3af', '#1f2937', '#111827', '#d1d5db'];

            // Inventory by category
            const invCat = {};
            db.inventory.forEach(i => invCat[i.category] = (invCat[i.category] || 0) + i.qty);
            renderChart('chartInventory', 'doughnut', Object.keys(invCat), Object.values(invCat), colors);

            // Admissions by month
            const admMonth = {};
            db.admissions.forEach(a => { const m = a.date.substring(0, 7); admMonth[m] = (admMonth[m] || 0) + 1; });
            renderChart('chartAdmissions', 'bar', Object.keys(admMonth).sort(), Object.keys(admMonth).sort().map(k => admMonth[k]), ['#374151']);

            // OPD by department
            const opdDept = {};
            db.opd.forEach(o => opdDept[o.dept] = (opdDept[o.dept] || 0) + 1);
            renderChart('chartOPD', 'pie', Object.keys(opdDept), Object.values(opdDept), colors);

            // Staff by role
            const empRole = {};
            db.employees.forEach(e => empRole[e.role] = (empRole[e.role] || 0) + 1);
            renderChart('chartStaff', 'pie', Object.keys(empRole), Object.values(empRole), colors);

            // Pharmacy
            const pharmMed = {};
            db.pharmacy.forEach(p => pharmMed[p.med] = (pharmMed[p.med] || 0) + p.qty);
            renderChart('chartPharmacy', 'bar', Object.keys(pharmMed), Object.values(pharmMed), ['#4b5563']);
        }

        function renderChart(id, type, labels, data, colors) {
            if (charts[id]) charts[id].destroy();
            const ctx = document.getElementById(id);
            charts[id] = new Chart(ctx, {
                type: type,
                data: { labels: labels, datasets: [{ data: data, backgroundColor: colors, borderColor: type === 'bar' ? colors[0] : undefined, borderWidth: type === 'bar' ? 0 : 1 }] },
                options: { responsive: true, maintainAspectRatio: true, plugins: { legend: { display: type !== 'bar', position: 'bottom' } } }
            });
        }

        // Download functions
        function downloadCSV(type) {
            let csv = '';
            const headers = {
                inventory: ['ID','Code','Name','Category','Quantity','Unit','Min Stock'],
                patients: ['ID','Name','DOB','Gender','Phone','Address','Blood Group'],
                admissions: ['ID','Patient','Date','Ward','Bed','Doctor','Diagnosis','Status'],
                opd: ['ID','Patient','Date','Department','Doctor','Complaint','Diagnosis'],
                pharmacy: ['ID','Patient','Date','Medication','Dosage','Quantity','Doctor'],
                childhealth: ['ID','Name','DOB','Guardian','Service','Treatment','Date','Next Visit'],
                dentistry: ['ID','Patient','Date','Procedure','Dentist','Teeth','Notes'],
                transfers: ['ID','Patient','Date','From','To','Reason','Status'],
                employees: ['ID','Name','Role','Department','Phone','Email','Hire Date']
            };
            csv = headers[type].join(',') + '\n';
            db[type].forEach(r => {
                const row = Object.values(r).map(v => `"${v}"`).join(',');
                csv += row + '\n';
            });
            downloadFile(csv, `${type}_data.csv`, 'text/csv');
        }

        function downloadAllData() {
            const allData = { ...db, exportDate: new Date().toISOString(), hospital: 'Paradise Hospital' };
            downloadFile(JSON.stringify(allData, null, 2), 'paradise_hospital_data.json', 'application/json');
        }

        function downloadFile(content, filename, type) {
            const blob = new Blob([content], { type: type });
            const url = URL.createObjectURL(blob);
            const a = document.createElement('a');
            a.href = url;
            a.download = filename;
            a.click();
            URL.revokeObjectURL(url);
        }

        // Confirm dialog
        function showConfirm(msg, onConfirm) {
            const modal = document.createElement('div');
            modal.className = 'modal active';
            modal.innerHTML = `<div class="modal-content" style="max-width:350px;text-align:center"><p class="mb-4">${msg}</p><div class="flex gap-2 justify-center"><button class="btn btn-danger" onclick="this.closest('.modal').remove();(${onConfirm})()">Yes</button><button class="btn btn-secondary" onclick="this.closest('.modal').remove()">No</button></div></div>`;
            document.body.appendChild(modal);
        }

        // Dark mode
        function toggleDark() { document.documentElement.classList.toggle('dark'); }
        if (window.matchMedia('(prefers-color-scheme: dark)').matches) document.documentElement.classList.add('dark');
    </script>
</body>
</html>
