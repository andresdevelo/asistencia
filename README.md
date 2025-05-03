<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Sistema de Asistencia ELYON YIREH</title>
<style>
        :root {
            --primary: #2c73d2;
            --primary-dark: #1c5eb0;
            --primary-light: #e6f0ff;
            --secondary: #6c757d;
            --success: #28a745;
            --danger: #dc3545;
            --light: #f5f5f5;
            --dark: #333;
            --gray: #555;
            --gray-light: #ddd;
            --white: #fff;
            --shadow: 0 4px 12px rgba(0, 0, 0, 0.08);
            --transition: all 0.3s ease;
        }

        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
            font-family: 'Segoe UI', 'Arial', sans-serif;
        }
        
        body {
            background-color: var(--light);
            color: var(--dark);
            line-height: 1.6;
            font-size: 16px;
        }
        
        .container {
            max-width: 1200px;
            margin: 0 auto;
            padding: 20px;
        }
        
        .hidden {
            display: none;
        }
        
        /* Auth Styles */
        .auth-container {
            max-width: 500px;
            margin: 80px auto;
            background: var(--white);
            padding: 40px;
            border-radius: 16px;
            box-shadow: var(--shadow);
            transition: var(--transition);
        }
        
        .auth-title {
            text-align: center;
            margin-bottom: 10px;
            color: var(--primary);
            font-size: 32px;
            font-weight: 700;
            letter-spacing: -0.5px;
        }
        
        .auth-subtitle {
            text-align: center;
            margin-bottom: 30px;
            color: var(--gray);
            font-size: 16px;
        }
        
        .auth-options {
            display: flex;
            margin-bottom: 30px;
            border-bottom: 1px solid var(--gray-light);
        }
        
        .auth-option {
            flex: 1;
            text-align: center;
            padding: 16px 10px;
            cursor: pointer;
            color: var(--gray);
            font-weight: 600;
            transition: var(--transition);
            position: relative;
        }
        
        .auth-option.active {
            color: var(--primary);
        }
        
        .auth-option.active::after {
            content: '';
            position: absolute;
            bottom: 0;
            left: 0;
            width: 100%;
            height: 3px;
            background-color: var(--primary);
            border-radius: 3px 3px 0 0;
        }
        
        .auth-form {
            display: none;
        }
        
        .auth-form.active {
            display: block;
            animation: fadeIn 0.4s ease;
        }
        
        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(10px); }
            to { opacity: 1; transform: translateY(0); }
        }
        
        .form-group {
            margin-bottom: 24px;
            position: relative;
        }
        
        label {
            display: block;
            margin-bottom: 8px;
            font-weight: 600;
            color: var(--gray);
            font-size: 15px;
        }
        
        input, select {
            width: 100%;
            padding: 14px 16px;
            border: 2px solid var(--gray-light);
            border-radius: 10px;
            font-size: 15px;
            transition: var(--transition);
            background-color: var(--white);
        }
        
        input:focus, select:focus {
            border-color: var(--primary);
            outline: none;
            box-shadow: 0 0 0 3px rgba(44, 115, 210, 0.15);
        }
        
        .toggle-password {
            position: absolute;
            right: 16px;
            top: 42px;
            cursor: pointer;
            color: var(--gray);
            transition: var(--transition);
        }
        
        .toggle-password:hover {
            color: var(--primary);
        }
        
        button {
            background-color: var(--primary);
            color: var(--white);
            padding: 14px 24px;
            border: none;
            border-radius: 10px;
            cursor: pointer;
            font-weight: 600;
            font-size: 16px;
            width: 100%;
            margin-top: 16px;
            transition: var(--transition);
            position: relative;
            overflow: hidden;
        }
        
        button:hover {
            background-color: var(--primary-dark);
            transform: translateY(-2px);
            box-shadow: 0 6px 12px rgba(44, 115, 210, 0.2);
        }
        
        button:active {
            transform: translateY(0);
            box-shadow: 0 3px 6px rgba(44, 115, 210, 0.1);
        }
        
        .btn-secondary {
            background-color: var(--secondary);
        }
        
        .btn-secondary:hover {
            background-color: #5a6268;
            box-shadow: 0 6px 12px rgba(108, 117, 125, 0.2);
        }
        
        .btn-danger {
            background-color: var(--danger);
            padding: 8px 14px;
            font-size: 14px;
            width: auto;
        }
        
        .btn-danger:hover {
            background-color: #c82333;
            box-shadow: 0 6px 12px rgba(220, 53, 69, 0.2);
        }
        
        .btn-edit {
            background-color: var(--success);
            padding: 8px 14px;
            font-size: 14px;
            width: auto;
            margin-right: 10px;
        }
        
        .btn-edit:hover {
            background-color: #218838;
            box-shadow: 0 6px 12px rgba(40, 167, 69, 0.2);
        }
        
        /* Notification */
        .notification {
            padding: 16px;
            margin-bottom: 24px;
            border-radius: 10px;
            text-align: center;
            display: none;
            font-weight: 600;
            animation: slideDown 0.4s ease;
        }
        
        @keyframes slideDown {
            from { transform: translateY(-20px); opacity: 0; }
            to { transform: translateY(0); opacity: 1; }
        }
        
        .notification.success {
            background-color: #d4edda;
            color: #155724;
            border-left: 4px solid #28a745;
        }
        
        .notification.error {
            background-color: #f8d7da;
            color: #721c24;
            border-left: 4px solid #dc3545;
        }
        
        /* Monitor Page */
        .monitor-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            background-color: var(--white);
            padding: 24px;
            border-radius: 12px;
            margin-bottom: 24px;
            box-shadow: var(--shadow);
        }
        
        .search-container {
            display: flex;
            margin-bottom: 24px;
            border-radius: 10px;
            overflow: hidden;
            box-shadow: 0 2px 8px rgba(0, 0, 0, 0.05);
        }
        
        .search-container input {
            flex: 1;
            margin-right: 0;
            border-radius: 10px 0 0 10px;
            border-right: none;
        }
        
        .search-container button {
            width: auto;
            border-radius: 0 10px 10px 0;
            margin-top: 0;
        }
        
        table {
            width: 100%;
            border-collapse: separate;
            border-spacing: 0;
            margin-bottom: 24px;
            background-color: var(--white);
            box-shadow: var(--shadow);
            border-radius: 12px;
            overflow: hidden;
        }
        
        th, td {
            padding: 16px 20px;
            text-align: left;
        }
        
        th {
            background-color: var(--primary-light);
            color: var(--primary-dark);
            font-weight: 600;
            white-space: nowrap;
        }
        
        tr:not(:last-child) td {
            border-bottom: 1px solid var(--gray-light);
        }
        
        tbody tr {
            transition: var(--transition);
        }
        
        tbody tr:hover {
            background-color: rgba(44, 115, 210, 0.05);
        }
        
        /* Admin Page */
        .dashboard-cards {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
            gap: 24px;
            margin-bottom: 36px;
        }
        
        .card {
            background-color: var(--white);
            padding: 24px;
            border-radius: 12px;
            box-shadow: var(--shadow);
            text-align: center;
            transition: var(--transition);
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
        }
        
        .card:hover {
            transform: translateY(-5px);
            box-shadow: 0 8px 24px rgba(0, 0, 0, 0.1);
        }
        
        .card h3 {
            margin-bottom: 12px;
            font-size: 18px;
            color: var(--gray);
            font-weight: 600;
        }
        
        .number {
            font-size: 36px;
            font-weight: 700;
            color: var(--primary);
            margin-bottom: 10px;
        }
        
        .tabs {
            display: flex;
            margin-bottom: 24px;
            background-color: var(--white);
            border-radius: 12px;
            overflow: hidden;
            box-shadow: var(--shadow);
        }
        
        .tab {
            padding: 16px 24px;
            cursor: pointer;
            flex: 1;
            text-align: center;
            color: var(--gray);
            font-weight: 600;
            transition: var(--transition);
        }
        
        .tab:hover:not(.active) {
            background-color: rgba(44, 115, 210, 0.05);
        }
        
        .tab.active {
            background-color: var(--primary);
            color: var(--white);
        }
        
        .tab-content {
            display: none;
            background-color: var(--white);
            padding: 30px;
            border-radius: 12px;
            margin-bottom: 24px;
            box-shadow: var(--shadow);
        }
        
        .tab-content.active {
            display: block;
            animation: fadeIn 0.4s ease;
        }
        
        .tab-content h2 {
            margin-bottom: 24px;
            color: var(--dark);
            font-weight: 700;
            font-size: 24px;
        }
        
        /* Modal */
        .modal {
            display: none;
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background-color: rgba(0, 0, 0, 0.6);
            z-index: 1000;
            justify-content: center;
            align-items: center;
            backdrop-filter: blur(4px);
        }
        
        .modal-content {
            background-color: var(--white);
            padding: 30px;
            border-radius: 16px;
            width: 500px;
            max-width: 90%;
            box-shadow: 0 10px 40px rgba(0, 0, 0, 0.15);
            transform: scale(0.95);
            transition: transform 0.3s ease;
        }
        
        .modal.active .modal-content {
            transform: scale(1);
        }
        
        .modal-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 20px;
            padding-bottom: 14px;
            border-bottom: 1px solid var(--gray-light);
        }
        
        .modal-header h2 {
            margin: 0;
            color: var(--primary);
            font-size: 22px;
        }
        
        .close-modal {
            font-size: 24px;
            cursor: pointer;
            color: var(--gray);
            transition: var(--transition);
            width: 36px;
            height: 36px;
            display: flex;
            align-items: center;
            justify-content: center;
            border-radius: 50%;
        }
        
        .close-modal:hover {
            background-color: rgba(220, 53, 69, 0.1);
            color: var(--danger);
        }
        
        /* Responsive */
        @media (max-width: 768px) {
            .monitor-header {
                flex-direction: column;
                gap: 16px;
                align-items: flex-start;
            }
            
            .search-container {
                flex-direction: column;
            }
            
            .search-container input {
                border-radius: 10px;
                border-right: 2px solid var(--gray-light);
                margin-bottom: 10px;
            }
            
            .search-container button {
                border-radius: 10px;
            }
            
            .tabs {
                flex-wrap: wrap;
            }
            
            .tab {
                flex: 0 0 50%;
                padding: 14px;
            }
            
            .auth-container {
                padding: 30px 20px;
            }
            
            th, td {
                padding: 12px 10px;
            }
            
            table {
                display: block;
                overflow-x: auto;
            }
        }
        
        /* Login Specific Styles */
        .login-container {
            max-width: 450px;
            margin: 80px auto;
            padding: 40px;
            background: var(--white);
            border-radius: 16px;
            box-shadow: var(--shadow);
            transition: var(--transition);
        }

        .login-container h2 {
            text-align: center;
            margin-bottom: 24px;
            color: var(--primary);
            font-size: 32px;
            font-weight: 700;
            letter-spacing: -0.5px;
        }

        .login-container .form-group {
            margin-bottom: 24px;
            position: relative;
        }

        .login-container label {
            display: block;
            margin-bottom: 8px;
            color: var(--gray);
            font-weight: 600;
            font-size: 15px;
        }

        .login-container input[type="text"],
        .login-container input[type="password"] {
            width: 100%;
            padding: 14px 16px;
            padding-right: 46px;
            border: 2px solid var(--gray-light);
            border-radius: 10px;
            font-size: 15px;
            transition: var(--transition);
        }

        .login-container input[type="text"]:focus,
        .login-container input[type="password"]:focus {
            border-color: var(--primary);
            outline: none;
            box-shadow: 0 0 0 3px rgba(44, 115, 210, 0.15);
        }

        .login-container .toggle-password {
            position: absolute;
            right: 16px;
            top: 42px;
            cursor: pointer;
            font-size: 18px;
            color: var(--gray);
            transition: var(--transition);
        }

        .login-container .toggle-password:hover {
            color: var(--primary);
        }

        .login-container button {
            width: 100%;
            padding: 14px;
            background-color: var(--primary);
            border: none;
            border-radius: 10px;
            color: white;
            font-weight: 600;
            font-size: 16px;
            cursor: pointer;
            transition: var(--transition);
            position: relative;
            overflow: hidden;
        }

        .login-container button:hover {
            background-color: var(--primary-dark);
            transform: translateY(-2px);
            box-shadow: 0 6px 12px rgba(44, 115, 210, 0.2);
        }

        .login-container button:active {
            transform: translateY(0);
            box-shadow: 0 3px 6px rgba(44, 115, 210, 0.1);
        }
        
        /* Animations */
        @keyframes pulse {
            0% { transform: scale(1); }
            50% { transform: scale(1.05); }
            100% { transform: scale(1); }
        }
    </style>
