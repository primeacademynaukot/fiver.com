<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Qaimkhani Stationery & Hardware Store Management System</title>
    <link href="https://fonts.googleapis.com/css2?family=Poppins:wght@300;400;500;600;700&display=swap" rel="stylesheet">
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/xlsx/0.18.5/xlsx.full.min.js"></script>
    <!-- Firebase SDK -->
    <script src="https://www.gstatic.com/firebasejs/9.22.1/firebase-app-compat.js"></script>
    <script src="https://www.gstatic.com/firebasejs/9.22.1/firebase-auth-compat.js"></script>
    <script src="https://www.gstatic.com/firebasejs/9.22.1/firebase-database-compat.js"></script>
    <style>
        /* CSS styles from the original file - optimized and cleaned */
        :root {
            --primary: #1E88E5;
            --accent: #42A5F5;
            --background: #F5F7FA;
            --sidebar-bg: #0D47A1;
            --card-bg: #FFFFFF;
            --text-primary: #212121;
            --text-secondary: #616161;
            --success: #43A047;
            --error: #E53935;
            --warning: #FF9800;
            --radius: 10px;
            --shadow: 0px 4px 12px rgba(0,0,0,0.08);
            --transition: all 0.3s ease;
        }

        .dark-mode {
            --primary: #42A5F5;
            --accent: #64B5F6;
            --background: #121212;
            --sidebar-bg: #0A2540;
            --card-bg: #1F1F1F;
            --text-primary: #E0E0E0;
            --text-secondary: #B0B0B0;
            --success: #4CAF50;
            --error: #F44336;
            --warning: #FFB74D;
        }

        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: 'Poppins', sans-serif;
        }

        body {
            background-color: var(--background);
            color: var(--text-primary);
            transition: var(--transition);
        }

        /* Login Styles */
        .login-container {
            display: flex;
            align-items: center;
            justify-content: center;
            height: 100vh;
            background-color: var(--background);
        }

        .login-wrapper {
            display: flex;
            width: 100%;
            max-width: 900px;
            box-shadow: var(--shadow);
            border-radius: var(--radius);
            overflow: hidden;
        }

        .login-image {
            flex: 1;
            background: linear-gradient(135deg, var(--primary), var(--accent));
            color: white;
            display: flex;
            align-items: center;
            justify-content: center;
            padding: 40px;
        }

        .login-image-content {
            text-align: center;
        }

        .login-image-content h2 {
            font-size: 2rem;
            margin-bottom: 10px;
        }

        .login-image-content p {
            opacity: 0.9;
        }

        .login-card {
            background-color: var(--card-bg);
            border-radius: var(--radius);
            width: 100%;
            max-width: 400px;
            padding: 30px;
        }

        .login-header {
            text-align: center;
            margin-bottom: 30px;
        }

        .login-header h2 {
            color: var(--primary);
            margin-bottom: 10px;
        }

        .form-group {
            margin-bottom: 15px;
        }

        .form-group label {
            display: block;
            margin-bottom: 5px;
            font-weight: 500;
        }

        .form-control {
            width: 100%;
            padding: 10px 15px;
            border-radius: var(--radius);
            border: 1px solid #ddd;
            outline: none;
            transition: var(--transition);
            background-color: var(--card-bg);
            color: var(--text-primary);
        }

        .form-control:focus {
            border-color: var(--accent);
            box-shadow: 0 0 0 2px rgba(66, 165, 245, 0.2);
        }

        .btn {
            padding: 10px 15px;
            border-radius: var(--radius);
            border: none;
            cursor: pointer;
            font-weight: 500;
            transition: var(--transition);
            display: inline-flex;
            align-items: center;
            justify-content: center;
            gap: 5px;
        }

        .btn-primary {
            background-color: var(--primary);
            color: white;
        }

        .btn-primary:hover {
            background-color: var(--accent);
        }

        .btn-success {
            background-color: var(--success);
            color: white;
        }

        .btn-danger {
            background-color: var(--error);
            color: white;
        }

        .btn-warning {
            background-color: var(--warning);
            color: white;
        }

        .btn-outline {
            background-color: transparent;
            border: 1px solid var(--primary);
            color: var(--primary);
        }

        .btn-sm {
            padding: 5px 10px;
            font-size: 0.85rem;
        }

        .alert {
            padding: 10px 15px;
            border-radius: var(--radius);
            margin-bottom: 15px;
            display: none;
        }

        .alert-error {
            background-color: rgba(229, 57, 53, 0.1);
            color: var(--error);
            border-left: 4px solid var(--error);
        }

        .loading {
            display: inline-block;
            width: 20px;
            height: 20px;
            border: 3px solid rgba(255,255,255,.3);
            border-radius: 50%;
            border-top-color: #fff;
            animation: spin 1s ease-in-out infinite;
            display: none;
        }

        @keyframes spin {
            to { transform: rotate(360deg); }
        }

        /* Main App Styles */
        #app {
            display: none;
        }

        .sidebar {
            width: 250px;
            background-color: var(--sidebar-bg);
            color: white;
            height: 100vh;
            position: fixed;
            overflow-y: auto;
            transition: var(--transition);
            z-index: 100;
        }

        .sidebar-header {
            padding: 20px;
            text-align: center;
            border-bottom: 1px solid rgba(255,255,255,0.1);
        }

        .sidebar-menu {
            list-style: none;
            padding: 10px 0;
        }

        .sidebar-menu li {
            margin-bottom: 5px;
        }

        .sidebar-menu a {
            display: flex;
            align-items: center;
            padding: 12px 20px;
            color: white;
            text-decoration: none;
            transition: var(--transition);
        }

        .sidebar-menu a:hover, .sidebar-menu a.active {
            background-color: rgba(255,255,255,0.1);
        }

        .sidebar-menu i {
            margin-right: 10px;
            width: 20px;
            text-align: center;
        }

        .main-content {
            margin-left: 250px;
            transition: var(--transition);
        }

        .topbar {
            background-color: var(--card-bg);
            padding: 15px 30px;
            display: flex;
            justify-content: space-between;
            align-items: center;
            box-shadow: var(--shadow);
            position: sticky;
            top: 0;
            z-index: 99;
        }

        .topbar-left {
            display: flex;
            align-items: center;
            gap: 15px;
        }

        .topbar-left h1 {
            font-size: 1.5rem;
            color: var(--primary);
        }

        .mobile-menu-btn {
            display: none;
            background: none;
            border: none;
            font-size: 1.5rem;
            color: var(--primary);
            cursor: pointer;
        }

        .content-area {
            padding: 30px;
        }

        .page-title {
            margin-bottom: 20px;
            display: flex;
            justify-content: space-between;
            align-items: center;
        }

        .page-title h2 {
            font-size: 1.8rem;
            color: var(--primary);
        }

        .card-container {
            display: grid;
            grid-template-columns: repeat(auto-fill, minmax(280px, 1fr));
            gap: 20px;
            margin-bottom: 30px;
        }

        .card {
            background-color: var(--card-bg);
            border-radius: var(--radius);
            box-shadow: var(--shadow);
            padding: 20px;
            transition: var(--transition);
        }

        .card:hover {
            transform: translateY(-5px);
            box-shadow: 0 10px 20px rgba(0,0,0,0.1);
        }

        .card-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 15px;
        }

        .card-title {
            font-size: 1rem;
            color: var(--text-secondary);
        }

        .card-icon {
            width: 40px;
            height: 40px;
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            color: white;
        }

        .card-value {
            font-size: 1.8rem;
            font-weight: 600;
            color: var(--primary);
            margin-bottom: 5px;
        }

        .card-footer {
            font-size: 0.85rem;
            color: var(--text-secondary);
        }

        .table-container {
            background-color: var(--card-bg);
            border-radius: var(--radius);
            box-shadow: var(--shadow);
            overflow: hidden;
            margin-bottom: 30px;
        }

        .table-header {
            padding: 15px 20px;
            background-color: var(--primary);
            color: white;
            display: flex;
            justify-content: space-between;
            align-items: center;
        }

        table {
            width: 100%;
            border-collapse: collapse;
        }

        th, td {
            padding: 12px 15px;
            text-align: left;
            border-bottom: 1px solid #eee;
        }

        th {
            background-color: rgba(0,0,0,0.05);
            font-weight: 600;
            color: var(--text-primary);
        }

        .page-content {
            display: none;
        }

        .page-content.active {
            display: block;
        }

        .toast {
            position: fixed;
            top: 20px;
            right: 20px;
            padding: 15px 20px;
            border-radius: var(--radius);
            color: white;
            z-index: 10000;
            display: none;
            box-shadow: var(--shadow);
            background-color: var(--success);
        }

        @media (max-width: 768px) {
            .sidebar {
                width: 0;
            }
            .main-content {
                margin-left: 0;
            }
            .sidebar.active {
                width: 250px;
            }
            .mobile-menu-btn {
                display: block;
            }
        }

        /* Additional styles for all features */
        .status-badge {
            padding: 4px 10px;
            border-radius: 20px;
            font-size: 0.8rem;
            font-weight: 500;
        }

        .status-pending {
            background-color: rgba(255, 152, 0, 0.1);
            color: var(--warning);
        }

        .status-completed {
            background-color: rgba(67, 160, 71, 0.1);
            color: var(--success);
        }

        .status-active {
            background-color: rgba(67, 160, 71, 0.1);
            color: var(--success);
        }

        .status-inactive {
            background-color: rgba(229, 57, 53, 0.1);
            color: var(--error);
        }

        .modal {
            display: none;
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background-color: rgba(0,0,0,0.5);
            z-index: 1000;
            align-items: center;
            justify-content: center;
        }

        .modal-content {
            background-color: var(--card-bg);
            border-radius: var(--radius);
            width: 90%;
            max-width: 800px;
            max-height: 90vh;
            box-shadow: 0 10px 30px rgba(0,0,0,0.2);
            display: flex;
            flex-direction: column;
        }

        .modal-header {
            padding: 15px 20px;
            background-color: var(--primary);
            color: white;
            border-radius: var(--radius) var(--radius) 0 0;
            display: flex;
            justify-content: space-between;
            align-items: center;
        }

        .modal-body {
            padding: 20px;
            overflow-y: auto;
            flex: 1;
        }

        .modal-footer {
            padding: 15px 20px;
            border-top: 1px solid #eee;
            display: flex;
            justify-content: flex-end;
            gap: 10px;
        }

        .close {
            font-size: 1.5rem;
            cursor: pointer;
        }

        .form-row {
            display: flex;
            gap: 15px;
        }

        .form-row .form-group {
            flex: 1;
        }

        .search-bar {
            position: relative;
        }

        .search-bar input {
            padding: 8px 15px 8px 35px;
            border-radius: 20px;
            border: 1px solid #ddd;
            width: 250px;
            outline: none;
            transition: var(--transition);
            background-color: var(--card-bg);
            color: var(--text-primary);
        }

        .search-bar input:focus {
            border-color: var(--accent);
            box-shadow: 0 0 0 2px rgba(66, 165, 245, 0.2);
        }

        .search-bar i {
            position: absolute;
            left: 12px;
            top: 50%;
            transform: translateY(-50%);
            color: var(--text-secondary);
        }

        .topbar-right {
            display: flex;
            align-items: center;
            gap: 20px;
        }

        .user-profile {
            display: flex;
            align-items: center;
            gap: 10px;
            cursor: pointer;
        }

        .user-avatar {
            width: 40px;
            height: 40px;
            border-radius: 50%;
            background-color: var(--accent);
            display: flex;
            align-items: center;
            justify-content: center;
            color: white;
            font-weight: bold;
        }

        .theme-toggle {
            background: none;
            border: none;
            color: var(--text-primary);
            cursor: pointer;
            font-size: 1.2rem;
        }

        .chart-container {
            background-color: var(--card-bg);
            border-radius: var(--radius);
            box-shadow: var(--shadow);
            padding: 20px;
            margin-bottom: 30px;
        }

        .tabs {
            display: flex;
            border-bottom: 1px solid #ddd;
            margin-bottom: 20px;
        }

        .tab {
            padding: 10px 20px;
            cursor: pointer;
            border-bottom: 3px solid transparent;
        }

        .tab.active {
            border-bottom: 3px solid var(--primary);
            color: var(--primary);
            font-weight: 500;
        }

        .tab-content {
            display: none;
        }

        .tab-content.active {
            display: block;
        }

        .export-btn {
            background-color: var(--success);
            color: white;
        }

        .table-actions {
            display: flex;
            gap: 10px;
        }

        .empty-state {
            text-align: center;
            padding: 40px 20px;
            color: var(--text-secondary);
        }

        .empty-state i {
            font-size: 3rem;
            margin-bottom: 15px;
            opacity: 0.5;
        }

        .low-stock {
            color: var(--error);
            font-weight: 500;
        }

        .credit {
            color: var(--success);
        }

        .debit {
            color: var(--error);
        }

        .cash {
            color: var(--primary);
        }

        .bill-item {
            display: flex;
            gap: 10px;
            margin-bottom: 15px;
            padding: 10px;
            border: 1px solid #eee;
            border-radius: var(--radius);
        }

        .bill-item select, .bill-item input {
            flex: 1;
        }

        .bill-item-actions {
            display: flex;
            align-items: center;
        }

        .payment-options {
            display: flex;
            gap: 10px;
            margin-bottom: 15px;
        }

        .payment-option {
            flex: 1;
            padding: 10px;
            text-align: center;
            border: 2px solid #ddd;
            border-radius: var(--radius);
            cursor: pointer;
            transition: var(--transition);
        }

        .payment-option.active {
            border-color: var(--primary);
            background-color: rgba(30, 136, 229, 0.1);
        }

        .payment-option i {
            font-size: 1.5rem;
            margin-bottom: 5px;
            display: block;
        }

        .khatta-row {
            display: flex;
            gap: 10px;
            margin-bottom: 10px;
            padding: 10px;
            border: 1px solid #eee;
            border-radius: var(--radius);
        }

        .khatta-row select, .khatta-row input {
            flex: 1;
        }

        .khatta-actions {
            display: flex;
            align-items: center;
        }

        .khatta-table {
            width: 100%;
            border-collapse: collapse;
        }

        .khatta-table th, .khatta-table td {
            padding: 12px 15px;
            text-align: left;
            border-bottom: 1px solid #eee;
        }

        .khatta-table th {
            background-color: rgba(0,0,0,0.05);
            font-weight: 600;
        }

        .action-buttons {
            display: flex;
            gap: 5px;
        }
        
        .notification-badge {
            background-color: var(--error);
            color: white;
            border-radius: 50%;
            width: 18px;
            height: 18px;
            font-size: 0.7rem;
            display: flex;
            align-items: center;
            justify-content: center;
            position: absolute;
            top: -5px;
            right: -5px;
        }
        
        .notification-container {
            position: relative;
        }
        
        .print-btn {
            background-color: var(--warning);
            color: white;
        }
        
        .badge {
            padding: 3px 8px;
            border-radius: 12px;
            font-size: 0.7rem;
            font-weight: 500;
        }
        
        .badge-success {
            background-color: var(--success);
            color: white;
        }
        
        .badge-warning {
            background-color: var(--warning);
            color: white;
        }
        
        .badge-danger {
            background-color: var(--error);
            color: white;
        }
        
        .badge-info {
            background-color: var(--primary);
            color: white;
        }
        
        .quick-actions {
            display: flex;
            gap: 10px;
            margin-bottom: 20px;
            flex-wrap: wrap;
        }
        
        .quick-action-btn {
            flex: 1;
            min-width: 150px;
            padding: 15px;
            text-align: center;
            border-radius: var(--radius);
            background-color: var(--card-bg);
            box-shadow: var(--shadow);
            cursor: pointer;
            transition: var(--transition);
        }
        
        .quick-action-btn:hover {
            transform: translateY(-3px);
            box-shadow: 0 5px 15px rgba(0,0,0,0.1);
        }
        
        .quick-action-btn i {
            font-size: 2rem;
            margin-bottom: 10px;
            color: var(--primary);
        }
        
        .stats-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
            gap: 15px;
            margin-bottom: 20px;
        }
        
        .stat-card {
            background-color: var(--card-bg);
            border-radius: var(--radius);
            padding: 15px;
            box-shadow: var(--shadow);
            text-align: center;
        }
        
        .stat-value {
            font-size: 1.5rem;
            font-weight: 600;
            color: var(--primary);
            margin-bottom: 5px;
        }
        
        .stat-label {
            font-size: 0.85rem;
            color: var(--text-secondary);
        }
        
        .dashboard-widget {
            background-color: var(--card-bg);
            border-radius: var(--radius);
            padding: 20px;
            box-shadow: var(--shadow);
            margin-bottom: 20px;
        }
        
        .dashboard-widget-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 15px;
        }
        
        .dashboard-widget-title {
            font-size: 1.2rem;
            font-weight: 600;
            color: var(--primary);
        }
        
        .dashboard-widget-actions {
            display: flex;
            gap: 10px;
        }
        
        .dashboard-widget-content {
            min-height: 200px;
        }
        
        .notification-item {
            padding: 10px 15px;
            border-bottom: 1px solid #eee;
            display: flex;
            align-items: center;
            gap: 10px;
        }
        
        .notification-item:last-child {
            border-bottom: none;
        }
        
        .notification-icon {
            width: 40px;
            height: 40px;
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            color: white;
        }
        
        .notification-content {
            flex: 1;
        }
        
        .notification-title {
            font-weight: 500;
            margin-bottom: 3px;
        }
        
        .notification-time {
            font-size: 0.8rem;
            color: var(--text-secondary);
        }
        
        .inventory-alert {
            background-color: rgba(229, 57, 53, 0.1);
            border-left: 4px solid var(--error);
            padding: 10px 15px;
            margin-bottom: 10px;
            border-radius: 0 var(--radius) var(--radius) 0;
        }
        
        .inventory-alert.warning {
            background-color: rgba(255, 152, 0, 0.1);
            border-left-color: var(--warning);
        }
        
        .inventory-alert.info {
            background-color: rgba(30, 136, 229, 0.1);
            border-left-color: var(--primary);
        }
        
        .filter-bar {
            display: flex;
            gap: 15px;
            margin-bottom: 20px;
            flex-wrap: wrap;
        }
        
        .filter-group {
            display: flex;
            gap: 10px;
            align-items: center;
        }
        
        .filter-label {
            font-weight: 500;
            color: var(--text-secondary);
        }
        
        .pagination {
            display: flex;
            justify-content: center;
            gap: 5px;
            margin-top: 20px;
        }
        
        .pagination-btn {
            width: 35px;
            height: 35px;
            border-radius: var(--radius);
            display: flex;
            align-items: center;
            justify-content: center;
            background-color: var(--card-bg);
            border: 1px solid #ddd;
            cursor: pointer;
            transition: var(--transition);
        }
        
        .pagination-btn.active {
            background-color: var(--primary);
            color: white;
            border-color: var(--primary);
        }
        
        .pagination-btn:hover:not(.active) {
            background-color: #f5f5f5;
        }
        
        .dashboard-overview {
            display: grid;
            grid-template-columns: 2fr 1fr;
            gap: 20px;
            margin-bottom: 20px;
        }
        
        @media (max-width: 992px) {
            .dashboard-overview {
                grid-template-columns: 1fr;
            }
        }
        
        .recent-activity {
            background-color: var(--card-bg);
            border-radius: var(--radius);
            padding: 20px;
            box-shadow: var(--shadow);
        }
        
        .activity-item {
            display: flex;
            gap: 15px;
            padding: 10px 0;
            border-bottom: 1px solid #eee;
        }
        
        .activity-item:last-child {
            border-bottom: none;
        }
        
        .activity-icon {
            width: 40px;
            height: 40px;
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            color: white;
            flex-shrink: 0;
        }
        
        .activity-content {
            flex: 1;
        }
        
        .activity-title {
            font-weight: 500;
            margin-bottom: 5px;
        }
        
        .activity-time {
            font-size: 0.8rem;
            color: var(--text-secondary);
        }
        
        .activity-amount {
            font-weight: 600;
            color: var(--primary);
        }
        
        .dashboard-notifications {
            background-color: var(--card-bg);
            border-radius: var(--radius);
            padding: 20px;
            box-shadow: var(--shadow);
        }
        
        .notification-list {
            max-height: 300px;
            overflow-y: auto;
        }
        
        .supplier-products {
            margin-top: 20px;
        }
        
        .credit-option {
            margin: 15px 0;
            padding: 10px;
            border: 1px solid #ddd;
            border-radius: var(--radius);
            background-color: rgba(67, 160, 71, 0.05);
        }
        
        .returned-bill {
            background-color: rgba(229, 57, 53, 0.05);
        }
        
        .returned-bill td {
            color: var(--error);
        }
    </style>
</head>
<body>
<!-- Login Screen -->
<div id="loginScreen" class="login-container">
    <div class="login-wrapper">
        <div class="login-image">
            <div class="login-image-content">
                <h2>Qaimkhani Store</h2>
                <p>Stationery & Hardware Management System</p>
                <div style="margin-top: 30px; font-size: 5rem; opacity: 0.8;">
                    <i class="fas fa-chart-line"></i>
                </div>
            </div>
        </div>
        <div class="login-card">
            <div class="login-header">
                <h2>LOGIN</h2>
                <p>Access your account</p>
            </div>
            <form id="loginForm">
                <div class="form-group">
                    <label for="username">EMAIL</label>
                    <input type="email" id="username" class="form-control" placeholder="Enter your email" required>
                </div>
                <div class="form-group">
                    <label for="password">PASSWORD</label>
                    <input type="password" id="password" class="form-control" placeholder="Enter password" required>
                </div>
                <button type="submit" class="btn btn-primary" style="width: 100%;">
                    <span id="loginBtnText">Login</span>
                    <span id="loginSpinner" class="loading"></span>
                </button>
            </form>
            <div id="loginAlert" class="alert alert-error">
                Invalid email or password. Please try again.
            </div>
            <div style="margin-top: 20px; text-align: center;">
                <p>Demo Login: admin@qaimkhani.com / password123</p>
            </div>
        </div>
    </div>
</div>

