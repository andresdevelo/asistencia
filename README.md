<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Sistema de Asistencia</title>
    <style>
        /* General styles */
        body {
            font-family: 'Arial', sans-serif;
            line-height: 1.6;
            margin: 0;
            padding: 0;
            background-color: #f5f5f5;
            color: #333;
        }
        
        .container {
            max-width: 1200px;
            margin: 0 auto;
            padding: 20px;
        }
        
        h1, h2, h3, h4 {
            color: #2c73d2;
            margin-top: 0;
        }
        
        /* Auth styles */
        .auth-container {
            max-width: 500px;
            margin: 50px auto;
            background: #fff;
            padding: 20px;
            border-radius: 5px;
            box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
        }
        
        .auth-options {
            display: flex;
            margin-bottom: 20px;
            border-bottom: 1px solid #ddd;
        }
        
        .auth-option {
            padding: 10px 20px;
            cursor: pointer;
            transition: background 0.3s;
        }
        
        .auth-option:hover {
            background: #f0f0f0;
        }
        
        .auth-option.active {
            border-bottom: 2px solid #2c73d2;
            font-weight: bold;
            color: #2c73d2;
        }
        
        .auth-form {
            display: none;
        }
        
        .auth-form.active {
            display: block;
        }
        
        .form-group {
            margin-bottom: 15px;
        }
        
        label {
            display: block;
            margin-bottom: 5px;
            font-weight: bold;
        }
        
        input, select {
            width: 100%;
            padding: 8px;
            border: 1px solid #ddd;
            border-radius: 4px;
            font-size: 16px;
        }
        
        button {
            background: #2c73d2;
            color: white;
            border: none;
            padding: 10px 15px;
            border-radius: 4px;
            cursor: pointer;
            font-size: 16px;
            transition: background 0.3s;
        }
        
        button:hover {
            background: #1e5ba8;
        }
        
        .btn-secondary {
            background: #6c757d;
        }
        
        .btn-secondary:hover {
            background: #5a6268;
        }
        
        .btn-danger {
            background: #dc3545;
            padding: 5px 10px;
            font-size: 14px;
        }
        
        .btn-danger:hover {
            background: #c82333;
        }
        
        /* Notification */
        .notification {
            padding: 10px;
            margin-bottom: 20px;
            border-radius: 4px;
            display: none;
        }
        
        .notification.success {
            background-color: #d4edda;
            color: #155724;
            border: 1px solid #c3e6cb;
        }
        
        .notification.error {
            background-color: #f8d7da;
            color: #721c24;
            border: 1px solid #f5c6cb;
        }
        
        /* Monitor page */
        .monitor-header {
            display: flex;
            justify-content: space-between;
            background: #fff;
            padding: 15px;
            border-radius: 5px;
            box-shadow: 0 0 5px rgba(0, 0, 0, 0.1);
            margin-bottom: 20px;
        }
        
        .search-container {
            display: flex;
            margin-bottom: 20px;
        }
        
        .search-container input {
            flex: 1;
            margin-right: 10px;
        }
        
        table {
            width: 100%;
            border-collapse: collapse;
            margin-bottom: 20px;
            background: #fff;
            box-shadow: 0 0 5px rgba(0, 0, 0, 0.1);
        }
        
        th, td {
            padding: 12px 15px;
            text-align: left;
            border-bottom: 1px solid #ddd;
        }
        
        th {
            background-color: #f8f9fa;
            font-weight: bold;
        }
        
        tr:hover {
            background-color: #f5f5f5;
        }
        
        .hidden {
            display: none;
        }
        
        /* Admin page */
        .dashboard-cards {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
            gap: 20px;
            margin-bottom: 20px;
        }
        
        .card {
            background: #fff;
            padding: 20px;
            border-radius: 5px;
            box-shadow: 0 0 5px rgba(0, 0, 0, 0.1);
            text-align: center;
        }
        
        .card .number {
            font-size: 32px;
            font-weight: bold;
            color: #2c73d2;
        }
        
        .tabs {
            display: flex;
            margin-bottom: 20px;
            border-bottom: 1px solid #ddd;
        }
        
        .tab {
            padding: 10px 20px;
            cursor: pointer;
            transition: background 0.3s;
        }
        
        .tab:hover {
            background: #f0f0f0;
        }
        
        .tab.active {
            border-bottom: 2px solid #2c73d2;
            font-weight: bold;
            color: #2c73d2;
        }
        
        .tab-content {
            display: none;
            background: #fff;
            padding: 20px;
            border-radius: 5px;
            box-shadow: 0 0 5px rgba(0, 0, 0, 0.1);
        }
        
        .tab-content.active {
            display: block;
        }
    </style>