</head>
<body>
    <!-- Auth Container (Login/Register) -->
    <div id="auth-container" class="container">
        <div class="auth-container">
            <h1 class="auth-title">𝑬𝑳𝒀𝑶𝑵 𝒀𝑰𝑹𝑬𝑯</h1>
            <p class="auth-subtitle">Portal Institucional</p>
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
                <div class="form-group" style="position: relative;">
    <label for="password">Contraseña:</label>
    <input type="password" id="password" name="password" required style="width: 100%;">
    <span id="toggle-login-password" style="position: absolute; right: 10px; top: 35px; cursor: pointer;">👁️</span>
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
                    <input type="tel" id="reg-phone" name="reg-phone" placeholder="3XXXXXXXXX" required>
                </div>
                <div class="form-group">
                    <label for="reg-career">Carrera:</label>
                    <select id="reg-career" name="reg-career" required>
                        <option value="">Seleccione una carrera</option>
                        <option value="Derecho">Derecho</option>
                        <option value="Arquitectura">Arquitectura</option>
                        <option value="Contabilidad">Contabilidad</option>
                        <option value="Ingeniería de Sistemas">Ingeniería de Sistemas</option>
                        <option value="Medicina">Medicina</option>
                        <option value="Psicología">Psicología</option>
                        <option value="Ingeniería Civil">Ingeniería Civil</option>
                        <option value="Economía">Economía</option>
                    </select>
                </div>
                <div class="form-group">
                    <label for="reg-module">Nombre del Módulo:</label>
                    <input type="text" id="reg-module" name="reg-module" required>
                </div>
                <div class="form-group">
                    <label for="reg-horario">Horario:</label>
                    <select id="reg-horario" name="reg-horario" required>
                        <option value="">Seleccione un horario</option>
                        <option value="08:00 - 10:00">08:00 - 10:00</option>
                        <option value="10:00 - 12:00">10:00 - 12:00</option>
                        <option value="12:00 - 14:00">12:00 - 14:00</option>
                        <option value="14:00 - 16:00">14:00 - 16:00</option>
                    </select>
                </div>
<div class="form-group">
    <label for="reg-semester">Semestre:</label>
    <select id="reg-semester" name="reg-semester" required>
        <option value="">Seleccione un semestre</option>
        <option value="1">1</option>
        <option value="2">2</option>
        <option value="3">3</option>
        <option value="4">4</option>
    </select>
</div>
                <div class="form-group">
                    <label for="reg-username">Usuario:</label>
                    <input type="text" id="reg-username" name="reg-username" required>
                </div>
                <div class="form-group" style="position: relative;">
    <label for="reg-password">Contraseña:</label>
    <input type="password" id="reg-password" name="reg-password" required style="width: 100%;">
    <span id="toggle-password" style="position: absolute; right: 10px; top: 35px; cursor: pointer;">👁️</span>
</div>

                <button id="register-btn">Registrarse</button>