<!-- Main Application -->
<div id="app">
    <!-- Sidebar -->
    <div class="sidebar">
        <div class="sidebar-header">
            <h3>Qaimkhani Store</h3>
            <p style="font-size: 0.8rem; opacity: 0.8; margin-top: 5px;">Management System</p>
        </div>
        <ul class="sidebar-menu">
            <li><a href="#" class="active" data-page="dashboard"><i class="fas fa-tachometer-alt"></i> <span class="menu-text">Dashboard</span></a></li>
            <li><a href="#" data-page="products"><i class="fas fa-boxes"></i> <span class="menu-text">Products</span></a></li>
            <li><a href="#" data-page="customers"><i class="fas fa-users"></i> <span class="menu-text">Customers</span></a></li>
            <li><a href="#" data-page="khatta"><i class="fas fa-book"></i> <span class="menu-text">Khatta/Ledger</span></a></li>
            <li><a href="#" data-page="areas"><i class="fas fa-map-marker-alt"></i> <span class="menu-text">Areas</span></a></li>
            <li><a href="#" data-page="salesmen"><i class="fas fa-user-tie"></i> <span class="menu-text">Salesmen</span></a></li>
            <li><a href="#" data-page="suppliers"><i class="fas fa-truck"></i> <span class="menu-text">Suppliers</span></a></li>
            <li><a href="#" data-page="purchase-orders"><i class="fas fa-clipboard-list"></i> <span class="menu-text">Purchase Orders</span></a></li>
            <li><a href="#" data-page="grn"><i class="fas fa-clipboard-check"></i> <span class="menu-text">GRN</span></a></li>
            <li><a href="#" data-page="spot-sales"><i class="fas fa-cash-register"></i> <span class="menu-text">Spot Sales</span></a></li>
            <li><a href="#" data-page="returns"><i class="fas fa-undo-alt"></i> <span class="menu-text">Bill Returns</span></a></li>
            <li><a href="#" data-page="reports"><i class="fas fa-chart-bar"></i> <span class="menu-text">Reports</span></a></li>
            <li><a href="#" data-page="settings"><i class="fas fa-cogs"></i> <span class="menu-text">Settings</span></a></li>
            <li><a href="#" id="logoutBtn"><i class="fas fa-sign-out-alt"></i> <span class="menu-text">Logout</span></a></li>
        </ul>
    </div>

    <!-- Main Content -->
    <div class="main-content">
        <div class="topbar">
            <div class="topbar-left">
                <button class="mobile-menu-btn"><i class="fas fa-bars"></i></button>
                <h1>Dashboard</h1>
            </div>
            <div class="topbar-right">
                <div class="search-bar">
                    <i class="fas fa-search"></i>
                    <input type="text" id="globalSearch" placeholder="Search...">
                </div>
                <div class="notification-container">
                    <button class="theme-toggle" id="themeToggle"><i class="fas fa-moon"></i></button>
                    <span class="notification-badge" id="notificationCount">3</span>
                </div>
                <div class="user-profile">
                    <div class="user-avatar">A</div>
                    <span>Admin</span>
                </div>
            </div>
        </div>

        <div class="content-area">
            <!-- Dashboard Content -->
            <div id="dashboard" class="page-content active">
                <div class="page-title">
                    <h2>Dashboard</h2>
                    <div class="table-actions">
                        <button class="btn btn-primary" id="refreshData"><i class="fas fa-sync-alt"></i> Refresh Data</button>
                    </div>
                </div>

                <!-- Quick Actions -->
                <div class="quick-actions">
                    <div class="quick-action-btn" data-page="spot-sales">
                        <i class="fas fa-cash-register"></i>
                        <div>Create Bill</div>
                    </div>
                    <div class="quick-action-btn" data-page="products">
                        <i class="fas fa-boxes"></i>
                        <div>Add Product</div>
                    </div>
                    <div class="quick-action-btn" data-page="customers">
                        <i class="fas fa-user-plus"></i>
                        <div>Add Customer</div>
                    </div>
                    <div class="quick-action-btn" data-page="purchase-orders">
                        <i class="fas fa-clipboard-list"></i>
                        <div>Create PO</div>
                    </div>
                    <div class="quick-action-btn" data-page="khatta">
                        <i class="fas fa-book"></i>
                        <div>Add Khatta</div>
                    </div>
                </div>

                <!-- Stats Overview -->
                <div class="stats-grid">
                    <div class="stat-card">
                        <div class="stat-value" id="todaySales">PKR 0</div>
                        <div class="stat-label">Today's Sales</div>
                    </div>
                    <div class="stat-card">
                        <div class="stat-value" id="totalCustomers">0</div>
                        <div class="stat-label">Total Customers</div>
                    </div>
                    <div class="stat-card">
                        <div class="stat-value" id="totalProducts">0</div>
                        <div class="stat-label">Total Products</div>
                    </div>
                    <div class="stat-card">
                        <div class="stat-value" id="lowStockCount">0</div>
                        <div class="stat-label">Low Stock Items</div>
                    </div>
                    <div class="stat-card">
                        <div class="stat-value" id="pendingOrders">0</div>
                        <div class="stat-label">Pending Orders</div>
                    </div>
                    <div class="stat-card">
                        <div class="stat-value" id="monthlySales">PKR 0</div>
                        <div class="stat-label">Monthly Sales</div>
                    </div>
                </div>

                <!-- Dashboard Overview -->
                <div class="dashboard-overview">
                    <div>
                        <!-- Sales Chart -->
                        <div class="dashboard-widget">
                            <div class="dashboard-widget-header">
                                <div class="dashboard-widget-title">Sales Overview</div>
                                <div class="dashboard-widget-actions">
                                    <select id="chartPeriod" class="form-control">
                                        <option value="7">Last 7 Days</option>
                                        <option value="30">Last 30 Days</option>
                                        <option value="90">Last 90 Days</option>
                                    </select>
                                </div>
                            </div>
                            <div class="dashboard-widget-content">
                                <canvas id="salesChart" height="300"></canvas>
                            </div>
                        </div>

                        <!-- Recent Transactions -->
                        <div class="dashboard-widget">
                            <div class="dashboard-widget-header">
                                <div class="dashboard-widget-title">Recent Transactions</div>
                                <div class="dashboard-widget-actions">
                                    <button class="btn btn-outline btn-sm" id="viewAllTransactions">View All</button>
                                </div>
                            </div>
                            <div class="dashboard-widget-content">
                                <div class="recent-activity">
                                    <div id="recentTransactions">
                                        <div class="empty-state">
                                            <i class="fas fa-history"></i>
                                            <h3>No Recent Transactions</h3>
                                            <p>Your recent transactions will appear here</p>
                                        </div>
                                    </div>
                                </div>
                            </div>
                        </div>
                    </div>

                    <div>
                        <!-- Inventory Alerts -->
                        <div class="dashboard-widget">
                            <div class="dashboard-widget-header">
                                <div class="dashboard-widget-title">Inventory Alerts</div>
                                <div class="dashboard-widget-actions">
                                    <button class="btn btn-outline btn-sm" id="viewAllAlerts">View All</button>
                                </div>
                            </div>
                            <div class="dashboard-widget-content">
                                <div class="dashboard-notifications">
                                    <div id="inventoryAlerts">
                                        <div class="empty-state">
                                            <i class="fas fa-check-circle"></i>
                                            <h3>No Alerts</h3>
                                            <p>All products are well stocked</p>
                                        </div>
                                    </div>
                                </div>
                            </div>
                        </div>

                        <!-- Top Products -->
                        <div class="dashboard-widget">
                            <div class="dashboard-widget-header">
                                <div class="dashboard-widget-title">Top Selling Products</div>
                                <div class="dashboard-widget-actions">
                                    <button class="btn btn-outline btn-sm" id="viewAllProducts">View All</button>
                                </div>
                            </div>
                            <div class="dashboard-widget-content">
                                <div id="topProducts">
                                    <div class="empty-state">
                                        <i class="fas fa-chart-line"></i>
                                        <h3>No Data</h3>
                                        <p>Top selling products will appear here</p>
                                    </div>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
            </div>

            <!-- Products Content -->
            <div id="products" class="page-content">
                <div class="page-title">
                    <h2>Products</h2>
                    <div class="table-actions">
                        <button class="btn btn-primary" id="addProductBtn"><i class="fas fa-plus"></i> Add Product</button>
                        <button class="btn btn-outline" id="exportProducts"><i class="fas fa-file-export"></i> Export</button>
                    </div>
                </div>

                <div class="filter-bar">
                    <div class="filter-group">
                        <span class="filter-label">Category:</span>
                        <select id="categoryFilter" class="form-control">
                            <option value="">All Categories</option>
                            <option value="Stationery">Stationery</option>
                            <option value="Hardware">Hardware</option>
                            <option value="Electronics">Electronics</option>
                            <option value="Tools">Tools</option>
                        </select>
                    </div>
                    <div class="filter-group">
                        <span class="filter-label">Stock Status:</span>
                        <select id="stockFilter" class="form-control">
                            <option value="">All</option>
                            <option value="low">Low Stock</option>
                            <option value="out">Out of Stock</option>
                            <option value="in">In Stock</option>
                        </select>
                    </div>
                </div>

                <div class="table-container">
                    <div class="table-header">
                        <h3>Product List</h3>
                        <div class="search-bar">
                            <i class="fas fa-search"></i>
                            <input type="text" id="productSearch" placeholder="Search products...">
                        </div>
                    </div>
                    <div class="table-responsive">
                        <table>
                            <thead>
                                <tr>
                                    <th>ID</th>
                                    <th>Name</th>
                                    <th>Category</th>
                                    <th>Price</th>
                                    <th>Stock</th>
                                    <th>Status</th>
                                    <th>Actions</th>
                                </tr>
                            </thead>
                            <tbody id="productsTable">
                                <!-- Product data will be populated here -->
                            </tbody>
                        </table>
                    </div>
                </div>
            </div>

            <!-- Customers Content -->
            <div id="customers" class="page-content">
                <div class="page-title">
                    <h2>Customers</h2>
                    <div class="table-actions">
                        <button class="btn btn-primary" id="addCustomerBtn"><i class="fas fa-plus"></i> Add Customer</button>
                        <button class="btn btn-outline" id="exportCustomers"><i class="fas fa-file-export"></i> Export</button>
                    </div>
                </div>

                <div class="filter-bar">
                    <div class="filter-group">
                        <span class="filter-label">Area:</span>
                        <select id="areaFilter" class="form-control">
                            <option value="">All Areas</option>
                        </select>
                    </div>
                    <div class="filter-group">
                        <span class="filter-label">Salesman:</span>
                        <select id="salesmanFilter" class="form-control">
                            <option value="">All Salesmen</option>
                        </select>
                    </div>
                </div>

                <div class="table-container">
                    <div class="table-header">
                        <h3>Customer List</h3>
                        <div class="search-bar">
                            <i class="fas fa-search"></i>
                            <input type="text" id="customerSearch" placeholder="Search customers...">
                        </div>
                    </div>
                    <div class="table-responsive">
                        <table>
                            <thead>
                                <tr>
                                    <th>ID</th>
                                    <th>Name</th>
                                    <th>Phone</th>
                                    <th>Area</th>
                                    <th>Salesman</th>
                                    <th>Balance</th>
                                    <th>Actions</th>
                                </tr>
                            </thead>
                            <tbody id="customersTable">
                                <!-- Customer data will be populated here -->
                            </tbody>
                        </table>
                    </div>
                </div>
            </div>

            <!-- Khatta/Ledger Content -->
            <div id="khatta" class="page-content">
                <div class="page-title">
                    <h2>Khatta/Ledger</h2>
                    <div class="table-actions">
                        <button class="btn btn-primary" id="addKhattaBtn"><i class="fas fa-plus"></i> Add Entry</button>
                        <button class="btn btn-outline" id="exportKhatta"><i class="fas fa-file-export"></i> Export</button>
                    </div>
                </div>

                <div class="filter-bar">
                    <div class="filter-group">
                        <span class="filter-label">Customer:</span>
                        <select id="khattaCustomerFilter" class="form-control">
                            <option value="">All Customers</option>
                        </select>
                    </div>
                    <div class="filter-group">
                        <span class="filter-label">Type:</span>
                        <select id="khattaTypeFilter" class="form-control">
                            <option value="">All Types</option>
                            <option value="credit">Credit</option>
                            <option value="debit">Debit</option>
                        </select>
                    </div>
                    <div class="filter-group">
                        <span class="filter-label">Date Range:</span>
                        <input type="date" id="khattaStartDate" class="form-control">
                        <span>to</span>
                        <input type="date" id="khattaEndDate" class="form-control">
                    </div>
                </div>

                <div class="table-container">
                    <div class="table-header">
                        <h3>Khatta Entries</h3>
                        <div class="search-bar">
                            <i class="fas fa-search"></i>
                            <input type="text" id="khattaSearch" placeholder="Search khatta...">
                        </div>
                    </div>
                    <div class="table-responsive">
                        <table class="khatta-table">
                            <thead>
                                <tr>
                                    <th>Date</th>
                                    <th>Customer</th>
                                    <th>Description</th>
                                    <th>Credit</th>
                                    <th>Debit</th>
                                    <th>Balance</th>
                                    <th>Actions</th>
                                </tr>
                            </thead>
                            <tbody id="khattaTable">
                                <!-- Khatta data will be populated here -->
                            </tbody>
                        </table>
                    </div>
                </div>
            </div>

            <!-- Areas Content -->
            <div id="areas" class="page-content">
                <div class="page-title">
                    <h2>Areas</h2>
                    <div class="table-actions">
                        <button class="btn btn-primary" id="addAreaBtn"><i class="fas fa-plus"></i> Add Area</button>
                    </div>
                </div>

                <div class="table-container">
                    <div class="table-header">
                        <h3>Area List</h3>
                        <div class="search-bar">
                            <i class="fas fa-search"></i>
                            <input type="text" id="areaSearch" placeholder="Search areas...">
                        </div>
                    </div>
                    <div class="table-responsive">
                        <table>
                            <thead>
                                <tr>
                                    <th>ID</th>
                                    <th>Name</th>
                                    <th>Description</th>
                                    <th>Customers</th>
                                    <th>Actions</th>
                                </tr>
                            </thead>
                            <tbody id="areasTable">
                                <!-- Area data will be populated here -->
                            </tbody>
                        </table>
                    </div>
                </div>
            </div>

            <!-- Salesmen Content -->
            <div id="salesmen" class="page-content">
                <div class="page-title">
                    <h2>Salesmen</h2>
                    <div class="table-actions">
                        <button class="btn btn-primary" id="addSalesmanBtn"><i class="fas fa-plus"></i> Add Salesman</button>
                    </div>
                </div>

                <div class="table-container">
                    <div class="table-header">
                        <h3>Salesman List</h3>
                        <div class="search-bar">
                            <i class="fas fa-search"></i>
                            <input type="text" id="salesmanSearch" placeholder="Search salesmen...">
                        </div>
                    </div>
                    <div class="table-responsive">
                        <table>
                            <thead>
                                <tr>
                                    <th>ID</th>
                                    <th>Name</th>
                                    <th>Phone</th>
                                    <th>Email</th>
                                    <th>Area</th>
                                    <th>Status</th>
                                    <th>Actions</th>
                                </tr>
                            </thead>
                            <tbody id="salesmenTable">
                                <!-- Salesman data will be populated here -->
                            </tbody>
                        </table>
                    </div>
                </div>
            </div>

            <!-- Suppliers Content -->
            <div id="suppliers" class="page-content">
                <div class="page-title">
                    <h2>Supplier Management</h2>
                    <div class="table-actions">
                        <button class="btn btn-primary" id="addSupplierBtn"><i class="fas fa-plus"></i> Add Supplier</button>
                        <button class="btn btn-outline" id="exportSuppliers"><i class="fas fa-file-export"></i> Export</button>
                    </div>
                </div>

                <div class="card-container">
                    <div class="card">
                        <div class="card-header">
                            <div class="card-title">Supplier Statistics</div>
                        </div>
                        <div class="stats-grid">
                            <div class="stat-card">
                                <div class="stat-value" id="totalSuppliers">0</div>
                                <div class="stat-label">Total Suppliers</div>
                            </div>
                            <div class="stat-card">
                                <div class="stat-value" id="activeSuppliers">0</div>
                                <div class="stat-label">Active Suppliers</div>
                            </div>
                            <div class="stat-card">
                                <div class="stat-value" id="supplierProducts">0</div>
                                <div class="stat-label">Supplier Products</div>
                            </div>
                            <div class="stat-card">
                                <div class="stat-value" id="pendingPOs">0</div>
                                <div class="stat-label">Pending POs</div>
                            </div>
                        </div>
                    </div>
                </div>

                <div class="filter-bar">
                    <div class="filter-group">
                        <span class="filter-label">Status:</span>
                        <select id="supplierStatusFilter" class="form-control">
                            <option value="">All Status</option>
                            <option value="active">Active</option>
                            <option value="inactive">Inactive</option>
                        </select>
                    </div>
                </div>

                <div class="table-container">
                    <div class="table-header">
                        <h3>Supplier List</h3>
                        <div class="search-bar">
                            <i class="fas fa-search"></i>
                            <input type="text" id="supplierSearch" placeholder="Search suppliers...">
                        </div>
                    </div>
                    <div class="table-responsive">
                        <table>
                            <thead>
                                <tr>
                                    <th>ID</th>
                                    <th>Company Name</th>
                                    <th>Contact Person</th>
                                    <th>Phone</th>
                                    <th>Email</th>
                                    <th>Products</th>
                                    <th>Status</th>
                                    <th>Actions</th>
                                </tr>
                            </thead>
                            <tbody id="suppliersTable">
                                <!-- Supplier data will be populated here -->
                            </tbody>
                        </table>
                    </div>
                </div>

                <!-- Supplier Products Section -->
                <div class="supplier-products" id="supplierProductsSection" style="display: none;">
                    <div class="table-container">
                        <div class="table-header">
                            <h3>Supplier Products</h3>
                            <div class="search-bar">
                                <i class="fas fa-search"></i>
                                <input type="text" id="supplierProductSearch" placeholder="Search supplier products...">
                            </div>
                        </div>
                        <div class="table-responsive">
                            <table>
                                <thead>
                                    <tr>
                                        <th>Product Name</th>
                                        <th>Category</th>
                                        <th>Cost Price</th>
                                        <th>Stock</th>
                                        <th>Status</th>
                                    </tr>
                                </thead>
                                <tbody id="supplierProductsTable">
                                    <!-- Supplier product data will be populated here -->
                                </tbody>
                            </table>
                        </div>
                    </div>
                </div>
            </div>

            <!-- Purchase Orders Content -->
            <div id="purchase-orders" class="page-content">
                <div class="page-title">
                    <h2>Purchase Orders</h2>
                    <div class="table-actions">
                        <button class="btn btn-primary" id="addPurchaseOrderBtn"><i class="fas fa-plus"></i> Create PO</button>
                        <button class="btn btn-outline" id="exportPurchaseOrders"><i class="fas fa-file-export"></i> Export</button>
                    </div>
                </div>

                <div class="filter-bar">
                    <div class="filter-group">
                        <span class="filter-label">Status:</span>
                        <select id="poStatusFilter" class="form-control">
                            <option value="">All Status</option>
                            <option value="pending">Pending</option>
                            <option value="approved">Approved</option>
                            <option value="completed">Completed</option>
                        </select>
                    </div>
                    <div class="filter-group">
                        <span class="filter-label">Date Range:</span>
                        <input type="date" id="poStartDate" class="form-control">
                        <span>to</span>
                        <input type="date" id="poEndDate" class="form-control">
                    </div>
                </div>

                <div class="table-container">
                    <div class="table-header">
                        <h3>Purchase Orders</h3>
                        <div class="search-bar">
                            <i class="fas fa-search"></i>
                            <input type="text" id="poSearch" placeholder="Search purchase orders...">
                        </div>
                    </div>
                    <div class="table-responsive">
                        <table>
                            <thead>
                                <tr>
                                    <th>PO Code</th>
                                    <th>Supplier</th>
                                    <th>Date</th>
                                    <th>Total Amount</th>
                                    <th>GRN Status</th>
                                    <th>Actions</th>
                                </tr>
                            </thead>
                            <tbody id="purchaseOrdersTable">
                                <!-- Purchase order data will be populated here -->
                            </tbody>
                        </table>
                    </div>
                </div>
            </div>

            <!-- GRN Content -->
            <div id="grn" class="page-content">
                <div class="page-title">
                    <h2>Goods Received Notes (GRN)</h2>
                    <div class="table-actions">
                        <button class="btn btn-primary" id="searchPOBtn"><i class="fas fa-search"></i> Search PO</button>
                        <button class="btn btn-outline" id="exportGrn"><i class="fas fa-file-export"></i> Export</button>
                    </div>
                </div>

                <div class="filter-bar">
                    <div class="filter-group">
                        <span class="filter-label">PO Code:</span>
                        <input type="text" id="grnPOCode" class="form-control" placeholder="Enter PO Code">
                        <button class="btn btn-primary" id="searchPOForGRN">Search</button>
                    </div>
                </div>

                <!-- PO Details for GRN -->
                <div id="poDetailsForGRN" style="display: none;">
                    <div class="card">
                        <div class="card-header">
                            <h3>Purchase Order Details</h3>
                        </div>
                        <div class="card-body">
                            <div class="form-row">
                                <div class="form-group">
                                    <label>PO Code:</label>
                                    <span id="poCodeDisplay"></span>
                                </div>
                                <div class="form-group">
                                    <label>Date:</label>
                                    <span id="poDateDisplay"></span>
                                </div>
                            </div>
                            <div class="form-row">
                                <div class="form-group">
                                    <label>Supplier:</label>
                                    <span id="poSupplierDisplay"></span>
                                </div>
                                <div class="form-group">
                                    <label>Product:</label>
                                    <span id="poProductDisplay"></span>
                                </div>
                            </div>
                            <div class="form-row">
                                <div class="form-group">
                                    <label>Quantity:</label>
                                    <span id="poQuantityDisplay"></span>
                                </div>
                                <div class="form-group">
                                    <label>Price:</label>
                                    <span id="poPriceDisplay"></span>
                                </div>
                            </div>
                            <div class="form-row">
                                <div class="form-group">
                                    <label>Discount:</label>
                                    <span id="poDiscountDisplay"></span>
                                </div>
                                <div class="form-group">
                                    <label>Total Amount:</label>
                                    <span id="poTotalDisplay"></span>
                                </div>
                            </div>
                            <div class="form-row">
                                <div class="form-group">
                                    <label>Net Amount:</label>
                                    <span id="poNetAmountDisplay"></span>
                                </div>
                            </div>
                            <div class="form-group">
                                <button class="btn btn-success" id="submitGRNBtn">Submit GRN</button>
                            </div>
                        </div>
                    </div>
                </div>

                <div class="table-container">
                    <div class="table-header">
                        <h3>GRN Records</h3>
                        <div class="search-bar">
                            <i class="fas fa-search"></i>
                            <input type="text" id="grnSearch" placeholder="Search GRN...">
                        </div>
                    </div>
                    <div class="table-responsive">
                        <table>
                            <thead>
                                <tr>
                                    <th>Date</th>
                                    <th>PO Code</th>
                                    <th>Product</th>
                                    <th>Supplier</th>
                                    <th>Quantity</th>
                                    <th>Total Amount</th>
                                    <th>Actions</th>
                                </tr>
                            </thead>
                            <tbody id="grnTable">
                                <!-- GRN data will be populated here -->
                            </tbody>
                        </table>
                    </div>
                </div>
            </div>

            <!-- Spot Sales Content -->
            <div id="spot-sales" class="page-content">
                <div class="page-title">
                    <h2>Spot Sales</h2>
                    <div class="table-actions">
                        <button class="btn btn-primary" id="createBillBtn"><i class="fas fa-receipt"></i> Create Bill</button>
                        <button class="btn btn-outline" id="exportBills"><i class="fas fa-file-export"></i> Export</button>
                    </div>
                </div>

                <div class="filter-bar">
                    <div class="filter-group">
                        <span class="filter-label">Date Range:</span>
                        <input type="date" id="billStartDate" class="form-control">
                        <span>to</span>
                        <input type="date" id="billEndDate" class="form-control">
                    </div>
                    <div class="filter-group">
                        <span class="filter-label">Payment Type:</span>
                        <select id="paymentTypeFilter" class="form-control">
                            <option value="">All Types</option>
                            <option value="cash">Cash</option>
                            <option value="card">Card</option>
                            <option value="upi">UPI</option>
                            <option value="credit">Credit</option>
                        </select>
                    </div>
                </div>

                <div class="table-container">
                    <div class="table-header">
                        <h3>Sales Bills</h3>
                        <div class="search-bar">
                            <i class="fas fa-search"></i>
                            <input type="text" id="billSearch" placeholder="Search bills...">
                        </div>
                    </div>
                    <div class="table-responsive">
                        <table>
                            <thead>
                                <tr>
                                    <th>SB Code</th>
                                    <th>Date</th>
                                    <th>Customer</th>
                                    <th>Salesman</th>
                                    <th>Items</th>
                                    <th>Total Amount</th>
                                    <th>Payment Type</th>
                                    <th>Status</th>
                                    <th>Actions</th>
                                </tr>
                            </thead>
                            <tbody id="billsTable">
                                <!-- Bill data will be populated here -->
                            </tbody>
                        </table>
                    </div>
                </div>
            </div>

            <!-- Bill Returns Content -->
            <div id="returns" class="page-content">
                <div class="page-title">
                    <h2>Bill Returns</h2>
                    <div class="table-actions">
                        <button class="btn btn-primary" id="searchSBForReturnBtn"><i class="fas fa-search"></i> Search SB</button>
                        <button class="btn btn-outline" id="exportReturns"><i class="fas fa-file-export"></i> Export</button>
                    </div>
                </div>

                <div class="filter-bar">
                    <div class="filter-group">
                        <span class="filter-label">SB Code:</span>
                        <input type="text" id="returnSBCode" class="form-control" placeholder="Enter SB Code">
                        <button class="btn btn-primary" id="searchSBForReturn">Search</button>
                    </div>
                </div>

                <!-- SB Details for Return -->
                <div id="sbDetailsForReturn" style="display: none;">
                    <div class="card">
                        <div class="card-header">
                            <h3>Bill Details</h3>
                        </div>
                        <div class="card-body">
                            <div class="form-row">
                                <div class="form-group">
                                    <label>SB Code:</label>
                                    <span id="sbCodeDisplay"></span>
                                </div>
                                <div class="form-group">
                                    <label>Date:</label>
                                    <span id="sbDateDisplay"></span>
                                </div>
                            </div>
                            <div class="form-row">
                                <div class="form-group">
                                    <label>Customer:</label>
                                    <span id="sbCustomerDisplay"></span>
                                </div>
                                <div class="form-group">
                                    <label>Total Amount:</label>
                                    <span id="sbTotalDisplay"></span>
                                </div>
                            </div>
                            <div class="form-group">
                                <label>Items:</label>
                                <div id="sbItemsDisplay"></div>
                            </div>
                            <div class="form-group">
                                <button class="btn btn-danger" id="submitReturnBtn">Submit Return</button>
                            </div>
                        </div>
                    </div>
                </div>

                <div class="table-container">
                    <div class="table-header">
                        <h3>Return Records</h3>
                        <div class="search-bar">
                            <i class="fas fa-search"></i>
                            <input type="text" id="returnSearch" placeholder="Search returns...">
                        </div>
                    </div>
                    <div class="table-responsive">
                        <table>
                            <thead>
                                <tr>
                                    <th>Return Date</th>
                                    <th>SB Code</th>
                                    <th>Customer</th>
                                    <th>Items</th>
                                    <th>Total Amount</th>
                                    <th>Actions</th>
                                </tr>
                            </thead>
                            <tbody id="returnsTable">
                                <!-- Return data will be populated here -->
                            </tbody>
                        </table>
                    </div>
                </div>
            </div>

            <!-- Reports Content -->
            <div id="reports" class="page-content">
                <div class="page-title">
                    <h2>Reports</h2>
                    <div class="table-actions">
                        <button class="btn btn-primary" id="generateReportBtn"><i class="fas fa-chart-bar"></i> Generate Report</button>
                        <button class="btn btn-outline" id="exportReport"><i class="fas fa-file-export"></i> Export</button>
                    </div>
                </div>

                <div class="tabs">
                    <div class="tab active" data-report="sales">Sales Report</div>
                    <div class="tab" data-report="purchase">Purchase Order Report</div>
                    <div class="tab" data-report="daily">Daily Billing Report</div>
                    <div class="tab" data-report="return">Return Bills Report</div>
                    <div class="tab" data-report="stock">Stock Report</div>
                    <div class="tab" data-report="customer">Customer Report</div>
                    <div class="tab" data-report="khatta">Khatta Report</div>
                    <div class="tab" data-report="supplier">Supplier Report</div>
                </div>

                <div class="filter-bar">
                    <div class="filter-group">
                        <span class="filter-label">Date Range:</span>
                        <input type="date" id="reportStartDate" class="form-control">
                        <span>to</span>
                        <input type="date" id="reportEndDate" class="form-control">
                    </div>
                    <div class="filter-group">
                        <span class="filter-label">Report Type:</span>
                        <select id="reportType" class="form-control">
                            <option value="daily">Daily</option>
                            <option value="weekly">Weekly</option>
                            <option value="monthly">Monthly</option>
                            <option value="yearly">Yearly</option>
                        </select>
                    </div>
                </div>

                <div class="tab-content active" id="salesReport">
                    <div class="chart-container">
                        <canvas id="salesReportChart" height="300"></canvas>
                    </div>
                    <div class="table-container">
                        <div class="table-header">
                            <h3>Sales Data</h3>
                        </div>
                        <div class="table-responsive">
                            <table>
                                <thead>
                                    <tr>
                                        <th>Date</th>
                                        <th>SB Code</th>
                                        <th>Customer</th>
                                        <th>Items</th>
                                        <th>Amount</th>
                                        <th>Payment Type</th>
                                    </tr>
                                </thead>
                                <tbody id="salesReportTable">
                                    <!-- Sales report data will be populated here -->
                                </tbody>
                            </table>
                        </div>
                    </div>
                </div>

                <div class="tab-content" id="purchaseReport">
                    <div class="chart-container">
                        <canvas id="purchaseReportChart" height="300"></canvas>
                    </div>
                    <div class="table-container">
                        <div class="table-header">
                            <h3>Purchase Order Data</h3>
                        </div>
                        <div class="table-responsive">
                            <table>
                                <thead>
                                    <tr>
                                        <th>PO Code</th>
                                        <th>Date</th>
                                        <th>Supplier</th>
                                        <th>Product</th>
                                        <th>Quantity</th>
                                        <th>Amount</th>
                                        <th>Status</th>
                                    </tr>
                                </thead>
                                <tbody id="purchaseReportTable">
                                    <!-- Purchase report data will be populated here -->
                                </tbody>
                            </table>
                        </div>
                    </div>
                </div>

                <div class="tab-content" id="dailyReport">
                    <div class="table-container">
                        <div class="table-header">
                            <h3>Daily Billing Data</h3>
                        </div>
                        <div class="table-responsive">
                            <table>
                                <thead>
                                    <tr>
                                        <th>SB Code</th>
                                        <th>Time</th>
                                        <th>Customer</th>
                                        <th>Items</th>
                                        <th>Amount</th>
                                        <th>Payment Type</th>
                                    </tr>
                                </thead>
                                <tbody id="dailyReportTable">
                                    <!-- Daily report data will be populated here -->
                                </tbody>
                            </table>
                        </div>
                    </div>
                </div>

                <div class="tab-content" id="returnReport">
                    <div class="table-container">
                        <div class="table-header">
                            <h3>Return Bills Data</h3>
                        </div>
                        <div class="table-responsive">
                            <table>
                                <thead>
                                    <tr>
                                        <th>Return Date</th>
                                        <th>SB Code</th>
                                        <th>Customer</th>
                                        <th>Items</th>
                                        <th>Amount</th>
                                        <th>Reason</th>
                                    </tr>
                                </thead>
                                <tbody id="returnReportTable">
                                    <!-- Return report data will be populated here -->
                                </tbody>
                            </table>
                        </div>
                    </div>
                </div>

                <div class="tab-content" id="stockReport">
                    <div class="table-container">
                        <div class="table-header">
                            <h3>Stock Data</h3>
                        </div>
                        <div class="table-responsive">
                            <table>
                                <thead>
                                    <tr>
                                        <th>Product</th>
                                        <th>Category</th>
                                        <th>Stock</th>
                                        <th>Sold</th>
                                        <th>Revenue</th>
                                    </tr>
                                </thead>
                                <tbody id="stockReportTable">
                                    <!-- Stock report data will be populated here -->
                                </tbody>
                            </table>
                        </div>
                    </div>
                </div>

                <div class="tab-content" id="customerReport">
                    <div class="chart-container">
                        <canvas id="customerReportChart" height="300"></canvas>
                    </div>
                    <div class="table-container">
                        <div class="table-header">
                            <h3>Customer Data</h3>
                        </div>
                        <div class="table-responsive">
                            <table>
                                <thead>
                                    <tr>
                                        <th>Customer</th>
                                        <th>Phone</th>
                                        <th>Total Purchases</th>
                                        <th>Last Purchase</th>
                                        <th>Outstanding</th>
                                    </tr>
                                </thead>
                                <tbody id="customerReportTable">
                                    <!-- Customer report data will be populated here -->
                                </tbody>
                            </table>
                        </div>
                    </div>
                </div>

                <div class="tab-content" id="khattaReport">
                    <div class="chart-container">
                        <canvas id="khattaReportChart" height="300"></canvas>
                    </div>
                    <div class="table-container">
                        <div class="table-header">
                            <h3>Khatta Data</h3>
                        </div>
                        <div class="table-responsive">
                            <table>
                                <thead>
                                    <tr>
                                        <th>Date</th>
                                        <th>Customer</th>
                                        <th>Description</th>
                                        <th>Credit</th>
                                        <th>Debit</th>
                                        <th>Balance</th>
                                    </tr>
                                </thead>
                                <tbody id="khattaReportTable">
                                    <!-- Khatta report data will be populated here -->
                                </tbody>
                            </table>
                        </div>
                    </div>
                </div>

                <div class="tab-content" id="supplierReport">
                    <div class="chart-container">
                        <canvas id="supplierReportChart" height="300"></canvas>
                    </div>
                    <div class="table-container">
                        <div class="table-header">
                            <h3>Supplier Data</h3>
                        </div>
                        <div class="table-responsive">
                            <table>
                                <thead>
                                    <tr>
                                        <th>Supplier</th>
                                        <th>Contact</th>
                                        <th>Total Products</th>
                                        <th>Total Orders</th>
                                        <th>Last Order</th>
                                    </tr>
                                </thead>
                                <tbody id="supplierReportTable">
                                    <!-- Supplier report data will be populated here -->
                                </tbody>
                            </table>
                        </div>
                    </div>
                </div>
            </div>

            <!-- Settings Content -->
            <div id="settings" class="page-content">
                <div class="page-title">
                    <h2>Settings</h2>
                </div>

                <div class="card-container">
                    <div class="card">
                        <div class="card-header">
                            <div class="card-title">Store Information</div>
                        </div>
                        <div class="card-body">
                            <form id="storeSettingsForm">
                                <div class="form-group">
                                    <label for="storeName">Store Name</label>
                                    <input type="text" id="storeName" class="form-control" placeholder="Qaimkhani Stationery & Hardware Store">
                                </div>
                                <div class="form-group">
                                    <label for="storePhone">Phone</label>
                                    <input type="text" id="storePhone" class="form-control" placeholder="+91 XXXXXXXXXX">
                                </div>
                                <div class="form-group">
                                    <label for="storeAddress">Address</label>
                                    <textarea id="storeAddress" class="form-control" rows="3" placeholder="Store address"></textarea>
                                </div>
                                <div class="form-group">
                                    <label for="storeGST">GST Number</label>
                                    <input type="text" id="storeGST" class="form-control" placeholder="GSTIN Number">
                                </div>
                                <div class="form-group">
                                    <label for="storeEmail">Email</label>
                                    <input type="email" id="storeEmail" class="form-control" placeholder="store@email.com">
                                </div>
                                <button type="submit" class="btn btn-primary">Save Settings</button>
                            </form>
                        </div>
                    </div>

                    <div class="card">
                        <div class="card-header">
                            <div class="card-title">System Settings</div>
                        </div>
                        <div class="card-body">
                            <div class="form-group">
                                <label for="currency">Currency</label>
                                <select id="currency" class="form-control">
                                    <option value="PKR">PKR - Pakistani Rupee</option>
                                    <option value="INR">INR - Indian Rupee</option>
                                    <option value="USD">USD - US Dollar</option>
                                </select>
                            </div>
                            <div class="form-group">
                                <label for="dateFormat">Date Format</label>
                                <select id="dateFormat" class="form-control">
                                    <option value="dd/mm/yyyy">DD/MM/YYYY</option>
                                    <option value="mm/dd/yyyy">MM/DD/YYYY</option>
                                    <option value="yyyy-mm-dd">YYYY-MM-DD</option>
                                </select>
                            </div>
                            <div class="form-group">
                                <label for="taxRate">Default Tax Rate (%)</label>
                                <input type="number" id="taxRate" class="form-control" value="0" min="0" max="100">
                            </div>
                            <div class="form-group">
                                <label for="lowStockThreshold">Low Stock Threshold</label>
                                <input type="number" id="lowStockThreshold" class="form-control" value="10" min="1">
                            </div>
                            <button class="btn btn-primary">Save System Settings</button>
                        </div>
                    </div>

                    <div class="card">
                        <div class="card-header">
                            <div class="card-title">Data Management</div>
                        </div>
                        <div class="card-body">
                            <div class="form-group">
                                <button class="btn btn-outline" id="backupDataBtn"><i class="fas fa-download"></i> Backup Data</button>
                            </div>
                            <div class="form-group">
                                <button class="btn btn-outline" id="restoreDataBtn"><i class="fas fa-upload"></i> Restore Data</button>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>