</head>
<body>
    <!-- Auth Container (Login/Register) -->
    <div id="auth-container" class="container">
        <div class="auth-container">
            <div class="auth-options">
                <div class="auth-option active" data-form="login-form">Iniciar Sesión</div>
                <div class="auth-option" data-form="register-form">Registrarse</div>
            </div>
            
            <div id="auth-notification" class="notification"></div>
            
            <!-- Login Form -->
            <div id="login-form" class="auth-form active">
                <h2>Iniciar Sesión</h2>
                <div class="form-group">
                    <label for="username">Usuario:</label>
                    <input type="text" id="username" name="username" required>
                </div>
                <div class="form-group">
                    <label for="password">Contraseña:</label>
                    <input type="password" id="password" name="password" required>
                </div>
                <div class="form-group">
                    <label for="user-type">Tipo de Usuario:</label>
                    <select id="user-type" name="user-type">
                        <option value="monitor">Monitor</option>
                        <option value="admin">Administrador</option>
                    </select>
                </div>
                <button id="login-btn">Iniciar Sesión</button>
            </div>
            
            <!-- Register Form -->
            <div id="register-form" class="auth-form">
                <h2>Registro de Monitor</h2>
                <div class="form-group">
                    <label for="reg-fullname">Nombre Completo:</label>
                    <input type="text" id="reg-fullname" name="reg-fullname" required>
                </div>
                <div class="form-group">
                    <label for="reg-id">Número de Identificación:</label>
                    <input type="text" id="reg-id" name="reg-id" required>
                </div>
                <div class="form-group">
                    <label for="reg-phone">Número de Teléfono:</label>
                    <input type="tel" id="reg-phone" name="reg-phone" required>
                </div>
                <div class="form-group">
                    <label for="reg-career">Carrera:</label>
                    <select id="reg-career" name="reg-career" required>
                        <option value="">Seleccione una carrera</option>
                        <option value="Ingeniería de Sistemas">Ingeniería de Sistemas</option>
                        <option value="Gastronomía">Gastronomía</option>
                        <option value="Medicina">Medicina</option>
                        <option value="Derecho">Derecho</option>
                        <option value="Administración de Empresas">Administración de Empresas</option>
                    </select>
                </div>
                <div class="form-group">
                    <label for="reg-semester">Semestre:</label>
                    <select id="reg-semester" name="reg-semester" required>
                        <option value="">Seleccione un semestre</option>
                        <option value="1">Semestre 1</option>
                        <option value="2">Semestre 2</option>
                        <option value="3">Semestre 3</option>
                        <option value="4">Semestre 4</option>
                    </select>
                </div>
                <div class="form-group">
                    <label for="reg-module">Nombre del Módulo:</label>
                    <input type="text" id="reg-module" name="reg-module" required>
                </div>
                <div class="form-group">
                    <label for="reg-username">Usuario:</label>
                    <input type="text" id="reg-username" name="reg-username" required>
                </div>
                <div class="form-group">
                    <label for="reg-password">Contraseña:</label>
                    <input type="password" id="reg-password" name="reg-password" required>
                </div>
                <button id="register-btn">Registrarse</button>
            </div>
        </div>
    </div>

    <!-- Monitor Page -->
    <div id="monitor-page" class="container hidden">
        <h1>Sistema de Asistencia - Monitor</h1>
        <div id="monitor-notification" class="notification"></div>
        
        <div class="monitor-header">
            <div class="monitor-info">
                <h3 id="monitor-name">Nombre del Monitor</h3>
                <p id="monitor-career">Carrera</p>
                <p id="monitor-semester">Semestre</p>
                <p id="monitor-module">Módulo</p>
            </div>
            <div class="monitor-schedule">
                <p id="monitor-date">Fecha: </p>
                <p id="monitor-time">Hora: </p>
            </div>
        </div>
        
        <div class="search-container">
            <input type="text" id="student-search" placeholder="Buscar por nombre o número de documento">
            <button id="search-btn">Buscar</button>
        </div>
        
        <div>
            <h2>Lista de Estudiantes</h2>
            <table id="students-table">
                <thead>
                    <tr>
                        <th>Documento</th>
                        <th>Nombre</th>
                        <th>Número de Teléfono</th>
                        <th>Estado</th>
                        <th>Asistencia</th>
                        <th>Acciones</th>
                    </tr>
                </thead>
                <tbody>
                    <!-- Student data will be loaded here -->
                </tbody>
            </table>
            
            <h3>Agregar Estudiante Manualmente</h3>
            <div class="form-group">
                <label for="new-student-doc">Documento:</label>
                <input type="text" id="new-student-doc">
            </div>
            <div class="form-group">
                <label for="new-student-name">Nombre:</label>
                <input type="text" id="new-student-name">
            </div>
            <div class="form-group">
                <label for="new-student-phone">Número de Teléfono:</label>
                <input type="tel" id="new-student-phone">
            </div>
            <div class="form-group">
                <label for="new-student-career">Carrera:</label>
                <select id="new-student-career">
                    <option value="">Seleccione una carrera</option>
                    <option value="Ingeniería de Sistemas">Ingeniería de Sistemas</option>
                    <option value="Gastronomía">Gastronomía</option>
                    <option value="Medicina">Medicina</option>
                    <option value="Derecho">Derecho</option>
                    <option value="Administración de Empresas">Administración de Empresas</option>
                </select>
            </div>
            <button id="add-student-btn">Agregar Estudiante</button>
            
            <div style="margin-top: 20px;">
                <button id="save-attendance-btn">Guardar Asistencia</button>
                <button id="monitor-logout-btn" class="btn-secondary">Cerrar Sesión</button>
            </div>
        </div>
    </div>

    <!-- Admin Page -->
    <div id="admin-page" class="container hidden">
        <h1>Sistema de Asistencia - Administrador</h1>
        <div id="admin-notification" class="notification"></div>
        
        <div class="dashboard-cards">
            <div class="card">
                <h3>Total Estudiantes</h3>
                <div class="number" id="total-students">0</div>
            </div>
            <div class="card">
                <h3>Asistencia Diaria</h3>
                <div class="number" id="today-attendance">0</div>
            </div>
            <div class="card">
                <h3>% Asistencia</h3>
                <div class="number" id="attendance-percentage">0%</div>
            </div>
            <div class="card">
                <h3>No Matriculados</h3>
                <div class="number" id="unregistered-students">0</div>
            </div>
        </div>
        
        <div class="tabs">
            <div class="tab active" data-tab="attendance-data">Datos de Asistencia</div>
            <div class="tab" data-tab="statistics">Estadísticas</div>
            <div class="tab" data-tab="reports">Informes</div>
            <div class="tab" data-tab="user-management">Gestión de Usuarios</div>
        </div>
        
        <div id="attendance-data" class="tab-content active">
            <h2>Datos de Asistencia</h2>
            
            <div class="form-group">
                <label>Filtrar por:</label>
                <div style="display: flex; gap: 10px; margin-bottom: 10px;">
                    <div style="flex: 1;">
                        <label for="filter-date">Fecha:</label>
                        <input type="date" id="filter-date">
                    </div>
                    <div style="flex: 1;">
                        <label for="filter-career">Carrera:</label>
                        <select id="filter-career">
                            <option value="">Todas</option>
                            <option value="Ingeniería de Sistemas">Ingeniería de Sistemas</option>
                            <option value="Gastronomía">Gastronomía</option>
                            <option value="Medicina">Medicina</option>
                            <option value="Derecho">Derecho</option>
                            <option value="Administración de Empresas">Administración de Empresas</option>
                        </select>
                    </div>
                    <div style="flex: 1;">
                        <label for="filter-monitor">Monitor:</label>
                        <select id="filter-monitor">
                            <option value="">Todos</option>
                            <!-- Monitor options will be loaded here -->
                        </select>
                    </div>
                    <div style="flex: 1;">
                        <label for="filter-status">Estado:</label>
                        <select id="filter-status">
                            <option value="">Todos</option>
                            <option value="matriculado">Matriculado</option>
                            <option value="no-matriculado">No Matriculado</option>
                        </select>
                    </div>
                </div>
                <button id="apply-filters-btn">Aplicar Filtros</button>
            </div>
            
            <table id="admin-attendance-table">
                <thead>
                    <tr>
                        <th>Documento</th>
                        <th>Nombre</th>
                        <th>Teléfono</th>
                        <th>Carrera</th>
                        <th>Monitor</th>
                        <th>Fecha</th>
                        <th>Hora</th>
                        <th>Estado</th>
                        <th>Asistencia</th>
                    </tr>
                </thead>
                <tbody>
                    <!-- Attendance data will be loaded here -->
                </tbody>
            </table>
        </div>
        
        <div id="statistics" class="tab-content">
            <h2>Estadísticas</h2>
            <div style="display: flex; gap: 20px; flex-wrap: wrap;">
                <div style="flex: 1; min-width: 300px;">
                    <h3>Asistencia por Carrera</h3>
                    <div id="career-chart" style="height: 300px; background: #f9f9f9;">
                        <!-- Chart will be rendered here -->
                    </div>
                </div>
                <div style="flex: 1; min-width: 300px;">
                    <h3>Asistencia por Monitor</h3>
                    <div id="monitor-chart" style="height: 300px; background: #f9f9f9;">
                        <!-- Chart will be rendered here -->
                    </div>
                </div>
                <div style="flex: 1; min-width: 300px;">
                    <h3>Tendencia de Asistencia</h3>
                    <div id="trend-chart" style="height: 300px; background: #f9f9f9;">
                        <!-- Chart will be rendered here -->
                    </div>
                </div>
            </div>
        </div>
        
        <div id="reports" class="tab-content">
            <h2>Informes</h2>
            <div class="form-group">
                <label for="report-type">Tipo de Informe:</label>
                <select id="report-type">
                    <option value="daily">Asistencia Diaria</option>
                    <option value="weekly">Asistencia Semanal</option>
                    <option value="monthly">Asistencia Mensual</option>
                    <option value="by-career">Asistencia por Carrera</option>
                    <option value="by-monitor">Asistencia por Monitor</option>
                    <option value="by-semester">Asistencia por Semestre</option>
                    <option value="unregistered">Estudiantes No Matriculados</option>
                </select>
            </div>
            <div class="form-group">
                <label for="report-date-start">Fecha Inicio:</label>
                <input type="date" id="report-date-start">
            </div>
            <div class="form-group">
                <label for="report-date-end">Fecha Fin:</label>
                <input type="date" id="report-date-end">
            </div>
            <div class="form-group">
                <label for="report-career">Carrera (opcional):</label>
                <select id="report-career">
                    <option value="">Todas</option>
                    <option value="Ingeniería de Sistemas">Ingeniería de Sistemas</option>
                    <option value="Gastronomía">Gastronomía</option>
                    <option value="Medicina">Medicina</option>
                    <option value="Derecho">Derecho</option>
                    <option value="Administración de Empresas">Administración de Empresas</option>
                </select>
            </div>
            <div class="form-group">
                <label for="report-monitor">Monitor (opcional):</label>
                <select id="report-monitor">
                    <option value="">Todos</option>
                    <!-- Monitor options will be loaded here -->
                </select>
            </div>
            <button id="generate-report-btn">Generar Informe</button>
            <button id="download-report-btn" class="btn-secondary">Descargar</button>
            
            <div id="report-preview" style="margin-top: 20px;">
                <!-- Report preview will be displayed here -->
            </div>
        </div>
        
        <div id="user-management" class="tab-content">
            <h2>Gestión de Usuarios</h2>
            <table id="users-table">
                <thead>
                    <tr>
                        <th>Usuario</th>
                        <th>Nombre</th>
                        <th>Tipo</th>
                        <th>Carrera</th>
                        <th>Semestre</th>
                        <th>Módulo</th>
                        <th>Último Acceso</th>
                        <th>Acciones</th>
                    </tr>
                </thead>
                <tbody>
                    <!-- Users will be loaded here -->
                </tbody>
            </table>
            
            <h3>Agregar Usuario</h3>
            <div class="form-group">
                <label for="new-user-username">Usuario:</label>
                <input type="text" id="new-user-username">
            </div>
            <div class="form-group">
                <label for="new-user-name">Nombre Completo:</label>
                <input type="text" id="new-user-name">
            </div>
            <div class="form-group">
                <label for="new-user-password">Contraseña:</label>
                <input type="password" id="new-user-password">
            </div>
            <div class="form-group">
                <label for="new-user-type">Tipo:</label>
                <select id="new-user-type">
                    <option value="monitor">Monitor</option>
                    <option value="admin">Administrador</option>
                </select>
            </div>
            <div class="form-group monitor-fields">
                <label for="new-user-career">Carrera:</label>
                <select id="new-user-career">
                    <option value="">Seleccione una carrera</option>
                    <option value="Ingeniería de Sistemas">Ingeniería de Sistemas</option>
                    <option value="Gastronomía">Gastronomía</option>
                    <option value="Medicina">Medicina</option>
                    <option value="Derecho">Derecho</option>
                    <option value="Administración de Empresas">Administración de Empresas</option>
                </select>
            </div>
            <div class="form-group monitor-fields">
                <label for="new-user-semester">Semestre:</label>
                <select id="new-user-semester">
                    <option value="">Seleccione un semestre</option>
                    <option value="1">Semestre 1</option>
                    <option value="2">Semestre 2</option>
                    <option value="3">Semestre 3</option>
                    <option value="4">Semestre 4</option>
                </select>
            </div>
            <div class="form-group monitor-fields">
                <label for="new-user-module">Módulo:</label>
                <input type="text" id="new-user-module">
            </div>
            <button id="add-user-btn">Agregar Usuario</button>
        </div>
        
        <div style="margin-top: 20px;">
            <button id="admin-logout-btn" class="btn-secondary">Cerrar Sesión</button>
        </div>
    </div>

    <!-- Google Sheets API Integration (commented out, to be implemented) -->
    <!-- <script src="https://apis.google.com/js/api.js"></script> -->

    <!-- Load Google Charts -->
    <script src="https://www.gstatic.com/charts/loader.js"></script>
    
    <script>
       // Script corregido para el sistema de gestión de asistencia