<div id="register-notification" class="notification"></div>


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
                <p id="monitor-module">Módulo</p>
                <p id="monitor-horario">Horario</p>
                <p id="monitor-semester">Semestre</p>
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
                <input type="tel" id="new-student-phone" placeholder="3XXXXXXXXX">
            </div>
            <div class="form-group">
                <label for="new-student-career">Carrera:</label>
                <select id="new-student-career">
                    <option value="">Seleccione una carrera</option>
                    <option value="Derecho">Derecho</option>
                    <option value="Arquitectura">Arquitectura</option>
                    <option value="Contabilidad">Contabilidad</option>
                    <option value="Ingeniería de Sistemas">Ingeniería de Sistemas</option>
                    <option value="Medicina">Medicina</option>
                    <option value="Psicología">Psicología</option>
                    <option value="Ingeniería Civil">Ingeniería Civil</option>
                    <option value="Economía">Economía</option>
                </select>
            </div>
            <div class="form-group">
                <label for="new-student-status">Estado:</label>
                <select id="new-student-status">
                    <option value="matriculado">Matriculado</option>
                    <option value="no-matriculado">No Matriculado</option>
                </select>
            </div>
            <button id="add-student-btn">Agregar Estudiante</button>
            
            <div style="margin-top: 20px;">
                <button id="save-attendance-btn">Guardar Asistencia</button>
                <button id="monitor-logout-btn" class="btn-secondary">Cerrar Sesión</button>
            </div>
        </div>
    </div>

    <!-- Student Edit Modal -->
    <div id="edit-student-modal" class="modal">
        <div class="modal-content">
            <div class="modal-header">
                <h2>Editar Estudiante</h2>
                <span class="close-modal" id="close-edit-student">&times;</span>
            </div>
            <div class="form-group">
                <label for="edit-student-doc">Documento:</label>
                <input type="text" id="edit-student-doc" readonly>
            </div>
            <div class="form-group">
                <label for="edit-student-name">Nombre:</label>
                <input type="text" id="edit-student-name">
            </div>
            <div class="form-group">
                <label for="edit-student-phone">Número de Teléfono:</label>
                <input type="tel" id="edit-student-phone" placeholder="3XXXXXXXXX">
            </div>
            <div class="form-group">
                <label for="edit-student-career">Carrera:</label>
                <select id="edit-student-career">
                    <option value="">Seleccione una carrera</option>
                    <option value="Derecho">Derecho</option>
                    <option value="Arquitectura">Arquitectura</option>
                    <option value="Contabilidad">Contabilidad</option>
                    <option value="Ingeniería de Sistemas">Ingeniería de Sistemas</option>
                    <option value="Medicina">Medicina</option>
                    <option value="Psicología">Psicología</option>
                    <option value="Ingeniería Civil">Ingeniería Civil</option>
                    <option value="Economía">Economía</option>
                </select>
            </div>
            <div class="form-group">
                <label for="edit-student-status">Estado:</label>
                <select id="edit-student-status">
                    <option value="matriculado">Matriculado</option>
                    <option value="no-matriculado">No Matriculado</option>
                </select>
            </div>
            <button id="save-edit-student-btn">Guardar Cambios</button>
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
            <div class="tab" data-tab="students-mgmt">Gestión de Estudiantes</div>
            <div class="tab" data-tab="user-management">Gestión de Usuarios</div>
        </div>
        
 <div id="attendance-data" class="tab-content active">
    <h2>Datos de Asistencia</h2>
    <div class="form-group">
        <label>Filtrar por:</label>
        <div style="display: flex; flex-wrap: wrap; gap: 10px; margin-bottom: 10px;">
            <input type="date" id="filter-date" style="flex: 1;">
            <select id="filter-career" style="flex: 1;">
                <option value="">Todas las carreras</option>
                <option value="Derecho">Derecho</option>
                <option value="Arquitectura">Arquitectura</option>
                <option value="Contabilidad">Contabilidad</option>
                <option value="Ingeniería de Sistemas">Ingeniería de Sistemas</option>
                <option value="Medicina">Medicina</option>
                <option value="Psicología">Psicología</option>
                <option value="Ingeniería Civil">Ingeniería Civil</option>
                <option value="Economía">Economía</option>
            </select>
            <select id="filter-status" style="flex: 1;">
                <option value="">Todos los estados</option>
                <option value="matriculado">Matriculado</option>
                <option value="no-matriculado">No Matriculado</option>
            </select>
            <select id="filter-attendance" style="flex: 1;">
                <option value="">Todas las asistencias</option>
                <option value="presente">Presente</option>
                <option value="ausente">Ausente</option>
            </select>
            <input type="text" id="filter-doc" placeholder="Buscar por documento o nombre" style="flex: 2;">
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
                <th>Fecha</th>
                <th>Hora</th>
                <th>Estado</th>
                <th>Asistencia</th>
                <th>Acciones</th>
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
                    <option value="Derecho">Derecho</option>
                    <option value="Arquitectura">Arquitectura</option>
                    <option value="Contabilidad">Contabilidad</option>
                    <option value="Ingeniería de Sistemas">Ingeniería de Sistemas</option>
                    <option value="Medicina">Medicina</option>
                    <option value="Psicología">Psicología</option>
                    <option value="Ingeniería Civil">Ingeniería Civil</option>
                    <option value="Economía">Economía</option>
                </select>
            </div>
            <button id="generate-report-btn">Generar Informe</button>
            <button id="download-report-btn" class="btn-secondary">Descargar</button>
            
            <div id="report-preview" style="margin-top: 20px;">
                <!-- Report preview will be displayed here -->
            </div>
        </div>
        
        <div id="students-mgmt" class="tab-content">
            <h2>Gestión de Estudiantes</h2>
            <div class="search-container">
                <input type="text" id="admin-student-search" placeholder="Buscar por nombre o número de documento">
                <button id="admin-search-btn">Buscar</button>
            </div>
            <table id="admin-students-table">
                <thead>
    <tr>
        <th>Documento</th>
        <th>Nombre</th>
        <th>Teléfono</th>
        <th>Carrera</th>
        <th>Estado</th>
        <th>Monitor</th> <!-- NUEVO -->
        <th>Acciones</th>
    </tr>