</div>

<!-- Toast Notification -->
<div id="toast" class="toast"></div>

<!-- Modals -->
<div id="productModal" class="modal">
    <div class="modal-content">
        <div class="modal-header">
            <h3 id="productModalTitle">Add Product</h3>
            <span class="close">&times;</span>
        </div>
        <div class="modal-body">
            <form id="productForm">
                <input type="hidden" id="productId">
                <div class="form-row">
                    <div class="form-group">
                        <label for="productName">Product Name</label>
                        <input type="text" id="productName" class="form-control" required>
                    </div>
                    <div class="form-group">
                        <label for="productCategory">Category</label>
                        <select id="productCategory" class="form-control" required>
                            <option value="">Select Category</option>
                            <option value="Stationery">Stationery</option>
                            <option value="Hardware">Hardware</option>
                            <option value="Electronics">Electronics</option>
                            <option value="Tools">Tools</option>
                            <option value="Other">Other</option>
                        </select>
                    </div>
                </div>
                <div class="form-row">
                    <div class="form-group">
                        <label for="productPrice">Price (PKR)</label>
                        <input type="number" id="productPrice" class="form-control" min="0" step="0.01" required>
                    </div>
                    <div class="form-group">
                        <label for="productCost">Cost (PKR)</label>
                        <input type="number" id="productCost" class="form-control" min="0" step="0.01" required>
                    </div>
                </div>
                <div class="form-row">
                    <div class="form-group">
                        <label for="productStock">Stock</label>
                        <input type="number" id="productStock" class="form-control" min="0" required>
                    </div>
                    <div class="form-group">
                        <label for="productMinStock">Min Stock</label>
                        <input type="number" id="productMinStock" class="form-control" min="0" value="5">
                    </div>
                </div>
                <div class="form-row">
                    <div class="form-group">
                        <label for="productSupplier">Supplier</label>
                        <select id="productSupplier" class="form-control">
                            <option value="">Select Supplier</option>
                        </select>
                    </div>
                    <div class="form-group">
                        <label for="productBarcode">Barcode</label>
                        <input type="text" id="productBarcode" class="form-control">
                    </div>
                </div>
                <div class="form-group">
                    <label for="productDescription">Description</label>
                    <textarea id="productDescription" class="form-control" rows="3"></textarea>
                </div>
            </form>
        </div>
        <div class="modal-footer">
            <button class="btn btn-outline close">Cancel</button>
            <button id="saveProductBtn" class="btn btn-primary">Save Product</button>
        </div>
    </div>
</div>

<div id="customerModal" class="modal">
    <div class="modal-content">
        <div class="modal-header">
            <h3 id="customerModalTitle">Add Customer</h3>
            <span class="close">&times;</span>
        </div>
        <div class="modal-body">
            <form id="customerForm">
                <input type="hidden" id="customerId">
                <div class="form-row">
                    <div class="form-group">
                        <label for="customerName">Full Name</label>
                        <input type="text" id="customerName" class="form-control" required>
                    </div>
                    <div class="form-group">
                        <label for="customerPhone">Phone</label>
                        <input type="text" id="customerPhone" class="form-control" required>
                    </div>
                </div>
                <div class="form-row">
                    <div class="form-group">
                        <label for="customerEmail">Email</label>
                        <input type="email" id="customerEmail" class="form-control">
                    </div>
                    <div class="form-group">
                        <label for="customerType">Customer Type</label>
                        <select id="customerType" class="form-control">
                            <option value="Regular">Regular</option>
                            <option value="Wholesale">Wholesale</option>
                            <option value="VIP">VIP</option>
                        </select>
                    </div>
                </div>
                <div class="form-row">
                    <div class="form-group">
                        <label for="customerArea">Area</label>
                        <select id="customerArea" class="form-control">
                            <option value="">Select Area</option>
                        </select>
                    </div>
                    <div class="form-group">
                        <label for="customerSalesman">Salesman</label>
                        <select id="customerSalesman" class="form-control">
                            <option value="">Select Salesman</option>
                        </select>
                    </div>
                </div>
                <div class="form-group">
                    <label for="customerAddress">Address</label>
                    <textarea id="customerAddress" class="form-control" rows="3"></textarea>
                </div>
            </form>
        </div>
        <div class="modal-footer">
            <button class="btn btn-outline close">Cancel</button>
            <button id="saveCustomerBtn" class="btn btn-primary">Save Customer</button>
        </div>
    </div>
</div>

<div id="khattaModal" class="modal">
    <div class="modal-content">
        <div class="modal-header">
            <h3 id="khattaModalTitle">Add Khatta Entry</h3>
            <span class="close">&times;</span>
        </div>
        <div class="modal-body">
            <form id="khattaForm">
                <input type="hidden" id="khattaId">
                <div class="form-row">
                    <div class="form-group">
                        <label for="khattaCustomer">Customer</label>
                        <select id="khattaCustomer" class="form-control" required>
                            <option value="">Select Customer</option>
                        </select>
                    </div>
                    <div class="form-group">
                        <label for="khattaDate">Date</label>
                        <input type="date" id="khattaDate" class="form-control" required>
                    </div>
                </div>
                <div class="form-row">
                    <div class="form-group">
                        <label for="khattaType">Type</label>
                        <select id="khattaType" class="form-control" required>
                            <option value="credit">Credit</option>
                            <option value="debit">Debit</option>
                        </select>
                    </div>
                    <div class="form-group">
                        <label for="khattaAmount">Amount (PKR)</label>
                        <input type="number" id="khattaAmount" class="form-control" min="0" step="0.01" required>
                    </div>
                </div>
                <div class="form-group">
                    <label for="khattaDescription">Description</label>
                    <textarea id="khattaDescription" class="form-control" rows="3" required></textarea>
                </div>
            </form>
        </div>
        <div class="modal-footer">
            <button class="btn btn-outline close">Cancel</button>
            <button id="saveKhattaBtn" class="btn btn-primary">Save Entry</button>
        </div>
    </div>
</div>