document.addEventListener('DOMContentLoaded', function() {
    // Cargar necesarias librerías para gráficos
    loadScripts();
    
    // Referencias a elementos DOM
    initializeDOMReferences();
    
    // Mostrar formulario de inicio de sesión por defecto
    authOptions[0].click();
    
    // Configurar event listeners
    setupEventListeners();
});

// Cargar scripts necesarios dinámicamente
function loadScripts() {
    // Cargar Google Charts
    const googleChartsScript = document.createElement('script');
    googleChartsScript.src = 'https://www.gstatic.com/charts/loader.js';
    googleChartsScript.onload = function() {
        google.charts.load('current', {'packages':['corechart']});
        google.charts.setOnLoadCallback(initCharts);
    };
    document.head.appendChild(googleChartsScript);
    
    // Cargar Chart.js
    const chartJsScript = document.createElement('script');
    chartJsScript.src = 'https://cdn.jsdelivr.net/npm/chart.js';
    document.head.appendChild(chartJsScript);
}

// Inicializar referencias a elementos DOM
function initializeDOMReferences() {
    // DOM elements
    window.authContainer = document.getElementById('auth-container');
    window.monitorPage = document.getElementById('monitor-page');
    window.adminPage = document.getElementById('admin-page');
    
    // Auth elements
    window.authOptions = document.querySelectorAll('.auth-option');
    window.authForms = document.querySelectorAll('.auth-form');
    window.loginBtn = document.getElementById('login-btn');
    window.registerBtn = document.getElementById('register-btn');
    window.authNotification = document.getElementById('auth-notification');
    
    // Monitor page elements
    window.monitorName = document.getElementById('monitor-name');
    window.monitorCareer = document.getElementById('monitor-career');
    window.monitorSemester = document.getElementById('monitor-semester');
    window.monitorModule = document.getElementById('monitor-module');
    window.monitorDate = document.getElementById('monitor-date');
    window.monitorTime = document.getElementById('monitor-time');
    window.studentSearch = document.getElementById('student-search');
    window.searchBtn = document.getElementById('search-btn');
    window.studentsTable = document.getElementById('students-table')?.querySelector('tbody');
    window.addStudentBtn = document.getElementById('add-student-btn');
    window.saveAttendanceBtn = document.getElementById('save-attendance-btn');
    window.monitorLogoutBtn = document.getElementById('monitor-logout-btn');
    window.monitorNotification = document.getElementById('monitor-notification');
    
    // Admin page elements
    window.totalStudents = document.getElementById('total-students');
    window.todayAttendance = document.getElementById('today-attendance');
    window.attendancePercentage = document.getElementById('attendance-percentage');
    window.unregisteredStudents = document.getElementById('unregistered-students');
    window.tabs = document.querySelectorAll('.tab');
    window.tabContents = document.querySelectorAll('.tab-content');
    window.adminAttendanceTable = document.getElementById('admin-attendance-table')?.querySelector('tbody');
    window.applyFiltersBtn = document.getElementById('apply-filters-btn');
    window.filterMonitor = document.getElementById('filter-monitor');
    window.generateReportBtn = document.getElementById('generate-report-btn');
    window.downloadReportBtn = document.getElementById('download-report-btn');
    window.reportPreview = document.getElementById('report-preview');
    window.usersTable = document.getElementById('users-table')?.querySelector('tbody');
    window.addUserBtn = document.getElementById('add-user-btn');
    window.adminLogoutBtn = document.getElementById('admin-logout-btn');
    window.adminNotification = document.getElementById('admin-notification');
}