</thead>

                <tbody>
                    <!-- Students data will be loaded here -->
                </tbody>
            </table>
            
            <h3>Agregar Estudiante</h3>
            <div class="form-group">
                <label for="admin-new-student-doc">Documento:</label>
                <input type="text" id="admin-new-student-doc">
            </div>
            <div class="form-group">
                <label for="admin-new-student-name">Nombre:</label>
                <input type="text" id="admin-new-student-name">
            </div>
            <div class="form-group">
                <label for="admin-new-student-phone">Número de Teléfono:</label>
                <input type="tel" id="admin-new-student-phone" placeholder="3XXXXXXXXX">
            </div>
            <div class="form-group">
                <label for="admin-new-student-career">Carrera:</label>
                <select id="admin-new-student-career">
                    <option value="">Seleccione una carrera</option>
                    <option value="Derecho">Derecho</option>
                    <option value="Arquitectura">Arquitectura</option>
                    <option value="Contabilidad">Contabilidad</option>
                    <option value="Ingeniería de Sistemas">Ingeniería de Sistemas</option>
                    <option value="Medicina">Medicina</option>
                    <option value="Psicología">Psicología</option>
                    <option value="Ingeniería Civil">Ingeniería Civil</option>
                    <option value="Economía">Economía</option>
                </select>
            </div>
            <div class="form-group">
                <label for="admin-new-student-status">Estado:</label>
                <select id="admin-new-student-status">
                    <option value="matriculado">Matriculado</option>
                    <option value="no-matriculado">No Matriculado</option>
                </select>
            </div>
            <button id="admin-add-student-btn">Agregar Estudiante</button>
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
                        <th>Módulo</th>
                        <th>Horario</th>
                       <th>Acciones</th>
                    </tr>
                </thead>
                <tbody>
                    <!-- Users data will be loaded here -->
                </tbody>
            </table>
            
            <h3>Agregar Usuario</h3>
            <div class="form-group">
                <label for="new-user-fullname">Nombre Completo:</label>
                <input type="text" id="new-user-fullname">
            </div>
            <div class="form-group">
                <label for="new-user-id">Número de Identificación:</label>
                <input type="text" id="new-user-id">
            </div>
            <div class="form-group">
                <label for="new-user-phone">Número de Teléfono:</label>
                <input type="tel" id="new-user-phone" placeholder="3XXXXXXXXX">
            </div>
            <div class="form-group">
                <label for="new-user-type">Tipo de Usuario:</label>
                <select id="new-user-type">
                    <option value="monitor">Monitor</option>
                    <option value="admin">Administrador</option>
                </select>
            </div>
            <div class="form-group monitor-fields">
                <label for="new-user-career">Carrera:</label>
                <select id="new-user-career">
                    <option value="">Seleccione una carrera</option>
                    <option value="Derecho">Derecho</option>
                    <option value="Arquitectura">Arquitectura</option>
                    <option value="Contabilidad">Contabilidad</option>
                    <option value="Ingeniería de Sistemas">Ingeniería de Sistemas</option>
                    <option value="Medicina">Medicina</option>
                    <option value="Psicología">Psicología</option>
                    <option value="Ingeniería Civil">Ingeniería Civil</option>
                    <option value="Economía">Economía</option>
                </select>
            </div>
            <div class="form-group monitor-fields">
                <label for="new-user-module">Nombre del Módulo:</label>
                <input type="text" id="new-user-module">
            </div>
            <div class="form-group monitor-fields">
                <label for="new-user-horario">Horario:</label>
                <select id="new-user-horario">
                    <option value="">Seleccione un horario</option>
                    <option value="08:00 - 10:00">08:00 - 10:00</option>
                    <option value="10:00 - 12:00">10:00 - 12:00</option>
                    <option value="12:00 - 14:00">12:00 - 14:00</option>
                    <option value="14:00 - 16:00">14:00 - 16:00</option>
                </select>
            </div>
            <div class="form-group">
                <label for="new-user-username">Usuario:</label>
                <input type="text" id="new-user-username">
            </div>
            <div class="form-group">
                <label for="new-user-password">Contraseña:</label>
                <input type="password" id="new-user-password">
            </div>
            <button id="add-user-btn">Agregar Usuario</button>
        </div>
        
        <div style="margin-top: 20px;">
            <button id="admin-logout-btn" class="btn-secondary">Cerrar Sesión</button>
        </div>
    </div>
    
    <!-- User Edit Modal -->
    <div id="edit-user-modal" class="modal">
        <div class="modal-content">
            <div class="modal-header">
                <h2>Editar Usuario</h2>
                <span class="close-modal" id="close-edit-user">&times;</span>
            </div>
            <div class="form-group">
                <label for="edit-user-username">Usuario:</label>
                <input type="text" id="edit-user-username" readonly>
            </div>
            <div class="form-group">
                <label for="edit-user-fullname">Nombre Completo:</label>
                <input type="text" id="edit-user-fullname">
            </div>
            <div class="form-group">
                <label for="edit-user-id">Número de Identificación:</label>
                <input type="text" id="edit-user-id">
            </div>
            <div class="form-group">
                <label for="edit-user-phone">Número de Teléfono:</label>
                <input type="tel" id="edit-user-phone" placeholder="3XXXXXXXXX">
            </div>
            <div class="form-group">
                <label for="edit-user-type">Tipo de Usuario:</label>
                <select id="edit-user-type">
                    <option value="monitor">Monitor</option>
                    <option value="admin">Administrador</option>
                </select>
            </div>
            <div class="form-group edit-monitor-fields">
                <label for="edit-user-career">Carrera:</label>
                <select id="edit-user-career">
                    <option value="">Seleccione una carrera</option>
                    <option value="Derecho">Derecho</option>
                    <option value="Arquitectura">Arquitectura</option>
                    <option value="Contabilidad">Contabilidad</option>
                    <option value="Ingeniería de Sistemas">Ingeniería de Sistemas</option>
                    <option value="Medicina">Medicina</option>
                    <option value="Psicología">Psicología</option>
                    <option value="Ingeniería Civil">Ingeniería Civil</option>
                    <option value="Economía">Economía</option>
                </select>
            </div>
            <div class="form-group edit-monitor-fields">
                <label for="edit-user-module">Nombre del Módulo:</label>
                <input type="text" id="edit-user-module">
            </div>
            <div class="form-group edit-monitor-fields">
                <label for="edit-user-horario">Horario:</label>
                <select id="edit-user-horario">
                    <option value="">Seleccione un horario</option>
                    <option value="08:00 - 10:00">08:00 - 10:00</option>
                    <option value="10:00 - 12:00">10:00 - 12:00</option>
                    <option value="12:00 - 14:00">12:00 - 14:00</option>
                    <option value="14:00 - 16:00">14:00 - 16:00</option>
                </select>
            </div>
            <div class="form-group">
                <label for="edit-user-password">Nueva Contraseña (dejar en blanco para mantener la actual):</label>
                <input type="password" id="edit-user-password">
            </div>
            <button id="save-edit-user-btn">Guardar Cambios</button>
        </div>
    </div>
