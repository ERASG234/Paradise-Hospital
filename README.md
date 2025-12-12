
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Paradise Hospital - Management System</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <script src="https://cdn.jsdelivr.net/npm/chart.js@4.4.0/dist/chart.umd.min.js"></script>
    <link href="https://fonts.googleapis.com/css2?family=Poppins:wght@300;400;500;600;700&family=Fira+Code:wght@400;500&display=swap" rel="stylesheet">
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <style>
        * {
            font-family: 'Poppins', sans-serif;
        }

 :root {
            --primary: #374151;
            --primary-dark: #1f2937;
            --secondary: #4b5563;
            --accent: #6b7280;
            --bg-light: #f3f4f6;
            --bg-card: #ffffff;
            --text-dark: #111827;
            --border: #d1d5db;
            --success: #10b981;
            --warning: #f59e0b;
            --danger: #ef4444;
            --chart-1: #374151;
            --chart-2: #6b7280;
            --chart-3: #9ca3af;
            --chart-4: #4b5563;
            --chart-5: #1f2937;
        }

  .dark {
            --bg-light: #111827;
            --bg-card: #1f2937;
            --text-dark: #f9fafb;
            --border: #374151;
        }

        body {
            background: linear-gradient(135deg, var(--bg-light) 0%, #e5e7eb 100%);
            color: var(--text-dark);
            min-height: 100vh;
        }

        .dark body {
            background: linear-gradient(135deg, #111827 0%, #1f2937 100%);
        }

        .card {
            background: var(--bg-card);
            border-radius: 16px;
            box-shadow: 0 4px 6px -1px rgba(0, 0, 0, 0.1), 0 2px 4px -1px rgba(0, 0, 0, 0.06);
            border: 1px solid var(--border);
            transition: all 0.3s ease;
        }

        .card:hover {
            transform: translateY(-2px);
            box-shadow: 0 10px 15px -3px rgba(0, 0, 0, 0.1), 0 4px 6px -2px rgba(0, 0, 0, 0.05);
        }

        .stat-card {
            background: linear-gradient(135deg, #374151 0%, #4b5563 100%);
            color: white;
            border: none;
            animation: slideIn 0.5s ease forwards;
            opacity: 0;
        }

        .stat-card:nth-child(1) { animation-delay: 0.1s; background: linear-gradient(135deg, #374151 0%, #4b5563 100%); }
        .stat-card:nth-child(2) { animation-delay: 0.2s; background: linear-gradient(135deg, #4b5563 0%, #6b7280 100%); }
        .stat-card:nth-child(3) { animation-delay: 0.3s; background: linear-gradient(135deg, #1f2937 0%, #374151 100%); }
        .stat-card:nth-child(4) { animation-delay: 0.4s; background: linear-gradient(135deg, #6b7280 0%, #9ca3af 100%); }

        @keyframes slideIn {
            from {
                opacity: 0;
                transform: translateY(20px);
            }
            to {
                opacity: 1;
                transform: translateY(0);
            }
        }

        .btn {
            padding: 0.5rem 1.5rem;
            border-radius: 8px;
            font-weight: 500;
            transition: all 0.3s ease;
            cursor: pointer;
            border: none;
        }

        .btn-primary {
            background: var(--primary);
            color: white;
        }

        .btn-primary:hover {
            background: var(--primary-dark);
            transform: scale(1.05);
        }

        .btn-success {
            background: var(--success);
            color: white;
        }

        .btn-success:hover {
            background: #059669;
        }

        .btn-danger {
            background: var(--danger);
            color: white;
        }

        .btn-danger:hover {
            background: #dc2626;
        }

        .nav-item {
            padding: 1rem 1.5rem;
            cursor: pointer;
            transition: all 0.3s ease;
            border-left: 3px solid transparent;
            display: flex;
            align-items: center;
            gap: 0.75rem;
        }

        .nav-item:hover {
            background: rgba(6, 182, 212, 0.1);
            border-left-color: var(--primary);
        }

        .nav-item.active {
            background: rgba(6, 182, 212, 0.15);
            border-left-color: var(--primary);
            font-weight: 600;
            color: var(--primary);
        }

        .section {
            display: none;
        }

        .section.active {
            display: block;
            animation: fadeIn 0.3s ease;
        }

        @keyframes fadeIn {
            from {
                opacity: 0;
            }
            to {
                opacity: 1;
            }
        }

        input, select, textarea {
            width: 100%;
            padding: 0.75rem;
            border: 2px solid var(--border);
            border-radius: 8px;
            background: var(--bg-card);
            color: var(--text-dark);
            font-size: 16px;
            transition: all 0.3s ease;
        }

        input:focus, select:focus, textarea:focus {
            outline: none;
            border-color: var(--primary);
            box-shadow: 0 0 0 3px rgba(6, 182, 212, 0.1);
        }

        table {
            width: 100%;
            border-collapse: separate;
            border-spacing: 0;
        }

        th {
            background: var(--primary);
            color: white;
            padding: 1rem;
            text-align: left;
            font-weight: 600;
        }

        th:first-child {
            border-top-left-radius: 8px;
        }

        th:last-child {
            border-top-right-radius: 8px;
        }

        td {
            padding: 1rem;
            border-bottom: 1px solid var(--border);
        }

        tr:hover {
            background: rgba(6, 182, 212, 0.05);
        }

        .badge {
            padding: 0.25rem 0.75rem;
            border-radius: 12px;
            font-size: 0.875rem;
            font-weight: 500;
            display: inline-block;
        }

        .badge-success {
            background: #d1fae5;
            color: #065f46;
        }

        .badge-warning {
            background: #fef3c7;
            color: #92400e;
        }

        .badge-danger {
            background: #fee2e2;
            color: #991b1b;
        }

        .badge-info {
            background: #dbeafe;
            color: #1e40af;
        }

        .dark .badge-success {
            background: #065f46;
            color: #d1fae5;
        }

        .dark .badge-warning {
            background: #92400e;
            color: #fef3c7;
        }

        .dark .badge-danger {
            background: #991b1b;
            color: #fee2e2;
        }

        .dark .badge-info {
            background: #1e40af;
            color: #dbeafe;
        }

        .modal {
            display: none;
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: rgba(0, 0, 0, 0.5);
            z-index: 1000;
            align-items: center;
            justify-content: center;
            backdrop-filter: blur(4px);
        }

        .modal.active {
            display: flex;
            animation: fadeIn 0.3s ease;
        }

        .modal-content {
            background: var(--bg-card);
            padding: 2rem;
            border-radius: 16px;
            max-width: 600px;
            width: 90%;
            max-height: 90vh;
            overflow-y: auto;
            animation: slideUp 0.3s ease;
        }

        @keyframes slideUp {
            from {
                transform: translateY(50px);
                opacity: 0;
            }
            to {
                transform: translateY(0);
                opacity: 1;
            }
        }

        .search-box {
            position: relative;
        }

        .search-box i {
            position: absolute;
            left: 1rem;
            top: 50%;
            transform: translateY(-50%);
            color: var(--primary);
        }

        .search-box input {
            padding-left: 3rem;
        }

        @media (max-width: 768px) {
            .sidebar {
                position: fixed;
                left: -100%;
                top: 0;
                height: 100vh;
                z-index: 999;
                transition: left 0.3s ease;
            }

            .sidebar.active {
                left: 0;
            }

            .mobile-menu-btn {
                display: block !important;
            }
        }

        .login-container {
            min-height: 100vh;
            display: flex;
            align-items: center;
            justify-content: center;
            background: linear-gradient(135deg, #1f2937 0%, #374151 50%, #4b5563 100%);
            padding: 2rem;
        }

        .login-card {
            background: white;
            border-radius: 24px;
            padding: 3rem;
            max-width: 450px;
            width: 100%;
            box-shadow: 0 20px 60px rgba(0, 0, 0, 0.3);
            animation: loginSlideIn 0.6s ease;
        }

        @keyframes loginSlideIn {
            from {
                opacity: 0;
                transform: translateY(30px);
            }
            to {
                opacity: 1;
                transform: translateY(0);
            }
        }

        .login-logo {
            width: 80px;
            height: 80px;
            background: linear-gradient(135deg, #374151 0%, #4b5563 100%);
            border-radius: 20px;
            display: flex;
            align-items: center;
            justify-content: center;
            margin: 0 auto 1.5rem;
            font-size: 2.5rem;
            color: white;
            box-shadow: 0 10px 30px rgba(55, 65, 81, 0.4);
        }

        .app-container {
            display: none;
        }

        .app-container.active {
            display: flex;
        }

        .error-message {
            background: #fee2e2;
            color: #991b1b;
            padding: 0.75rem;
            border-radius: 8px;
            margin-bottom: 1rem;
            display: none;
            animation: shake 0.3s ease;
        }

        .error-message.show {
            display: block;
        }

        @keyframes shake {
            0%, 100% { transform: translateX(0); }
            25% { transform: translateX(-10px); }
            75% { transform: translateX(10px); }
        }
    </style>
</head>
<body>
    <!-- Login Screen -->
    <div id="loginScreen" class="login-container">
        <div class="login-card">
            <div class="login-logo">
                <i class="fas fa-hospital"></i>
            </div>
            <h1 class="text-3xl font-bold text-center mb-2" style="color: #374151;">Paradise Hospital</h1>
            <p class="text-center text-gray-600 mb-6">Management System</p>

            <div id="loginError" class="error-message">
                <i class="fas fa-exclamation-circle"></i> Invalid username or password
            </div>

            <form onsubmit="handleLogin(event)" class="space-y-4">
                <div>
                    <label class="block mb-2 font-medium text-gray-700">Username</label>
                    <div class="relative">
                        <i class="fas fa-user absolute left-3 top-1/2 transform -translate-y-1/2" style="color: #374151;"></i>
                        <input type="text" id="loginUsername" required style="padding-left: 2.5rem;" placeholder="Enter username">
                    </div>
                </div>
                <div>
                    <label class="block mb-2 font-medium text-gray-700">Password</label>
                    <div class="relative">
                        <i class="fas fa-lock absolute left-3 top-1/2 transform -translate-y-1/2" style="color: #374151;"></i>
                        <input type="password" id="loginPassword" required style="padding-left: 2.5rem;" placeholder="Enter password">
                    </div>
                </div>
                <button type="submit" class="btn btn-primary w-full" style="margin-top: 1.5rem; padding: 0.875rem;">
                    <i class="fas fa-sign-in-alt"></i> Login
                </button>
            </form>

            <div class="mt-6 pt-6 border-t border-gray-200 text-center text-sm text-gray-500">
                <p>Powered by <span class="font-semibold" style="color: #374151;">Exalt Consulting</span></p>
            </div>
        </div>
    </div>

    <!-- Main Application -->
    <div id="appContainer" class="app-container flex min-h-screen">
        <!-- Sidebar -->
        <div class="sidebar w-64 card rounded-none border-r" style="border-radius: 0;">
            <div class="p-6 border-b border-gray-200 dark:border-gray-700">
                <h1 class="text-2xl font-bold flex items-center gap-2" style="color: var(--primary);">
                    <i class="fas fa-hospital"></i>
                    Paradise Hospital
                </h1>
                <p class="text-sm text-gray-500 mt-1">Management System</p>
            </div>

            <nav class="p-4">
                <div class="nav-item active" data-section="dashboard">
                    <i class="fas fa-chart-line"></i>
                    Dashboard
                </div>
                <div class="nav-item" data-section="inventory">
                    <i class="fas fa-boxes"></i>
                    Inventory
                </div>
                <div class="nav-item" data-section="patients">
                    <i class="fas fa-user-injured"></i>
                    Patients
                </div>
                <div class="nav-item" data-section="visits">
                    <i class="fas fa-notes-medical"></i>
                    Visits
                </div>
                <div class="nav-item" data-section="employees">
                    <i class="fas fa-user-md"></i>
                    Employees
                </div>
            </nav>

            <div class="p-4 mt-auto border-t border-gray-200 dark:border-gray-700">
                <div class="mb-3 px-2 py-2 bg-gray-100 dark:bg-gray-800 rounded-lg">
                    <p class="text-xs text-gray-600 dark:text-gray-400">Logged in as:</p>
                    <p class="font-semibold text-sm" id="loggedInUser">Tendai Manjeru</p>
                </div>
                <button onclick="toggleDarkMode()" class="btn btn-primary w-full mb-2">
                    <i class="fas fa-moon"></i> Toggle Theme
                </button>
                <button onclick="handleLogout()" class="btn btn-danger w-full">
                    <i class="fas fa-sign-out-alt"></i> Logout
                </button>
            </div>
        </div>

        <!-- Main Content -->
        <div class="flex-1 p-8">
            <button class="mobile-menu-btn btn btn-primary mb-4 hidden">
                <i class="fas fa-bars"></i> Menu
            </button>

            <!-- Dashboard Section -->
            <div id="dashboard" class="section active">
                <div class="flex justify-between items-center mb-6">
                    <h2 class="text-3xl font-bold">Dashboard Overview</h2>
                    <button onclick="downloadAllData()" class="btn btn-success">
                        <i class="fas fa-download"></i> Download All Data
                    </button>
                </div>

                <!-- Stats Grid -->
                <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-4 gap-6 mb-8">
                    <div class="stat-card card p-6">
                        <div class="flex items-center justify-between">
                            <div>
                                <p class="text-sm opacity-90">Total Inventory Items</p>
                                <h3 class="text-3xl font-bold mt-2" id="stat-inventory">0</h3>
                            </div>
                            <i class="fas fa-boxes text-4xl opacity-50"></i>
                        </div>
                    </div>

                    <div class="stat-card card p-6" style="background: linear-gradient(135deg, #8b5cf6 0%, #a78bfa 100%);">
                        <div class="flex items-center justify-between">
                            <div>
                                <p class="text-sm opacity-90">Total Patients</p>
                                <h3 class="text-3xl font-bold mt-2" id="stat-patients">0</h3>
                            </div>
                            <i class="fas fa-user-injured text-4xl opacity-50"></i>
                        </div>
                    </div>

                    <div class="stat-card card p-6" style="background: linear-gradient(135deg, #f59e0b 0%, #fbbf24 100%);">
                        <div class="flex items-center justify-between">
                            <div>
                                <p class="text-sm opacity-90">Hospital Visits</p>
                                <h3 class="text-3xl font-bold mt-2" id="stat-visits">0</h3>
                            </div>
                            <i class="fas fa-notes-medical text-4xl opacity-50"></i>
                        </div>
                    </div>

                    <div class="stat-card card p-6" style="background: linear-gradient(135deg, #10b981 0%, #34d399 100%);">
                        <div class="flex items-center justify-between">
                            <div>
                                <p class="text-sm opacity-90">Staff Members</p>
                                <h3 class="text-3xl font-bold mt-2" id="stat-employees">0</h3>
                            </div>
                            <i class="fas fa-user-md text-4xl opacity-50"></i>
                        </div>
                    </div>
                </div>

                <!-- Charts -->
                <div class="grid grid-cols-1 lg:grid-cols-2 gap-6 mb-8">
                    <div class="card p-6">
                        <h3 class="text-xl font-semibold mb-4">Inventory Status</h3>
                        <canvas id="inventoryChart"></canvas>
                    </div>

                    <div class="card p-6">
                        <h3 class="text-xl font-semibold mb-4">Patient Admissions Trend</h3>
                        <canvas id="admissionsChart"></canvas>
                    </div>
                </div>

                <div class="grid grid-cols-1 lg:grid-cols-2 gap-6">
                    <div class="card p-6">
                        <h3 class="text-xl font-semibold mb-4">Visits by Department</h3>
                        <canvas id="visitsChart"></canvas>
                    </div>

                    <div class="card p-6">
                        <h3 class="text-xl font-semibold mb-4">Employee Distribution</h3>
                        <canvas id="employeeChart"></canvas>
                    </div>
                </div>
            </div>

            <!-- Inventory Section -->
            <div id="inventory" class="section">
                <div class="flex justify-between items-center mb-6">
                    <h2 class="text-3xl font-bold">Inventory Management</h2>
                    <div class="flex gap-3">
                        <button onclick="downloadData('inventory')" class="btn btn-success">
                            <i class="fas fa-download"></i> Export
                        </button>
                        <button onclick="openModal('inventoryModal')" class="btn btn-primary">
                            <i class="fas fa-plus"></i> Add Item
                        </button>
                    </div>
                </div>

                <div class="card p-6 mb-6">
                    <div class="search-box">
                        <i class="fas fa-search"></i>
                        <input type="text" id="inventorySearch" placeholder="Search inventory..." onkeyup="filterTable('inventoryTable', 'inventorySearch')">
                    </div>
                </div>

                <div class="card overflow-hidden">
                    <div style="overflow-x: auto;">
                        <table id="inventoryTable">
                            <thead>
                                <tr>
                                    <th>Item Name</th>
                                    <th>Category</th>
                                    <th>Quantity</th>
                                    <th>Unit Price</th>
                                    <th>Supplier</th>
                                    <th>Status</th>
                                    <th>Actions</th>
                                </tr>
                            </thead>
                            <tbody id="inventoryBody">
                            </tbody>
                        </table>
                    </div>
                </div>
            </div>

            <!-- Patients Section -->
            <div id="patients" class="section">
                <div class="flex justify-between items-center mb-6">
                    <h2 class="text-3xl font-bold">Patient Registration & Admissions</h2>
                    <div class="flex gap-3">
                        <button onclick="downloadData('patients')" class="btn btn-success">
                            <i class="fas fa-download"></i> Export
                        </button>
                        <button onclick="openModal('patientModal')" class="btn btn-primary">
                            <i class="fas fa-plus"></i> Register Patient
                        </button>
                    </div>
                </div>

                <div class="card p-6 mb-6">
                    <div class="search-box">
                        <i class="fas fa-search"></i>
                        <input type="text" id="patientSearch" placeholder="Search patients..." onkeyup="filterTable('patientTable', 'patientSearch')">
                    </div>
                </div>

                <div class="card overflow-hidden">
                    <div style="overflow-x: auto;">
                        <table id="patientTable">
                            <thead>
                                <tr>
                                    <th>Patient ID</th>
                                    <th>Name</th>
                                    <th>Age</th>
                                    <th>Gender</th>
                                    <th>Contact</th>
                                    <th>Admission Date</th>
                                    <th>Status</th>
                                    <th>Actions</th>
                                </tr>
                            </thead>
                            <tbody id="patientBody">
                            </tbody>
                        </table>
                    </div>
                </div>
            </div>

            <!-- Visits Section -->
            <div id="visits" class="section">
                <div class="flex justify-between items-center mb-6">
                    <h2 class="text-3xl font-bold">Hospital Visits Log</h2>
                    <div class="flex gap-3">
                        <button onclick="downloadData('visits')" class="btn btn-success">
                            <i class="fas fa-download"></i> Export
                        </button>
                        <button onclick="openModal('visitModal')" class="btn btn-primary">
                            <i class="fas fa-plus"></i> Log Visit
                        </button>
                    </div>
                </div>

                <div class="card p-6 mb-6">
                    <div class="search-box">
                        <i class="fas fa-search"></i>
                        <input type="text" id="visitSearch" placeholder="Search visits..." onkeyup="filterTable('visitTable', 'visitSearch')">
                    </div>
                </div>

                <div class="card overflow-hidden">
                    <div style="overflow-x: auto;">
                        <table id="visitTable">
                            <thead>
                                <tr>
                                    <th>Visit ID</th>
                                    <th>Patient Name</th>
                                    <th>Department</th>
                                    <th>Doctor</th>
                                    <th>Visit Date</th>
                                    <th>Purpose</th>
                                    <th>Actions</th>
                                </tr>
                            </thead>
                            <tbody id="visitBody">
                            </tbody>
                        </table>
                    </div>
                </div>
            </div>

            <!-- Employees Section -->
            <div id="employees" class="section">
                <div class="flex justify-between items-center mb-6">
                    <h2 class="text-3xl font-bold">Employee Database</h2>
                    <div class="flex gap-3">
                        <button onclick="downloadData('employees')" class="btn btn-success">
                            <i class="fas fa-download"></i> Export
                        </button>
                        <button onclick="openModal('employeeModal')" class="btn btn-primary">
                            <i class="fas fa-plus"></i> Add Employee
                        </button>
                    </div>
                </div>

                <div class="card p-6 mb-6">
                    <div class="search-box">
                        <i class="fas fa-search"></i>
                        <input type="text" id="employeeSearch" placeholder="Search employees..." onkeyup="filterTable('employeeTable', 'employeeSearch')">
                    </div>
                </div>

                <div class="card overflow-hidden">
                    <div style="overflow-x: auto;">
                        <table id="employeeTable">
                            <thead>
                                <tr>
                                    <th>Employee ID</th>
                                    <th>Name</th>
                                    <th>Role</th>
                                    <th>Department</th>
                                    <th>Contact</th>
                                    <th>Hire Date</th>
                                    <th>Salary</th>
                                    <th>Actions</th>
                                </tr>
                            </thead>
                            <tbody id="employeeBody">
                            </tbody>
                        </table>
                    </div>
                </div>
            </div>
        </div>
    </div>

    <!-- Inventory Modal -->
    <div id="inventoryModal" class="modal">
        <div class="modal-content">
            <div class="flex justify-between items-center mb-6">
                <h3 class="text-2xl font-bold">Add Inventory Item</h3>
                <button onclick="closeModal('inventoryModal')" class="text-2xl">&times;</button>
            </div>
            <form onsubmit="saveInventory(event)">
                <div class="space-y-4">
                    <input type="hidden" id="inv-id">
                    <div>
                        <label class="block mb-2 font-medium">Item Name</label>
                        <input type="text" id="inv-name" required>
                    </div>
                    <div>
                        <label class="block mb-2 font-medium">Category</label>
                        <select id="inv-category" required>
                            <option value="">Select Category</option>
                            <option value="Medical Supplies">Medical Supplies</option>
                            <option value="Equipment">Equipment</option>
                            <option value="Pharmaceuticals">Pharmaceuticals</option>
                            <option value="Surgical">Surgical</option>
                            <option value="PPE">PPE</option>
                        </select>
                    </div>
                    <div class="grid grid-cols-2 gap-4">
                        <div>
                            <label class="block mb-2 font-medium">Quantity</label>
                            <input type="number" id="inv-quantity" required min="0">
                        </div>
                        <div>
                            <label class="block mb-2 font-medium">Unit Price ($)</label>
                            <input type="number" step="0.01" id="inv-price" required min="0">
                        </div>
                    </div>
                    <div>
                        <label class="block mb-2 font-medium">Supplier</label>
                        <input type="text" id="inv-supplier" required>
                    </div>
                </div>
                <div class="flex gap-3 mt-6">
                    <button type="submit" class="btn btn-primary flex-1">Save</button>
                    <button type="button" onclick="closeModal('inventoryModal')" class="btn btn-danger">Cancel</button>
                </div>
            </form>
        </div>
    </div>

    <!-- Patient Modal -->
    <div id="patientModal" class="modal">
        <div class="modal-content">
            <div class="flex justify-between items-center mb-6">
                <h3 class="text-2xl font-bold">Register Patient</h3>
                <button onclick="closeModal('patientModal')" class="text-2xl">&times;</button>
            </div>
            <form onsubmit="savePatient(event)">
                <div class="space-y-4">
                    <input type="hidden" id="pat-id">
                    <div>
                        <label class="block mb-2 font-medium">Full Name</label>
                        <input type="text" id="pat-name" required>
                    </div>
                    <div class="grid grid-cols-2 gap-4">
                        <div>
                            <label class="block mb-2 font-medium">Age</label>
                            <input type="number" id="pat-age" required min="0" max="150">
                        </div>
                        <div>
                            <label class="block mb-2 font-medium">Gender</label>
                            <select id="pat-gender" required>
                                <option value="">Select Gender</option>
                                <option value="Male">Male</option>
                                <option value="Female">Female</option>
                                <option value="Other">Other</option>
                            </select>
                        </div>
                    </div>
                    <div>
                        <label class="block mb-2 font-medium">Contact Number</label>
                        <input type="tel" id="pat-contact" required>
                    </div>
                    <div>
                        <label class="block mb-2 font-medium">Admission Date</label>
                        <input type="date" id="pat-admission" required>
                    </div>
                    <div>
                        <label class="block mb-2 font-medium">Status</label>
                        <select id="pat-status" required>
                            <option value="Admitted">Admitted</option>
                            <option value="Discharged">Discharged</option>
                            <option value="Observation">Observation</option>
                        </select>
                    </div>
                </div>
                <div class="flex gap-3 mt-6">
                    <button type="submit" class="btn btn-primary flex-1">Save</button>
                    <button type="button" onclick="closeModal('patientModal')" class="btn btn-danger">Cancel</button>
                </div>
            </form>
        </div>
    </div>

    <!-- Visit Modal -->
    <div id="visitModal" class="modal">
        <div class="modal-content">
            <div class="flex justify-between items-center mb-6">
                <h3 class="text-2xl font-bold">Log Hospital Visit</h3>
                <button onclick="closeModal('visitModal')" class="text-2xl">&times;</button>
            </div>
            <form onsubmit="saveVisit(event)">
                <div class="space-y-4">
                    <input type="hidden" id="vis-id">
                    <div>
                        <label class="block mb-2 font-medium">Patient Name</label>
                        <input type="text" id="vis-patient" required>
                    </div>
                    <div>
                        <label class="block mb-2 font-medium">Department</label>
                        <select id="vis-department" required>
                            <option value="">Select Department</option>
                            <option value="Emergency">Emergency</option>
                            <option value="Cardiology">Cardiology</option>
                            <option value="Pediatrics">Pediatrics</option>
                            <option value="Orthopedics">Orthopedics</option>
                            <option value="General Medicine">General Medicine</option>
                            <option value="Surgery">Surgery</option>
                        </select>
                    </div>
                    <div>
                        <label class="block mb-2 font-medium">Doctor Name</label>
                        <input type="text" id="vis-doctor" required>
                    </div>
                    <div>
                        <label class="block mb-2 font-medium">Visit Date</label>
                        <input type="date" id="vis-date" required>
                    </div>
                    <div>
                        <label class="block mb-2 font-medium">Purpose of Visit</label>
                        <textarea id="vis-purpose" rows="3" required></textarea>
                    </div>
                </div>
                <div class="flex gap-3 mt-6">
                    <button type="submit" class="btn btn-primary flex-1">Save</button>
                    <button type="button" onclick="closeModal('visitModal')" class="btn btn-danger">Cancel</button>
                </div>
            </form>
        </div>
    </div>

    <!-- Employee Modal -->
    <div id="employeeModal" class="modal">
        <div class="modal-content">
            <div class="flex justify-between items-center mb-6">
                <h3 class="text-2xl font-bold">Add Employee</h3>
                <button onclick="closeModal('employeeModal')" class="text-2xl">&times;</button>
            </div>
            <form onsubmit="saveEmployee(event)">
                <div class="space-y-4">
                    <input type="hidden" id="emp-id">
                    <div>
                        <label class="block mb-2 font-medium">Full Name</label>
                        <input type="text" id="emp-name" required>
                    </div>
                    <div>
                        <label class="block mb-2 font-medium">Role</label>
                        <select id="emp-role" required>
                            <option value="">Select Role</option>
                            <option value="Doctor">Doctor</option>
                            <option value="Nurse">Nurse</option>
                            <option value="Pharmacist">Pharmacist</option>
                            <option value="Technician">Technician</option>
                            <option value="Administrator">Administrator</option>
                            <option value="Support Staff">Support Staff</option>
                        </select>
                    </div>
                    <div>
                        <label class="block mb-2 font-medium">Department</label>
                        <select id="emp-department" required>
                            <option value="">Select Department</option>
                            <option value="Emergency">Emergency</option>
                            <option value="Cardiology">Cardiology</option>
                            <option value="Pediatrics">Pediatrics</option>
                            <option value="Orthopedics">Orthopedics</option>
                            <option value="General Medicine">General Medicine</option>
                            <option value="Surgery">Surgery</option>
                            <option value="Administration">Administration</option>
                        </select>
                    </div>
                    <div>
                        <label class="block mb-2 font-medium">Contact Number</label>
                        <input type="tel" id="emp-contact" required>
                    </div>
                    <div class="grid grid-cols-2 gap-4">
                        <div>
                            <label class="block mb-2 font-medium">Hire Date</label>
                            <input type="date" id="emp-hire" required>
                        </div>
                        <div>
                            <label class="block mb-2 font-medium">Salary ($)</label>
                            <input type="number" step="0.01" id="emp-salary" required min="0">
                        </div>
                    </div>
                </div>
                <div class="flex gap-3 mt-6">
                    <button type="submit" class="btn btn-primary flex-1">Save</button>
                    <button type="button" onclick="closeModal('employeeModal')" class="btn btn-danger">Cancel</button>
                </div>
            </form>
        </div>
    </div>

    <script>
        // Authentication
        const validCredentials = {
            username: 'Tendai Manjeru',
            password: 'admin'
        };

        let currentUser = null;

        function handleLogin(e) {
            e.preventDefault();
            const username = document.getElementById('loginUsername').value;
            const password = document.getElementById('loginPassword').value;
            const errorDiv = document.getElementById('loginError');

            if (username === validCredentials.username && password === validCredentials.password) {
                currentUser = username;
                document.getElementById('loggedInUser').textContent = username;
                document.getElementById('loginScreen').style.display = 'none';
                document.getElementById('appContainer').classList.add('active');
                errorDiv.classList.remove('show');

                // Initialize app after successful login
                loadData();
            } else {
                errorDiv.classList.add('show');
                setTimeout(() => {
                    errorDiv.classList.remove('show');
                }, 3000);
            }
        }

        function handleLogout() {
            showConfirmDialog('Are you sure you want to logout?', () => {
                currentUser = null;
                document.getElementById('loginScreen').style.display = 'flex';
                document.getElementById('appContainer').classList.remove('active');
                document.getElementById('loginUsername').value = '';
                document.getElementById('loginPassword').value = '';
            });
        }

        // Data Storage
        let data = {
            inventory: [],
            patients: [],
            visits: [],
            employees: []
        };

        // Load data from memory (initialized on page load)
        function loadData() {
            // Initialize with sample data
            initializeSampleData();
            refreshAllTables();
            updateDashboard();
        }

        // Save data to memory and update UI
        function saveData() {
            // Data is already in memory, just update the dashboard
            updateDashboard();
        }

        // Initialize with sample data
        function initializeSampleData() {
            data.inventory = [
                { id: 1, name: 'Surgical Masks', category: 'PPE', quantity: 500, price: 0.50, supplier: 'MedSupply Co' },
                { id: 2, name: 'Hand Sanitizer', category: 'Medical Supplies', quantity: 200, price: 5.99, supplier: 'HealthCare Inc' },
                { id: 3, name: 'Stethoscope', category: 'Equipment', quantity: 25, price: 89.99, supplier: 'MedEquip Ltd' },
                { id: 4, name: 'Paracetamol', category: 'Pharmaceuticals', quantity: 1000, price: 0.25, supplier: 'Pharma Solutions' },
                { id: 5, name: 'Blood Pressure Monitor', category: 'Equipment', quantity: 15, price: 149.99, supplier: 'MedEquip Ltd' }
            ];

            data.patients = [
                { id: 'P001', name: 'John Smith', age: 45, gender: 'Male', contact: '555-0101', admission: '2024-01-15', status: 'Admitted' },
                { id: 'P002', name: 'Sarah Johnson', age: 32, gender: 'Female', contact: '555-0102', admission: '2024-01-16', status: 'Observation' },
                { id: 'P003', name: 'Michael Brown', age: 67, gender: 'Male', contact: '555-0103', admission: '2024-01-10', status: 'Discharged' },
                { id: 'P004', name: 'Emily Davis', age: 28, gender: 'Female', contact: '555-0104', admission: '2024-01-18', status: 'Admitted' }
            ];

            data.visits = [
                { id: 'V001', patient: 'John Smith', department: 'Cardiology', doctor: 'Dr. Wilson', date: '2024-01-15', purpose: 'Chest pain evaluation' },
                { id: 'V002', patient: 'Sarah Johnson', department: 'Pediatrics', doctor: 'Dr. Martinez', date: '2024-01-16', purpose: 'Child wellness check' },
                { id: 'V003', patient: 'Michael Brown', department: 'Orthopedics', doctor: 'Dr. Lee', date: '2024-01-10', purpose: 'Hip replacement follow-up' },
                { id: 'V004', patient: 'Emily Davis', department: 'Emergency', doctor: 'Dr. Patel', date: '2024-01-18', purpose: 'Acute abdominal pain' }
            ];

            data.employees = [
                { id: 'E001', name: 'Dr. James Wilson', role: 'Doctor', department: 'Cardiology', contact: '555-1001', hire: '2020-03-15', salary: 180000 },
                { id: 'E002', name: 'Dr. Maria Martinez', role: 'Doctor', department: 'Pediatrics', contact: '555-1002', hire: '2019-06-20', salary: 165000 },
                { id: 'E003', name: 'Nurse Jennifer Lee', role: 'Nurse', department: 'Emergency', contact: '555-1003', hire: '2021-01-10', salary: 75000 },
                { id: 'E004', name: 'Dr. Raj Patel', role: 'Doctor', department: 'Emergency', contact: '555-1004', hire: '2018-09-05', salary: 175000 }
            ];

            saveData();
        }

        // Navigation
        document.querySelectorAll('.nav-item').forEach(item => {
            item.addEventListener('click', () => {
                const section = item.getAttribute('data-section');

                // Update nav
                document.querySelectorAll('.nav-item').forEach(n => n.classList.remove('active'));
                item.classList.add('active');

                // Update sections
                document.querySelectorAll('.section').forEach(s => s.classList.remove('active'));
                document.getElementById(section).classList.add('active');
            });
        });

        // Modal functions
        function openModal(modalId) {
            document.getElementById(modalId).classList.add('active');
            // Reset form if opening for new entry
            const form = document.querySelector(`#${modalId} form`);
            if (form) form.reset();
        }

        function closeModal(modalId) {
            document.getElementById(modalId).classList.remove('active');
        }

        // Inventory functions
        function saveInventory(e) {
            e.preventDefault();
            const id = document.getElementById('inv-id').value;
            const item = {
                id: id || Date.now(),
                name: document.getElementById('inv-name').value,
                category: document.getElementById('inv-category').value,
                quantity: parseInt(document.getElementById('inv-quantity').value),
                price: parseFloat(document.getElementById('inv-price').value),
                supplier: document.getElementById('inv-supplier').value
            };

            if (id) {
                const index = data.inventory.findIndex(i => i.id == id);
                data.inventory[index] = item;
            } else {
                data.inventory.push(item);
            }

            saveData();
            refreshTable('inventory');
            closeModal('inventoryModal');
        }

        function editInventory(id) {
            const item = data.inventory.find(i => i.id == id);
            document.getElementById('inv-id').value = item.id;
            document.getElementById('inv-name').value = item.name;
            document.getElementById('inv-category').value = item.category;
            document.getElementById('inv-quantity').value = item.quantity;
            document.getElementById('inv-price').value = item.price;
            document.getElementById('inv-supplier').value = item.supplier;
            openModal('inventoryModal');
        }

        function deleteInventory(id) {
            showConfirmDialog('Are you sure you want to delete this item?', () => {
                data.inventory = data.inventory.filter(i => i.id != id);
                saveData();
                refreshTable('inventory');
            });
        }

        // Patient functions
        function savePatient(e) {
            e.preventDefault();
            const id = document.getElementById('pat-id').value;
            const patient = {
                id: id || 'P' + String(data.patients.length + 1).padStart(3, '0'),
                name: document.getElementById('pat-name').value,
                age: parseInt(document.getElementById('pat-age').value),
                gender: document.getElementById('pat-gender').value,
                contact: document.getElementById('pat-contact').value,
                admission: document.getElementById('pat-admission').value,
                status: document.getElementById('pat-status').value
            };

            if (id) {
                const index = data.patients.findIndex(p => p.id === id);
                data.patients[index] = patient;
            } else {
                data.patients.push(patient);
            }

            saveData();
            refreshTable('patients');
            closeModal('patientModal');
        }

        function editPatient(id) {
            const patient = data.patients.find(p => p.id === id);
            document.getElementById('pat-id').value = patient.id;
            document.getElementById('pat-name').value = patient.name;
            document.getElementById('pat-age').value = patient.age;
            document.getElementById('pat-gender').value = patient.gender;
            document.getElementById('pat-contact').value = patient.contact;
            document.getElementById('pat-admission').value = patient.admission;
            document.getElementById('pat-status').value = patient.status;
            openModal('patientModal');
        }

        function deletePatient(id) {
            showConfirmDialog('Are you sure you want to delete this patient record?', () => {
                data.patients = data.patients.filter(p => p.id !== id);
                saveData();
                refreshTable('patients');
            });
        }

        // Visit functions
        function saveVisit(e) {
            e.preventDefault();
            const id = document.getElementById('vis-id').value;
            const visit = {
                id: id || 'V' + String(data.visits.length + 1).padStart(3, '0'),
                patient: document.getElementById('vis-patient').value,
                department: document.getElementById('vis-department').value,
                doctor: document.getElementById('vis-doctor').value,
                date: document.getElementById('vis-date').value,
                purpose: document.getElementById('vis-purpose').value
            };

            if (id) {
                const index = data.visits.findIndex(v => v.id === id);
                data.visits[index] = visit;
            } else {
                data.visits.push(visit);
            }

            saveData();
            refreshTable('visits');
            closeModal('visitModal');
        }

        function editVisit(id) {
            const visit = data.visits.find(v => v.id === id);
            document.getElementById('vis-id').value = visit.id;
            document.getElementById('vis-patient').value = visit.patient;
            document.getElementById('vis-department').value = visit.department;
            document.getElementById('vis-doctor').value = visit.doctor;
            document.getElementById('vis-date').value = visit.date;
            document.getElementById('vis-purpose').value = visit.purpose;
            openModal('visitModal');
        }

        function deleteVisit(id) {
            showConfirmDialog('Are you sure you want to delete this visit record?', () => {
                data.visits = data.visits.filter(v => v.id !== id);
                saveData();
                refreshTable('visits');
            });
        }

        // Employee functions
        function saveEmployee(e) {
            e.preventDefault();
            const id = document.getElementById('emp-id').value;
            const employee = {
                id: id || 'E' + String(data.employees.length + 1).padStart(3, '0'),
                name: document.getElementById('emp-name').value,
                role: document.getElementById('emp-role').value,
                department: document.getElementById('emp-department').value,
                contact: document.getElementById('emp-contact').value,
                hire: document.getElementById('emp-hire').value,
                salary: parseFloat(document.getElementById('emp-salary').value)
            };

            if (id) {
                const index = data.employees.findIndex(e => e.id === id);
                data.employees[index] = employee;
            } else {
                data.employees.push(employee);
            }

            saveData();
            refreshTable('employees');
            closeModal('employeeModal');
        }

        function editEmployee(id) {
            const employee = data.employees.find(e => e.id === id);
            document.getElementById('emp-id').value = employee.id;
            document.getElementById('emp-name').value = employee.name;
            document.getElementById('emp-role').value = employee.role;
            document.getElementById('emp-department').value = employee.department;
            document.getElementById('emp-contact').value = employee.contact;
            document.getElementById('emp-hire').value = employee.hire;
            document.getElementById('emp-salary').value = employee.salary;
            openModal('employeeModal');
        }

        function deleteEmployee(id) {
            showConfirmDialog('Are you sure you want to delete this employee record?', () => {
                data.employees = data.employees.filter(e => e.id !== id);
                saveData();
                refreshTable('employees');
            });
        }

        // Refresh tables
        function refreshTable(type) {
            switch(type) {
                case 'inventory':
                    const invBody = document.getElementById('inventoryBody');
                    invBody.innerHTML = data.inventory.map(item => {
                        const status = item.quantity < 50 ? 'Low Stock' : item.quantity < 100 ? 'Medium' : 'In Stock';
                        const badge = item.quantity < 50 ? 'badge-danger' : item.quantity < 100 ? 'badge-warning' : 'badge-success';
                        return `
                            <tr>
                                <td>${item.name}</td>
                                <td>${item.category}</td>
                                <td>${item.quantity}</td>
                                <td>$${item.price.toFixed(2)}</td>
                                <td>${item.supplier}</td>
                                <td><span class="badge ${badge}">${status}</span></td>
                                <td>
                                    <button onclick="editInventory(${item.id})" class="btn btn-primary" style="padding: 0.25rem 0.75rem; margin-right: 0.5rem;">
                                        <i class="fas fa-edit"></i>
                                    </button>
                                    <button onclick="deleteInventory(${item.id})" class="btn btn-danger" style="padding: 0.25rem 0.75rem;">
                                        <i class="fas fa-trash"></i>
                                    </button>
                                </td>
                            </tr>
                        `;
                    }).join('');
                    break;

                case 'patients':
                    const patBody = document.getElementById('patientBody');
                    patBody.innerHTML = data.patients.map(patient => {
                        const badge = patient.status === 'Admitted' ? 'badge-info' : patient.status === 'Discharged' ? 'badge-success' : 'badge-warning';
                        return `
                            <tr>
                                <td>${patient.id}</td>
                                <td>${patient.name}</td>
                                <td>${patient.age}</td>
                                <td>${patient.gender}</td>
                                <td>${patient.contact}</td>
                                <td>${patient.admission}</td>
                                <td><span class="badge ${badge}">${patient.status}</span></td>
                                <td>
                                    <button onclick="editPatient('${patient.id}')" class="btn btn-primary" style="padding: 0.25rem 0.75rem; margin-right: 0.5rem;">
                                        <i class="fas fa-edit"></i>
                                    </button>
                                    <button onclick="deletePatient('${patient.id}')" class="btn btn-danger" style="padding: 0.25rem 0.75rem;">
                                        <i class="fas fa-trash"></i>
                                    </button>
                                </td>
                            </tr>
                        `;
                    }).join('');
                    break;

                case 'visits':
                    const visBody = document.getElementById('visitBody');
                    visBody.innerHTML = data.visits.map(visit => `
                        <tr>
                            <td>${visit.id}</td>
                            <td>${visit.patient}</td>
                            <td>${visit.department}</td>
                            <td>${visit.doctor}</td>
                            <td>${visit.date}</td>
                            <td>${visit.purpose}</td>
                            <td>
                                <button onclick="editVisit('${visit.id}')" class="btn btn-primary" style="padding: 0.25rem 0.75rem; margin-right: 0.5rem;">
                                    <i class="fas fa-edit"></i>
                                </button>
                                <button onclick="deleteVisit('${visit.id}')" class="btn btn-danger" style="padding: 0.25rem 0.75rem;">
                                    <i class="fas fa-trash"></i>
                                </button>
                            </td>
                        </tr>
                    `).join('');
                    break;

                case 'employees':
                    const empBody = document.getElementById('employeeBody');
                    empBody.innerHTML = data.employees.map(employee => `
                        <tr>
                            <td>${employee.id}</td>
                            <td>${employee.name}</td>
                            <td>${employee.role}</td>
                            <td>${employee.department}</td>
                            <td>${employee.contact}</td>
                            <td>${employee.hire}</td>
                            <td>$${employee.salary.toLocaleString()}</td>
                            <td>
                                <button onclick="editEmployee('${employee.id}')" class="btn btn-primary" style="padding: 0.25rem 0.75rem; margin-right: 0.5rem;">
                                    <i class="fas fa-edit"></i>
                                </button>
                                <button onclick="deleteEmployee('${employee.id}')" class="btn btn-danger" style="padding: 0.25rem 0.75rem;">
                                    <i class="fas fa-trash"></i>
                                </button>
                            </td>
                        </tr>
                    `).join('');
                    break;
            }
        }

        function refreshAllTables() {
            refreshTable('inventory');
            refreshTable('patients');
            refreshTable('visits');
            refreshTable('employees');
        }

        // Search/Filter
        function filterTable(tableId, searchId) {
            const input = document.getElementById(searchId).value.toLowerCase();
            const table = document.getElementById(tableId);
            const rows = table.getElementsByTagName('tr');

            for (let i = 1; i < rows.length; i++) {
                const row = rows[i];
                const text = row.textContent.toLowerCase();
                row.style.display = text.includes(input) ? '' : 'none';
            }
        }

        // Download functions
        function downloadData(type) {
            let csvContent = '';
            let filename = '';

            switch(type) {
                case 'inventory':
                    csvContent = 'ID,Name,Category,Quantity,Price,Supplier\n';
                    data.inventory.forEach(item => {
                        csvContent += `${item.id},"${item.name}","${item.category}",${item.quantity},${item.price},"${item.supplier}"\n`;
                    });
                    filename = 'inventory_data.csv';
                    break;

                case 'patients':
                    csvContent = 'ID,Name,Age,Gender,Contact,Admission Date,Status\n';
                    data.patients.forEach(patient => {
                        csvContent += `${patient.id},"${patient.name}",${patient.age},"${patient.gender}","${patient.contact}","${patient.admission}","${patient.status}"\n`;
                    });
                    filename = 'patient_data.csv';
                    break;

                case 'visits':
                    csvContent = 'ID,Patient,Department,Doctor,Date,Purpose\n';
                    data.visits.forEach(visit => {
                        csvContent += `${visit.id},"${visit.patient}","${visit.department}","${visit.doctor}","${visit.date}","${visit.purpose}"\n`;
                    });
                    filename = 'visits_data.csv';
                    break;

                case 'employees':
                    csvContent = 'ID,Name,Role,Department,Contact,Hire Date,Salary\n';
                    data.employees.forEach(employee => {
                        csvContent += `${employee.id},"${employee.name}","${employee.role}","${employee.department}","${employee.contact}","${employee.hire}",${employee.salary}\n`;
                    });
                    filename = 'employee_data.csv';
                    break;
            }

            const blob = new Blob([csvContent], { type: 'text/csv' });
            const url = window.URL.createObjectURL(blob);
            const a = document.createElement('a');
            a.href = url;
            a.download = filename;
            a.click();
            window.URL.revokeObjectURL(url);
        }

        function downloadAllData() {
            const allData = {
                inventory: data.inventory,
                patients: data.patients,
                visits: data.visits,
                employees: data.employees,
                exportDate: new Date().toISOString()
            };

            const blob = new Blob([JSON.stringify(allData, null, 2)], { type: 'application/json' });
            const url = window.URL.createObjectURL(blob);
            const a = document.createElement('a');
            a.href = url;
            a.download = 'hospital_complete_data.json';
            a.click();
            window.URL.revokeObjectURL(url);
        }

        // Dashboard updates
        function updateDashboard() {
            document.getElementById('stat-inventory').textContent = data.inventory.length;
            document.getElementById('stat-patients').textContent = data.patients.length;
            document.getElementById('stat-visits').textContent = data.visits.length;
            document.getElementById('stat-employees').textContent = data.employees.length;

            updateCharts();
        }

        // Charts
        let charts = {};

        function updateCharts() {
            // Inventory Chart
            const inventoryCtx = document.getElementById('inventoryChart');
            if (charts.inventory) charts.inventory.destroy();

            const categories = {};
            data.inventory.forEach(item => {
                categories[item.category] = (categories[item.category] || 0) + item.quantity;
            });

            charts.inventory = new Chart(inventoryCtx, {
                type: 'doughnut',
                data: {
                    labels: Object.keys(categories),
                    datasets: [{
                        data: Object.values(categories),
                        backgroundColor: ['#374151', '#4b5563', '#6b7280', '#9ca3af', '#1f2937']
                    }]
                },
                options: {
                    responsive: true,
                    maintainAspectRatio: true,
                    plugins: {
                        legend: {
                            position: 'bottom'
                        }
                    }
                }
            });

            // Admissions Chart
            const admissionsCtx = document.getElementById('admissionsChart');
            if (charts.admissions) charts.admissions.destroy();

            const admissionsByMonth = {};
            data.patients.forEach(patient => {
                const month = patient.admission.substring(0, 7);
                admissionsByMonth[month] = (admissionsByMonth[month] || 0) + 1;
            });

            charts.admissions = new Chart(admissionsCtx, {
                type: 'line',
                data: {
                    labels: Object.keys(admissionsByMonth).sort(),
                    datasets: [{
                        label: 'Patient Admissions',
                        data: Object.keys(admissionsByMonth).sort().map(k => admissionsByMonth[k]),
                        borderColor: '#374151',
                        backgroundColor: 'rgba(55, 65, 81, 0.1)',
                        tension: 0.4,
                        fill: true
                    }]
                },
                options: {
                    responsive: true,
                    maintainAspectRatio: true,
                    plugins: {
                        legend: {
                            display: false
                        }
                    }
                }
            });

            // Visits Chart
            const visitsCtx = document.getElementById('visitsChart');
            if (charts.visits) charts.visits.destroy();

            const visitsByDept = {};
            data.visits.forEach(visit => {
                visitsByDept[visit.department] = (visitsByDept[visit.department] || 0) + 1;
            });

            charts.visits = new Chart(visitsCtx, {
                type: 'bar',
                data: {
                    labels: Object.keys(visitsByDept),
                    datasets: [{
                        label: 'Visits',
                        data: Object.values(visitsByDept),
                        backgroundColor: '#4b5563'
                    }]
                },
                options: {
                    responsive: true,
                    maintainAspectRatio: true,
                    plugins: {
                        legend: {
                            display: false
                        }
                    }
                }
            });

            // Employee Chart
            const employeeCtx = document.getElementById('employeeChart');
            if (charts.employee) charts.employee.destroy();

            const empByRole = {};
            data.employees.forEach(emp => {
                empByRole[emp.role] = (empByRole[emp.role] || 0) + 1;
            });

            charts.employee = new Chart(employeeCtx, {
                type: 'pie',
                data: {
                    labels: Object.keys(empByRole),
                    datasets: [{
                        data: Object.values(empByRole),
                        backgroundColor: ['#374151', '#4b5563', '#6b7280', '#9ca3af', '#1f2937', '#111827']
                    }]
                },
                options: {
                    responsive: true,
                    maintainAspectRatio: true,
                    plugins: {
                        legend: {
                            position: 'bottom'
                        }
                    }
                }
            });
        }

        // Dark mode
        function toggleDarkMode() {
            document.documentElement.classList.toggle('dark');
        }

        // Custom confirm dialog
        function showConfirmDialog(message, onConfirm) {
            const modal = document.createElement('div');
            modal.className = 'modal active';
            modal.innerHTML = `
                <div class="modal-content" style="max-width: 400px;">
                    <h3 class="text-xl font-semibold mb-4">Confirm Action</h3>
                    <p class="mb-6">${message}</p>
                    <div class="flex gap-3">
                        <button class="btn btn-danger flex-1" onclick="this.closest('.modal').remove(); (${onConfirm})()">Confirm</button>
                        <button class="btn btn-primary flex-1" onclick="this.closest('.modal').remove()">Cancel</button>
                    </div>
                </div>
            `;
            document.body.appendChild(modal);
        }

        // Mobile menu
        document.querySelector('.mobile-menu-btn').addEventListener('click', () => {
            document.querySelector('.sidebar').classList.toggle('active');
        });

        // Initialize
        if (window.matchMedia && window.matchMedia('(prefers-color-scheme: dark)').matches) {
            document.documentElement.classList.add('dark');
        }
        window.matchMedia('(prefers-color-scheme: dark)').addEventListener('change', event => {
            if (event.matches) {
                document.documentElement.classList.add('dark');
            } else {
                document.documentElement.classList.remove('dark');
            }
        });

        // Don't load data until logged in - login handler will call loadData()
    </script>
</body>
</html>