function setupEventListeners() {
    // Configuración de los toggles de autenticación
    if (window.authOptions) {
        window.authOptions.forEach(option => {
            option.addEventListener('click', () => {
                // Remove active class from all options
                window.authOptions.forEach(o => o.classList.remove('active'));
                // Add active class to clicked option
                option.classList.add('active');
                
                // Hide all forms
                window.authForms.forEach(form => form.classList.remove('active'));
                // Show selected form
                const formId = option.getAttribute('data-form');
                document.getElementById(formId)?.classList.add('active');
            });
        });
    }
    
    // Login functionality
    if (window.loginBtn) {
        window.loginBtn.addEventListener('click', handleLogin);
    }
    
    // Register functionality
    if (window.registerBtn) {
        window.registerBtn.addEventListener('click', handleRegister);
    }
    
    // Logout functionality
    if (window.monitorLogoutBtn) {
        window.monitorLogoutBtn.addEventListener('click', logout);
    }
    
    if (window.adminLogoutBtn) {
        window.adminLogoutBtn.addEventListener('click', logout);
    }
    
    // Add student
    if (window.addStudentBtn) {
        window.addStudentBtn.addEventListener('click', addStudent);
    }
    
    // Search functionality
    if (window.searchBtn) {
        window.searchBtn.addEventListener('click', searchStudents);
    }
    
    // Save attendance
    if (window.saveAttendanceBtn) {
        window.saveAttendanceBtn.addEventListener('click', saveAttendance);
    }
    
    // Filter attendance data
    if (window.applyFiltersBtn) {
        window.applyFiltersBtn.addEventListener('click', applyFilters);
    }
    
    // Generate report
    if (window.generateReportBtn) {
        window.generateReportBtn.addEventListener('click', generateReport);
    }
    
    // Download report
    if (window.downloadReportBtn) {
        window.downloadReportBtn.addEventListener('click', downloadReport);
    }
    
    // Add user
    if (window.addUserBtn) {
        window.addUserBtn.addEventListener('click', addUser);
    }
    
    // Tab switching
    if (window.tabs) {
        window.tabs.forEach(tab => {
            tab.addEventListener('click', () => {
                // Remove active class from all tabs
                window.tabs.forEach(t => t.classList.remove('active'));
                // Add active class to clicked tab
                tab.classList.add('active');
                
                // Hide all tab contents
                window.tabContents.forEach(content => content.classList.remove('active'));
                // Show selected tab content
                const contentId = tab.getAttribute('data-tab');
                document.getElementById(contentId)?.classList.add('active');
            });
        });
    }
}

// App data
let currentUser = null;

// Mock data - unificar definiciones que estaban duplicadas
const mockMonitors = [
    { username: 'monitor1', name: 'Juan Monitor', id: '1009876543', phone: '3109876543', career: 'Ingeniería de Sistemas', semester: '3', module: 'Programación' },
    { username: 'monitor2', name: 'María Cocina', id: '1008765432', phone: '3108765432', career: 'Gastronomía', semester: '2', module: 'Cocina Internacional' }
];

const mockUsers = [
    { username: 'admin', name: 'Administrador Principal', type: 'admin', career: '', semester: '', module: '', lastAccess: '2025-04-30 15:45' },
    { username: 'monitor1', name: 'Juan Monitor', type: 'monitor', career: 'Ingeniería de Sistemas', semester: '3', module: 'Programación', lastAccess: '2025-04-30 08:30' },
    { username: 'monitor2', name: 'María Cocina', type: 'monitor', career: 'Gastronomía', semester: '2', module: 'Cocina Internacional', lastAccess: '2025-04-30 09:15' }
];

// Student data per monitor
const mockStudentsByMonitor = {
    'monitor1': [
        { doc: '1001234567', name: 'Juan Pérez', phone: '3101234567', career: 'Ingeniería de Sistemas', status: 'matriculado', attendance: false },
        { doc: '1002345678', name: 'María García', phone: '3202345678', career: 'Ingeniería de Sistemas', status: 'matriculado', attendance: false },
        { doc: '1003456789', name: 'Carlos Rodríguez', phone: '3133456789', career: 'Ingeniería de Sistemas', status: 'matriculado', attendance: false }
    ],
    'monitor2': [
        { doc: '1004567890', name: 'Ana Martínez', phone: '3154567890', career: 'Gastronomía', status: 'matriculado', attendance: false },
        { doc: '1005678901', name: 'Luis Sánchez', phone: '3005678901', career: 'Gastronomía', status: 'matriculado', attendance: false }
    ]
};

// Attendance records with monitor information
const mockAttendance = [
    { doc: '1001234567', name: 'Juan Pérez', phone: '3101234567', career: 'Ingeniería de Sistemas', monitor: 'Juan Monitor', date: '2025-04-30', time: '08:15', status: 'matriculado', attendance: 'Presente' },
    { doc: '1002345678', name: 'María García', phone: '3202345678', career: 'Ingeniería de Sistemas', monitor: 'Juan Monitor', date: '2025-04-30', time: '08:20', status: 'matriculado', attendance: 'Presente' },
    { doc: '1003456789', name: 'Carlos Rodríguez', phone: '3133456789', career: 'Ingeniería de Sistemas', monitor: 'Juan Monitor', date: '2025-04-30', time: '08:30', status: 'matriculado', attendance: 'Presente' },
    { doc: '1004567890', name: 'Ana Martínez', phone: '3154567890', career: 'Gastronomía', monitor: 'María Cocina', date: '2025-04-30', time: '09:15', status: 'matriculado', attendance: 'Presente' },
    { doc: '1005678901', name: 'Luis Sánchez', phone: '3005678901', career: 'Gastronomía', monitor: 'María Cocina', date: '2025-04-30', time: '09:20', status: 'matriculado', attendance: 'Presente' },
    { doc: '1006789012', name: 'Pedro Gómez', phone: '3166789012', career: 'Ingeniería de Sistemas', monitor: 'Juan Monitor', date: '2025-04-30', time: '08:45', status: 'no-matriculado', attendance: 'Presente' },
    { doc: '1007890123', name: 'Laura López', phone: '3177890123', career: 'Gastronomía', monitor: 'María Cocina', date: '2025-04-29', time: '09:10', status: 'matriculado', attendance: 'Ausente' },
    { doc: '1008901234', name: 'Diego Torres', phone: '3128901234', career: 'Ingeniería de Sistemas', monitor: 'Juan Monitor', date: '2025-04-29', time: '08:15', status: 'matriculado', attendance: 'Presente' }
];

// Initialize charts - asegurar que solo se ejecuta cuando las librerías están cargadas
function initCharts() {
    // Los gráficos se inicializarán cuando los datos estén disponibles
    console.log("Google Charts cargado con éxito");
}

// Login functionality
function handleLogin() {
  const username = document.getElementById('username').value;
  const password = document.getElementById('password').value;
  const userType = document.getElementById('user-type').value;

  if (!username || !password) {
    showNotification(authNotification, 'Por favor complete todos los campos', 'error');
    return;
  }

  fetch('https://script.google.com/macros/s/AKfycbzc3QMv0YtKi5fUTos6lDyuV5WSHsVZHkPSG5ttfutu3GEh5hHA9FNCFqDXJ5T18RE/exec', {
    method: 'POST',
    body: JSON.stringify({ action: 'login', username, password }),
    headers: { 'Content-Type': 'application/json' }
  })
  .then(res => res.json())
  .then(data => {
    if (data.success) {
      currentUser = {
        username,
        type: data.type,
        name: data.name,
        career: data.career,
        semester: data.semester,
        module: data.module
      };
      data.type === 'admin' ? showAdminPage() : showMonitorPage();
    } else {
      showNotification(authNotification, 'Credenciales incorrectas', 'error');
    }
  });
}