<!-- JavaScript -->
    <script>
        // Configuración de Firebase
        // Import the functions you need from the SDKs you need
        import { initializeApp } from "firebase/app";
        import { getAnalytics } from "firebase/analytics";
        import { getDatabase, ref, set, get, child, update, remove, onValue } from "firebase/database";
        import { getAuth, createUserWithEmailAndPassword, signInWithEmailAndPassword, signOut } from "firebase/auth";
        
        // Your web app's Firebase configuration
        const firebaseConfig = {
          apiKey: "AIzaSyBCO6nEnKWjucuZbZsSnZJG95OkAXMXXXs",
          authDomain: "portalinstitucional-c35c6.firebaseapp.com",
          databaseURL: "https://portalinstitucional-c35c6-default-rtdb.firebaseio.com",
          projectId: "portalinstitucional-c35c6",
          storageBucket: "portalinstitucional-c35c6.firebasestorage.app",
          messagingSenderId: "1096756653839",
          appId: "1:1096756653839:web:ee2107a77e8183c453d87a",
          measurementId: "G-58XKMNPH2B"
        };
        
        // Initialize Firebase
        const app = initializeApp(firebaseConfig);
        const analytics = getAnalytics(app);
        const database = getDatabase(app);
        const auth = getAuth(app);

        // Global variables
        let currentUser = null;
        let students = [];
        let attendanceData = [];
        let users = [];
        
        // Initial setup
        document.addEventListener('DOMContentLoaded', function() {
            document.getElementById('apply-filters-btn').addEventListener('click', handleApplyFilters);

            // Cargar datos desde Firebase en vez de mock data
            loadDataFromFirebase();
            
            // Authentication tab switching
            const authOptions = document.querySelectorAll('.auth-option');
            authOptions.forEach(option => {
                option.addEventListener('click', function() {
                    const formId = this.getAttribute('data-form');
                    
                    // Update active tab
                    authOptions.forEach(o => o.classList.remove('active'));
                    this.classList.add('active');
                    
                    // Show selected form
                    document.querySelectorAll('.auth-form').forEach(form => {
                        form.classList.remove('active');
                    });
                    document.getElementById(formId).classList.add('active');
                });
            });
            // Mostrar/Ocultar contraseña en login
            const toggleLogin = document.getElementById('toggle-login-password');
            const inputLogin = document.getElementById('password');

            if (toggleLogin && inputLogin) {
                toggleLogin.addEventListener('click', function () {
                    const isPassword = inputLogin.type === 'password';
                    inputLogin.type = isPassword ? 'text' : 'password';
                    this.textContent = isPassword ? '🙈' : '👁️';
                });
            }

            // Mostrar/Ocultar contraseña en registro
            const toggle = document.getElementById('toggle-password');
            const input = document.getElementById('reg-password');

            if (toggle && input) {
                toggle.addEventListener('click', function () {
                    const isPassword = input.type === 'password';
                    input.type = isPassword ? 'text' : 'password';
                    this.textContent = isPassword ? '🙈' : '👁️';
                });
            }

            
            // Admin tab switching
            const adminTabs = document.querySelectorAll('.tab');
            adminTabs.forEach(tab => {
                tab.addEventListener('click', function() {
                    const tabId = this.getAttribute('data-tab');
                    
                    // Update active tab
                    adminTabs.forEach(t => t.classList.remove('active'));
                    this.classList.add('active');
                    
                    // Show selected content
                    document.querySelectorAll('.tab-content').forEach(content => {
                        content.classList.remove('active');
                    });
                    document.getElementById(tabId).classList.add('active');
                });
            });
            
            // Event listeners for buttons
            document.getElementById('login-btn').addEventListener('click', handleLogin);
            document.getElementById('register-btn').addEventListener('click', handleRegister);
            document.getElementById('monitor-logout-btn').addEventListener('click', handleLogout);
            document.getElementById('admin-logout-btn').addEventListener('click', handleLogout);
            
            document.getElementById('admin-search-btn').addEventListener('click', handleAdminStudentSearch);
            document.getElementById('add-student-btn').addEventListener('click', handleAddStudent);
            document.getElementById('admin-add-student-btn').addEventListener('click', handleAdminAddStudent);
            document.getElementById('save-attendance-btn').addEventListener('click', handleSaveAttendance);
            document.getElementById('close-edit-student').addEventListener('click', closeEditStudentModal);
            document.getElementById('save-edit-student-btn').addEventListener('click', handleSaveEditStudent);
            document.getElementById('close-edit-user').addEventListener('click', closeEditUserModal);
            document.getElementById('save-edit-user-btn').addEventListener('click', handleSaveEditUser);
            document.getElementById('apply-filters-btn').addEventListener('click', handleApplyFilters);
            document.getElementById('generate-report-btn').addEventListener('click', handleGenerateReport);
            document.getElementById('download-report-btn').addEventListener('click', handleDownloadReport);
            document.getElementById('add-user-btn').addEventListener('click', handleAddUser);
            
            // Show user type specific fields
            document.getElementById('new-user-type').addEventListener('change', function() {
                const monitorFields = document.querySelectorAll('.monitor-fields');
                if (this.value === 'monitor') {
                    monitorFields.forEach(field => field.style.display = 'block');
                } else {
                    monitorFields.forEach(field => field.style.display = 'none');
                }
            });
            
            document.getElementById('edit-user-type').addEventListener('change', function() {
                const monitorFields = document.querySelectorAll('.edit-monitor-fields');
                if (this.value === 'monitor') {
                    monitorFields.forEach(field => field.style.display = 'block');
                } else {
                    monitorFields.forEach(field => field.style.display = 'none');
                }
            });
            
            // Update date and time
            updateDateTime();
            setInterval(updateDateTime, 1000);
        });
        
        // Cargar datos desde Firebase
        function loadDataFromFirebase() {
            // Cargar usuarios
            const usersRef = ref(database, 'users');
            onValue(usersRef, (snapshot) => {
                if (snapshot.exists()) {
                    users = Object.values(snapshot.val());
                } else {
                    // Si no hay datos, cargar usuarios de prueba
                    loadMockUsers();
                    // Y guardarlos en Firebase
                    saveUsersToFirebase();
                }
            });
            
            // Cargar estudiantes
            const studentsRef = ref(database, 'students');
            onValue(studentsRef, (snapshot) => {
                if (snapshot.exists()) {
                    students = Object.values(snapshot.val());
                } else {
                    // Si no hay datos, cargar estudiantes de prueba
                    loadMockStudents();
                    // Y guardarlos en Firebase
                    saveStudentsToFirebase();
                }
            });
            
            // Cargar asistencias
            const attendanceRef = ref(database, 'attendance');
            onValue(attendanceRef, (snapshot) => {
                if (snapshot.exists()) {
                    attendanceData = Object.values(snapshot.val());
                } else {
                    // Si no hay datos, cargar asistencias de prueba
                    loadMockAttendance();
                    // Y guardarlas en Firebase
                    saveAttendanceToFirebase();
                }
            });
        }
        
        // Guardar usuarios en Firebase
        function saveUsersToFirebase() {
            set(ref(database, 'users'), users);
        }
        
        // Guardar estudiantes en Firebase
        function saveStudentsToFirebase() {
            set(ref(database, 'students'), students);
        }
        
        // Guardar asistencias en Firebase
        function saveAttendanceToFirebase() {
            set(ref(database, 'attendance'), attendanceData);
        }
        
        // Load mock data for demo purposes
        function loadMockData() {
            loadMockUsers();
            loadMockStudents();
            loadMockAttendance();
        }
        
        // Load mock users
        function loadMockUsers() {
            users = [
                {
                    username: 'admin',
                    password: 'admin123',
                    fullName: 'Administrador Principal',
                    id: '1234567890',
                    phone: '3001234567',
                    type: 'admin'
                },
                {
                    username: 'monitor1',
                    password: 'monitor123',
                    fullName: 'Juan Pérez',
                    id: '1098765432',
                    phone: '3107654321',
                    type: 'monitor',
                    career: 'Ingeniería de Sistemas',
                    module: 'Programación Web',
                    horario: '10:00 - 12:00'
                }
            ];
        }
        
        // Load mock students
        function loadMockStudents() {
            students = [
                {
                    doc: '1001234567',
                    name: 'María González',
                    phone: '3112345678',
                    career: 'Ingeniería de Sistemas',
                    status: 'matriculado'
                },
                {
                    doc: '1002345678',
                    name: 'Carlos Rodríguez',
                    phone: '3223456789',
                    career: 'Ingeniería de Sistemas',
                    status: 'matriculado'
                },
                {
                    doc: '1003456789',
                    name: 'Ana Martínez',
                    phone: '3134567890',
                    career: 'Derecho',
                    status: 'no-matriculado'
                }
            ];
        }
        
        // Load mock attendance data
        function loadMockAttendance() {
            const today = new Date().toISOString().split('T')[0];
            attendanceData = [
                {
                    doc: '1001234567',
                    name: 'María González',
                    phone: '3112345678',
                    career: 'Ingeniería de Sistemas',
                    date: today,
                    time: '10:15',
                    status: 'matriculado',
                    attendance: 'presente',
                    monitorUsername: 'monitor1'
                },
                {
                    doc: '1002345678',
                    name: 'Carlos Rodríguez',
                    phone: '3223456789',
                    career: 'Ingeniería de Sistemas',
                    date: today,
                    time: '10:20',
                    status: 'matriculado',
                    attendance: 'presente',
                    monitorUsername: 'monitor1'
                }
            ];
        }
        
        // Update date and time display
        function updateDateTime() {
            const now = new Date();
            const dateOptions = { weekday: 'long', year: 'numeric', month: 'long', day: 'numeric' };
            const timeOptions = { hour: '2-digit', minute: '2-digit', second: '2-digit', hour12: true };
            
            document.getElementById('monitor-date').textContent = 'Fecha: ' + now.toLocaleDateString('es-CO', dateOptions);
            document.getElementById('monitor-time').textContent = 'Hora: ' + now.toLocaleTimeString('es-CO', timeOptions);
        }
        
        // Show notification
        function showNotification(containerId, message, type) {
            const notification = document.getElementById(containerId);
            notification.textContent = message;
            notification.className = 'notification ' + type;
            notification.style.display = 'block';
            
            // Hide after 3 seconds
            setTimeout(() => {
                notification.style.display = 'none';
            }, 3000);
        }
        
        // Handle login
        function handleLogin() {
            const username = document.getElementById('username').value;
            const password = document.getElementById('password').value;
            const userType = document.getElementById('user-type').value;
            
            // Find user
            const user = users.find(u => u.username === username && u.password === password && u.type === userType);
                
            if (user) {
                currentUser = user;
                
                // Hide auth container
                document.getElementById('auth-container').classList.add('hidden');
                
                // Show appropriate page based on user type
                if (user.type === 'monitor') {
                    document.getElementById('monitor-page').classList.remove('hidden');
                    
                    // Update monitor info
                    document.getElementById('monitor-name').textContent = user.fullName;
                    document.getElementById('monitor-career').textContent = 'Carrera: ' + user.career;
                    document.getElementById('monitor-module').textContent = 'Módulo: ' + user.module;
                    document.getElementById('monitor-horario').textContent = 'Horario: ' + user.horario;
                    document.getElementById('monitor-semester').textContent = 'Semestre: ' + user.semester;
                    
                    // Load students
                    loadStudentsTable();
                } else {
                    document.getElementById('admin-page').classList.remove('hidden');
                    
                    // Load admin data
                    loadAdminDashboard();
                    loadAttendanceTable();
                    loadAdminStudentsTable();
                    loadUsersTable();
                }
                
                showNotification(user.type + '-notification', '¡Bienvenido, ' + user.fullName + '!', 'success');
            } else {
                showNotification('auth-notification', 'Usuario o contraseña incorrectos', 'error');
            }
        }    
        // Handle register
        function handleRegister() {
            const fullname = document.getElementById('reg-fullname').value;
            const id = document.getElementById('reg-id').value;
            const phone = document.getElementById('reg-phone').value;
            const career = document.getElementById('reg-career').value;
            const module = document.getElementById('reg-module').value;
            const horario = document.getElementById('reg-horario').value;
            const username = document.getElementById('reg-username').value;
            const password = document.getElementById('reg-password').value;
            
            // Validate fields
          if (!fullname || !id || !phone || !career || !module || !horario || !username || !password) {
                showNotification('register-notification', 'Por favor complete todos los campos', 'error');
                return;
            }
            
            // Check if username already exists
            if (users.some(u => u.username === username)) {
                showNotification('register-notification', 'Este nombre de usuario ya está en uso', 'error');
                return; // ⛔ Salimos sin mostrar el mensaje de éxito
            }
            
            // Add new monitor
            const semester = document.getElementById('reg-semester').value;

            const newUser = {
                username: username,
                password: password,
                fullName: fullname,
                id: id,
                phone: phone,
                type: 'monitor',
                career: career,
                module: module,
                horario: horario,
                semester: semester
            };
            
            users.push(newUser);
            
            // Guardar en Firebase
            saveUsersToFirebase();

            showNotification('register-notification', 'Registro exitoso. Puede iniciar sesión ahora', 'success');

            // Espera 1 segundo y cambia a la pestaña de login
            setTimeout(() => {
                document.querySelector('.auth-option[data-form="login-form"]').click();
                document.getElementById('register-form').reset();

                // (Opcional) Autocompletar el nombre de usuario en el login
                document.getElementById('login-username').value = username;
                document.getElementById('login-password').focus();
            }, 1000);

            // Reset form
            document.getElementById('register-form').reset();
            
            // Mostrar el mensaje primero
            showNotification('register-notification', 'Registro exitoso. Puede iniciar sesión ahora', 'success');

            // Esperar 1 segundo y luego cambiar a la pestaña "Iniciar Sesión"
            setTimeout(() => {
                document.querySelector('.auth-option[data-form="login-form"]').click();

                // Opcional: limpiar campos del registro
                document.getElementById('register-form').reset();

                // Opcional: limpiar notificación después de cambiar
                setTimeout(() => {
                    document.getElementById('register-notification').style.display = 'none';
                }, 1000);
            }, 1000);
        }
        
        // Handle logout
        function handleLogout() {
            // Reset current user
            currentUser = null;
            
            // Hide all pages
            document.getElementById('monitor-page').classList.add('hidden');
            document.getElementById('admin-page').classList.add('hidden');
            
            // Show auth container
            document.getElementById('auth-container').classList.remove('hidden');
            
            // Reset forms
            document.getElementById('login-form').reset();
            document.getElementById('register-form').reset();
        }
        
        // Load students table for monitor
       function loadStudentsTable() {
            const tableBody = document.querySelector('#students-table tbody');
            tableBody.innerHTML = '';

            students
                .filter(student => student.monitorUsername === currentUser.username)
                .forEach(student => {
                    const today = new Date().toISOString().split('T')[0];
                    const studentAttendance = attendanceData.find(a => 
                        a.doc === student.doc && 
                        a.date === today && 
                        a.monitorUsername === currentUser.username
                    );

                    const row = document.createElement('tr');
                    row.innerHTML = `
                        <td>${student.doc}</td>
                        <td>${student.name}</td>
                        <td>${student.phone}</td>
                        <td>${student.status === 'matriculado' ? 'Matriculado' : 'No Matriculado'}</td>
                        <td>
                            <select class="attendance-select" data-doc="${student.doc}">
                                <option value="">Seleccione</option>
                                <option value="presente" ${studentAttendance && studentAttendance.attendance === 'presente' ? 'selected' : ''}>Presente</option>
                                <option value="ausente" ${studentAttendance && studentAttendance.attendance === 'ausente' ? 'selected' : ''}>Ausente</option>
                            </select>
                        </td>
                        <td>
                            <button class="btn-edit" onclick="openEditStudentModal('${student.doc}')">Editar</button>
                            <button class="btn-danger" onclick="deleteStudent('${student.doc}')">Eliminar</button>
                        </td>
                    `;
                    tableBody.appendChild(row);
                });
        }
        
        // Handle student search
        function handleStudentSearch() {
            const searchText = document.getElementById('student-search').value.toLowerCase();
            
            const tableBody = document.querySelector('#students-table tbody');
            const rows = tableBody.querySelectorAll('tr');
            
            rows.forEach(row => {
                const doc = row.cells[0].textContent.toLowerCase();
                const name = row.cells[1].textContent.toLowerCase();
                
                if (doc.includes(searchText) || name.includes(searchText)) {
                    row.style.display = '';
                } else {
                    row.style.display = 'none';
                }
            });
        }
        
        // Handle add student
        function handleAddStudent() {
            const doc = document.getElementById('new-student-doc').value;
            const name = document.getElementById('new-student-name').value;
            const phone = document.getElementById('new-student-phone').value;
            const career = document.getElementById('new-student-career').value;
            const status = document.getElementById('new-student-status').value;
            
            // Validate fields
            if (!doc || !name || !phone || !career) {
                showNotification('monitor-notification', 'Por favor complete todos los campos', 'error');
                return;
            }
            
            // Check if student already exists
            if (students.some(s => s.doc === doc)) {
                showNotification('monitor-notification', 'El estudiante ya existe', 'error');
                return;
            }
            
            // Add new student
            const newStudent = {
                doc: doc,
                name: name,
                phone: phone,
                career: career,
                status: status,
                monitorUsername: currentUser.username
            };
            
            students.push(newStudent);
            
            // Guardar en Firebase
            saveStudentsToFirebase();
            
            // Reset form
            document.getElementById('new-student-doc').value = '';
            document.getElementById('new-student-name').value = '';
            document.getElementById('new-student-phone').value = '';
            document.getElementById('new-student-career').value = '';
            
            // Reload table
            loadStudentsTable();
            
            showNotification('monitor-notification', 'Estudiante agregado correctamente', 'success');
        }
        
        // Handle save attendance
        function handleSaveAttendance() {
            const attendanceSelects = document.querySelectorAll('.attendance-select');
            const today = new Date().toISOString().split('T')[0];
            const now = new Date();
            const time = now.getHours().toString().padStart(2, '0') + ':' + now.getMinutes().toString().padStart(2, '0');
            
            let savedCount = 0;
            
            attendanceSelects.forEach(select => {
                const doc = select.getAttribute('data-doc');
                const attendance = select.value;
                
                if (attendance) {
                    // Find student
                    const student = students.find(s => s.doc === doc);
                    
                    // Check if record already exists
                    const existingIndex = attendanceData.findIndex(a => 
                        a.doc === doc && 
                        a.date === today && 
                        a.monitorUsername === currentUser.username
                    );
                    
                    const attendanceRecord = {
                        doc: doc,
                        name: student.name,
                        phone: student.phone,
                        career: student.career,
                        date: today,
                        time: time,
                        status: student.status,
                        attendance: attendance,
                        monitorUsername: currentUser.username
                    };
                    
                    if (existingIndex !== -1) {
                        // Update existing record
                        attendanceData[existingIndex] = attendanceRecord;
                    } else {
                        // Add new record
                        attendanceData.push(attendanceRecord);
                    }
                    
                    savedCount++;
                }
            });
            
            // Guardar en Firebase
            saveAttendanceToFirebase();
            
            if (savedCount > 0) {
                showNotification('monitor-notification', `Asistencia guardada para ${savedCount} estudiantes`, 'success');
            } else {
                showNotification('monitor-notification', 'No se guardó ninguna asistencia', 'error');
            }
        }
        
        // Open edit student modal
        function openEditStudentModal(docId) {
            const student = students.find(s => s.doc === docId);
            
            if (student) {
                document.getElementById('edit-student-doc').value = student.doc;
                document.getElementById('edit-student-name').value = student.name;
                document.getElementById('edit-student-phone').value = student.phone;
                document.getElementById('edit-student-career').value = student.career;
                document.getElementById('edit-student-status').value = student.status;
                
                const modal = document.getElementById('edit-student-modal');
                modal.style.display = 'flex';
            }
        }
        
        // Close edit student modal
        function closeEditStudentModal() {
            const modal = document.getElementById('edit-student-modal');
            modal.style.display = 'none';
        }
        
        // Handle save edit student
        function handleSaveEditStudent() {
            const doc = document.getElementById('edit-student-doc').value;
            const name = document.getElementById('edit-student-name').value;
            const phone = document.getElementById('edit-student-phone').value;
            const career = document.getElementById('edit-student-career').value;
            const status = document.getElementById('edit-student-status').value;
            
            // Find student index
            const studentIndex = students.findIndex(s => s.doc === doc);
            
            if (studentIndex !== -1) {
                // Update student
                students[studentIndex].name = name;
                students[studentIndex].phone = phone;
                students[studentIndex].career = career;
                students[studentIndex].status = status;
                
                // Update attendance records
                attendanceData.forEach((record, index) => {
                    if (record.doc === doc) {
                        attendanceData[index].name = name;
                        attendanceData[index].phone = phone;
                        attendanceData[index].career = career;
                        attendanceData[index].status = status;
                    }
                });
                
                // Guardar en Firebase
                saveStudentsToFirebase();
                saveAttendanceToFirebase();
                
                // Reload table
                if (currentUser.type === 'monitor') {
                    loadStudentsTable();
                } else {
                    loadAdminStudentsTable();
                }
                
                // Close modal
                closeEditStudentModal();
                
                showNotification(currentUser.type + '-notification', 'Estudiante actualizado correctamente', 'success');
            }
        }
        
        // Delete student
        function deleteStudent(docId) {
            if (confirm('¿Está seguro de eliminar este estudiante?')) {
                // Find student index
                const studentIndex = students.findIndex(s => s.doc === docId);
                
                if (studentIndex !== -1) {
                    // Remove student
                    students.splice(studentIndex, 1);
                    
                    // Remove attendance records
                    attendanceData = attendanceData.filter(record => record.doc !== docId);
                    
                    // Guardar en Firebase
                    saveStudentsToFirebase();
                    saveAttendanceToFirebase();
                    
                    // Reload table
                    if (currentUser.type === 'monitor') {
                        loadStudentsTable();
                    } else {
                        loadAdminStudentsTable();
                        loadAttendanceTable();
                    }
                    
                    showNotification(currentUser.type + '-notification', 'Estudiante eliminado correctamente', 'success');
                }
            }
        }
        
        // Load admin dashboard
        function loadAdminDashboard() {
            const today = new Date().toISOString().split('T')[0];
            
            // Total students
            document.getElementById('total-students').textContent = students.length;
            
            // Today's attendance
            const todayAttendance = attendanceData.filter(record => 
                record.date === today && record.attendance === 'presente'
            ).length;
            document.getElementById('today-attendance').textContent = todayAttendance;
            
            // Attendance percentage
            const percentage = students.length > 0 ? Math.round((todayAttendance / students.length) * 100) : 0;
            document.getElementById('attendance-percentage').textContent = percentage + '%';
            
            // Unregistered students
            const unregistered = students.filter(s => s.status === 'no-matriculado').length;
            document.getElementById('unregistered-students').textContent = unregistered;
        }
        
        // Load attendance table for admin
        function loadAttendanceTable() {
            const tableBody = document.querySelector('#admin-attendance-table tbody');
            tableBody.innerHTML = '';
            
            attendanceData.forEach(record => {
                const row = document.createElement('tr');
                row.innerHTML = `
                    <td>${record.doc}</td>
                    <td>${record.name}</td>
                    <td>${record.phone}</td>
                    <td>${record.career}</td>
                    <td>${record.date}</td>
                    <td>${record.time}</td>
                    <td>${record.status === 'matriculado' ? 'Matriculado' : 'No Matriculado'}</td>
                    <td>${record.attendance === 'presente' ? 'Presente' : 'Ausente'}</td>
                    <td>
                        <button class="btn-danger" onclick="deleteAttendanceRecord('${record.doc}', '${record.date}')">Eliminar</button>
                    </td>
                `;
                tableBody.appendChild(row);
            });
        }
        
        // Delete attendance record
        function deleteAttendanceRecord(docId, date) {
            if (confirm('¿Está seguro de eliminar este registro de asistencia?')) {
                // Find record index
                const recordIndex = attendanceData.findIndex(record => 
                    record.doc === docId && record.date === date
                );
                
                if (recordIndex !== -1) {
                    // Remove record
                    attendanceData.splice(recordIndex, 1);
                    
                    // Guardar en Firebase
                    saveAttendanceToFirebase();
                    
                    // Reload table
                    loadAttendanceTable();
                    
                    // Update dashboard
                    loadAdminDashboard();
                    
                    showNotification('admin-notification', 'Registro de asistencia eliminado correctamente', 'success');
                }
            }
        }
        
        // Apply filters to attendance table
        function handleApplyFilters() {
            const date = document.getElementById('filter-date').value;
            const career = document.getElementById('filter-career').value;
            const status = document.getElementById('filter-status').value;
            const attendance = document.getElementById('filter-attendance').value;

            const tableBody = document.querySelector('#admin-attendance-table tbody');
            const rows = tableBody.querySelectorAll('tr');

            rows.forEach(row => {
                const rowDate = row.cells[4].textContent;
                const rowCareer = row.cells[3].textContent;
                const rowStatus = row.cells[6].textContent;
                const rowAttendance = row.cells[7].textContent;

                let show = true;

                if (date && rowDate !== date) show = false;
                if (career && rowCareer !== career) show = false;
                if (status && ((status === 'matriculado' && rowStatus !== 'Matriculado') || 
                    (status === 'no-matriculado' && rowStatus !== 'No Matriculado'))) show = false;
                if (attendance && ((attendance === 'presente' && rowAttendance !== 'Presente') || 
                    (attendance === 'ausente' && rowAttendance !== 'Ausente'))) show = false;

                row.style.display = show ? '' : 'none';
            });
        }
        
        // Generate report based on filters
        function handleGenerateReport() {
            const date = document.getElementById('filter-date').value;
            const career = document.getElementById('filter-career').value;
            const status = document.getElementById('filter-status').value;
            const attendance = document.getElementById('filter-attendance').value;
            
            // Filter data
            let filteredData = [...attendanceData];
            
            if (date) {
                filteredData = filteredData.filter(record => record.date === date);
            }
            
            if (career) {
                filteredData = filteredData.filter(record => record.career === career);
            }
            
            if (status) {
                filteredData = filteredData.filter(record => record.status === status);
            }
            
            if (attendance) {
                filteredData = filteredData.filter(record => record.attendance === attendance);
            }
            
            // Generate report
            const reportContainer = document.getElementById('report-container');
            const reportData = document.getElementById('report-data');
            
            // Clear previous report
            reportData.innerHTML = '';
            
            // Generate report header
            const header = document.createElement('div');
            header.className = 'report-header';
            header.innerHTML = `
                <h2>Reporte de Asistencia</h2>
                <p>Fecha: ${date || 'Todas'}</p>
                <p>Carrera: ${career || 'Todas'}</p>
                <p>Estado: ${status === 'matriculado' ? 'Matriculado' : status === 'no-matriculado' ? 'No Matriculado' : 'Todos'}</p>
                <p>Asistencia: ${attendance === 'presente' ? 'Presente' : attendance === 'ausente' ? 'Ausente' : 'Todos'}</p>
                <p>Total registros: ${filteredData.length}</p>
            `;
            reportData.appendChild(header);
            
            // Generate table
            const table = document.createElement('table');
            table.className = 'report-table';
            
            // Table header
            const tableHeader = document.createElement('thead');
            tableHeader.innerHTML = `
                <tr>
                    <th>Documento</th>
                    <th>Nombre</th>
                    <th>Teléfono</th>
                    <th>Carrera</th>
                    <th>Fecha</th>
                    <th>Hora</th>
                    <th>Estado</th>
                    <th>Asistencia</th>
                </tr>
            `;
            table.appendChild(tableHeader);
            
            // Table body
            const tableBody = document.createElement('tbody');
            
            filteredData.forEach(record => {
                const row = document.createElement('tr');
                row.innerHTML = `
                    <td>${record.doc}</td>
                    <td>${record.name}</td>
                    <td>${record.phone}</td>
                    <td>${record.career}</td>
                    <td>${record.date}</td>
                    <td>${record.time}</td>
                    <td>${record.status === 'matriculado' ? 'Matriculado' : 'No Matriculado'}</td>
                    <td>${record.attendance === 'presente' ? 'Presente' : 'Ausente'}</td>
                `;
                tableBody.appendChild(row);
            });
            
            table.appendChild(tableBody);
            reportData.appendChild(table);
            
            // Show report container
            reportContainer.style.display = 'block';
        }
        
        // Download report as PDF
        function handleDownloadReport() {
            // This is just a placeholder. In a real app, you would use a library like jsPDF or html2pdf
            alert('Función de descarga de PDF no implementada en esta versión de demostración');
        }
        
        // Load students table for admin
        function loadAdminStudentsTable() {
            const tableBody = document.querySelector('#admin-students-table tbody');
            tableBody.innerHTML = '';
            
            students.forEach(student => {
                const row = document.createElement('tr');
                row.innerHTML = `
                    <td>${student.doc}</td>
                    <td>${student.name}</td>
                    <td>${student.phone}</td>
                    <td>${student.career}</td>
                    <td>${student.status === 'matriculado' ? 'Matriculado' : 'No Matriculado'}</td>
                    <td>${student.monitorUsername || 'N/A'}</td>
                    <td>
                        <button class="btn-edit" onclick="openEditStudentModal('${student.doc}')">Editar</button>
                        <button class="btn-danger" onclick="deleteStudent('${student.doc}')">Eliminar</button>
                    </td>
                `;
                tableBody.appendChild(row);
            });
        }
        
        // Handle admin student search
        function handleAdminStudentSearch() {
            const searchText = document.getElementById('admin-student-search').value.toLowerCase();
            
            const tableBody = document.querySelector('#admin-students-table tbody');
            const rows = tableBody.querySelectorAll('tr');
            
            rows.forEach(row => {
                const doc = row.cells[0].textContent.toLowerCase();
                const name = row.cells[1].textContent.toLowerCase();
                
                if (doc.includes(searchText) || name.includes(searchText)) {
                    row.style.display = '';
                } else {
                    row.style.display = 'none';
                }
            });
        }
        
        // Handle admin add student
        function handleAdminAddStudent() {
            const doc = document.getElementById('admin-new-student-doc').value;
            const name = document.getElementById('admin-new-student-name').value;
            const phone = document.getElementById('admin-new-student-phone').value;
            const career = document.getElementById('admin-new-student-career').value;
            const status = document.getElementById('admin-new-student-status').value;
            const monitor = document.getElementById('admin-new-student-monitor').value;
            
            // Validate fields
            if (!doc || !name || !phone || !career) {
                showNotification('admin-notification', 'Por favor complete todos los campos', 'error');
                return;
            }
            
            // Check if student already exists
            if (students.some(s => s.doc === doc)) {
                showNotification('admin-notification', 'El estudiante ya existe', 'error');
                return;
            }
            
            // Add new student
            const newStudent = {
                doc: doc,
                name: name,
                phone: phone,
                career: career,
                status: status,
                monitorUsername: monitor
            };
            
            students.push(newStudent);
            
            // Guardar en Firebase
            saveStudentsToFirebase();
            
            // Reset form
            document.getElementById('admin-new-student-doc').value = '';
            document.getElementById('admin-new-student-name').value = '';
            document.getElementById('admin-new-student-phone').value = '';
            document.getElementById('admin-new-student-career').value = '';
            
            // Reload table
            loadAdminStudentsTable();
            
            showNotification('admin-notification', 'Estudiante agregado correctamente', 'success');
        }
        
        // Load users table
        function loadUsersTable() {
            const tableBody = document.querySelector('#users-table tbody');
            tableBody.innerHTML = '';
            
            users.forEach(user => {
                const row = document.createElement('tr');
                row.innerHTML = `
                    <td>${user.username}</td>
                    <td>${user.fullName}</td>
                    <td>${user.id}</td>
                    <td>${user.phone}</td>
                    <td>${user.type === 'admin' ? 'Administrador' : 'Monitor'}</td>
                    <td>${user.type === 'monitor' ? user.career : 'N/A'}</td>
                    <td>${user.type === 'monitor' ? user.module : 'N/A'}</td>
                    <td>
                        <button class="btn-edit" onclick="openEditUserModal('${user.username}')">Editar</button>
                        <button class="btn-danger" onclick="deleteUser('${user.username}')">Eliminar</button>
                    </td>
                `;
                tableBody.appendChild(row);
            });
        }
        
        // Open edit user modal
        function openEditUserModal(username) {
            const user = users.find(u => u.username === username);
            
            if (user) {
                document.getElementById('edit-user-username').value = user.username;
                document.getElementById('edit-user-fullname').value = user.fullName;
                document.getElementById('edit-user-id').value = user.id;
                document.getElementById('edit-user-phone').value = user.phone;
                document.getElementById('edit-user-type').value = user.type;
                
                // Show/hide monitor fields
                const monitorFields = document.querySelectorAll('.edit-monitor-fields');
                if (user.type === 'monitor') {
                    monitorFields.forEach(field => field.style.display = 'block');
                    document.getElementById('edit-user-career').value = user.career || '';
                    document.getElementById('edit-user-module').value = user.module || '';
                    document.getElementById('edit-user-horario').value = user.horario || '';
                    document.getElementById('edit-user-semester').value = user.semester || '';
                } else {
                    monitorFields.forEach(field => field.style.display = 'none');
                }
                
                const modal = document.getElementById('edit-user-modal');
                modal.style.display = 'flex';
            }
        }
        
        // Close edit user modal
        function closeEditUserModal() {
            const modal = document.getElementById('edit-user-modal');
            modal.style.display = 'none';
        }
        
        // Handle save edit user
        function handleSaveEditUser() {
            const username = document.getElementById('edit-user-username').value;
            const fullName = document.getElementById('edit-user-fullname').value;
            const id = document.getElementById('edit-user-id').value;
            const phone = document.getElementById('edit-user-phone').value;
            const type = document.getElementById('edit-user-type').value;
            
            // Find user index
            const userIndex = users.findIndex(u => u.username === username);
            
            if (userIndex !== -1) {
                // Update user
                users[userIndex].fullName = fullName;
                users[userIndex].id = id;
                users[userIndex].phone = phone;
                users[userIndex].type = type;
                
                // Update monitor specific fields
                if (type === 'monitor') {
                    users[userIndex].career = document.getElementById('edit-user-career').value;
                    users[userIndex].module = document.getElementById('edit-user-module').value;
                    users[userIndex].horario = document.getElementById('edit-user-horario').value;
                    users[userIndex].semester = document.getElementById('edit-user-semester').value;
                }
                
                // Guardar en Firebase
                saveUsersToFirebase();
                
                // Reload table
                loadUsersTable();
                
                // Close modal
                closeEditUserModal();
                
                showNotification('admin-notification', 'Usuario actualizado correctamente', 'success');
            }
        }
        
        // Delete user
        function deleteUser(username) {
            // Check if it's the last admin
            if (users.find(u => u.username === username).type === 'admin' && 
                users.filter(u => u.type === 'admin').length === 1) {
                showNotification('admin-notification', 'No se puede eliminar el último administrador', 'error');
                return;
            }
            
            if (confirm('¿Está seguro de eliminar este usuario?')) {
                // Find user index
                const userIndex = users.findIndex(u => u.username === username);
                
                if (userIndex !== -1) {
                    // Remove user
                    users.splice(userIndex, 1);
                    
                    // Guardar en Firebase
                    saveUsersToFirebase();
                    
                    // Reload table
                    loadUsersTable();
                    
                    showNotification('admin-notification', 'Usuario eliminado correctamente', 'success');
                }
            }
        }
        
        // Handle add user
        function handleAddUser() {
            const username = document.getElementById('new-user-username').value;
            const password = document.getElementById('new-user-password').value;
            const fullName = document.getElementById('new-user-fullname').value;
            const id = document.getElementById('new-user-id').value;
            const phone = document.getElementById('new-user-phone').value;
            const type = document.getElementById('new-user-type').value;
            
            // Validate fields
            if (!username || !password || !fullName || !id || !phone) {
                showNotification('admin-notification', 'Por favor complete todos los campos', 'error');
                return;
            }
            
            // Check if username already exists
            if (users.some(u => u.username === username)) {
                showNotification('admin-notification', 'Este nombre de usuario ya está en uso', 'error');
                return;
            }
            
            // Add new user
            const newUser = {
                username: username,
                password: password,
                fullName: fullName,
                id: id,
                phone: phone,
                type: type
            };
            
            // Add monitor specific fields
            if (type === 'monitor') {
                newUser.career = document.getElementById('new-user-career').value;
                newUser.module = document.getElementById('new-user-module').value;
                newUser.horario = document.getElementById('new-user-horario').value;
                newUser.semester = document.getElementById('new-user-semester').value;
            }
            
            users.push(newUser);
            
            // Guardar en Firebase
            saveUsersToFirebase();
            
            // Reset form
            document.getElementById('new-user-username').value = '';
            document.getElementById('new-user-password').value = '';
            document.getElementById('new-user-fullname').value = '';
            document.getElementById('new-user-id').value = '';
            document.getElementById('new-user-phone').value = '';
            
            // Reload table
            loadUsersTable();
            
            showNotification('admin-notification', 'Usuario agregado correctamente', 'success');
        }

        // Hacer funciones accesibles globalmente
        window.openEditStudentModal = openEditStudentModal;
        window.deleteStudent = deleteStudent;
        window.deleteAttendanceRecord = deleteAttendanceRecord;
        window.openEditUserModal = openEditUserModal;
        window.deleteUser = deleteUser;
    </script>
</body>
</html>