<div id="areaModal" class="modal">
    <div class="modal-content">
        <div class="modal-header">
            <h3 id="areaModalTitle">Add Area</h3>
            <span class="close">&times;</span>
        </div>
        <div class="modal-body">
            <form id="areaForm">
                <input type="hidden" id="areaId">
                <div class="form-group">
                    <label for="areaName">Area Name</label>
                    <input type="text" id="areaName" class="form-control" required>
                </div>
                <div class="form-group">
                    <label for="areaDescription">Description</label>
                    <textarea id="areaDescription" class="form-control" rows="3"></textarea>
                </div>
            </form>
        </div>
        <div class="modal-footer">
            <button class="btn btn-outline close">Cancel</button>
            <button id="saveAreaBtn" class="btn btn-primary">Save Area</button>
        </div>
    </div>
</div>

<div id="salesmanModal" class="modal">
    <div class="modal-content">
        <div class="modal-header">
            <h3 id="salesmanModalTitle">Add Salesman</h3>
            <span class="close">&times;</span>
        </div>
        <div class="modal-body">
            <form id="salesmanForm">
                <input type="hidden" id="salesmanId">
                <div class="form-row">
                    <div class="form-group">
                        <label for="salesmanName">Full Name</label>
                        <input type="text" id="salesmanName" class="form-control" required>
                    </div>
                    <div class="form-group">
                        <label for="salesmanPhone">Phone</label>
                        <input type="text" id="salesmanPhone" class="form-control" required>
                    </div>
                </div>
                <div class="form-row">
                    <div class="form-group">
                        <label for="salesmanEmail">Email</label>
                        <input type="email" id="salesmanEmail" class="form-control">
                    </div>
                    <div class="form-group">
                        <label for="salesmanArea">Area</label>
                        <select id="salesmanArea" class="form-control">
                            <option value="">Select Area</option>
                        </select>
                    </div>
                </div>
                <div class="form-group">
                    <label for="salesmanAddress">Address</label>
                    <textarea id="salesmanAddress" class="form-control" rows="3"></textarea>
                </div>
            </form>
        </div>
        <div class="modal-footer">
            <button class="btn btn-outline close">Cancel</button>
            <button id="saveSalesmanBtn" class="btn btn-primary">Save Salesman</button>
        </div>
    </div>
</div>

<!-- Supplier Modal -->
<div id="supplierModal" class="modal">
    <div class="modal-content">
        <div class="modal-header">
            <h3 id="supplierModalTitle">Add Supplier</h3>
            <span class="close">&times;</span>
        </div>
        <div class="modal-body">
            <form id="supplierForm">
                <input type="hidden" id="supplierId">
                <div class="form-row">
                    <div class="form-group">
                        <label for="supplierCompany">Company Name</label>
                        <input type="text" id="supplierCompany" class="form-control" required>
                    </div>
                    <div class="form-group">
                        <label for="supplierContact">Contact Person</label>
                        <input type="text" id="supplierContact" class="form-control" required>
                    </div>
                </div>
                <div class="form-row">
                    <div class="form-group">
                        <label for="supplierPhone">Phone</label>
                        <input type="text" id="supplierPhone" class="form-control" required>
                    </div>
                    <div class="form-group">
                        <label for="supplierEmail">Email</label>
                        <input type="email" id="supplierEmail" class="form-control">
                    </div>
                </div>
                <div class="form-row">
                    <div class="form-group">
                        <label for="supplierAddress">Address</label>
                        <textarea id="supplierAddress" class="form-control" rows="3"></textarea>
                    </div>
                </div>
                <div class="form-row">
                    <div class="form-group">
                        <label for="supplierGST">GST Number</label>
                        <input type="text" id="supplierGST" class="form-control">
                    </div>
                    <div class="form-group">
                        <label for="supplierStatus">Status</label>
                        <select id="supplierStatus" class="form-control">
                            <option value="active">Active</option>
                            <option value="inactive">Inactive</option>
                        </select>
                    </div>
                </div>
            </form>
        </div>
        <div class="modal-footer">
            <button class="btn btn-outline close">Cancel</button>
            <button id="saveSupplierBtn" class="btn btn-primary">Save Supplier</button>
        </div>
    </div>
</div>

<div id="purchaseOrderModal" class="modal">
    <div class="modal-content">
        <div class="modal-header">
            <h3 id="purchaseOrderModalTitle">Create Purchase Order</h3>
            <span class="close">&times;</span>
        </div>
        <div class="modal-body">
            <form id="purchaseOrderForm">
                <input type="hidden" id="purchaseOrderId">
                <div class="form-row">
                    <div class="form-group">
                        <label for="poCode">PO Code</label>
                        <input type="text" id="poCode" class="form-control" required>
                    </div>
                    <div class="form-group">
                        <label for="poSupplier">Supplier</label>
                        <select id="poSupplier" class="form-control" required>
                            <option value="">Select Supplier</option>
                        </select>
                    </div>
                </div>
                <div class="form-row">
                    <div class="form-group">
                        <label for="poDate">Date</label>
                        <input type="date" id="poDate" class="form-control" required>
                    </div>
                    <div class="form-group">
                        <label for="poProduct">Product</label>
                        <select id="poProduct" class="form-control" required>
                            <option value="">Select Product</option>
                        </select>
                    </div>
                </div>
                <div class="form-row">
                    <div class="form-group">
                        <label for="poQuantity">Quantity</label>
                        <input type="number" id="poQuantity" class="form-control" min="1" required>
                    </div>
                    <div class="form-group">
                        <label for="poPrice">Price per Unit (PKR)</label>
                        <input type="number" id="poPrice" class="form-control" min="0" step="0.01" required>
                    </div>
                </div>
                <div class="form-row">
                    <div class="form-group">
                        <label for="poDiscount">Discount (PKR)</label>
                        <input type="number" id="poDiscount" class="form-control" min="0" value="0">
                    </div>
                    <div class="form-group">
                        <label for="poTotal">Total Amount (PKR)</label>
                        <input type="number" id="poTotal" class="form-control" readonly>
                    </div>
                </div>
            </form>
        </div>
        <div class="modal-footer">
            <button class="btn btn-outline close">Cancel</button>
            <button id="savePurchaseOrderBtn" class="btn btn-primary">Save Purchase Order</button>
        </div>
    </div>
</div>

<div id="billModal" class="modal">
    <div class="modal-content">
        <div class="modal-header">
            <h3 id="billModalTitle">Create Bill</h3>
            <span class="close">&times;</span>
        </div>
        <div class="modal-body">
            <form id="billForm">
                <input type="hidden" id="billId">
                <div class="form-row">
                    <div class="form-group">
                        <label for="billCustomer">Customer</label>
                        <select id="billCustomer" class="form-control">
                            <option value="">Select Customer</option>
                        </select>
                    </div>
                    <div class="form-group">
                        <label for="billSalesman">Salesman</label>
                        <select id="billSalesman" class="form-control">
                            <option value="">Select Salesman</option>
                        </select>
                    </div>
                </div>
                <div class="form-row">
                    <div class="form-group">
                        <label for="billDate">Date</label>
                        <input type="date" id="billDate" class="form-control">
                    </div>
                </div>
                <div class="form-group">
                    <label>Add Products</label>
                    <div class="table-responsive">
                        <table>
                            <thead>
                                <tr>
                                    <th>Product</th>
                                    <th>Price</th>
                                    <th>Quantity</th>
                                    <th>Total</th>
                                    <th>Action</th>
                                </tr>
                            </thead>
                            <tbody id="billItemsTable">
                                <!-- Bill items will be populated here -->
                            </tbody>
                        </table>
                    </div>
                    <div class="form-group" style="margin-top: 10px;">
                        <select id="billProductSelect" class="form-control">
                            <option value="">Select Product to Add</option>
                        </select>
                    </div>
                </div>
                <div class="form-group">
                    <label>Payment Method</label>
                    <div class="payment-options">
                        <div class="payment-option active" data-payment="cash">
                            <i class="fas fa-money-bill-wave"></i>
                            <div>Cash</div>
                        </div>
                        <div class="payment-option" data-payment="card">
                            <i class="fas fa-credit-card"></i>
                            <div>Card</div>
                        </div>
                        <div class="payment-option" data-payment="upi">
                            <i class="fas fa-mobile-alt"></i>
                            <div>UPI</div>
                        </div>
                        <div class="payment-option" data-payment="credit">
                            <i class="fas fa-book"></i>
                            <div>Credit</div>
                        </div>
                    </div>
                </div>
                <div class="credit-option" id="creditOption" style="display: none;">
                    <div class="form-group">
                        <label for="creditTerms">Credit Terms (Days)</label>
                        <input type="number" id="creditTerms" class="form-control" value="30" min="1">
                    </div>
                    <div class="form-group">
                        <label for="creditDueDate">Due Date</label>
                        <input type="date" id="creditDueDate" class="form-control">
                    </div>
                </div>
                <div class="form-row">
                    <div class="form-group">
                        <label for="billSubtotal">Subtotal (PKR)</label>
                        <input type="number" id="billSubtotal" class="form-control" readonly>
                    </div>
                    <div class="form-group">
                        <label for="billTax">Tax (PKR)</label>
                        <input type="number" id="billTax" class="form-control" readonly>
                    </div>
                </div>
                <div class="form-row">
                    <div class="form-group">
                        <label for="billDiscount">Discount (PKR)</label>
                        <input type="number" id="billDiscount" class="form-control" value="0" min="0">
                    </div>
                    <div class="form-group">
                        <label for="billGrandTotal">Grand Total (PKR)</label>
                        <input type="number" id="billGrandTotal" class="form-control" readonly>
                    </div>
                </div>
            </form>
        </div>
        <div class="modal-footer">
            <button class="btn btn-outline close">Cancel</button>
            <button id="saveBillBtn" class="btn btn-primary">Save Bill</button>
            <button id="printBillBtn" class="btn btn-success">Print Bill</button>
        </div>
    </div>
</div>

<!-- View Bill Modal -->
<div id="viewBillModal" class="modal">
    <div class="modal-content">
        <div class="modal-header">
            <h3 id="viewBillModalTitle">Bill Details</h3>
            <span class="close">&times;</span>
        </div>
        <div class="modal-body">
            <div id="billDetailsContent">
                <!-- Bill details will be populated here -->
            </div>
        </div>
        <div class="modal-footer">
            <button class="btn btn-outline close">Close</button>
            <button id="downloadBillBtn" class="btn btn-primary">Download</button>
            <button id="printViewBillBtn" class="btn btn-success">Print</button>
        </div>
    </div>
</div>

<!-- Firebase Configuration -->
<script>
    // Firebase configuration
    const firebaseConfig = {
        apiKey: "AIzaSyCwOAzbY1puo8PwnKQrmeYB5RZJFQGn1kQ",
        authDomain: "hardware-2f5af.firebaseapp.com",
        databaseURL: "https://hardware-2f5af-default-rtdb.firebaseio.com",
        projectId: "hardware-2f5af",
        storageBucket: "hardware-2f5af.firebasestorage.app",
        messagingSenderId: "73708108967",
        appId: "1:73708108967:web:f623d9ba85e049824d6f82",
        measurementId: "G-YL92FTP6VZ"
    };

    // Initialize Firebase
    firebase.initializeApp(firebaseConfig);
    const auth = firebase.auth();
    const db = firebase.database();
</script>