// Register functionality
function handleRegister() {
  const fullname = document.getElementById('reg-fullname').value;
  const id = document.getElementById('reg-id').value;
  const phone = document.getElementById('reg-phone').value;
  const career = document.getElementById('reg-career').value;
  const semester = document.getElementById('reg-semester').value;
  const module = document.getElementById('reg-module').value;
  const username = document.getElementById('reg-username').value;
  const password = document.getElementById('reg-password').value;

  if (!fullname || !id || !phone || !career || !semester || !module || !username || !password) {
    showNotification(authNotification, 'Por favor complete todos los campos', 'error');
    return;
  }

  fetch('https://script.google.com/macros/s/AKfycbzc3QMv0YtKi5fUTos6lDyuV5WSHsVZHkPSG5ttfutu3GEh5hHA9FNCFqDXJ5T18RE/exec', {
    method: 'POST',
    body: JSON.stringify({
      action: 'register',
      username,
      password,
      type: 'monitor',
      fullname,
      career,
      semester,
      module
    }),
    headers: { 'Content-Type': 'application/json' }
  })
  .then(res => res.json())
  .then(data => {
    if (data.success) {
      showNotification(authNotification, 'Registro exitoso. Ahora inicie sesión.', 'success');
      authOptions[0].click(); // Cambia a formulario de login
    } else {
      showNotification(authNotification, data.message || 'Error en el registro', 'error');
    }
  });
}

// Show monitor page
function showMonitorPage() {
    window.authContainer.classList.add('hidden');
    window.monitorPage.classList.remove('hidden');
    window.adminPage.classList.add('hidden');
    
    // Update monitor info
    window.monitorName.textContent = currentUser.name;
    window.monitorCareer.textContent = `Carrera: ${currentUser.career}`;
    window.monitorSemester.textContent = `Semestre: ${currentUser.semester}`;
    window.monitorModule.textContent = `Módulo: ${currentUser.module}`;
    
    // Update date and time
    updateDateTime();
    setInterval(updateDateTime, 1000);
    
    // Load students for this monitor
    loadStudents();
}

// Show admin page
function showAdminPage() {
    window.authContainer.classList.add('hidden');
    window.monitorPage.classList.add('hidden');
    window.adminPage.classList.remove('hidden');
    
    // Load dashboard data
    loadDashboardData();
    
    // Load attendance data
    loadAttendanceData();
    
    // Load monitor options for filters
    loadMonitorOptions();
    
    // Load users table
    loadUsersTable();
    
    // Verificar que Chart.js está cargado antes de intentar dibujar los gráficos
    if (typeof Chart !== 'undefined') {
        drawCharts();
    } else {
        // Si Chart.js aún no está cargado, esperar a que se cargue
        const checkChartInterval = setInterval(() => {
            if (typeof Chart !== 'undefined') {
                clearInterval(checkChartInterval);
                drawCharts();
            }
        }, 100);
    }
}

// Initialize date and time display for monitor page
function updateDateTime() {
    const now = new Date();
    window.monitorDate.textContent = `Fecha: ${now.toLocaleDateString()}`;
    window.monitorTime.textContent = `Hora: ${now.toLocaleTimeString()}`;
}

// Logout functionality
function logout() {
    currentUser = null;
    window.authContainer.classList.remove('hidden');
    window.monitorPage.classList.add('hidden');
    window.adminPage.classList.add('hidden');
    
    // Reset forms
    document.getElementById('username').value = '';
    document.getElementById('password').value = '';
    window.authOptions[0].click();
}

// Load students for the current monitor
function loadStudents() {
    if (!window.studentsTable) return;
    
    window.studentsTable.innerHTML = '';
    
    const students = mockStudentsByMonitor[currentUser.username] || [];
    
    students.forEach(student => {
        const row = document.createElement('tr');
        row.innerHTML = `
            <td>${student.doc}</td>
            <td>${student.name}</td>
            <td>${student.phone}</td>
            <td>${student.status}</td>
            <td>
                <input type="checkbox" class="attendance-checkbox" data-doc="${student.doc}" ${student.attendance ? 'checked' : ''}>
            </td>
            <td>
                <button class="btn-danger delete-student" data-doc="${student.doc}">Eliminar</button>
            </td>
        `;
        window.studentsTable.appendChild(row);
    });
    
    // Add event listeners to checkboxes
    document.querySelectorAll('.attendance-checkbox').forEach(checkbox => {
        checkbox.addEventListener('change', function() {
            const doc = this.getAttribute('data-doc');
            const students = mockStudentsByMonitor[currentUser.username];
            const studentIndex = students.findIndex(s => s.doc === doc);
            if (studentIndex !== -1) {
                students[studentIndex].attendance = this.checked;
            }
        });
    });
    
    // Add event listeners to delete buttons
    document.querySelectorAll('.delete-student').forEach(button => {
        button.addEventListener('click', function() {
            const doc = this.getAttribute('data-doc');
            deleteStudent(doc);
        });
    });
}

// Delete student
function deleteStudent(doc) {
    const students = mockStudentsByMonitor[currentUser.username];
    const studentIndex = students.findIndex(s => s.doc === doc);
    if (studentIndex !== -1) {
        students.splice(studentIndex, 1);
        loadStudents();
        showNotification(window.monitorNotification, 'Estudiante eliminado correctamente', 'success');
    }
}

// Add student
function addStudent() {
    const doc = document.getElementById('new-student-doc')?.value;
    const name = document.getElementById('new-student-name')?.value;
    const phone = document.getElementById('new-student-phone')?.value;
    const career = document.getElementById('new-student-career')?.value;
    
    // Simple validation
    if (!doc || !name || !phone || !career) {
        showNotification(window.monitorNotification, 'Por favor, complete todos los campos', 'error');
        return;
    }
    
    // Check if student already exists
    const students = mockStudentsByMonitor[currentUser.username] || [];
    if (students.some(s => s.doc === doc)) {
        showNotification(window.monitorNotification, 'El estudiante ya existe', 'error');
        return;
    }
    
    // Add student
    if (!mockStudentsByMonitor[currentUser.username]) {
        mockStudentsByMonitor[currentUser.username] = [];
    }
    
    mockStudentsByMonitor[currentUser.username].push({
        doc,
        name,
        phone,
        career,
        status: 'no-matriculado', // New students are marked as not enrolled
        attendance: false
    });
    
    // Reset form
    document.getElementById('new-student-doc').value = '';
    document.getElementById('new-student-name').value = '';
    document.getElementById('new-student-phone').value = '';
    document.getElementById('new-student-career').value = '';
    
    // Reload students
    loadStudents();
    
    showNotification(window.monitorNotification, 'Estudiante agregado correctamente', 'success');
}

// Search functionality
function searchStudents() {
    if (!window.studentsTable) return;
    
    const searchTerm = window.studentSearch.value.toLowerCase();
    const rows = window.studentsTable.querySelectorAll('tr');
    
    rows.forEach(row => {
        const doc = row.querySelector('td:first-child').textContent.toLowerCase();
        const name = row.querySelector('td:nth-child(2)').textContent.toLowerCase();
        
        if (doc.includes(searchTerm) || name.includes(searchTerm)) {
            row.style.display = '';
        } else {
            row.style.display = 'none';
        }
    });
}

// Save attendance
function saveAttendance() {
  const students = mockStudentsByMonitor[currentUser.username] || [];
  const now = new Date();
  const date = now.toISOString().split('T')[0];
  const time = now.toLocaleTimeString();

  const attendanceData = students.map(student => ({
    date,
    time,
    doc: student.doc,
    name: student.name,
    phone: student.phone,
    career: student.career,
    monitor: currentUser.name,
    status: student.status,
    attendance: student.attendance ? 'Presente' : 'Ausente'
  }));

  fetch('https://script.google.com/macros/s/AKfycbzc3QMv0YtKi5fUTos6lDyuV5WSHsVZHkPSG5ttfutu3GEh5hHA9FNCFqDXJ5T18RE/exec', {
    method: 'POST',
    body: JSON.stringify({
      action: 'saveAttendance',
      attendance: attendanceData
    }),
    headers: { 'Content-Type': 'application/json' }
  })
  .then(res => res.json())
  .then(data => {
    if (data.success) {
      showNotification(monitorNotification, 'Asistencia guardada en Google Sheets', 'success');
    } else {
      showNotification(monitorNotification, 'Error al guardar asistencia', 'error');
    }
  });
}

// Admin page functions

// Load dashboard data
function loadDashboardData() {
    if (!window.totalStudents) return;
    
    // Total students
    let studentsCount = 0;
    Object.values(mockStudentsByMonitor).forEach(students => {
        studentsCount += students.length;
    });
    window.totalStudents.textContent = studentsCount;
    
    // Today's attendance
    const today = new Date().toISOString().split('T')[0];
    const todayRecords = mockAttendance.filter(record => record.date === today && record.attendance === 'Presente');
    window.todayAttendance.textContent = todayRecords.length;
    
    // Attendance percentage
    const percentage = studentsCount > 0 ? Math.round((todayRecords.length / studentsCount) * 100) : 0;
    window.attendancePercentage.textContent = percentage + '%';
    
    // Unregistered students
    const unregistered = mockAttendance.filter(record => record.status === 'no-matriculado' && record.date === today);
    window.unregisteredStudents.textContent = unregistered.length;
}

// Load attendance data
function loadAttendanceData() {
    if (!window.adminAttendanceTable) return;

    window.adminAttendanceTable.innerHTML = '';
    
    mockAttendance.forEach(record => {
        const row = document.createElement('tr');
        row.innerHTML = `
            <td>${record.doc}</td>
            <td>${record.name}</td>
            <td>${record.phone}</td>
            <td>${record.career}</td>
            <td>${record.monitor}</td>
            <td>${record.date}</td>
            <td>${record.time}</td>
            <td>${record.status}</td>
            <td>${record.attendance}</td>
        `;
        window.adminAttendanceTable.appendChild(row);
    });
}

// Load monitor options for filters
function loadMonitorOptions() {
    // Load unique monitors from mock data
    const monitors = [...new Set(mockAttendance.map(record => record.monitor))];
    
    // Populate filter dropdown
    const filterMonitor = document.getElementById('filter-monitor');
    if (filterMonitor) {
        filterMonitor.innerHTML = '<option value="">Todos</option>';
        monitors.forEach(monitor => {
            const option = document.createElement('option');
            option.value = monitor;
            option.textContent = monitor;
            filterMonitor.appendChild(option);
        });
    }
    
    // Populate report dropdown
    const reportMonitor = document.getElementById('report-monitor');
    if (reportMonitor) {
        reportMonitor.innerHTML = '<option value="">Todos</option>';
        monitors.forEach(monitor => {
            const option = document.createElement('option');
            option.value = monitor;
            option.textContent = monitor;
            reportMonitor.appendChild(option);
        });
    }
}

// Filter attendance data
function applyFilters() {
    if (!window.adminAttendanceTable) return;
    
    const dateFilter = document.getElementById('filter-date')?.value;
    const careerFilter = document.getElementById('filter-career')?.value;
    const monitorFilter = window.filterMonitor?.value;
    const statusFilter = document.getElementById('filter-status')?.value;
    
    window.adminAttendanceTable.innerHTML = '';
    
    mockAttendance.forEach(record => {
        // Apply filters
        if (
            (!dateFilter || record.date === dateFilter) &&
            (!careerFilter || record.career === careerFilter) &&
            (!monitorFilter || record.monitor === monitorFilter) &&
            (!statusFilter || 
             (statusFilter === 'matriculado' && record.status === 'matriculado') ||
             (statusFilter === 'no-matriculado' && record.status === 'no-matriculado'))
        ) {
            const row = document.createElement('tr');
            row.innerHTML = `
                <td>${record.doc}</td>
                <td>${record.name}</td>
                <td>${record.phone}</td>
                <td>${record.career}</td>
                <td>${record.monitor}</td>
                <td>${record.date}</td>
                <td>${record.time}</td>
                <td>${record.status}</td>
                <td>${record.attendance}</td>
            `;
            window.adminAttendanceTable.appendChild(row);
        }
    });
}

// Generate report
function generateReport() {
    if (!window.reportPreview) return;
    
    const reportType = document.getElementById('report-type')?.value;
    const startDate = document.getElementById('report-date-start')?.value;
    const endDate = document.getElementById('report-date-end')?.value;
    const careerFilter = document.getElementById('report-career')?.value;
    const monitorFilter = document.getElementById('report-monitor')?.value;
    
    // Simple validation
    if (!startDate || !endDate) {
        showNotification(window.adminNotification, 'Por favor, seleccione fechas para el informe', 'error');
        return;
    }
    
    // Filter data based on criteria
    let filteredData = mockAttendance.filter(record => {
        return (
            record.date >= startDate && 
            record.date <= endDate &&
            (!careerFilter || record.career === careerFilter) &&
            (!monitorFilter || record.monitor === monitorFilter)
        );
    });
    
    // Generate report HTML
    let reportHTML = '<h3>Informe de Asistencia</h3>';
    reportHTML += `<p>Período: ${startDate} a ${endDate}</p>`;
    
    if (careerFilter) {
        reportHTML += `<p>Carrera: ${careerFilter}</p>`;
    }
    
    if (monitorFilter) {
        reportHTML += `<p>Monitor: ${monitorFilter}</p>`;
    }
    
    // Statistics based on report type
    switch(reportType) {
        case 'daily':
            reportHTML += generateDailyReport(filteredData);
            break;
        case 'weekly':
            reportHTML += generateWeeklyReport(filteredData, startDate, endDate);
            break;
        case 'monthly':
            reportHTML += generateMonthlyReport(filteredData);
            break;
        case 'by-career':
            reportHTML += generateCareerReport(filteredData);
            break;
        case 'by-monitor':
            reportHTML += generateMonitorReport(filteredData);
            break;
        case 'by-semester':
            reportHTML += '<p>No hay datos de semestre disponibles para los estudiantes en este momento.</p>';
            break;
        case 'unregistered':
            reportHTML += generateUnregisteredReport(filteredData);
            break;
        default:
            reportHTML += '<p>Seleccione un tipo de informe válido.</p>';
    }
    
    // Display report
    window.reportPreview.innerHTML = reportHTML;
    
    showNotification(window.adminNotification, 'Informe generado correctamente', 'success');
}