<!-- Application Script -->
<script>
    // Global variables
    let currentUser = null;
    let products = [];
    let customers = [];
    let areas = [];
    let salesmen = [];
    let suppliers = [];
    let supplierProducts = [];
    let khattaEntries = [];
    let purchaseOrders = [];
    let grns = [];
    let bills = [];
    let returns = [];
    let currentBillItems = [];
    let currentReturnItems = [];

    // DOM Elements
    const loginScreen = document.getElementById('loginScreen');
    const app = document.getElementById('app');
    const loginForm = document.getElementById('loginForm');
    const loginAlert = document.getElementById('loginAlert');
    const loginBtnText = document.getElementById('loginBtnText');
    const loginSpinner = document.getElementById('loginSpinner');
    const logoutBtn = document.getElementById('logoutBtn');
    const mobileMenuBtn = document.querySelector('.mobile-menu-btn');
    const sidebar = document.querySelector('.sidebar');
    const themeToggle = document.getElementById('themeToggle');
    const menuItems = document.querySelectorAll('.sidebar-menu a');
    const pageContents = document.querySelectorAll('.page-content');
    const quickActions = document.querySelectorAll('.quick-action-btn');
    const searchInputs = document.querySelectorAll('.search-bar input');
    const tabs = document.querySelectorAll('.tab');
    const tabContents = document.querySelectorAll('.tab-content');
    const modals = document.querySelectorAll('.modal');
    const closeButtons = document.querySelectorAll('.close');
    const toast = document.getElementById('toast');

    // Initialize the application
    document.addEventListener('DOMContentLoaded', function() {
        // Check if user is logged in
        auth.onAuthStateChanged(user => {
            if (user) {
                currentUser = user;
                loginScreen.style.display = 'none';
                app.style.display = 'block';
                loadInitialData();
            } else {
                loginScreen.style.display = 'flex';
                app.style.display = 'none';
            }
        });

        // Login form
        loginForm.addEventListener('submit', handleLogin);

        // Logout
        logoutBtn.addEventListener('click', handleLogout);

        // Mobile menu toggle
        mobileMenuBtn.addEventListener('click', () => {
            sidebar.classList.toggle('active');
        });

        // Theme toggle
        themeToggle.addEventListener('click', toggleTheme);

        // Menu navigation
        menuItems.forEach(item => {
            if (item.id !== 'logoutBtn') {
                item.addEventListener('click', function(e) {
                    e.preventDefault();
                    const page = this.getAttribute('data-page');
                    
                    // Update active menu item
                    menuItems.forEach(i => i.classList.remove('active'));
                    this.classList.add('active');
                    
                    // Update active page
                    pageContents.forEach(content => content.classList.remove('active'));
                    document.getElementById(page).classList.add('active');
                    
                    // Update page title
                    document.querySelector('.topbar-left h1').textContent = 
                        this.querySelector('.menu-text').textContent;
                });
            }
        });

        // Quick actions
        quickActions.forEach(action => {
            action.addEventListener('click', function() {
                const page = this.getAttribute('data-page');
                menuItems.forEach(i => i.classList.remove('active'));
                document.querySelector(`.sidebar-menu a[data-page="${page}"]`).classList.add('active');
                pageContents.forEach(content => content.classList.remove('active'));
                document.getElementById(page).classList.add('active');
                document.querySelector('.topbar-left h1').textContent = 
                    document.querySelector(`.sidebar-menu a[data-page="${page}"] .menu-text`).textContent;
            });
        });

        // Tabs
        tabs.forEach(tab => {
            tab.addEventListener('click', function() {
                const report = this.getAttribute('data-report');
                
                // Update active tab
                tabs.forEach(t => t.classList.remove('active'));
                this.classList.add('active');
                
                // Update active tab content
                tabContents.forEach(content => content.classList.remove('active'));
                document.getElementById(report + 'Report').classList.add('active');
            });
        });

        // Close modals
        closeButtons.forEach(button => {
            button.addEventListener('click', function() {
                this.closest('.modal').style.display = 'none';
            });
        });

        // Close modal when clicking outside
        window.addEventListener('click', function(e) {
            modals.forEach(modal => {
                if (e.target === modal) {
                    modal.style.display = 'none';
                }
            });
        });

        // Initialize demo data
        initializeDemoData();
        
        // Add event listeners for calculation fields
        document.getElementById('poQuantity').addEventListener('input', calculatePOTotal);
        document.getElementById('poPrice').addEventListener('input', calculatePOTotal);
        document.getElementById('poDiscount').addEventListener('input', calculatePOTotal);
        
        document.getElementById('billDiscount').addEventListener('input', calculateBillTotals);
        
        // Add event listeners for new features
        document.getElementById('searchPOForGRN').addEventListener('click', searchPOForGRN);
        document.getElementById('submitGRNBtn').addEventListener('click', submitGRN);
        document.getElementById('searchSBForReturn').addEventListener('click', searchSBForReturn);
        document.getElementById('submitReturnBtn').addEventListener('click', submitReturn);
        
        // Add event listeners for supplier products toggle
        document.querySelectorAll('.show-products-btn').forEach(btn => {
            btn.addEventListener('click', function() {
                const supplierId = this.getAttribute('data-id');
                toggleSupplierProducts(supplierId);
            });
        });
    });

    // Functions
    function handleLogin(e) {
        e.preventDefault();
        const email = document.getElementById('username').value;
        const password = document.getElementById('password').value;

        // Show loading state
        loginBtnText.textContent = 'Logging in...';
        loginSpinner.style.display = 'inline-block';
        loginAlert.style.display = 'none';

        // Simple authentication (replace with Firebase auth in production)
        setTimeout(() => {
            if (email === 'admin@qaimkhani.com' && password === 'password123') {
                // Simulate successful login
                currentUser = { uid: 'demo-user', email: email };
                loginScreen.style.display = 'none';
                app.style.display = 'block';
                loadInitialData();
                showToast('Login successful!', 'success');
            } else {
                // Show error
                loginAlert.style.display = 'block';
            }
            
            // Reset login button
            loginBtnText.textContent = 'Login';
            loginSpinner.style.display = 'none';
        }, 1000);
    }

    function handleLogout() {
        auth.signOut().then(() => {
            currentUser = null;
            loginScreen.style.display = 'flex';
            app.style.display = 'none';
            showToast('Logged out successfully!', 'success');
        });
    }

    function toggleTheme() {
        document.body.classList.toggle('dark-mode');
        const icon = themeToggle.querySelector('i');
        if (document.body.classList.contains('dark-mode')) {
            icon.classList.remove('fa-moon');
            icon.classList.add('fa-sun');
        } else {
            icon.classList.remove('fa-sun');
            icon.classList.add('fa-moon');
        }
    }

    function loadInitialData() {
        // Load products
        db.ref('products').on('value', snapshot => {
            products = [];
            snapshot.forEach(childSnapshot => {
                products.push({
                    id: childSnapshot.key,
                    ...childSnapshot.val()
                });
            });
            renderProductsTable();
            updateDashboardStats();
            populateProductSelects();
        });

        // Load customers
        db.ref('customers').on('value', snapshot => {
            customers = [];
            snapshot.forEach(childSnapshot => {
                customers.push({
                    id: childSnapshot.key,
                    ...childSnapshot.val()
                });
            });
            renderCustomersTable();
            updateDashboardStats();
            populateCustomerSelects();
        });

        // Load areas
        db.ref('areas').on('value', snapshot => {
            areas = [];
            snapshot.forEach(childSnapshot => {
                areas.push({
                    id: childSnapshot.key,
                    ...childSnapshot.val()
                });
            });
            renderAreasTable();
            populateAreaSelects();
        });

        // Load salesmen
        db.ref('salesmen').on('value', snapshot => {
            salesmen = [];
            snapshot.forEach(childSnapshot => {
                salesmen.push({
                    id: childSnapshot.key,
                    ...childSnapshot.val()
                });
            });
            renderSalesmenTable();
            populateSalesmanSelects();
        });

        // Load suppliers
        db.ref('suppliers').on('value', snapshot => {
            suppliers = [];
            snapshot.forEach(childSnapshot => {
                suppliers.push({
                    id: childSnapshot.key,
                    ...childSnapshot.val()
                });
            });
            renderSuppliersTable();
            populateSupplierSelects();
            updateSupplierStats();
        });

        // Load khatta entries
        db.ref('khatta').on('value', snapshot => {
            khattaEntries = [];
            snapshot.forEach(childSnapshot => {
                khattaEntries.push({
                    id: childSnapshot.key,
                    ...childSnapshot.val()
                });
            });
            renderKhattaTable();
            updateDashboardStats();
            updateCustomerBalances();
        });

        // Load purchase orders
        db.ref('purchaseOrders').on('value', snapshot => {
            purchaseOrders = [];
            snapshot.forEach(childSnapshot => {
                purchaseOrders.push({
                    id: childSnapshot.key,
                    ...childSnapshot.val()
                });
            });
            renderPurchaseOrdersTable();
            updateDashboardStats();
            updateSupplierStats();
        });

        // Load GRNs
        db.ref('grns').on('value', snapshot => {
            grns = [];
            snapshot.forEach(childSnapshot => {
                grns.push({
                    id: childSnapshot.key,
                    ...childSnapshot.val()
                });
            });
            renderGrnTable();
        });

        // Load bills
        db.ref('bills').on('value', snapshot => {
            bills = [];
            snapshot.forEach(childSnapshot => {
                bills.push({
                    id: childSnapshot.key,
                    ...childSnapshot.val()
                });
            });
            renderBillsTable();
            updateDashboardStats();
        });

        // Load returns
        db.ref('returns').on('value', snapshot => {
            returns = [];
            snapshot.forEach(childSnapshot => {
                returns.push({
                    id: childSnapshot.key,
                    ...childSnapshot.val()
                });
            });
            renderReturnsTable();
        });

        // Initialize charts
        initializeCharts();
    }

    function initializeDemoData() {
        // Check if demo data already exists
        db.ref('products').once('value').then(snapshot => {
            if (!snapshot.exists() || snapshot.numChildren() === 0) {
                // Add demo products
                const demoProducts = [
                    { name: 'Blue Pen', category: 'Stationery', price: 10, cost: 5, stock: 100, minStock: 20, description: 'Blue ballpoint pen' },
                    { name: 'Notebook', category: 'Stationery', price: 50, cost: 30, stock: 50, minStock: 10, description: 'A4 size notebook' },
                    { name: 'Hammer', category: 'Hardware', price: 250, cost: 150, stock: 15, minStock: 5, description: 'Steel hammer' },
                    { name: 'Screwdriver Set', category: 'Tools', price: 300, cost: 180, stock: 20, minStock: 5, description: 'Set of 5 screwdrivers' },
                    { name: 'LED Bulb', category: 'Electronics', price: 120, cost: 80, stock: 30, minStock: 10, description: '10W LED bulb' }
                ];

                demoProducts.forEach(product => {
                    db.ref('products').push(product);
                });

                // Add demo customers
                const demoCustomers = [
                    { name: 'Ali Ahmed', phone: '03001234567', email: 'ali@example.com', type: 'Regular', area: 'Gulshan', salesman: 'Saleem', address: 'House 123, Gulshan', balance: 0 },
                    { name: 'Fatima Khan', phone: '03111234567', email: 'fatima@example.com', type: 'VIP', area: 'Clifton', salesman: 'Rashid', address: 'Flat 45, Clifton', balance: 0 },
                    { name: 'Bilal Siddiqui', phone: '03211234567', email: 'bilal@example.com', type: 'Wholesale', area: 'North Nazimabad', salesman: 'Saleem', address: 'Shop 12, North Nazimabad', balance: 0 }
                ];

                demoCustomers.forEach(customer => {
                    db.ref('customers').push(customer);
                });

                // Add demo areas
                const demoAreas = [
                    { name: 'Gulshan', description: 'Gulshan-e-Iqbal area' },
                    { name: 'Clifton', description: 'Clifton and Defence area' },
                    { name: 'North Nazimabad', description: 'North Nazimabad and surrounding areas' }
                ];

                demoAreas.forEach(area => {
                    db.ref('areas').push(area);
                });

                // Add demo salesmen
                const demoSalesmen = [
                    { name: 'Saleem Ahmed', phone: '03331234567', email: 'saleem@example.com', area: 'Gulshan', address: 'House 45, Gulshan' },
                    { name: 'Rashid Khan', phone: '03441234567', email: 'rashid@example.com', area: 'Clifton', address: 'Flat 23, Clifton' }
                ];

                demoSalesmen.forEach(salesman => {
                    db.ref('salesmen').push(salesman);
                });

                // Add demo suppliers
                const demoSuppliers = [
                    { companyName: 'Stationery Wholesalers', contactPerson: 'Mr. Khan', phone: '02134567890', email: 'info@stationery.com', address: 'Wholesale Market, Karachi', gstNumber: 'GST123456', status: 'active' },
                    { companyName: 'Hardware Distributors', contactPerson: 'Mr. Ahmed', phone: '02134567891', email: 'sales@hardware.com', address: 'Saddar, Karachi', gstNumber: 'GST123457', status: 'active' }
                ];

                demoSuppliers.forEach(supplier => {
                    db.ref('suppliers').push(supplier);
                });

                showToast('Demo data initialized!', 'success');
            }
        });
    }

    function renderProductsTable() {
        const tableBody = document.getElementById('productsTable');
        tableBody.innerHTML = '';

        if (products.length === 0) {
            tableBody.innerHTML = `
                <tr>
                    <td colspan="7" class="empty-state">
                        <i class="fas fa-box-open"></i>
                        <h3>No Products Found</h3>
                        <p>Add your first product to get started</p>
                    </td>
                </tr>
            `;
            return;
        }

        products.forEach(product => {
            const row = document.createElement('tr');
            const stockStatus = product.stock === 0 ? 'Out of Stock' : 
                              product.stock <= product.minStock ? 'Low Stock' : 'In Stock';
            const statusClass = product.stock === 0 ? 'badge-danger' : 
                              product.stock <= product.minStock ? 'badge-warning' : 'badge-success';
            
            row.innerHTML = `
                <td>${product.id.substring(0, 8)}</td>
                <td>${product.name}</td>
                <td>${product.category}</td>
                <td>PKR ${product.price}</td>
                <td class="${product.stock <= product.minStock ? 'low-stock' : ''}">${product.stock}</td>
                <td><span class="badge ${statusClass}">${stockStatus}</span></td>
                <td>
                    <div class="action-buttons">
                        <button class="btn btn-sm btn-outline edit-product" data-id="${product.id}">
                            <i class="fas fa-edit"></i>
                        </button>
                        <button class="btn btn-sm btn-danger delete-product" data-id="${product.id}">
                            <i class="fas fa-trash"></i>
                        </button>
                    </div>
                </td>
            `;
            tableBody.appendChild(row);
        });

        // Add event listeners for edit and delete buttons
        document.querySelectorAll('.edit-product').forEach(button => {
            button.addEventListener('click', function() {
                const productId = this.getAttribute('data-id');
                openProductModal(productId);
            });
        });

        document.querySelectorAll('.delete-product').forEach(button => {
            button.addEventListener('click', function() {
                const productId = this.getAttribute('data-id');
                deleteProduct(productId);
            });
        });
    }

    function renderCustomersTable() {
        const tableBody = document.getElementById('customersTable');
        tableBody.innerHTML = '';

        if (customers.length === 0) {
            tableBody.innerHTML = `
                <tr>
                    <td colspan="7" class="empty-state">
                        <i class="fas fa-users"></i>
                        <h3>No Customers Found</h3>
                        <p>Add your first customer to get started</p>
                    </td>
                </tr>
            `;
            return;
        }

        customers.forEach(customer => {
            const row = document.createElement('tr');
            row.innerHTML = `
                <td>${customer.id.substring(0, 8)}</td>
                <td>${customer.name}</td>
                <td>${customer.phone}</td>
                <td>${customer.area || '-'}</td>
                <td>${customer.salesman || '-'}</td>
                <td>PKR ${customer.balance || 0}</td>
                <td>
                    <div class="action-buttons">
                        <button class="btn btn-sm btn-outline edit-customer" data-id="${customer.id}">
                            <i class="fas fa-edit"></i>
                        </button>
                        <button class="btn btn-sm btn-danger delete-customer" data-id="${customer.id}">
                            <i class="fas fa-trash"></i>
                        </button>
                    </div>
                </td>
            `;
            tableBody.appendChild(row);
        });

        // Add event listeners for edit and delete buttons
        document.querySelectorAll('.edit-customer').forEach(button => {
            button.addEventListener('click', function() {
                const customerId = this.getAttribute('data-id');
                openCustomerModal(customerId);
            });
        });

        document.querySelectorAll('.delete-customer').forEach(button => {
            button.addEventListener('click', function() {
                const customerId = this.getAttribute('data-id');
                deleteCustomer(customerId);
            });
        });
    }

    function renderAreasTable() {
        const tableBody = document.getElementById('areasTable');
        tableBody.innerHTML = '';

        if (areas.length === 0) {
            tableBody.innerHTML = `
                <tr>
                    <td colspan="5" class="empty-state">
                        <i class="fas fa-map-marker-alt"></i>
                        <h3>No Areas Found</h3>
                        <p>Add your first area to get started</p>
                    </td>
                </tr>
            `;
            return;
        }

        areas.forEach(area => {
            const customerCount = customers.filter(c => c.area === area.name).length;
            const row = document.createElement('tr');
            row.innerHTML = `
                <td>${area.id.substring(0, 8)}</td>
                <td>${area.name}</td>
                <td>${area.description || '-'}</td>
                <td>${customerCount}</td>
                <td>
                    <div class="action-buttons">
                        <button class="btn btn-sm btn-outline edit-area" data-id="${area.id}">
                            <i class="fas fa-edit"></i>
                        </button>
                        <button class="btn btn-sm btn-danger delete-area" data-id="${area.id}">
                            <i class="fas fa-trash"></i>
                        </button>
                    </div>
                </td>
            `;
            tableBody.appendChild(row);
        });

        // Add event listeners for edit and delete buttons
        document.querySelectorAll('.edit-area').forEach(button => {
            button.addEventListener('click', function() {
                const areaId = this.getAttribute('data-id');
                openAreaModal(areaId);
            });
        });

        document.querySelectorAll('.delete-area').forEach(button => {
            button.addEventListener('click', function() {
                const areaId = this.getAttribute('data-id');
                deleteArea(areaId);
            });
        });
    }

    function renderSalesmenTable() {
        const tableBody = document.getElementById('salesmenTable');
        tableBody.innerHTML = '';

        if (salesmen.length === 0) {
            tableBody.innerHTML = `
                <tr>
                    <td colspan="7" class="empty-state">
                        <i class="fas fa-user-tie"></i>
                        <h3>No Salesmen Found</h3>
                        <p>Add your first salesman to get started</p>
                    </td>
                </tr>
            `;
            return;
        }

        salesmen.forEach(salesman => {
            const customerCount = customers.filter(c => c.salesman === salesman.name).length;
            const row = document.createElement('tr');
            row.innerHTML = `
                <td>${salesman.id.substring(0, 8)}</td>
                <td>${salesman.name}</td>
                <td>${salesman.phone}</td>
                <td>${salesman.email || '-'}</td>
                <td>${salesman.area || '-'}</td>
                <td><span class="badge badge-success">Active</span></td>
                <td>
                    <div class="action-buttons">
                        <button class="btn btn-sm btn-outline edit-salesman" data-id="${salesman.id}">
                            <i class="fas fa-edit"></i>
                        </button>
                        <button class="btn btn-sm btn-danger delete-salesman" data-id="${salesman.id}">
                            <i class="fas fa-trash"></i>
                        </button>
                    </div>
                </td>
            `;
            tableBody.appendChild(row);
        });

        // Add event listeners for edit and delete buttons
        document.querySelectorAll('.edit-salesman').forEach(button => {
            button.addEventListener('click', function() {
                const salesmanId = this.getAttribute('data-id');
                openSalesmanModal(salesmanId);
            });
        });

        document.querySelectorAll('.delete-salesman').forEach(button => {
            button.addEventListener('click', function() {
                const salesmanId = this.getAttribute('data-id');
                deleteSalesman(salesmanId);
            });
        });
    }

    function renderSuppliersTable() {
        const tableBody = document.getElementById('suppliersTable');
        tableBody.innerHTML = '';

        if (suppliers.length === 0) {
            tableBody.innerHTML = `
                <tr>
                    <td colspan="8" class="empty-state">
                        <i class="fas fa-truck"></i>
                        <h3>No Suppliers Found</h3>
                        <p>Add your first supplier to get started</p>
                    </td>
                </tr>
            `;
            return;
        }

        suppliers.forEach(supplier => {
            const productCount = products.filter(p => p.supplier === supplier.companyName).length;
            const row = document.createElement('tr');
            row.innerHTML = `
                <td>${supplier.id.substring(0, 8)}</td>
                <td>${supplier.companyName}</td>
                <td>${supplier.contactPerson}</td>
                <td>${supplier.phone}</td>
                <td>${supplier.email || '-'}</td>
                <td>${productCount}</td>
                <td><span class="badge ${supplier.status === 'active' ? 'badge-success' : 'badge-danger'}">${supplier.status}</span></td>
                <td>
                    <div class="action-buttons">
                        <button class="btn btn-sm btn-outline edit-supplier" data-id="${supplier.id}">
                            <i class="fas fa-edit"></i>
                        </button>
                        <button class="btn btn-sm btn-danger delete-supplier" data-id="${supplier.id}">
                            <i class="fas fa-trash"></i>
                        </button>
                        <button class="btn btn-sm btn-primary show-products-btn" data-id="${supplier.id}">
                            <i class="fas fa-eye"></i> Products
                        </button>
                    </div>
                </td>
            `;
            tableBody.appendChild(row);
        });

        // Add event listeners for edit and delete buttons
        document.querySelectorAll('.edit-supplier').forEach(button => {
            button.addEventListener('click', function() {
                const supplierId = this.getAttribute('data-id');
                openSupplierModal(supplierId);
            });
        });

        document.querySelectorAll('.delete-supplier').forEach(button => {
            button.addEventListener('click', function() {
                const supplierId = this.getAttribute('data-id');
                deleteSupplier(supplierId);
            });
        });

        // Add event listeners for show products buttons
        document.querySelectorAll('.show-products-btn').forEach(button => {
            button.addEventListener('click', function() {
                const supplierId = this.getAttribute('data-id');
                toggleSupplierProducts(supplierId);
            });
        });
    }

    function toggleSupplierProducts(supplierId) {
        const supplier = suppliers.find(s => s.id === supplierId);
        if (!supplier) return;

        const supplierProductsSection = document.getElementById('supplierProductsSection');
        const supplierProductsTable = document.getElementById('supplierProductsTable');
        
        // Check if already showing products for this supplier
        if (supplierProductsSection.style.display === 'block' && 
            supplierProductsSection.getAttribute('data-supplier-id') === supplierId) {
            // Hide products
            supplierProductsSection.style.display = 'none';
            return;
        }

        // Show products for this supplier
        const supplierProducts = products.filter(p => p.supplier === supplier.companyName);
        
        supplierProductsTable.innerHTML = '';
        
        if (supplierProducts.length === 0) {
            supplierProductsTable.innerHTML = `
                <tr>
                    <td colspan="5" class="empty-state">
                        <i class="fas fa-boxes"></i>
                        <h3>No Products Found</h3>
                        <p>No products associated with this supplier</p>
                    </td>
                </tr>
            `;
        } else {
            supplierProducts.forEach(product => {
                const row = document.createElement('tr');
                row.innerHTML = `
                    <td>${product.name}</td>
                    <td>${product.category}</td>
                    <td>PKR ${product.cost}</td>
                    <td class="${product.stock <= product.minStock ? 'low-stock' : ''}">${product.stock}</td>
                    <td><span class="badge ${product.stock === 0 ? 'badge-danger' : product.stock <= product.minStock ? 'badge-warning' : 'badge-success'}">${product.stock === 0 ? 'Out of Stock' : product.stock <= product.minStock ? 'Low Stock' : 'In Stock'}</span></td>
                `;
                supplierProductsTable.appendChild(row);
            });
        }
        
        supplierProductsSection.style.display = 'block';
        supplierProductsSection.setAttribute('data-supplier-id', supplierId);
    }

    function renderKhattaTable() {
        const tableBody = document.getElementById('khattaTable');
        tableBody.innerHTML = '';

        if (khattaEntries.length === 0) {
            tableBody.innerHTML = `
                <tr>
                    <td colspan="7" class="empty-state">
                        <i class="fas fa-book"></i>
                        <h3>No Khatta Entries</h3>
                        <p>Add your first khatta entry to get started</p>
                    </td>
                </tr>
            `;
            return;
        }

        // Calculate running balances for each customer
        const customerBalances = {};
        khattaEntries.forEach(entry => {
            if (!customerBalances[entry.customerId]) {
                customerBalances[entry.customerId] = 0;
            }
            
            if (entry.type === 'credit') {
                customerBalances[entry.customerId] += parseFloat(entry.amount);
            } else if (entry.type === 'debit') {
                customerBalances[entry.customerId] -= parseFloat(entry.amount);
            }
        });

        khattaEntries.forEach(entry => {
            const row = document.createElement('tr');
            const creditAmount = entry.type === 'credit' ? entry.amount : 0;
            const debitAmount = entry.type === 'debit' ? entry.amount : 0;
            
            row.innerHTML = `
                <td>${entry.date}</td>
                <td>${entry.customerName}</td>
                <td>${entry.description}</td>
                <td class="credit">${creditAmount > 0 ? 'PKR ' + creditAmount : ''}</td>
                <td class="debit">${debitAmount > 0 ? 'PKR ' + debitAmount : ''}</td>
                <td>PKR ${customerBalances[entry.customerId] || 0}</td>
                <td>
                    <div class="action-buttons">
                        <button class="btn btn-sm btn-outline edit-khatta" data-id="${entry.id}">
                            <i class="fas fa-edit"></i>
                        </button>
                    </div>
                </td>
            `;
            tableBody.appendChild(row);
        });

        // Add event listeners for edit buttons
        document.querySelectorAll('.edit-khatta').forEach(button => {
            button.addEventListener('click', function() {
                const khattaId = this.getAttribute('data-id');
                openKhattaModal(khattaId);
            });
        });
    }

    function renderPurchaseOrdersTable() {
        const tableBody = document.getElementById('purchaseOrdersTable');
        tableBody.innerHTML = '';

        if (purchaseOrders.length === 0) {
            tableBody.innerHTML = `
                <tr>
                    <td colspan="6" class="empty-state">
                        <i class="fas fa-clipboard-list"></i>
                        <h3>No Purchase Orders</h3>
                        <p>Create your first purchase order to get started</p>
                    </td>
                </tr>
            `;
            return;
        }

        purchaseOrders.forEach(po => {
            const grnStatus = grns.find(g => g.poCode === po.poCode) ? 'Yes' : 'No';
            const row = document.createElement('tr');
            row.innerHTML = `
                <td>${po.poCode}</td>
                <td>${po.supplierName}</td>
                <td>${po.date}</td>
                <td>PKR ${po.totalAmount}</td>
                <td><span class="badge ${grnStatus === 'Yes' ? 'badge-success' : 'badge-warning'}">${grnStatus}</span></td>
                <td>
                    <div class="action-buttons">
                        <button class="btn btn-sm btn-outline edit-purchase-order" data-id="${po.id}">
                            <i class="fas fa-edit"></i>
                        </button>
                        <button class="btn btn-sm btn-danger delete-purchase-order" data-id="${po.id}">
                            <i class="fas fa-trash"></i>
                        </button>
                    </div>
                </td>
            `;
            tableBody.appendChild(row);
        });

        // Add event listeners for edit and delete buttons
        document.querySelectorAll('.edit-purchase-order').forEach(button => {
            button.addEventListener('click', function() {
                const poId = this.getAttribute('data-id');
                openPurchaseOrderModal(poId);
            });
        });

        document.querySelectorAll('.delete-purchase-order').forEach(button => {
            button.addEventListener('click', function() {
                const poId = this.getAttribute('data-id');
                deletePurchaseOrder(poId);
            });
        });
    }

    function renderGrnTable() {
        const tableBody = document.getElementById('grnTable');
        tableBody.innerHTML = '';

        if (grns.length === 0) {
            tableBody.innerHTML = `
                <tr>
                    <td colspan="7" class="empty-state">
                        <i class="fas fa-clipboard-check"></i>
                        <h3>No GRN Records</h3>
                        <p>Create your first GRN to get started</p>
                    </td>
                </tr>
            `;
            return;
        }

        grns.forEach(grn => {
            const row = document.createElement('tr');
            row.innerHTML = `
                <td>${grn.date}</td>
                <td>${grn.poCode}</td>
                <td>${grn.productName}</td>
                <td>${grn.supplierName}</td>
                <td>${grn.quantity}</td>
                <td>PKR ${grn.totalAmount}</td>
                <td>
                    <div class="action-buttons">
                        <button class="btn btn-sm btn-outline view-grn" data-id="${grn.id}">
                            <i class="fas fa-eye"></i>
                        </button>
                    </div>
                </td>
            `;
            tableBody.appendChild(row);
        });

        // Add event listeners for view buttons
        document.querySelectorAll('.view-grn').forEach(button => {
            button.addEventListener('click', function() {
                const grnId = this.getAttribute('data-id');
                viewGrn(grnId);
            });
        });
    }

    function renderBillsTable() {
        const tableBody = document.getElementById('billsTable');
        tableBody.innerHTML = '';

        // Show all bills including returned ones
        const allBills = [...bills];

        if (allBills.length === 0) {
            tableBody.innerHTML = `
                <tr>
                    <td colspan="9" class="empty-state">
                        <i class="fas fa-receipt"></i>
                        <h3>No Bills Found</h3>
                        <p>Create your first bill to get started</p>
                    </td>
                </tr>
            `;
            return;
        }

        allBills.forEach(bill => {
            const statusBadge = bill.status === 'returned' ? 
                '<span class="badge badge-danger">Returned</span>' : 
                '<span class="badge badge-success">Active</span>';
            
            const rowClass = bill.status === 'returned' ? 'returned-bill' : '';
            
            const row = document.createElement('tr');
            row.className = rowClass;
            row.innerHTML = `
                <td>${bill.sbCode}</td>
                <td>${bill.date}</td>
                <td>${bill.customerName}</td>
                <td>${bill.salesmanName || '-'}</td>
                <td>${bill.items ? bill.items.length : 0} items</td>
                <td>PKR ${bill.grandTotal}</td>
                <td><span class="badge badge-info">${bill.paymentType || 'Cash'}</span></td>
                <td>${statusBadge}</td>
                <td>
                    <div class="action-buttons">
                        <button class="btn btn-sm btn-outline view-bill" data-id="${bill.id}">
                            <i class="fas fa-eye"></i>
                        </button>
                    </div>
                </td>
            `;
            tableBody.appendChild(row);
        });

        // Add event listeners for view buttons
        document.querySelectorAll('.view-bill').forEach(button => {
            button.addEventListener('click', function() {
                const billId = this.getAttribute('data-id');
                viewBill(billId);
            });
        });
    }

    function renderReturnsTable() {
        const tableBody = document.getElementById('returnsTable');
        tableBody.innerHTML = '';

        if (returns.length === 0) {
            tableBody.innerHTML = `
                <tr>
                    <td colspan="6" class="empty-state">
                        <i class="fas fa-undo-alt"></i>
                        <h3>No Return Records</h3>
                        <p>Add your first return to get started</p>
                    </td>
                </tr>
            `;
            return;
        }

        returns.forEach(returnItem => {
            const row = document.createElement('tr');
            row.innerHTML = `
                <td>${returnItem.returnDate}</td>
                <td>${returnItem.sbCode}</td>
                <td>${returnItem.customerName}</td>
                <td>${returnItem.items ? returnItem.items.length : 0} items</td>
                <td>PKR ${returnItem.totalAmount}</td>
                <td>
                    <div class="action-buttons">
                        <button class="btn btn-sm btn-outline view-return" data-id="${returnItem.id}">
                            <i class="fas fa-eye"></i>
                        </button>
                    </div>
                </td>
            `;
            tableBody.appendChild(row);
        });

        // Add event listeners for view buttons
        document.querySelectorAll('.view-return').forEach(button => {
            button.addEventListener('click', function() {
                const returnId = this.getAttribute('data-id');
                viewReturn(returnId);
            });
        });
    }

    function updateDashboardStats() {
        // Today's sales (only active bills)
        const today = new Date().toISOString().split('T')[0];
        const todaySales = bills
            .filter(bill => bill.date === today && bill.status !== 'returned')
            .reduce((sum, bill) => sum + bill.grandTotal, 0);
        document.getElementById('todaySales').textContent = `PKR ${todaySales}`;

        // Total customers
        document.getElementById('totalCustomers').textContent = customers.length;

        // Total products
        document.getElementById('totalProducts').textContent = products.length;

        // Low stock count
        const lowStockCount = products.filter(product => product.stock <= product.minStock).length;
        document.getElementById('lowStockCount').textContent = lowStockCount;

        // Pending orders
        const pendingOrders = purchaseOrders.filter(po => !grns.find(g => g.poCode === po.poCode)).length;
        document.getElementById('pendingOrders').textContent = pendingOrders;

        // Monthly sales (only active bills)
        const currentMonth = new Date().getMonth();
        const currentYear = new Date().getFullYear();
        const monthlySales = bills
            .filter(bill => {
                const billDate = new Date(bill.date);
                return billDate.getMonth() === currentMonth && 
                       billDate.getFullYear() === currentYear &&
                       bill.status !== 'returned';
            })
            .reduce((sum, bill) => sum + bill.grandTotal, 0);
        document.getElementById('monthlySales').textContent = `PKR ${monthlySales}`;

        // Update recent transactions (only active bills)
        updateRecentTransactions();

        // Update inventory alerts
        updateInventoryAlerts();

        // Update top products (only active bills)
        updateTopProducts();
    }

    function updateSupplierStats() {
        // Total suppliers
        document.getElementById('totalSuppliers').textContent = suppliers.length;

        // Active suppliers
        const activeSuppliers = suppliers.filter(s => s.status === 'active').length;
        document.getElementById('activeSuppliers').textContent = activeSuppliers;

        // Supplier products
        document.getElementById('supplierProducts').textContent = products.filter(p => p.supplier).length;

        // Pending POs
        const pendingPOs = purchaseOrders.filter(po => !grns.find(g => g.poCode === po.poCode)).length;
        document.getElementById('pendingPOs').textContent = pendingPOs;
    }

    function updateCustomerBalances() {
        // Update customer balances based on khatta entries
        customers.forEach(customer => {
            const customerKhatta = khattaEntries.filter(k => k.customerId === customer.id);
            let balance = 0;
            
            customerKhatta.forEach(entry => {
                if (entry.type === 'credit') {
                    balance += parseFloat(entry.amount);
                } else if (entry.type === 'debit') {
                    balance -= parseFloat(entry.amount);
                }
            });
            
            // Update customer balance in Firebase
            db.ref(`customers/${customer.id}/balance`).set(balance);
        });
    }

    function updateRecentTransactions() {
        const container = document.getElementById('recentTransactions');
        container.innerHTML = '';

        // Get recent active bills (last 5)
        const recentBills = bills
            .filter(bill => bill.status !== 'returned')
            .slice(-5)
            .reverse();
        
        if (recentBills.length === 0) {
            container.innerHTML = `
                <div class="empty-state">
                    <i class="fas fa-history"></i>
                    <h3>No Recent Transactions</h3>
                    <p>Your recent transactions will appear here</p>
                </div>
            `;
            return;
        }
        
        recentBills.forEach(bill => {
            const activityItem = document.createElement('div');
            activityItem.className = 'activity-item';
            activityItem.innerHTML = `
                <div class="activity-icon" style="background-color: var(--success);">
                    <i class="fas fa-shopping-cart"></i>
                </div>
                <div class="activity-content">
                    <div class="activity-title">Sale to ${bill.customerName}</div>
                    <div class="activity-time">${new Date(bill.date).toLocaleDateString()}</div>
                </div>
                <div class="activity-amount">PKR ${bill.grandTotal}</div>
            `;
            container.appendChild(activityItem);
        });
    }

    function updateInventoryAlerts() {
        const container = document.getElementById('inventoryAlerts');
        container.innerHTML = '';

        const lowStockProducts = products.filter(product => product.stock <= product.minStock);
        
        if (lowStockProducts.length === 0) {
            container.innerHTML = `
                <div class="empty-state">
                    <i class="fas fa-check-circle"></i>
                    <h3>No Alerts</h3>
                    <p>All products are well stocked</p>
                </div>
            `;
            return;
        }

        lowStockProducts.forEach(product => {
            const alertDiv = document.createElement('div');
            alertDiv.className = 'inventory-alert';
            alertDiv.innerHTML = `
                <strong>${product.name}</strong> - Only ${product.stock} items left (Reorder at ${product.minStock})
            `;
            container.appendChild(alertDiv);
        });
    }

    function updateTopProducts() {
        const container = document.getElementById('topProducts');
        container.innerHTML = '';

        // Calculate product sales from active bills only
        const productSales = {};
        bills
            .filter(bill => bill.status !== 'returned')
            .forEach(bill => {
                if (bill.items) {
                    bill.items.forEach(item => {
                        if (!productSales[item.productId]) {
                            productSales[item.productId] = 0;
                        }
                        productSales[item.productId] += item.quantity;
                    });
                }
            });

        // Sort products by sales
        const topProducts = Object.entries(productSales)
            .sort((a, b) => b[1] - a[1])
            .slice(0, 5);

        if (topProducts.length === 0) {
            container.innerHTML = `
                <div class="empty-state">
                    <i class="fas fa-chart-line"></i>
                    <h3>No Data</h3>
                    <p>Top selling products will appear here</p>
                </div>
            `;
            return;
        }

        topProducts.forEach(([productId, sales]) => {
            const product = products.find(p => p.id === productId);
            if (product) {
                const activityItem = document.createElement('div');
                activityItem.className = 'activity-item';
                activityItem.innerHTML = `
                    <div class="activity-icon" style="background-color: var(--primary);">
                        <i class="fas fa-box"></i>
                    </div>
                    <div class="activity-content">
                        <div class="activity-title">${product.name}</div>
                        <div class="activity-time">${product.category}</div>
                    </div>
                    <div class="activity-amount">${sales} sold</div>
                `;
                container.appendChild(activityItem);
            }
        });
    }

    function initializeCharts() {
        // Sales Chart
        const salesCtx = document.getElementById('salesChart').getContext('2d');
        const salesChart = new Chart(salesCtx, {
            type: 'line',
            data: {
                labels: ['Jan', 'Feb', 'Mar', 'Apr', 'May', 'Jun'],
                datasets: [{
                    label: 'Sales (PKR)',
                    data: [12000, 19000, 15000, 25000, 22000, 30000],
                    borderColor: '#1E88E5',
                    backgroundColor: 'rgba(30, 136, 229, 0.1)',
                    tension: 0.4,
                    fill: true
                }]
            },
            options: {
                responsive: true,
                maintainAspectRatio: false,
                plugins: {
                    legend: {
                        display: false
                    }
                },
                scales: {
                    y: {
                        beginAtZero: true,
                        grid: {
                            drawBorder: false
                        }
                    },
                    x: {
                        grid: {
                            display: false
                        }
                    }
                }
            }
        });

        // Sales Report Chart
        const salesReportCtx = document.getElementById('salesReportChart').getContext('2d');
        const salesReportChart = new Chart(salesReportCtx, {
            type: 'bar',
            data: {
                labels: ['Jan', 'Feb', 'Mar', 'Apr', 'May', 'Jun'],
                datasets: [{
                    label: 'Sales (PKR)',
                    data: [12000, 19000, 15000, 25000, 22000, 30000],
                    backgroundColor: '#1E88E5'
                }]
            },
            options: {
                responsive: true,
                maintainAspectRatio: false,
                scales: {
                    y: {
                        beginAtZero: true
                    }
                }
            }
        });

        // Purchase Report Chart
        const purchaseReportCtx = document.getElementById('purchaseReportChart').getContext('2d');
        const purchaseReportChart = new Chart(purchaseReportCtx, {
            type: 'bar',
            data: {
                labels: ['Jan', 'Feb', 'Mar', 'Apr', 'May', 'Jun'],
                datasets: [{
                    label: 'Purchase Orders (PKR)',
                    data: [8000, 12000, 10000, 15000, 13000, 18000],
                    backgroundColor: '#43A047'
                }]
            },
            options: {
                responsive: true,
                maintainAspectRatio: false,
                scales: {
                    y: {
                        beginAtZero: true
                    }
                }
            }
        });

        // Customer Report Chart
        const customerReportCtx = document.getElementById('customerReportChart').getContext('2d');
        const customerReportChart = new Chart(customerReportCtx, {
            type: 'pie',
            data: {
                labels: ['Regular', 'Wholesale', 'VIP'],
                datasets: [{
                    data: [60, 25, 15],
                    backgroundColor: [
                        '#1E88E5',
                        '#43A047',
                        '#FF9800'
                    ]
                }]
            },
            options: {
                responsive: true,
                maintainAspectRatio: false
            }
        });

        // Khatta Report Chart
        const khattaReportCtx = document.getElementById('khattaReportChart').getContext('2d');
        const khattaReportChart = new Chart(khattaReportCtx, {
            type: 'bar',
            data: {
                labels: ['Jan', 'Feb', 'Mar', 'Apr', 'May', 'Jun'],
                datasets: [
                    {
                        label: 'Credit',
                        data: [5000, 7000, 6000, 8000, 9000, 10000],
                        backgroundColor: '#43A047'
                    },
                    {
                        label: 'Debit',
                        data: [3000, 4000, 3500, 4500, 5000, 6000],
                        backgroundColor: '#E53935'
                    }
                ]
            },
            options: {
                responsive: true,
                maintainAspectRatio: false,
                scales: {
                    y: {
                        beginAtZero: true
                    }
                }
            }
        });

        // Supplier Report Chart
        const supplierReportCtx = document.getElementById('supplierReportChart').getContext('2d');
        const supplierReportChart = new Chart(supplierReportCtx, {
            type: 'bar',
            data: {
                labels: suppliers.slice(0, 5).map(s => s.companyName),
                datasets: [{
                    label: 'Products',
                    data: suppliers.slice(0, 5).map(s => 
                        products.filter(p => p.supplier === s.companyName).length
                    ),
                    backgroundColor: '#1E88E5'
                }]
            },
            options: {
                responsive: true,
                maintainAspectRatio: false,
                scales: {
                    y: {
                        beginAtZero: true
                    }
                }
            }
        });
    }

    function populateProductSelects() {
        const selects = document.querySelectorAll('#poProduct, #billProductSelect');
        selects.forEach(select => {
            select.innerHTML = '<option value="">Select Product</option>';
            products.forEach(product => {
                const option = document.createElement('option');
                option.value = product.id;
                option.textContent = product.name;
                select.appendChild(option);
            });
        });
    }

    function populateCustomerSelects() {
        const selects = document.querySelectorAll('#khattaCustomer, #billCustomer');
        selects.forEach(select => {
            select.innerHTML = '<option value="">Select Customer</option>';
            customers.forEach(customer => {
                const option = document.createElement('option');
                option.value = customer.id;
                option.textContent = customer.name;
                select.appendChild(option);
            });
        });
    }

    function populateAreaSelects() {
        const selects = document.querySelectorAll('#customerArea, #salesmanArea, #areaFilter');
        selects.forEach(select => {
            select.innerHTML = '<option value="">Select Area</option>';
            areas.forEach(area => {
                const option = document.createElement('option');
                option.value = area.name;
                option.textContent = area.name;
                select.appendChild(option);
            });
        });
    }

    function populateSalesmanSelects() {
        const selects = document.querySelectorAll('#customerSalesman, #salesmanFilter, #billSalesman');
        selects.forEach(select => {
            select.innerHTML = '<option value="">Select Salesman</option>';
            salesmen.forEach(salesman => {
                const option = document.createElement('option');
                option.value = salesman.id;
                option.textContent = salesman.name;
                select.appendChild(option);
            });
        });
    }

    function populateSupplierSelects() {
        const selects = document.querySelectorAll('#productSupplier, #poSupplier');
        selects.forEach(select => {
            select.innerHTML = '<option value="">Select Supplier</option>';
            suppliers.forEach(supplier => {
                const option = document.createElement('option');
                option.value = supplier.companyName;
                option.textContent = supplier.companyName;
                select.appendChild(option);
            });
        });
    }

    function openProductModal(productId = null) {
        const modal = document.getElementById('productModal');
        const title = document.getElementById('productModalTitle');
        const form = document.getElementById('productForm');
        
        form.reset();
        
        if (productId) {
            // Edit mode
            title.textContent = 'Edit Product';
            const product = products.find(p => p.id === productId);
            if (product) {
                document.getElementById('productId').value = product.id;
                document.getElementById('productName').value = product.name;
                document.getElementById('productCategory').value = product.category;
                document.getElementById('productPrice').value = product.price;
                document.getElementById('productCost').value = product.cost;
                document.getElementById('productStock').value = product.stock;
                document.getElementById('productMinStock').value = product.minStock;
                document.getElementById('productSupplier').value = product.supplier || '';
                document.getElementById('productBarcode').value = product.barcode || '';
                document.getElementById('productDescription').value = product.description || '';
            }
        } else {
            // Add mode
            title.textContent = 'Add Product';
            document.getElementById('productId').value = '';
        }
        
        modal.style.display = 'flex';
    }

    function openCustomerModal(customerId = null) {
        const modal = document.getElementById('customerModal');
        const title = document.getElementById('customerModalTitle');
        const form = document.getElementById('customerForm');
        
        form.reset();
        
        if (customerId) {
            // Edit mode
            title.textContent = 'Edit Customer';
            const customer = customers.find(c => c.id === customerId);
            if (customer) {
                document.getElementById('customerId').value = customer.id;
                document.getElementById('customerName').value = customer.name;
                document.getElementById('customerPhone').value = customer.phone;
                document.getElementById('customerEmail').value = customer.email || '';
                document.getElementById('customerType').value = customer.type || 'Regular';
                document.getElementById('customerArea').value = customer.area || '';
                document.getElementById('customerSalesman').value = customer.salesman || '';
                document.getElementById('customerAddress').value = customer.address || '';
            }
        } else {
            // Add mode
            title.textContent = 'Add Customer';
            document.getElementById('customerId').value = '';
        }
        
        modal.style.display = 'flex';
    }

    function openKhattaModal(khattaId = null) {
        const modal = document.getElementById('khattaModal');
        const title = document.getElementById('khattaModalTitle');
        const form = document.getElementById('khattaForm');
        
        form.reset();
        
        if (khattaId) {
            // Edit mode
            title.textContent = 'Edit Khatta Entry';
            const entry = khattaEntries.find(k => k.id === khattaId);
            if (entry) {
                document.getElementById('khattaId').value = entry.id;
                document.getElementById('khattaCustomer').value = entry.customerId;
                document.getElementById('khattaDate').value = entry.date;
                document.getElementById('khattaType').value = entry.type;
                document.getElementById('khattaAmount').value = entry.amount;
                document.getElementById('khattaDescription').value = entry.description;
            }
        } else {
            // Add mode
            title.textContent = 'Add Khatta Entry';
            document.getElementById('khattaDate').valueAsDate = new Date();
        }
        
        modal.style.display = 'flex';
    }

    function openAreaModal(areaId = null) {
        const modal = document.getElementById('areaModal');
        const title = document.getElementById('areaModalTitle');
        const form = document.getElementById('areaForm');
        
        form.reset();
        
        if (areaId) {
            // Edit mode
            title.textContent = 'Edit Area';
            const area = areas.find(a => a.id === areaId);
            if (area) {
                document.getElementById('areaId').value = area.id;
                document.getElementById('areaName').value = area.name;
                document.getElementById('areaDescription').value = area.description || '';
            }
        } else {
            // Add mode
            title.textContent = 'Add Area';
            document.getElementById('areaId').value = '';
        }
        
        modal.style.display = 'flex';
    }

    function openSalesmanModal(salesmanId = null) {
        const modal = document.getElementById('salesmanModal');
        const title = document.getElementById('salesmanModalTitle');
        const form = document.getElementById('salesmanForm');
        
        form.reset();
        
        if (salesmanId) {
            // Edit mode
            title.textContent = 'Edit Salesman';
            const salesman = salesmen.find(s => s.id === salesmanId);
            if (salesman) {
                document.getElementById('salesmanId').value = salesman.id;
                document.getElementById('salesmanName').value = salesman.name;
                document.getElementById('salesmanPhone').value = salesman.phone;
                document.getElementById('salesmanEmail').value = salesman.email || '';
                document.getElementById('salesmanArea').value = salesman.area || '';
                document.getElementById('salesmanAddress').value = salesman.address || '';
            }
        } else {
            // Add mode
            title.textContent = 'Add Salesman';
            document.getElementById('salesmanId').value = '';
        }
        
        modal.style.display = 'flex';
    }

    function openSupplierModal(supplierId = null) {
        const modal = document.getElementById('supplierModal');
        const title = document.getElementById('supplierModalTitle');
        const form = document.getElementById('supplierForm');
        
        form.reset();
        
        if (supplierId) {
            // Edit mode
            title.textContent = 'Edit Supplier';
            const supplier = suppliers.find(s => s.id === supplierId);
            if (supplier) {
                document.getElementById('supplierId').value = supplier.id;
                document.getElementById('supplierCompany').value = supplier.companyName;
                document.getElementById('supplierContact').value = supplier.contactPerson;
                document.getElementById('supplierPhone').value = supplier.phone;
                document.getElementById('supplierEmail').value = supplier.email || '';
                document.getElementById('supplierAddress').value = supplier.address || '';
                document.getElementById('supplierGST').value = supplier.gstNumber || '';
                document.getElementById('supplierStatus').value = supplier.status || 'active';
            }
        } else {
            // Add mode
            title.textContent = 'Add Supplier';
            document.getElementById('supplierId').value = '';
        }
        
        modal.style.display = 'flex';
    }

    function openPurchaseOrderModal(poId = null) {
        const modal = document.getElementById('purchaseOrderModal');
        const title = document.getElementById('purchaseOrderModalTitle');
        const form = document.getElementById('purchaseOrderForm');
        
        form.reset();
        
        if (poId) {
            // Edit mode
            title.textContent = 'Edit Purchase Order';
            const po = purchaseOrders.find(p => p.id === poId);
            if (po) {
                document.getElementById('purchaseOrderId').value = po.id;
                document.getElementById('poCode').value = po.poCode;
                document.getElementById('poSupplier').value = po.supplierId;
                document.getElementById('poDate').value = po.date;
                document.getElementById('poProduct').value = po.productId;
                document.getElementById('poQuantity').value = po.quantity;
                document.getElementById('poPrice').value = po.price;
                document.getElementById('poDiscount').value = po.discount || 0;
                document.getElementById('poTotal').value = po.totalAmount;
            }
        } else {
            // Add mode
            title.textContent = 'Create Purchase Order';
            document.getElementById('poDate').valueAsDate = new Date();
            calculatePOTotal();
        }
        
        modal.style.display = 'flex';
    }

    function openBillModal(billId = null) {
        const modal = document.getElementById('billModal');
        const title = document.getElementById('billModalTitle');
        const form = document.getElementById('billForm');
        
        form.reset();
        currentBillItems = [];
        
        if (billId) {
            // Edit mode
            title.textContent = 'Edit Bill';
            const bill = bills.find(b => b.id === billId);
            if (bill) {
                document.getElementById('billId').value = bill.id;
                document.getElementById('billCustomer').value = bill.customerId;
                document.getElementById('billSalesman').value = bill.salesmanId || '';
                document.getElementById('billDate').value = bill.date;
                currentBillItems = bill.items || [];
                renderBillItems();
                calculateBillTotals();
                
                // Set payment method
                document.querySelectorAll('.payment-option').forEach(option => {
                    option.classList.remove('active');
                    if (option.getAttribute('data-payment') === bill.paymentType) {
                        option.classList.add('active');
                    }
                });
                
                // Show credit options if payment is credit
                if (bill.paymentType === 'credit') {
                    document.getElementById('creditOption').style.display = 'block';
                    document.getElementById('creditTerms').value = bill.creditTerms || 30;
                    document.getElementById('creditDueDate').value = bill.creditDueDate || '';
                }
            }
        } else {
            // Add mode
            title.textContent = 'Create Bill';
            document.getElementById('billId').value = '';
            document.getElementById('billDate').valueAsDate = new Date();
            renderBillItems();
            calculateBillTotals();
        }
        
        modal.style.display = 'flex';
    }

    function saveProduct() {
        const productId = document.getElementById('productId').value;
        const productData = {
            name: document.getElementById('productName').value,
            category: document.getElementById('productCategory').value,
            price: parseFloat(document.getElementById('productPrice').value),
            cost: parseFloat(document.getElementById('productCost').value),
            stock: parseInt(document.getElementById('productStock').value),
            minStock: parseInt(document.getElementById('productMinStock').value),
            supplier: document.getElementById('productSupplier').value,
            barcode: document.getElementById('productBarcode').value,
            description: document.getElementById('productDescription').value,
            updatedAt: new Date().toISOString()
        };

        if (productId) {
            // Update existing product
            db.ref(`products/${productId}`).update(productData)
                .then(() => {
                    showToast('Product updated successfully!', 'success');
                    document.getElementById('productModal').style.display = 'none';
                })
                .catch(error => {
                    console.error('Error updating product:', error);
                    showToast('Error updating product. Please try again.', 'error');
                });
        } else {
            // Add new product
            productData.createdAt = new Date().toISOString();
            db.ref('products').push(productData)
                .then(() => {
                    showToast('Product added successfully!', 'success');
                    document.getElementById('productModal').style.display = 'none';
                })
                .catch(error => {
                    console.error('Error adding product:', error);
                    showToast('Error adding product. Please try again.', 'error');
                });
        }
    }

    function saveCustomer() {
        const customerId = document.getElementById('customerId').value;
        const customerData = {
            name: document.getElementById('customerName').value,
            phone: document.getElementById('customerPhone').value,
            email: document.getElementById('customerEmail').value,
            type: document.getElementById('customerType').value,
            area: document.getElementById('customerArea').value,
            salesman: document.getElementById('customerSalesman').value,
            address: document.getElementById('customerAddress').value,
            balance: 0,
            updatedAt: new Date().toISOString()
        };

        if (customerId) {
            // Update existing customer
            db.ref(`customers/${customerId}`).update(customerData)
                .then(() => {
                    showToast('Customer updated successfully!', 'success');
                    document.getElementById('customerModal').style.display = 'none';
                })
                .catch(error => {
                    console.error('Error updating customer:', error);
                    showToast('Error updating customer. Please try again.', 'error');
                });
        } else {
            // Add new customer
            customerData.createdAt = new Date().toISOString();
            db.ref('customers').push(customerData)
                .then(() => {
                    showToast('Customer added successfully!', 'success');
                    document.getElementById('customerModal').style.display = 'none';
                })
                .catch(error => {
                    console.error('Error adding customer:', error);
                    showToast('Error adding customer. Please try again.', 'error');
                });
        }
    }

    function saveKhatta() {
        const khattaId = document.getElementById('khattaId').value;
        const customerId = document.getElementById('khattaCustomer').value;
        
        const customer = customers.find(c => c.id === customerId);
        
        if (!customer) {
            showToast('Please select a valid customer', 'error');
            return;
        }
        
        const khattaData = {
            customerId: customerId,
            customerName: customer.name,
            date: document.getElementById('khattaDate').value,
            type: document.getElementById('khattaType').value,
            amount: parseFloat(document.getElementById('khattaAmount').value),
            description: document.getElementById('khattaDescription').value,
            updatedAt: new Date().toISOString()
        };

        if (khattaId) {
            // Update existing entry
            db.ref(`khatta/${khattaId}`).update(khattaData)
                .then(() => {
                    showToast('Khatta entry updated successfully!', 'success');
                    document.getElementById('khattaModal').style.display = 'none';
                    updateCustomerBalances();
                })
                .catch(error => {
                    console.error('Error updating khatta entry:', error);
                    showToast('Error updating khatta entry. Please try again.', 'error');
                });
        } else {
            // Add new entry
            khattaData.createdAt = new Date().toISOString();
            
            db.ref('khatta').push(khattaData)
                .then(() => {
                    showToast('Khatta entry added successfully!', 'success');
                    document.getElementById('khattaModal').style.display = 'none';
                    updateCustomerBalances();
                })
                .catch(error => {
                    console.error('Error adding khatta entry:', error);
                    showToast('Error adding khatta entry. Please try again.', 'error');
                });
        }
    }

    function saveArea() {
        const areaId = document.getElementById('areaId').value;
        const areaData = {
            name: document.getElementById('areaName').value,
            description: document.getElementById('areaDescription').value,
            updatedAt: new Date().toISOString()
        };

        if (areaId) {
            // Update existing area
            db.ref(`areas/${areaId}`).update(areaData)
                .then(() => {
                    showToast('Area updated successfully!', 'success');
                    document.getElementById('areaModal').style.display = 'none';
                })
                .catch(error => {
                    console.error('Error updating area:', error);
                    showToast('Error updating area. Please try again.', 'error');
                });
        } else {
            // Add new area
            areaData.createdAt = new Date().toISOString();
            db.ref('areas').push(areaData)
                .then(() => {
                    showToast('Area added successfully!', 'success');
                    document.getElementById('areaModal').style.display = 'none';
                })
                .catch(error => {
                    console.error('Error adding area:', error);
                    showToast('Error adding area. Please try again.', 'error');
                });
        }
    }

    function saveSalesman() {
        const salesmanId = document.getElementById('salesmanId').value;
        const salesmanData = {
            name: document.getElementById('salesmanName').value,
            phone: document.getElementById('salesmanPhone').value,
            email: document.getElementById('salesmanEmail').value,
            area: document.getElementById('salesmanArea').value,
            address: document.getElementById('salesmanAddress').value,
            updatedAt: new Date().toISOString()
        };

        if (salesmanId) {
            // Update existing salesman
            db.ref(`salesmen/${salesmanId}`).update(salesmanData)
                .then(() => {
                    showToast('Salesman updated successfully!', 'success');
                    document.getElementById('salesmanModal').style.display = 'none';
                })
                .catch(error => {
                    console.error('Error updating salesman:', error);
                    showToast('Error updating salesman. Please try again.', 'error');
                });
        } else {
            // Add new salesman
            salesmanData.createdAt = new Date().toISOString();
            db.ref('salesmen').push(salesmanData)
                .then(() => {
                    showToast('Salesman added successfully!', 'success');
                    document.getElementById('salesmanModal').style.display = 'none';
                })
                .catch(error => {
                    console.error('Error adding salesman:', error);
                    showToast('Error adding salesman. Please try again.', 'error');
                });
        }
    }

    function saveSupplier() {
        const supplierId = document.getElementById('supplierId').value;
        const supplierData = {
            companyName: document.getElementById('supplierCompany').value,
            contactPerson: document.getElementById('supplierContact').value,
            phone: document.getElementById('supplierPhone').value,
            email: document.getElementById('supplierEmail').value,
            address: document.getElementById('supplierAddress').value,
            gstNumber: document.getElementById('supplierGST').value,
            status: document.getElementById('supplierStatus').value,
            updatedAt: new Date().toISOString()
        };

        if (supplierId) {
            // Update existing supplier
            db.ref(`suppliers/${supplierId}`).update(supplierData)
                .then(() => {
                    showToast('Supplier updated successfully!', 'success');
                    document.getElementById('supplierModal').style.display = 'none';
                })
                .catch(error => {
                    console.error('Error updating supplier:', error);
                    showToast('Error updating supplier. Please try again.', 'error');
                });
        } else {
            // Add new supplier
            supplierData.createdAt = new Date().toISOString();
            db.ref('suppliers').push(supplierData)
                .then(() => {
                    showToast('Supplier added successfully!', 'success');
                    document.getElementById('supplierModal').style.display = 'none';
                })
                .catch(error => {
                    console.error('Error adding supplier:', error);
                    showToast('Error adding supplier. Please try again.', 'error');
                });
        }
    }

    function savePurchaseOrder() {
        const poId = document.getElementById('purchaseOrderId').value;
        const supplierName = document.getElementById('poSupplier').value;
        const productId = document.getElementById('poProduct').value;
        
        const supplier = suppliers.find(s => s.companyName === supplierName);
        const product = products.find(p => p.id === productId);
        
        if (!supplier || !product) {
            showToast('Please select valid supplier and product', 'error');
            return;
        }
        
        const poData = {
            poCode: document.getElementById('poCode').value,
            supplierId: supplier.id,
            supplierName: supplier.companyName,
            productId: productId,
            productName: product.name,
            date: document.getElementById('poDate').value,
            quantity: parseInt(document.getElementById('poQuantity').value),
            price: parseFloat(document.getElementById('poPrice').value),
            discount: parseFloat(document.getElementById('poDiscount').value) || 0,
            totalAmount: parseFloat(document.getElementById('poTotal').value),
            status: 'pending',
            updatedAt: new Date().toISOString()
        };

        if (poId) {
            // Update existing PO
            db.ref(`purchaseOrders/${poId}`).update(poData)
                .then(() => {
                    showToast('Purchase order updated successfully!', 'success');
                    document.getElementById('purchaseOrderModal').style.display = 'none';
                })
                .catch(error => {
                    console.error('Error updating purchase order:', error);
                    showToast('Error updating purchase order. Please try again.', 'error');
                });
        } else {
            // Add new PO
            poData.createdAt = new Date().toISOString();
            
            db.ref('purchaseOrders').push(poData)
                .then(() => {
                    showToast('Purchase order created successfully!', 'success');
                    document.getElementById('purchaseOrderModal').style.display = 'none';
                })
                .catch(error => {
                    console.error('Error creating purchase order:', error);
                    showToast('Error creating purchase order. Please try again.', 'error');
                });
        }
    }

    function saveBill() {
        if (currentBillItems.length === 0) {
            showToast('Please add at least one product to the bill.', 'error');
            return;
        }

        const customerId = document.getElementById('billCustomer').value;
        const customer = customers.find(c => c.id === customerId);
        const salesmanId = document.getElementById('billSalesman').value;
        const salesman = salesmen.find(s => s.id === salesmanId);
        const paymentOption = document.querySelector('.payment-option.active');
        const paymentType = paymentOption ? paymentOption.getAttribute('data-payment') : 'cash';
        
        const billData = {
            date: document.getElementById('billDate').value,
            customerId: customerId,
            customerName: customer ? customer.name : 'Walk-in Customer',
            salesmanId: salesmanId,
            salesmanName: salesman ? salesman.name : '',
            items: currentBillItems,
            subtotal: parseFloat(document.getElementById('billSubtotal').value),
            tax: parseFloat(document.getElementById('billTax').value),
            discount: parseFloat(document.getElementById('billDiscount').value),
            grandTotal: parseFloat(document.getElementById('billGrandTotal').value),
            paymentType: paymentType,
            status: 'active',
            createdAt: new Date().toISOString()
        };

        // Add credit information if payment is credit
        if (paymentType === 'credit') {
            billData.creditTerms = parseInt(document.getElementById('creditTerms').value);
            billData.creditDueDate = document.getElementById('creditDueDate').value;
            
            // Create khatta entry for credit
            const khattaData = {
                customerId: customerId,
                customerName: customer ? customer.name : 'Walk-in Customer',
                date: billData.date,
                type: 'credit',
                amount: billData.grandTotal,
                description: `Credit sale - Bill ${billData.sbCode}`,
                createdAt: new Date().toISOString()
            };
            
            db.ref('khatta').push(khattaData);
        }

        // Generate SB code
        const sbCode = 'SB' + Date.now().toString().slice(-6);
        billData.sbCode = sbCode;

        db.ref('bills').push(billData)
            .then(() => {
                // Update product stock
                currentBillItems.forEach(item => {
                    const product = products.find(p => p.id === item.productId);
                    if (product) {
                        const newStock = product.stock - item.quantity;
                        db.ref(`products/${item.productId}/stock`).set(newStock);
                    }
                });
                
                showToast('Bill created successfully!', 'success');
                document.getElementById('billModal').style.display = 'none';
            })
            .catch(error => {
                console.error('Error creating bill:', error);
                showToast('Error creating bill. Please try again.', 'error');
            });
    }

    function deleteProduct(productId) {
        if (confirm('Are you sure you want to delete this product?')) {
            db.ref(`products/${productId}`).remove()
                .then(() => {
                    showToast('Product deleted successfully!', 'success');
                })
                .catch(error => {
                    console.error('Error deleting product:', error);
                    showToast('Error deleting product. Please try again.', 'error');
                });
        }
    }

    function deleteCustomer(customerId) {
        if (confirm('Are you sure you want to delete this customer?')) {
            db.ref(`customers/${customerId}`).remove()
                .then(() => {
                    showToast('Customer deleted successfully!', 'success');
                })
                .catch(error => {
                    console.error('Error deleting customer:', error);
                    showToast('Error deleting customer. Please try again.', 'error');
                });
        }
    }

    function deleteArea(areaId) {
        if (confirm('Are you sure you want to delete this area?')) {
            db.ref(`areas/${areaId}`).remove()
                .then(() => {
                    showToast('Area deleted successfully!', 'success');
                })
                .catch(error => {
                    console.error('Error deleting area:', error);
                    showToast('Error deleting area. Please try again.', 'error');
                });
        }
    }

    function deleteSalesman(salesmanId) {
        if (confirm('Are you sure you want to delete this salesman?')) {
            db.ref(`salesmen/${salesmanId}`).remove()
                .then(() => {
                    showToast('Salesman deleted successfully!', 'success');
                })
                .catch(error => {
                    console.error('Error deleting salesman:', error);
                    showToast('Error deleting salesman. Please try again.', 'error');
                });
        }
    }

    function deleteSupplier(supplierId) {
        if (confirm('Are you sure you want to delete this supplier?')) {
            db.ref(`suppliers/${supplierId}`).remove()
                .then(() => {
                    showToast('Supplier deleted successfully!', 'success');
                })
                .catch(error => {
                    console.error('Error deleting supplier:', error);
                    showToast('Error deleting supplier. Please try again.', 'error');
                });
        }
    }

    function deleteKhatta(khattaId) {
        if (confirm('Are you sure you want to delete this khatta entry?')) {
            db.ref(`khatta/${khattaId}`).remove()
                .then(() => {
                    showToast('Khatta entry deleted successfully!', 'success');
                    updateCustomerBalances();
                })
                .catch(error => {
                    console.error('Error deleting khatta entry:', error);
                    showToast('Error deleting khatta entry. Please try again.', 'error');
                });
        }
    }

    function deletePurchaseOrder(poId) {
        if (confirm('Are you sure you want to delete this purchase order?')) {
            db.ref(`purchaseOrders/${poId}`).remove()
                .then(() => {
                    showToast('Purchase order deleted successfully!', 'success');
                })
                .catch(error => {
                    console.error('Error deleting purchase order:', error);
                    showToast('Error deleting purchase order. Please try again.', 'error');
                });
        }
    }

    function viewBill(billId) {
        const bill = bills.find(b => b.id === billId);
        if (bill) {
            const modal = document.getElementById('viewBillModal');
            const content = document.getElementById('billDetailsContent');
            
            let billDetails = `
                <div class="bill-header" style="text-align: center; margin-bottom: 20px;">
                    <h2>Qaimkhani Store</h2>
                    <p>Stationery & Hardware</p>
                </div>
                <div class="bill-details" style="margin-bottom: 20px;">
                    <p><strong>SB Code:</strong> ${bill.sbCode}</p>
                    <p><strong>Date:</strong> ${bill.date}</p>
                    <p><strong>Customer:</strong> ${bill.customerName}</p>
                    <p><strong>Salesman:</strong> ${bill.salesmanName || 'N/A'}</p>
                    <p><strong>Payment Type:</strong> ${bill.paymentType}</p>
                </div>
                <table style="width: 100%; border-collapse: collapse; margin-bottom: 20px;">
                    <thead>
                        <tr>
                            <th style="border: 1px solid #ddd; padding: 8px; text-align: left;">Product</th>
                            <th style="border: 1px solid #ddd; padding: 8px; text-align: left;">Price</th>
                            <th style="border: 1px solid #ddd; padding: 8px; text-align: left;">Qty</th>
                            <th style="border: 1px solid #ddd; padding: 8px; text-align: left;">Total</th>
                        </tr>
                    </thead>
                    <tbody>
            `;
            
            if (bill.items) {
                bill.items.forEach(item => {
                    billDetails += `
                        <tr>
                            <td style="border: 1px solid #ddd; padding: 8px;">${item.name}</td>
                            <td style="border: 1px solid #ddd; padding: 8px;">PKR ${item.price}</td>
                            <td style="border: 1px solid #ddd; padding: 8px;">${item.quantity}</td>
                            <td style="border: 1px solid #ddd; padding: 8px;">PKR ${item.total}</td>
                        </tr>
                    `;
                });
            }
            
            billDetails += `
                    </tbody>
                </table>
                <div class="bill-totals" style="text-align: right;">
                    <p><strong>Subtotal:</strong> PKR ${bill.subtotal}</p>
                    <p><strong>Tax:</strong> PKR ${bill.tax}</p>
                    <p><strong>Discount:</strong> PKR ${bill.discount}</p>
                    <p><strong>Grand Total:</strong> PKR ${bill.grandTotal}</p>
                </div>
                <div class="bill-footer" style="margin-top: 30px; text-align: center;">
                    <p>Thank you for your business!</p>
                </div>
            `;
            
            content.innerHTML = billDetails;
            modal.style.display = 'flex';
        }
    }

    function viewReturn(returnId) {
        const returnItem = returns.find(r => r.id === returnId);
        if (returnItem) {
            let returnDetails = `Return Number: ${returnItem.returnNumber}\n`;
            returnDetails += `Date: ${returnItem.returnDate}\n`;
            returnDetails += `SB Code: ${returnItem.sbCode}\n`;
            returnDetails += `Customer: ${returnItem.customerName}\n`;
            returnDetails += `Items:\n`;
            
            if (returnItem.items) {
                returnItem.items.forEach(item => {
                    returnDetails += `  - ${item.name}: ${item.returnQty} x PKR ${item.price} = PKR ${item.total}\n`;
                });
            }
            
            returnDetails += `Total Amount: PKR ${returnItem.totalAmount}\n`;
            
            alert(returnDetails);
        }
    }

    function viewGrn(grnId) {
        const grn = grns.find(g => g.id === grnId);
        if (grn) {
            let grnDetails = `GRN Date: ${grn.date}\n`;
            grnDetails += `PO Code: ${grn.poCode}\n`;
            grnDetails += `Supplier: ${grn.supplierName}\n`;
            grnDetails += `Product: ${grn.productName}\n`;
            grnDetails += `Quantity: ${grn.quantity}\n`;
            grnDetails += `Price: PKR ${grn.price}\n`;
            grnDetails += `Total Amount: PKR ${grn.totalAmount}\n`;
            
            alert(grnDetails);
        }
    }

    function renderBillItems() {
        const tableBody = document.getElementById('billItemsTable');
        tableBody.innerHTML = '';
        
        if (currentBillItems.length === 0) {
            tableBody.innerHTML = `
                <tr>
                    <td colspan="5" class="empty-state" style="padding: 10px;">
                        <i class="fas fa-receipt"></i>
                        <p>Add products to create a bill</p>
                    </td>
                </tr>
            `;
            return;
        }
        
        currentBillItems.forEach((item, index) => {
            const row = document.createElement('tr');
            row.innerHTML = `
                <td>${item.name}</td>
                <td>PKR ${item.price}</td>
                <td>
                    <input type="number" class="form-control bill-item-quantity" 
                           value="${item.quantity}" min="1" data-index="${index}">
                </td>
                <td>PKR ${item.total}</td>
                <td>
                    <button class="btn btn-sm btn-danger remove-bill-item" data-index="${index}">
                        <i class="fas fa-trash"></i>
                    </button>
                </td>
            `;
            tableBody.appendChild(row);
        });

        // Add event listeners for quantity changes and remove buttons
        document.querySelectorAll('.bill-item-quantity').forEach(input => {
            input.addEventListener('change', function() {
                const index = parseInt(this.getAttribute('data-index'));
                const quantity = parseInt(this.value);
                
                if (quantity > 0) {
                    currentBillItems[index].quantity = quantity;
                    currentBillItems[index].total = quantity * currentBillItems[index].price;
                    renderBillItems();
                    calculateBillTotals();
                }
            });
        });

        document.querySelectorAll('.remove-bill-item').forEach(button => {
            button.addEventListener('click', function() {
                const index = parseInt(this.getAttribute('data-index'));
                currentBillItems.splice(index, 1);
                renderBillItems();
                calculateBillTotals();
            });
        });
    }

    function calculateBillTotals() {
        const subtotal = currentBillItems.reduce((sum, item) => sum + item.total, 0);
        const taxRate = 0; // You can add tax rate setting
        const discount = parseFloat(document.getElementById('billDiscount').value) || 0;
        
        const taxAmount = subtotal * (taxRate / 100);
        const grandTotal = subtotal + taxAmount - discount;
        
        document.getElementById('billSubtotal').value = subtotal.toFixed(2);
        document.getElementById('billTax').value = taxAmount.toFixed(2);
        document.getElementById('billGrandTotal').value = grandTotal.toFixed(2);
    }

    function calculatePOTotal() {
        const quantity = parseInt(document.getElementById('poQuantity').value) || 0;
        const price = parseFloat(document.getElementById('poPrice').value) || 0;
        const discount = parseFloat(document.getElementById('poDiscount').value) || 0;
        const total = (quantity * price) - discount;
        
        document.getElementById('poTotal').value = total.toFixed(2);
    }

    function showToast(message, type = 'success') {
        toast.textContent = message;
        toast.style.backgroundColor = type === 'success' ? 'var(--success)' : 
                                    type === 'error' ? 'var(--error)' : 
                                    type === 'warning' ? 'var(--warning)' : 'var(--primary)';
        toast.style.display = 'block';
        
        setTimeout(() => {
            toast.style.display = 'none';
        }, 3000);
    }

    // New functions for GRN and Returns
    function searchPOForGRN() {
        const poCode = document.getElementById('grnPOCode').value;
        const po = purchaseOrders.find(p => p.poCode === poCode);
        
        if (!po) {
            showToast('Purchase order not found with this code', 'error');
            return;
        }
        
        // Check if GRN already exists for this PO
        const existingGRN = grns.find(g => g.poCode === poCode);
        if (existingGRN) {
            showToast('GRN already received for this purchase order', 'error');
            return;
        }
        
        // Display PO details
        document.getElementById('poCodeDisplay').textContent = po.poCode;
        document.getElementById('poDateDisplay').textContent = po.date;
        document.getElementById('poSupplierDisplay').textContent = po.supplierName;
        document.getElementById('poProductDisplay').textContent = po.productName;
        document.getElementById('poQuantityDisplay').textContent = po.quantity;
        document.getElementById('poPriceDisplay').textContent = 'PKR ' + po.price;
        document.getElementById('poDiscountDisplay').textContent = 'PKR ' + (po.discount || 0);
        document.getElementById('poTotalDisplay').textContent = 'PKR ' + po.totalAmount;
        document.getElementById('poNetAmountDisplay').textContent = 'PKR ' + (po.totalAmount - (po.discount || 0));
        
        document.getElementById('poDetailsForGRN').style.display = 'block';
        document.getElementById('poDetailsForGRN').setAttribute('data-po-id', po.id);
    }

    function submitGRN() {
        const poId = document.getElementById('poDetailsForGRN').getAttribute('data-po-id');
        const po = purchaseOrders.find(p => p.id === poId);
        
        if (!po) {
            showToast('Purchase order not found', 'error');
            return;
        }
        
        const grnData = {
            poCode: po.poCode,
            date: new Date().toISOString().split('T')[0],
            supplierId: po.supplierId,
            supplierName: po.supplierName,
            productId: po.productId,
            productName: po.productName,
            quantity: po.quantity,
            price: po.price,
            discount: po.discount || 0,
            totalAmount: po.totalAmount,
            netAmount: po.totalAmount - (po.discount || 0),
            createdAt: new Date().toISOString()
        };
        
        db.ref('grns').push(grnData)
            .then(() => {
                // Update product stock
                const product = products.find(p => p.id === po.productId);
                if (product) {
                    const newStock = product.stock + po.quantity;
                    db.ref(`products/${po.productId}/stock`).set(newStock);
                }
                
                // Update PO status
                db.ref(`purchaseOrders/${poId}/status`).set('completed');
                
                showToast('GRN submitted successfully!', 'success');
                document.getElementById('poDetailsForGRN').style.display = 'none';
                document.getElementById('grnPOCode').value = '';
            })
            .catch(error => {
                console.error('Error submitting GRN:', error);
                showToast('Error submitting GRN. Please try again.', 'error');
            });
    }

    function searchSBForReturn() {
        const sbCode = document.getElementById('returnSBCode').value;
        const bill = bills.find(b => b.sbCode === sbCode && b.status !== 'returned');
        
        if (!bill) {
            showToast('Bill not found or already returned', 'error');
            return;
        }
        
        // Display bill details
        document.getElementById('sbCodeDisplay').textContent = bill.sbCode;
        document.getElementById('sbDateDisplay').textContent = bill.date;
        document.getElementById('sbCustomerDisplay').textContent = bill.customerName;
        document.getElementById('sbTotalDisplay').textContent = 'PKR ' + bill.grandTotal;
        
        let itemsHtml = '';
        if (bill.items) {
            bill.items.forEach(item => {
                itemsHtml += `<p>${item.name} - ${item.quantity} x PKR ${item.price} = PKR ${item.total}</p>`;
            });
        }
        document.getElementById('sbItemsDisplay').innerHTML = itemsHtml;
        
        document.getElementById('sbDetailsForReturn').style.display = 'block';
        document.getElementById('sbDetailsForReturn').setAttribute('data-bill-id', bill.id);
    }

    function submitReturn() {
        const billId = document.getElementById('sbDetailsForReturn').getAttribute('data-bill-id');
        const bill = bills.find(b => b.id === billId);
        
        if (!bill) {
            showToast('Bill not found', 'error');
            return;
        }
        
        const returnData = {
            sbCode: bill.sbCode,
            returnDate: new Date().toISOString().split('T')[0],
            customerId: bill.customerId,
            customerName: bill.customerName,
            items: bill.items,
            totalAmount: bill.grandTotal,
            createdAt: new Date().toISOString()
        };
        
        db.ref('returns').push(returnData)
            .then(() => {
                // Update product stock
                if (bill.items) {
                    bill.items.forEach(item => {
                        const product = products.find(p => p.id === item.productId);
                        if (product) {
                            const newStock = product.stock + item.quantity;
                            db.ref(`products/${item.productId}/stock`).set(newStock);
                        }
                    });
                }
                
                // Update bill status
                db.ref(`bills/${billId}/status`).set('returned');
                
                showToast('Return submitted successfully!', 'success');
                document.getElementById('sbDetailsForReturn').style.display = 'none';
                document.getElementById('returnSBCode').value = '';
            })
            .catch(error => {
                console.error('Error submitting return:', error);
                showToast('Error submitting return. Please try again.', 'error');
            });
    }

    // Add event listeners for modal buttons
    document.getElementById('addProductBtn').addEventListener('click', () => openProductModal());
    document.getElementById('addCustomerBtn').addEventListener('click', () => openCustomerModal());
    document.getElementById('addKhattaBtn').addEventListener('click', () => openKhattaModal());
    document.getElementById('addAreaBtn').addEventListener('click', () => openAreaModal());
    document.getElementById('addSalesmanBtn').addEventListener('click', () => openSalesmanModal());
    document.getElementById('addSupplierBtn').addEventListener('click', () => openSupplierModal());
    document.getElementById('addPurchaseOrderBtn').addEventListener('click', () => openPurchaseOrderModal());
    document.getElementById('createBillBtn').addEventListener('click', () => openBillModal());

    document.getElementById('saveProductBtn').addEventListener('click', saveProduct);
    document.getElementById('saveCustomerBtn').addEventListener('click', saveCustomer);
    document.getElementById('saveKhattaBtn').addEventListener('click', saveKhatta);
    document.getElementById('saveAreaBtn').addEventListener('click', saveArea);
    document.getElementById('saveSalesmanBtn').addEventListener('click', saveSalesman);
    document.getElementById('saveSupplierBtn').addEventListener('click', saveSupplier);
    document.getElementById('savePurchaseOrderBtn').addEventListener('click', savePurchaseOrder);
    document.getElementById('saveBillBtn').addEventListener('click', saveBill);

    // Add event listeners for payment options
    document.querySelectorAll('.payment-option').forEach(option => {
        option.addEventListener('click', function() {
            document.querySelectorAll('.payment-option').forEach(o => o.classList.remove('active'));
            this.classList.add('active');
            
            // Show/hide credit options
            if (this.getAttribute('data-payment') === 'credit') {
                document.getElementById('creditOption').style.display = 'block';
                // Set default due date (today + 30 days)
                const dueDate = new Date();
                dueDate.setDate(dueDate.getDate() + 30);
                document.getElementById('creditDueDate').valueAsDate = dueDate;
            } else {
                document.getElementById('creditOption').style.display = 'none';
            }
        });
    });

    // Add event listener for bill product selection
    document.getElementById('billProductSelect').addEventListener('change', function() {
        const productId = this.value;
        if (productId) {
            const product = products.find(p => p.id === productId);
            if (product) {
                addProductToBill(product);
                this.value = '';
            }
        }
    });

    function addProductToBill(product) {
        // Check if product already exists in bill
        const existingItem = currentBillItems.find(item => item.productId === product.id);
        
        if (existingItem) {
            existingItem.quantity += 1;
            existingItem.total = existingItem.quantity * existingItem.price;
        } else {
            currentBillItems.push({
                productId: product.id,
                name: product.name,
                price: product.price,
                quantity: 1,
                total: product.price
            });
        }
        
        renderBillItems();
        calculateBillTotals();
    }

    // Add event listener for print bill button
    document.getElementById('printBillBtn').addEventListener('click', function() {
        // Print bill functionality
        const billData = {
            customerName: document.getElementById('billCustomer').options[document.getElementById('billCustomer').selectedIndex]?.text || 'Walk-in Customer',
            date: document.getElementById('billDate').value,
            items: currentBillItems,
            subtotal: document.getElementById('billSubtotal').value,
            tax: document.getElementById('billTax').value,
            discount: document.getElementById('billDiscount').value,
            grandTotal: document.getElementById('billGrandTotal').value,
            paymentType: document.querySelector('.payment-option.active')?.getAttribute('data-payment') || 'cash'
        };
        
        // Create a printable bill
        const printWindow = window.open('', '_blank');
        printWindow.document.write(`
            <html>
            <head>
                <title>Bill Print</title>
                <style>
                    body { font-family: Arial, sans-serif; margin: 20px; }
                    .bill-header { text-align: center; margin-bottom: 20px; }
                    .bill-details { margin-bottom: 20px; }
                    .bill-items { width: 100%; border-collapse: collapse; margin-bottom: 20px; }
                    .bill-items th, .bill-items td { border: 1px solid #ddd; padding: 8px; text-align: left; }
                    .bill-totals { text-align: right; }
                    .bill-footer { margin-top: 30px; text-align: center; }
                </style>
            </head>
            <body>
                <div class="bill-header">
                    <h2>Qaimkhani Store</h2>
                    <p>Stationery & Hardware</p>
                </div>
                <div class="bill-details">
                    <p><strong>Customer:</strong> ${billData.customerName}</p>
                    <p><strong>Date:</strong> ${billData.date}</p>
                    <p><strong>Payment Type:</strong> ${billData.paymentType}</p>
                </div>
                <table class="bill-items">
                    <thead>
                        <tr>
                            <th>Product</th>
                            <th>Price</th>
                            <th>Qty</th>
                            <th>Total</th>
                        </tr>
                    </thead>
                    <tbody>
                        ${billData.items.map(item => `
                            <tr>
                                <td>${item.name}</td>
                                <td>PKR ${item.price}</td>
                                <td>${item.quantity}</td>
                                <td>PKR ${item.total}</td>
                            </tr>
                        `).join('')}
                    </tbody>
                </table>
                <div class="bill-totals">
                    <p><strong>Subtotal:</strong> PKR ${billData.subtotal}</p>
                    <p><strong>Tax:</strong> PKR ${billData.tax}</p>
                    <p><strong>Discount:</strong> PKR ${billData.discount}</p>
                    <p><strong>Grand Total:</strong> PKR ${billData.grandTotal}</p>
                </div>
                <div class="bill-footer">
                    <p>Thank you for your business!</p>
                </div>
            </body>
            </html>
        `);
        printWindow.document.close();
        printWindow.print();
    });

    // Add event listener for print view bill button
    document.getElementById('printViewBillBtn').addEventListener('click', function() {
        const printContent = document.getElementById('billDetailsContent').innerHTML;
        const printWindow = window.open('', '_blank');
        printWindow.document.write(`
            <html>
            <head>
                <title>Bill Print</title>
                <style>
                    body { font-family: Arial, sans-serif; margin: 20px; }
                </style>
            </head>
            <body>
                ${printContent}
            </body>
            </html>
        `);
        printWindow.document.close();
        printWindow.print();
    });

    // Add event listener for download bill button
    document.getElementById('downloadBillBtn').addEventListener('click', function() {
        const billContent = document.getElementById('billDetailsContent').innerHTML;
        const blob = new Blob([billContent], {type: 'text/html'});
        const url = URL.createObjectURL(blob);
        const link = document.createElement('a');
        link.href = url;
        link.download = 'bill.html';
        document.body.appendChild(link);
        link.click();
        document.body.removeChild(link);
        URL.revokeObjectURL(url);
    });

    // Add event listeners for view all buttons
    document.getElementById('viewAllTransactions').addEventListener('click', function() {
        menuItems.forEach(i => i.classList.remove('active'));
        document.querySelector('.sidebar-menu a[data-page="spot-sales"]').classList.add('active');
        pageContents.forEach(content => content.classList.remove('active'));
        document.getElementById('spot-sales').classList.add('active');
        document.querySelector('.topbar-left h1').textContent = 'Spot Sales';
    });

    document.getElementById('viewAllAlerts').addEventListener('click', function() {
        menuItems.forEach(i => i.classList.remove('active'));
        document.querySelector('.sidebar-menu a[data-page="products"]').classList.add('active');
        pageContents.forEach(content => content.classList.remove('active'));
        document.getElementById('products').classList.add('active');
        document.querySelector('.topbar-left h1').textContent = 'Products';
    });

    document.getElementById('viewAllProducts').addEventListener('click', function() {
        menuItems.forEach(i => i.classList.remove('active'));
        document.querySelector('.sidebar-menu a[data-page="products"]').classList.add('active');
        pageContents.forEach(content => content.classList.remove('active'));
        document.getElementById('products').classList.add('active');
        document.querySelector('.topbar-left h1').textContent = 'Products';
    });

    // Add event listeners for export buttons
    document.querySelectorAll('.btn-outline').forEach(button => {
        if (button.id.includes('export')) {
            button.addEventListener('click', function() {
                // Export data to Excel
                let dataToExport = [];
                let fileName = '';
                
                if (this.id === 'exportProducts') {
                    dataToExport = products.map(p => ({
                        'Product ID': p.id,
                        'Name': p.name,
                        'Category': p.category,
                        'Price': p.price,
                        'Cost': p.cost,
                        'Stock': p.stock,
                        'Min Stock': p.minStock,
                        'Description': p.description
                    }));
                    fileName = 'products.xlsx';
                } else if (this.id === 'exportCustomers') {
                    dataToExport = customers.map(c => ({
                        'Customer ID': c.id,
                        'Name': c.name,
                        'Phone': c.phone,
                        'Email': c.email,
                        'Type': c.type,
                        'Area': c.area,
                        'Salesman': c.salesman,
                        'Balance': c.balance || 0
                    }));
                    fileName = 'customers.xlsx';
                } else if (this.id === 'exportKhatta') {
                    dataToExport = khattaEntries.map(k => ({
                        'Date': k.date,
                        'Customer': k.customerName,
                        'Description': k.description,
                        'Credit': k.type === 'credit' ? k.amount : 0,
                        'Debit': k.type === 'debit' ? k.amount : 0
                    }));
                    fileName = 'khatta.xlsx';
                } else if (this.id === 'exportSuppliers') {
                    dataToExport = suppliers.map(s => ({
                        'Supplier ID': s.id,
                        'Company Name': s.companyName,
                        'Contact Person': s.contactPerson,
                        'Phone': s.phone,
                        'Email': s.email,
                        'Status': s.status
                    }));
                    fileName = 'suppliers.xlsx';
                } else if (this.id === 'exportPurchaseOrders') {
                    dataToExport = purchaseOrders.map(po => ({
                        'PO Code': po.poCode,
                        'Supplier': po.supplierName,
                        'Date': po.date,
                        'Product': po.productName,
                        'Quantity': po.quantity,
                        'Price': po.price,
                        'Total Amount': po.totalAmount,
                        'Status': po.status
                    }));
                    fileName = 'purchase_orders.xlsx';
                } else if (this.id === 'exportGrn') {
                    dataToExport = grns.map(g => ({
                        'Date': g.date,
                        'PO Code': g.poCode,
                        'Supplier': g.supplierName,
                        'Product': g.productName,
                        'Quantity': g.quantity,
                        'Price': g.price,
                        'Total Amount': g.totalAmount
                    }));
                    fileName = 'grn.xlsx';
                } else if (this.id === 'exportBills') {
                    dataToExport = bills.map(b => ({
                        'SB Code': b.sbCode,
                        'Date': b.date,
                        'Customer': b.customerName,
                        'Salesman': b.salesmanName || '',
                        'Items Count': b.items ? b.items.length : 0,
                        'Subtotal': b.subtotal,
                        'Tax': b.tax,
                        'Discount': b.discount,
                        'Grand Total': b.grandTotal,
                        'Payment Type': b.paymentType,
                        'Status': b.status
                    }));
                    fileName = 'bills.xlsx';
                } else if (this.id === 'exportReturns') {
                    dataToExport = returns.map(r => ({
                        'Return Date': r.returnDate,
                        'SB Code': r.sbCode,
                        'Customer': r.customerName,
                        'Total Amount': r.totalAmount
                    }));
                    fileName = 'returns.xlsx';
                } else if (this.id === 'exportReport') {
                    // Export current report data
                    const activeTab = document.querySelector('.tab.active');
                    const reportType = activeTab ? activeTab.getAttribute('data-report') : 'sales';
                    
                    if (reportType === 'sales') {
                        dataToExport = bills.filter(b => b.status !== 'returned').map(b => ({
                            'Date': b.date,
                            'SB Code': b.sbCode,
                            'Customer': b.customerName,
                            'Items Count': b.items ? b.items.length : 0,
                            'Amount': b.grandTotal,
                            'Payment Type': b.paymentType
                        }));
                    } else if (reportType === 'purchase') {
                        dataToExport = purchaseOrders.map(po => ({
                            'PO Code': po.poCode,
                            'Date': po.date,
                            'Supplier': po.supplierName,
                            'Product': po.productName,
                            'Quantity': po.quantity,
                            'Amount': po.totalAmount,
                            'Status': po.status
                        }));
                    } else if (reportType === 'daily') {
                        const today = new Date().toISOString().split('T')[0];
                        dataToExport = bills.filter(b => b.date === today && b.status !== 'returned').map(b => ({
                            'SB Code': b.sbCode,
                            'Time': new Date().toLocaleTimeString(),
                            'Customer': b.customerName,
                            'Items Count': b.items ? b.items.length : 0,
                            'Amount': b.grandTotal,
                            'Payment Type': b.paymentType
                        }));
                    } else if (reportType === 'return') {
                        dataToExport = returns.map(r => ({
                            'Return Date': r.returnDate,
                            'SB Code': r.sbCode,
                            'Customer': r.customerName,
                            'Items Count': r.items ? r.items.length : 0,
                            'Amount': r.totalAmount
                        }));
                    } else if (reportType === 'stock') {
                        const startDate = document.getElementById('reportStartDate').value;
                        const endDate = document.getElementById('reportEndDate').value;
                        
                        // Filter products based on date range (this would need more complex logic in a real app)
                        dataToExport = products.map(p => ({
                            'Product': p.name,
                            'Category': p.category,
                            'Stock': p.stock,
                            'Price': p.price
                        }));
                    } else if (reportType === 'customer') {
                        dataToExport = customers.map(c => ({
                            'Customer': c.name,
                            'Phone': c.phone,
                            'Total Purchases': 0, // You would need to calculate this
                            'Last Purchase': '', // You would need to calculate this
                            'Outstanding': c.balance || 0
                        }));
                    } else if (reportType === 'khatta') {
                        dataToExport = khattaEntries.map(k => ({
                            'Date': k.date,
                            'Customer': k.customerName,
                            'Description': k.description,
                            'Credit': k.type === 'credit' ? k.amount : 0,
                            'Debit': k.type === 'debit' ? k.amount : 0
                        }));
                    } else if (reportType === 'supplier') {
                        dataToExport = suppliers.map(s => ({
                            'Supplier': s.companyName,
                            'Contact': s.contactPerson,
                            'Total Products': products.filter(p => p.supplier === s.companyName).length,
                            'Total Orders': purchaseOrders.filter(po => po.supplierName === s.companyName).length,
                            'Last Order': '' // You would need to calculate this
                        }));
                    }
                    
                    fileName = `${reportType}_report.xlsx`;
                }
                
                if (dataToExport.length > 0) {
                    const ws = XLSX.utils.json_to_sheet(dataToExport);
                    const wb = XLSX.utils.book_new();
                    XLSX.utils.book_append_sheet(wb, ws, "Data");
                    XLSX.writeFile(wb, fileName);
                    showToast('Data exported successfully!', 'success');
                } else {
                    showToast('No data to export', 'warning');
                }
            });
        }
    });

    // Add event listener for refresh data button
    document.getElementById('refreshData').addEventListener('click', function() {
        showToast('Data refreshed!', 'success');
        // In a real application, you would reload data from Firebase here
    });

    // Add event listeners for backup and restore buttons
    document.getElementById('backupDataBtn').addEventListener('click', function() {
        // Create a backup of all data
        const backupData = {
            products: products,
            customers: customers,
            areas: areas,
            salesmen: salesmen,
            suppliers: suppliers,
            khattaEntries: khattaEntries,
            purchaseOrders: purchaseOrders,
            grns: grns,
            bills: bills,
            returns: returns,
            timestamp: new Date().toISOString()
        };
        
        const dataStr = JSON.stringify(backupData, null, 2);
        const dataBlob = new Blob([dataStr], {type: 'application/json'});
        const url = URL.createObjectURL(dataBlob);
        const link = document.createElement('a');
        link.href = url;
        link.download = `store_backup_${new Date().toISOString().split('T')[0]}.json`;
        document.body.appendChild(link);
        link.click();
        document.body.removeChild(link);
        URL.revokeObjectURL(url);
        
        showToast('Backup created successfully!', 'success');
    });

    document.getElementById('restoreDataBtn').addEventListener('click', function() {
        // Create file input for restore
        const fileInput = document.createElement('input');
        fileInput.type = 'file';
        fileInput.accept = '.json';
        fileInput.addEventListener('change', function(e) {
            const file = e.target.files[0];
            if (file) {
                const reader = new FileReader();
                reader.onload = function(e) {
                    try {
                        const backupData = JSON.parse(e.target.result);
                        
                        // Restore data to Firebase
                        if (confirm('This will replace all current data. Are you sure?')) {
                            // Clear existing data
                            const refs = [
                                'products', 'customers', 'areas', 'salesmen', 
                                'suppliers', 'khatta', 
                                'purchaseOrders', 'grns', 'bills', 'returns'
                            ];
                            
                            const promises = refs.map(ref => db.ref(ref).remove());
                            
                            Promise.all(promises).then(() => {
                                // Add backup data
                                const addPromises = [];
                                
                                if (backupData.products) {
                                    backupData.products.forEach(product => {
                                        addPromises.push(db.ref('products').push(product));
                                    });
                                }
                                
                                if (backupData.customers) {
                                    backupData.customers.forEach(customer => {
                                        addPromises.push(db.ref('customers').push(customer));
                                    });
                                }
                                
                                if (backupData.areas) {
                                    backupData.areas.forEach(area => {
                                        addPromises.push(db.ref('areas').push(area));
                                    });
                                }
                                
                                if (backupData.salesmen) {
                                    backupData.salesmen.forEach(salesman => {
                                        addPromises.push(db.ref('salesmen').push(salesman));
                                    });
                                }
                                
                                if (backupData.suppliers) {
                                    backupData.suppliers.forEach(supplier => {
                                        addPromises.push(db.ref('suppliers').push(supplier));
                                    });
                                }
                                
                                if (backupData.khattaEntries) {
                                    backupData.khattaEntries.forEach(entry => {
                                        addPromises.push(db.ref('khatta').push(entry));
                                    });
                                }
                                
                                if (backupData.purchaseOrders) {
                                    backupData.purchaseOrders.forEach(po => {
                                        addPromises.push(db.ref('purchaseOrders').push(po));
                                    });
                                }
                                
                                if (backupData.grns) {
                                    backupData.grns.forEach(grn => {
                                        addPromises.push(db.ref('grns').push(grn));
                                    });
                                }
                                
                                if (backupData.bills) {
                                    backupData.bills.forEach(bill => {
                                        addPromises.push(db.ref('bills').push(bill));
                                    });
                                }
                                
                                if (backupData.returns) {
                                    backupData.returns.forEach(returnItem => {
                                        addPromises.push(db.ref('returns').push(returnItem));
                                    });
                                }
                                
                                Promise.all(addPromises).then(() => {
                                    showToast('Data restored successfully!', 'success');
                                }).catch(error => {
                                    console.error('Error restoring data:', error);
                                    showToast('Error restoring data. Please try again.', 'error');
                                });
                            }).catch(error => {
                                console.error('Error clearing data:', error);
                                showToast('Error restoring data. Please try again.', 'error');
                            });
                        }
                    } catch (error) {
                        console.error('Error parsing backup file:', error);
                        showToast('Invalid backup file. Please select a valid backup file.', 'error');
                    }
                };
                reader.readAsText(file);
            }
        });
        fileInput.click();
    });
</script>
</body>
</html>