// Report generation functions
function generateDailyReport(data) {
    // Group by date
    const dateGroups = {};
    data.forEach(record => {
        if (!dateGroups[record.date]) {
            dateGroups[record.date] = { total: 0, present: 0, absent: 0 };
        }
        
        dateGroups[record.date].total++;
        if (record.attendance === 'Presente') {
            dateGroups[record.date].present++;
        } else {
            dateGroups[record.date].absent++;
        }
    });
    
    let html = '<table><thead><tr><th>Fecha</th><th>Total</th><th>Presentes</th><th>Ausentes</th><th>% Asistencia</th></tr></thead><tbody>';
    
    Object.keys(dateGroups).sort().forEach(date => {
        const group = dateGroups[date];
        const attendance = group.total > 0 ? Math.round((group.present / group.total) * 100) : 0;
        
        html += `<tr>
            <td>${date}</td>
            <td>${group.total}</td>
            <td>${group.present}</td>
            <td>${group.absent}</td>
            <td>${attendance}%</td>
        </tr>`;
    });
    
    html += '</tbody></table>';
    return html;
}

function generateWeeklyReport(data, startDate, endDate) {
    // Convert date strings to Date objects
    const start = new Date(startDate);
    const end = new Date(endDate);
    
    // Calculate the number of weeks
    const diffTime = Math.abs(end - start);
    const diffDays = Math.ceil(diffTime / (1000 * 60 * 60 * 24));
    const numberOfWeeks = Math.ceil(diffDays / 7);
    
    // Group by week
    const weekGroups = {};
    
    data.forEach(record => {
        const recordDate = new Date(record.date);
        const weekNumber = Math.ceil((recordDate - start) / (1000 * 60 * 60 * 24 * 7));
        
        if (!weekGroups[weekNumber]) {
            weekGroups[weekNumber] = { total: 0, present: 0, absent: 0 };
        }
        
        weekGroups[weekNumber].total++;
        if (record.attendance === 'Presente') {
            weekGroups[weekNumber].present++;
        } else {
            weekGroups[weekNumber].absent++;
        }
    });
    
    let html = '<table><thead><tr><th>Semana</th><th>Total</th><th>Presentes</th><th>Ausentes</th><th>% Asistencia</th></tr></thead><tbody>';
    
    for (let i = 1; i <= numberOfWeeks; i++) {
        const group = weekGroups[i] || { total: 0, present: 0, absent: 0 };
        const attendance = group.total > 0 ? Math.round((group.present / group.total) * 100) : 0;
        
        html += `<tr>
            <td>Semana ${i}</td>
            <td>${group.total}</td>
            <td>${group.present}</td>
            <td>${group.absent}</td>
            <td>${attendance}%</td>
        </tr>`;
    }
    
    html += '</tbody></table>';
    return html;
}

function generateMonthlyReport(data) {
    // Group by month
    const monthGroups = {};
    
    data.forEach(record => {
        const month = record.date.substring(0, 7); // YYYY-MM
        
        if (!monthGroups[month]) {
            monthGroups[month] = { total: 0, present: 0, absent: 0 };
        }
        
        monthGroups[month].total++;
        if (record.attendance === 'Presente') {
            monthGroups[month].present++;
        } else {
            monthGroups[month].absent++;
        }
    });
    
    let html = '<table><thead><tr><th>Mes</th><th>Total</th><th>Presentes</th><th>Ausentes</th><th>% Asistencia</th></tr></thead><tbody>';
    
    Object.keys(monthGroups).sort().forEach(month => {
        const group = monthGroups[month];
        const attendance = group.total > 0 ? Math.round((group.present / group.total) * 100) : 0;
        
        // Format month from YYYY-MM to Month YYYY
        const date = new Date(month + '-01');
        const formattedMonth = date.toLocaleDateString('es-ES', { month: 'long', year: 'numeric' });
        
        html += `<tr>
            <td>${formattedMonth}</td>
            <td>${group.total}</td>
            <td>${group.present}</td>
            <td>${group.absent}</td>
            <td>${attendance}%</td>
        </tr>`;
    });
    
    html += '</tbody></table>';
    return html;
}

function generateCareerReport(data) {
    // Group by career
    const careerGroups = {};
    
    data.forEach(record => {
        if (!careerGroups[record.career]) {
            careerGroups[record.career] = { total: 0, present: 0, absent: 0 };
        }
        
        careerGroups[record.career].total++;
        if (record.attendance === 'Presente') {
            careerGroups[record.career].present++;
        } else {
            careerGroups[record.career].absent++;
        }
    });
    
    let html = '<table><thead><tr><th>Carrera</th><th>Total</th><th>Presentes</th><th>Ausentes</th><th>% Asistencia</th></tr></thead><tbody>';
    
    Object.keys(careerGroups).sort().forEach(career => {
        const group = careerGroups[career];
        const attendance = group.total > 0 ? Math.round((group.present / group.total) * 100) : 0;
        
        html += `<tr>
            <td>${career}</td>
            <td>${group.total}</td>
            <td>${group.present}</td>
            <td>${group.absent}</td>
            <td>${attendance}%</td>
        </tr>`;
    });
    
    html += '</tbody></table>';
    return html;
}

function generateMonitorReport(data) {
    // Group by monitor
    const monitorGroups = {};
    
    data.forEach(record => {
        if (!monitorGroups[record.monitor]) {
            monitorGroups[record.monitor] = { total: 0, present: 0, absent: 0 };
        }
        
        monitorGroups[record.monitor].total++;
        if (record.attendance === 'Presente') {
            monitorGroups[record.monitor].present++;
        } else {
            monitorGroups[record.monitor].absent++;
        }
    });
    
    let html = '<table><thead><tr><th>Monitor</th><th>Total</th><th>Presentes</th><th>Ausentes</th><th>% Asistencia</th></tr></thead><tbody>';
    
    Object.keys(monitorGroups).sort().forEach(monitor => {
        const group = monitorGroups[monitor];
        const attendance = group.total > 0 ? Math.round((group.present / group.total) * 100) : 0;
        
        html += `<tr>
            <td>${monitor}</td>
            <td>${group.total}</td>
            <td>${group.present}</td>
            <td>${group.absent}</td>
            <td>${attendance}%</td>
        </tr>`;
    });
    
    html += '</tbody></table>';
    return html;
}

function generateUnregisteredReport(data) {
    // Filter unregistered students
    const unregistered = data.filter(record => record.status === 'no-matriculado');
    
    let html = '<table><thead><tr><th>Documento</th><th>Nombre</th><th>Teléfono</th><th>Carrera</th><th>Monitor</th><th>Fecha</th></tr></thead><tbody>';
    
    unregistered.forEach(record => {
        html += `<tr>
            <td>${record.doc}</td>
            <td>${record.name}</td>
            <td>${record.phone}</td>
            <td>${record.career}</td>
            <td>${record.monitor}</td>
            <td>${record.date}</td>
        </tr>`;
    });
    
    if (unregistered.length === 0) {
        html += '<tr><td colspan="6">No hay estudiantes no matriculados para el período seleccionado.</td></tr>';
    }
    
    html += '</tbody></table>';
    return html;
}

// Download report
function downloadReport() {
    // In a real app, this would generate a PDF or Excel file
    const reportContent = window.reportPreview?.innerHTML || '';
    
    if (!reportContent) {
        showNotification(window.adminNotification, 'No hay informe para descargar', 'error');
        return;
    }
    
    // For demo purposes, let's create a simple text download
    const blob = new Blob([reportContent], { type: 'text/html' });
    const url = URL.createObjectURL(blob);
    const a = document.createElement('a');
    a.href = url;
    a.download = 'reporte_asistencia.html';
    document.body.appendChild(a);
    a.click();
    document.body.removeChild(a);
    URL.revokeObjectURL(url);
    
    showNotification(window.adminNotification, 'Informe descargado correctamente', 'success');
}

// Draw charts for admin dashboard
function drawCharts() {
    drawAttendanceChart();
    drawCareerChart();
    drawMonitorPerformanceChart();
}

// Draw attendance chart
function drawAttendanceChart() {
    const ctx = document.getElementById('attendance-chart');
    if (!ctx) return;
    
    // Get last 7 days of data
    const today = new Date();
    const dates = [];
    const present = [];
    const absent = [];
    
    for (let i = 6; i >= 0; i--) {
        const date = new Date(today);
        date.setDate(today.getDate() - i);
        const dateStr = date.toISOString().split('T')[0];
        dates.push(dateStr);
        
        const dayRecords = mockAttendance.filter(record => record.date === dateStr);
        present.push(dayRecords.filter(record => record.attendance === 'Presente').length);
        absent.push(dayRecords.filter(record => record.attendance === 'Ausente').length);
    }
    
    new Chart(ctx, {
        type: 'bar',
        data: {
            labels: dates,
            datasets: [
                {
                    label: 'Presentes',
                    data: present,
                    backgroundColor: 'rgba(75, 192, 192, 0.2)',
                    borderColor: 'rgba(75, 192, 192, 1)',
                    borderWidth: 1
                },
                {
                    label: 'Ausentes',
                    data: absent,
                    backgroundColor: 'rgba(255, 99, 132, 0.2)',
                    borderColor: 'rgba(255, 99, 132, 1)',
                    borderWidth: 1
                }
            ]
        },
        options: {
            scales: {
                y: {
                    beginAtZero: true
                }
            }
        }
    });
}

// Draw career distribution chart
function drawCareerChart() {
    const ctx = document.getElementById('career-chart');
    if (!ctx) return;
    
    // Group students by career
    const careerCounts = {};
    
    Object.values(mockStudentsByMonitor).forEach(students => {
        students.forEach(student => {
            if (!careerCounts[student.career]) {
                careerCounts[student.career] = 0;
            }
            careerCounts[student.career]++;
        });
    });
    
    new Chart(ctx, {
        type: 'pie',
        data: {
            labels: Object.keys(careerCounts),
            datasets: [
                {
                    data: Object.values(careerCounts),
                    backgroundColor: [
                        'rgba(255, 99, 132, 0.2)',
                        'rgba(54, 162, 235, 0.2)',
                        'rgba(255, 206, 86, 0.2)',
                        'rgba(75, 192, 192, 0.2)',
                        'rgba(153, 102, 255, 0.2)'
                    ],
                    borderColor: [
                        'rgba(255, 99, 132, 1)',
                        'rgba(54, 162, 235, 1)',
                        'rgba(255, 206, 86, 1)',
                        'rgba(75, 192, 192, 1)',
                        'rgba(153, 102, 255, 1)'
                    ],
                    borderWidth: 1
                }
            ]
        }
    });
}

// Draw monitor performance chart
function drawMonitorPerformanceChart() {
    const ctx = document.getElementById('monitor-chart');
    if (!ctx) return;
    
    // Calculate attendance rates by monitor
    const monitorPerformance = {};
    
    mockAttendance.forEach(record => {
        if (!monitorPerformance[record.monitor]) {
            monitorPerformance[record.monitor] = { total: 0, present: 0 };
        }
        
        monitorPerformance[record.monitor].total++;
        if (record.attendance === 'Presente') {
            monitorPerformance[record.monitor].present++;
        }
    });
    
    const monitors = Object.keys(monitorPerformance);
    const rates = monitors.map(monitor => {
        const { total, present } = monitorPerformance[monitor];
        return total > 0 ? Math.round((present / total) * 100) : 0;
    });
    
    new Chart(ctx, {
        type: 'bar',
        data: {
            labels: monitors,
            datasets: [
                {
                    label: '% Asistencia',
                    data: rates,
                    backgroundColor: 'rgba(153, 102, 255, 0.2)',
                    borderColor: 'rgba(153, 102, 255, 1)',
                    borderWidth: 1
                }
            ]
        },
        options: {
            scales: {
                y: {
                    beginAtZero: true,
                    max: 100
                }
            }
        }
    });
}

// Load users table
function loadUsersTable() {
    if (!window.usersTable) return;
    
    window.usersTable.innerHTML = '';
    
    mockUsers.forEach(user => {
        const row = document.createElement('tr');
        row.innerHTML = `
            <td>${user.username}</td>
            <td>${user.name}</td>
            <td>${user.type}</td>
            <td>${user.career || '-'}</td>
            <td>${user.semester || '-'}</td>
            <td>${user.module || '-'}</td>
            <td>${user.lastAccess}</td>
            <td>
                <button class="btn-warning edit-user" data-username="${user.username}">Editar</button>
                <button class="btn-danger delete-user" data-username="${user.username}">Eliminar</button>
            </td>
        `;
        window.usersTable.appendChild(row);
    });
    
    // Add event listeners to buttons
    document.querySelectorAll('.edit-user').forEach(button => {
        button.addEventListener('click', function() {
            const username = this.getAttribute('data-username');
            editUser(username);
        });
    });
    
    document.querySelectorAll('.delete-user').forEach(button => {
        button.addEventListener('click', function() {
            const username = this.getAttribute('data-username');
            deleteUser(username);
        });
    });
}

// Edit user
function editUser(username) {
    const user = mockUsers.find(u => u.username === username);
    if (!user) return;
    
    // In a real app, you would show a form to edit the user
    showNotification(window.adminNotification, 'Edición de usuario no implementada en esta demo', 'info');
}

// Delete user
function deleteUser(username) {
    const userIndex = mockUsers.findIndex(u => u.username === username);
    if (userIndex === -1) return;
    
    // Don't allow deleting the admin user
    if (username === 'admin') {
        showNotification(window.adminNotification, 'No se puede eliminar el usuario administrador', 'error');
        return;
    }
    
    // Remove from mock data
    mockUsers.splice(userIndex, 1);
    
    // Reload table
    loadUsersTable();
    
    showNotification(window.adminNotification, 'Usuario eliminado correctamente', 'success');
}

// Add user
function addUser() {
    const username = document.getElementById('new-user-username')?.value;
    const name = document.getElementById('new-user-name')?.value;
    const type = document.getElementById('new-user-type')?.value;
    const career = document.getElementById('new-user-career')?.value || '';
    const semester = document.getElementById('new-user-semester')?.value || '';
    const module = document.getElementById('new-user-module')?.value || '';
    
    // Simple validation
    if (!username || !name || !type) {
        showNotification(window.adminNotification, 'Por favor, complete los campos obligatorios', 'error');
        return;
    }
    
    // Check if user already exists
    if (mockUsers.some(u => u.username === username)) {
        showNotification(window.adminNotification, 'El usuario ya existe', 'error');
        return;
    }
    
    // Add user
    mockUsers.push({
        username,
        name,
        type,
        career,
        semester,
        module,
        lastAccess: '-'
    });
    
    // Reset form
    document.getElementById('new-user-username').value = '';
    document.getElementById('new-user-name').value = '';
    document.getElementById('new-user-type').value = 'monitor';
    document.getElementById('new-user-career').value = '';
    document.getElementById('new-user-semester').value = '';
    document.getElementById('new-user-module').value = '';
    
    // Reload table
    loadUsersTable();
    
    showNotification(window.adminNotification, 'Usuario agregado correctamente', 'success');
}

// Show notification
function showNotification(element, message, type) {
    if (!element) return;
    
    element.textContent = message;
    element.className = 'notification ' + type;
    element.style.display = 'block';
    
    // Auto-hide after 3 seconds
    setTimeout(() => {
        element.style.display = 'none';
    }, 3000);
}
 </script>
</body>
</html>
