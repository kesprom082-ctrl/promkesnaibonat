<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Survei Kepuasan Pasien</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link href="https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@300;400;500;600;700;800&display=swap" rel="stylesheet">
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.5.0/css/all.min.css">
    <script>
        tailwind.config = {
            theme: {
                extend: {
                    fontFamily: { sans: ['Plus Jakarta Sans', 'sans-serif'] },
                    colors: {
                        med: {
                            50: '#ECFDF5', 100: '#D1FAE5', 200: '#A7F3D0',
                            300: '#6EE7B7', 400: '#34D399', 500: '#10B981',
                            600: '#059669', 700: '#047857', 800: '#065F46', 900: '#064E3B'
                        }
                    }
                }
            }
        }
    </script>
    <style>
        :root {
            --bg: #F0FDF4;
            --fg: #1E293B;
            --muted: #64748B;
            --accent: #059669;
            --accent-light: #D1FAE5;
            --card: #FFFFFF;
            --border: #E2E8F0;
            --danger: #EF4444;
        }
        * { box-sizing: border-box; }
        html { scroll-behavior: smooth; }
        body { font-family: 'Plus Jakarta Sans', sans-serif; background: var(--bg); color: var(--fg); }

        .bg-pattern {
            position: fixed; inset: 0; z-index: 0; pointer-events: none; overflow: hidden;
            background: linear-gradient(160deg, #ECFDF5 0%, #F0FDFA 30%, #F8FAFC 60%, #ECFDF5 100%);
        }
        .bg-pattern::before {
            content: ''; position: absolute; width: 600px; height: 600px;
            background: radial-gradient(circle, rgba(16,185,129,0.08) 0%, transparent 70%);
            top: -200px; right: -100px; border-radius: 50%;
            animation: floatBlob 20s ease-in-out infinite;
        }
        .bg-pattern::after {
            content: ''; position: absolute; width: 500px; height: 500px;
            background: radial-gradient(circle, rgba(5,150,105,0.06) 0%, transparent 70%);
            bottom: -150px; left: -100px; border-radius: 50%;
            animation: floatBlob 25s ease-in-out infinite reverse;
        }
        @keyframes floatBlob {
            0%, 100% { transform: translate(0, 0) scale(1); }
            33% { transform: translate(30px, -30px) scale(1.05); }
            66% { transform: translate(-20px, 20px) scale(0.95); }
        }

        .floating-cell {
            position: fixed; border-radius: 50%; pointer-events: none; z-index: 0;
            background: rgba(16,185,129,0.04); border: 1px solid rgba(16,185,129,0.08);
        }
        .floating-cell:nth-child(1) { width: 80px; height: 80px; top: 15%; left: 8%; animation: cellFloat 18s ease-in-out infinite; }
        .floating-cell:nth-child(2) { width: 50px; height: 50px; top: 60%; right: 10%; animation: cellFloat 22s ease-in-out infinite 3s; }
        .floating-cell:nth-child(3) { width: 120px; height: 120px; bottom: 20%; left: 15%; animation: cellFloat 28s ease-in-out infinite 6s; }
        .floating-cell:nth-child(4) { width: 40px; height: 40px; top: 35%; right: 25%; animation: cellFloat 16s ease-in-out infinite 9s; }
        .floating-cell:nth-child(5) { width: 65px; height: 65px; top: 75%; left: 60%; animation: cellFloat 24s ease-in-out infinite 2s; }
        @keyframes cellFloat {
            0%, 100% { transform: translateY(0) rotate(0deg); opacity: 0.5; }
            50% { transform: translateY(-30px) rotate(180deg); opacity: 1; }
        }

        .progress-track { background: #E2E8F0; height: 4px; border-radius: 2px; overflow: hidden; }
        .progress-fill { height: 100%; background: linear-gradient(90deg, #10B981, #059669); border-radius: 2px; transition: width 0.5s cubic-bezier(0.4, 0, 0.2, 1); }

        .step-dot {
            width: 36px; height: 36px; border-radius: 50%; display: flex; align-items: center; justify-content: center;
            font-size: 14px; font-weight: 700; transition: all 0.4s cubic-bezier(0.4, 0, 0.2, 1);
            border: 2px solid var(--border); background: var(--card); color: var(--muted); position: relative; z-index: 2;
        }
        .step-dot.active { border-color: var(--accent); background: var(--accent); color: white; transform: scale(1.1); box-shadow: 0 0 0 4px rgba(5,150,105,0.15); }
        .step-dot.completed { border-color: var(--accent); background: var(--accent-light); color: var(--accent); }
        .step-line { height: 2px; flex: 1; background: var(--border); transition: background 0.4s; }
        .step-line.completed { background: var(--accent); }

        .form-step {
            display: none; opacity: 0; transform: translateX(40px);
            transition: opacity 0.5s ease, transform 0.5s ease;
        }
        .form-step.active { display: block; opacity: 1; transform: translateX(0); }
        .form-step.exit-left { opacity: 0; transform: translateX(-40px); }

        .input-field {
            width: 100%; padding: 12px 16px; border: 2px solid var(--border); border-radius: 10px;
            font-size: 15px; font-family: inherit; transition: all 0.3s; background: var(--card); color: var(--fg);
        }
        .input-field:focus { outline: none; border-color: var(--accent); box-shadow: 0 0 0 3px rgba(5,150,105,0.1); }
        .input-field.error { border-color: var(--danger); box-shadow: 0 0 0 3px rgba(239,68,68,0.1); }

        .select-field {
            width: 100%; padding: 12px 16px; border: 2px solid var(--border); border-radius: 10px;
            font-size: 15px; font-family: inherit; transition: all 0.3s; background: var(--card); color: var(--fg);
            appearance: none; background-image: url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' width='20' height='20' viewBox='0 0 24 24' fill='none' stroke='%2364748B' stroke-width='2' stroke-linecap='round' stroke-linejoin='round'%3E%3Cpath d='M6 9l6 6 6-6'/%3E%3C/svg%3E");
            background-repeat: no-repeat; background-position: right 14px center; padding-right: 40px;
        }
        .select-field:focus { outline: none; border-color: var(--accent); box-shadow: 0 0 0 3px rgba(5,150,105,0.1); }

        .scale-group { display: flex; gap: 8px; flex-wrap: wrap; }
        .scale-btn {
            flex: 1; min-width: 56px; padding: 14px 8px 28px; border: 2px solid var(--border); border-radius: 12px;
            background: var(--card); cursor: pointer; transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
            display: flex; flex-direction: column; align-items: center; gap: 6px; position: relative;
        }
        .scale-btn:hover { border-color: #A7F3D0; background: #F0FDF4; transform: translateY(-2px); }
        .scale-btn .num {
            width: 32px; height: 32px; border-radius: 50%; display: flex; align-items: center; justify-content: center;
            font-weight: 800; font-size: 14px; color: var(--muted); transition: all 0.3s;
        }
        .scale-btn .label { font-size: 10px; color: var(--muted); text-align: center; line-height: 1.2; font-weight: 500; transition: color 0.3s; }
        .scale-btn.selected { border-color: var(--accent); background: var(--accent-light); transform: translateY(-2px); box-shadow: 0 4px 12px rgba(5,150,105,0.15); }
        .scale-btn.selected .num { background: var(--accent); color: white; }
        .scale-btn.selected .label { color: var(--accent); font-weight: 600; }

        .scale-btn[data-val="1"].selected { border-color: #EF4444; background: #FEF2F2; }
        .scale-btn[data-val="1"].selected .num { background: #EF4444; }
        .scale-btn[data-val="1"].selected .label { color: #EF4444; }
        .scale-btn[data-val="2"].selected { border-color: #F97316; background: #FFF7ED; }
        .scale-btn[data-val="2"].selected .num { background: #F97316; }
        .scale-btn[data-val="2"].selected .label { color: #F97316; }
        .scale-btn[data-val="3"].selected { border-color: #EAB308; background: #FEFCE8; }
        .scale-btn[data-val="3"].selected .num { background: #EAB308; }
        .scale-btn[data-val="3"].selected .label { color: #EAB308; }
        .scale-btn[data-val="4"].selected { border-color: #22C55E; background: #F0FDF4; }
        .scale-btn[data-val="4"].selected .num { background: #22C55E; }
        .scale-btn[data-val="4"].selected .label { color: #22C55E; }
        .scale-btn[data-val="5"].selected { border-color: #059669; background: #ECFDF5; }
        .scale-btn[data-val="5"].selected .num { background: #059669; }
        .scale-btn[data-val="5"].selected .label { color: #059669; }

        .radio-card {
            padding: 14px 20px; border: 2px solid var(--border); border-radius: 12px;
            background: var(--card); cursor: pointer; transition: all 0.3s; display: flex; align-items: center; gap: 12px;
        }
        .radio-card:hover { border-color: #A7F3D0; background: #F0FDF4; }
        .radio-card.selected { border-color: var(--accent); background: var(--accent-light); }
        .radio-card .radio-dot {
            width: 20px; height: 20px; border-radius: 50%; border: 2px solid var(--border);
            display: flex; align-items: center; justify-content: center; transition: all 0.3s; flex-shrink: 0;
        }
        .radio-card.selected .radio-dot { border-color: var(--accent); }
        .radio-card.selected .radio-dot::after {
            content: ''; width: 10px; height: 10px; border-radius: 50%; background: var(--accent);
        }

        .btn-primary {
            padding: 14px 32px; background: linear-gradient(135deg, #10B981, #059669); color: white;
            border: none; border-radius: 12px; font-size: 16px; font-weight: 700; cursor: pointer;
            transition: all 0.3s; font-family: inherit; display: inline-flex; align-items: center; gap: 8px;
        }
        .btn-primary:hover { transform: translateY(-2px); box-shadow: 0 8px 24px rgba(5,150,105,0.3); }
        .btn-primary:active { transform: translateY(0); }
        .btn-primary:disabled { opacity: 0.6; cursor: not-allowed; transform: none; box-shadow: none; }

        .btn-secondary {
            padding: 14px 32px; background: transparent; color: var(--muted);
            border: 2px solid var(--border); border-radius: 12px; font-size: 16px; font-weight: 600;
            cursor: pointer; transition: all 0.3s; font-family: inherit; display: inline-flex; align-items: center; gap: 8px;
        }
        .btn-secondary:hover { border-color: var(--accent); color: var(--accent); background: #F0FDF4; }

        .toast-container { position: fixed; top: 20px; right: 20px; z-index: 9999; display: flex; flex-direction: column; gap: 8px; }
        .toast {
            padding: 14px 20px; border-radius: 12px; color: white; font-size: 14px; font-weight: 500;
            box-shadow: 0 8px 24px rgba(0,0,0,0.15); transform: translateX(120%); transition: transform 0.4s cubic-bezier(0.4, 0, 0.2, 1);
            display: flex; align-items: center; gap: 10px; max-width: 360px;
        }
        .toast.show { transform: translateX(0); }
        .toast.error { background: linear-gradient(135deg, #EF4444, #DC2626); }
        .toast.success { background: linear-gradient(135deg, #10B981, #059669); }
        .toast.info { background: linear-gradient(135deg, #0EA5E9, #0284C7); }

        .thankyou-page { display: none; text-align: center; padding: 60px 20px; }
        .thankyou-page.active { display: block; animation: fadeInUp 0.8s ease; }
        @keyframes fadeInUp {
            from { opacity: 0; transform: translateY(30px); }
            to { opacity: 1; transform: translateY(0); }
        }
        .checkmark-circle {
            width: 100px; height: 100px; border-radius: 50%; background: linear-gradient(135deg, #10B981, #059669);
            display: flex; align-items: center; justify-content: center; margin: 0 auto 30px;
            animation: scaleIn 0.6s cubic-bezier(0.175, 0.885, 0.32, 1.275) 0.2s both;
        }
        @keyframes scaleIn { from { transform: scale(0); } to { transform: scale(1); } }
        .checkmark-icon { font-size: 48px; color: white; animation: checkDraw 0.4s ease 0.6s both; }
        @keyframes checkDraw { from { opacity: 0; transform: scale(0.5); } to { opacity: 1; transform: scale(1); } }

        .confetti-piece {
            position: fixed; width: 10px; height: 10px; z-index: 9998; pointer-events: none;
            animation: confettiFall 3s ease-in forwards;
        }
        @keyframes confettiFall {
            0% { transform: translateY(-10vh) rotate(0deg); opacity: 1; }
            100% { transform: translateY(110vh) rotate(720deg); opacity: 0; }
        }

        .error-msg {
            color: var(--danger); font-size: 13px; font-weight: 500; margin-top: 6px;
            display: none; align-items: center; gap: 4px;
        }
        .error-msg.visible { display: flex; }

        .question-label {
            font-size: 15px; font-weight: 600; color: var(--fg); margin-bottom: 10px;
            display: flex; align-items: flex-start; gap: 6px; line-height: 1.5;
        }
        .question-label .req { color: var(--danger); font-weight: 700; }

        .spinner {
            width: 20px; height: 20px; border: 3px solid rgba(255,255,255,0.3);
            border-top-color: white; border-radius: 50%; animation: spin 0.8s linear infinite;
        }
        @keyframes spin { to { transform: rotate(360deg); } }

        @media (max-width: 640px) {
            .scale-group { gap: 4px; }
            .scale-btn { min-width: 48px; padding: 10px 4px 24px; border-radius: 8px; }
            .scale-btn .num { width: 26px; height: 26px; font-size: 12px; }
            .scale-btn .label { font-size: 8px; }
            .step-dot { width: 28px; height: 28px; font-size: 11px; }
        }
        @media (prefers-reduced-motion: reduce) {
            *, *::before, *::after { animation-duration: 0.01ms !important; transition-duration: 0.01ms !important; }
        }

        .pulse-ring {
            position: absolute; inset: -8px; border-radius: 50%; border: 2px solid rgba(16,185,129,0.3);
            animation: pulseRing 2s ease-out infinite;
        }
        @keyframes pulseRing { 0% { transform: scale(0.9); opacity: 1; } 100% { transform: scale(1.4); opacity: 0; } }

        .survey-card {
            background: var(--card); border-radius: 20px; box-shadow: 0 4px 24px rgba(0,0,0,0.04);
            border: 1px solid rgba(0,0,0,0.04); transition: box-shadow 0.3s;
        }
        textarea.input-field { resize: vertical; min-height: 120px; line-height: 1.6; }

        .section-icon {
            width: 48px; height: 48px; border-radius: 14px; display: flex; align-items: center; justify-content: center;
            font-size: 20px; flex-shrink: 0;
        }

        .submit-status-bar {
            display: none; align-items: center; gap: 10px; padding: 14px 18px;
            border-radius: 12px; margin-top: 16px; font-size: 13px; font-weight: 500;
            animation: fadeInUp 0.4s ease;
        }
        .submit-status-bar.sending { display: flex; background: #EFF6FF; color: #1E40AF; border: 1px solid #BFDBFE; }
        .submit-status-bar.done { display: flex; background: #ECFDF5; color: #065F46; border: 1px solid #A7F3D0; }
        .submit-status-bar.failed { display: flex; background: #FEF2F2; color: #991B1B; border: 1px solid #FECACA; }
    </style>
</head>
<body class="min-h-screen">
    <div class="bg-pattern"></div>
    <div class="floating-cell"></div>
    <div class="floating-cell"></div>
    <div class="floating-cell"></div>
    <div class="floating-cell"></div>
    <div class="floating-cell"></div>

    <div class="toast-container" id="toastContainer"></div>

    <header class="relative z-10 pt-6 pb-4 px-4">
        <div class="max-w-2xl mx-auto text-center">
            <div class="inline-flex items-center justify-center w-16 h-16 rounded-2xl bg-gradient-to-br from-med-500 to-med-700 mb-4 relative">
                <div class="pulse-ring"></div>
                <i class="fas fa-hospital text-white text-2xl"></i>
            </div>
            <h1 class="text-2xl sm:text-3xl font-extrabold text-gray-900 tracking-tight">Survei Kepuasan Pasien</h1>
            <p class="text-sm sm:text-base text-gray-500 mt-2 max-w-md mx-auto leading-relaxed">
                Pendapat Anda sangat berarti bagi peningkatan layanan kami. Seluruh data dijaga kerahasiaannya.
            </p>
        </div>
    </header>

    <div class="relative z-10 px-4 mb-2" id="progressSection">
        <div class="max-w-2xl mx-auto">
            <div class="flex items-center justify-between mb-3" id="stepDots"></div>
            <div class="progress-track">
                <div class="progress-fill" id="progressFill" style="width: 0%"></div>
            </div>
            <p class="text-xs text-gray-400 mt-2 text-center" id="stepLabel">Langkah 1 dari 7</p>
        </div>
    </div>

    <main class="relative z-10 px-4 pb-12">
        <div class="max-w-2xl mx-auto">
            <form id="surveyForm" novalidate>

                <!-- LANGKAH 1: Data Responden -->
                <section class="form-step active" data-step="1">
                    <div class="survey-card p-6 sm:p-8">
                        <div class="flex items-center gap-3 mb-6">
                            <div class="section-icon bg-med-100 text-med-700"><i class="fas fa-user-circle"></i></div>
                            <div>
                                <h2 class="text-lg font-bold text-gray-900">Data Responden</h2>
                                <p class="text-xs text-gray-400">Lengkapi informasi diri Anda</p>
                            </div>
                        </div>
                        <div class="space-y-5">
                            <div>
                                <label class="question-label"><span class="req">*</span> Nama Lengkap</label>
                                <input type="text" name="nama_lengkap" class="input-field" placeholder="Masukkan nama lengkap" required>
                                <div class="error-msg" id="err-nama_lengkap"><i class="fas fa-exclamation-circle"></i> Nama lengkap wajib diisi</div>
                            </div>
                            <div>
                                <label class="question-label"><span class="req">*</span> Usia</label>
                                <input type="number" name="usia" class="input-field" placeholder="Contoh: 35" min="1" max="120" required>
                                <div class="error-msg" id="err-usia"><i class="fas fa-exclamation-circle"></i> Usia wajib diisi (1-120)</div>
                            </div>
                            <div>
                                <label class="question-label"><span class="req">*</span> Jenis Kelamin</label>
                                <div class="grid grid-cols-2 gap-3" data-group="jenis_kelamin">
                                    <div class="radio-card" data-value="Laki-laki" tabindex="0" role="radio" aria-checked="false">
                                        <div class="radio-dot"></div><span class="font-medium text-sm">Laki-laki</span>
                                    </div>
                                    <div class="radio-card" data-value="Perempuan" tabindex="0" role="radio" aria-checked="false">
                                        <div class="radio-dot"></div><span class="font-medium text-sm">Perempuan</span>
                                    </div>
                                </div>
                                <input type="hidden" name="jenis_kelamin" required>
                                <div class="error-msg" id="err-jenis_kelamin"><i class="fas fa-exclamation-circle"></i> Pilih jenis kelamin</div>
                            </div>
                            <div>
                                <label class="question-label"><span class="req">*</span> No. Rekam Medis / No. Registrasi</label>
                                <input type="text" name="no_rm" class="input-field" placeholder="Contoh: RM-2024-001234" required>
                                <div class="error-msg" id="err-no_rm"><i class="fas fa-exclamation-circle"></i> No. Rekam Medis wajib diisi</div>
                            </div>
                            <div>
                                <label class="question-label"><span class="req">*</span> Tanggal Kunjungan</label>
                                <input type="date" name="tanggal_kunjungan" class="input-field" required>
                                <div class="error-msg" id="err-tanggal_kunjungan"><i class="fas fa-exclamation-circle"></i> Tanggal kunjungan wajib diisi</div>
                            </div>
                            <div>
                                <label class="question-label"><span class="req">*</span> Unit / Poliklinik yang Dikunjungi</label>
                                <select name="unit_poliklinik" class="select-field" required>
                                    <option value="">-- Pilih Unit / Poliklinik --</option>
                                    <option value="Poliklinik Umum">Poliklinik Umum</option>
                                    <option value="Poliklinik Penyakit Dalam">Poliklinik Penyakit Dalam</option>
                                    <option value="Poliklinik Anak">Poliklinik Anak</option>
                                    <option value="Poliklinik Kebidanan">Poliklinik Kebidanan</option>
                                    <option value="Poliklinik Bedah">Poliklinik Bedah</option>
                                    <option value="Poliklinik THT">Poliklinik THT</option>
                                    <option value="Poliklinik Mata">Poliklinik Mata</option>
                                    <option value="Poliklinik Kulit & Kelamin">Poliklinik Kulit & Kelamin</option>
                                    <option value="Poliklinik Gigi">Poliklinik Gigi</option>
                                    <option value="Poliklinik Orthopedi">Poliklinik Orthopedi</option>
                                    <option value="Poliklinik Saraf">Poliklinik Saraf</option>
                                    <option value="Poliklinik Jantung">Poliklinik Jantung</option>
                                    <option value="IGD">IGD (Instalasi Gawat Darurat)</option>
                                    <option value="Rawat Inap">Rawat Inap</option>
                                    <option value="Lainnya">Lainnya</option>
                                </select>
                                <div class="error-msg" id="err-unit_poliklinik"><i class="fas fa-exclamation-circle"></i> Pilih unit/poliklinik</div>
                            </div>
                            <div>
                                <label class="question-label"><span class="req">*</span> Jenis Kunjungan</label>
                                <div class="grid grid-cols-3 gap-3" data-group="jenis_kunjungan">
                                    <div class="radio-card" data-value="Rawat Jalan" tabindex="0" role="radio" aria-checked="false">
                                        <div class="radio-dot"></div><span class="font-medium text-sm">Rawat Jalan</span>
                                    </div>
                                    <div class="radio-card" data-value="Rawat Inap" tabindex="0" role="radio" aria-checked="false">
                                        <div class="radio-dot"></div><span class="font-medium text-sm">Rawat Inap</span>
                                    </div>
                                    <div class="radio-card" data-value="IGD" tabindex="0" role="radio" aria-checked="false">
                                        <div class="radio-dot"></div><span class="font-medium text-sm">IGD</span>
                                    </div>
                                </div>
                                <input type="hidden" name="jenis_kunjungan" required>
                                <div class="error-msg" id="err-jenis_kunjungan"><i class="fas fa-exclamation-circle"></i> Pilih jenis kunjungan</div>
                            </div>
                        </div>
                    </div>
                </section>

                <!-- LANGKAH 2: Pelayanan Pendaftaran -->
                <section class="form-step" data-step="2">
                    <div class="survey-card p-6 sm:p-8">
                        <div class="flex items-center gap-3 mb-2">
                            <div class="section-icon bg-blue-100 text-blue-700"><i class="fas fa-clipboard-list"></i></div>
                            <div>
                                <h2 class="text-lg font-bold text-gray-900">Pelayanan Pendaftaran</h2>
                                <p class="text-xs text-gray-400">Berikan penilaian pada aspek pelayanan pendaftaran</p>
                            </div>
                        </div>
                        <div class="bg-med-50 rounded-xl p-3 mb-6 text-xs text-med-800 font-medium flex items-center gap-2">
                            <i class="fas fa-info-circle"></i>
                            1 = Sangat Tidak Puas &nbsp;|&nbsp; 2 = Tidak Puas &nbsp;|&nbsp; 3 = Cukup Puas &nbsp;|&nbsp; 4 = Puas &nbsp;|&nbsp; 5 = Sangat Puas
                        </div>
                        <div class="space-y-7">
                            <div>
                                <label class="question-label"><span class="req">*</span> Kemudahan proses pendaftaran</label>
                                <div class="scale-group" data-name="kemudahan_pendaftaran"></div>
                                <input type="hidden" name="kemudahan_pendaftaran" required>
                                <div class="error-msg" id="err-kemudahan_pendaftaran"><i class="fas fa-exclamation-circle"></i> Berikan penilaian</div>
                            </div>
                            <div>
                                <label class="question-label"><span class="req">*</span> Keramahan petugas pendaftaran</label>
                                <div class="scale-group" data-name="keramahan_petugas_pendaftaran"></div>
                                <input type="hidden" name="keramahan_petugas_pendaftaran" required>
                                <div class="error-msg" id="err-keramahan_petugas_pendaftaran"><i class="fas fa-exclamation-circle"></i> Berikan penilaian</div>
                            </div>
                            <div>
                                <label class="question-label"><span class="req">*</span> Kecepatan pelayanan pendaftaran</label>
                                <div class="scale-group" data-name="kecepatan_pendaftaran"></div>
                                <input type="hidden" name="kecepatan_pendaftaran" required>
                                <div class="error-msg" id="err-kecepatan_pendaftaran"><i class="fas fa-exclamation-circle"></i> Berikan penilaian</div>
                            </div>
                            <div>
                                <label class="question-label"><span class="req">*</span> Kejelasan informasi dari petugas pendaftaran</label>
                                <div class="scale-group" data-name="kejelasan_info_pendaftaran"></div>
                                <input type="hidden" name="kejelasan_info_pendaftaran" required>
                                <div class="error-msg" id="err-kejelasan_info_pendaftaran"><i class="fas fa-exclamation-circle"></i> Berikan penilaian</div>
                            </div>
                        </div>
                    </div>
                </section>

                <!-- LANGKAH 3: Pelayanan Tenaga Medis -->
                <section class="form-step" data-step="3">
                    <div class="survey-card p-6 sm:p-8">
                        <div class="flex items-center gap-3 mb-2">
                            <div class="section-icon bg-rose-100 text-rose-700"><i class="fas fa-stethoscope"></i></div>
                            <div>
                                <h2 class="text-lg font-bold text-gray-900">Pelayanan Tenaga Medis</h2>
                                <p class="text-xs text-gray-400">Penilaian terhadap perawat dan dokter</p>
                            </div>
                        </div>
                        <div class="bg-med-50 rounded-xl p-3 mb-6 text-xs text-med-800 font-medium flex items-center gap-2">
                            <i class="fas fa-info-circle"></i>
                            1 = Sangat Tidak Puas &nbsp;|&nbsp; 2 = Tidak Puas &nbsp;|&nbsp; 3 = Cukup Puas &nbsp;|&nbsp; 4 = Puas &nbsp;|&nbsp; 5 = Sangat Puas
                        </div>
                        <div class="space-y-7">
                            <div>
                                <label class="question-label"><span class="req">*</span> Keramahan dan sikap perawat</label>
                                <div class="scale-group" data-name="keramahan_perawat"></div>
                                <input type="hidden" name="keramahan_perawat" required>
                                <div class="error-msg" id="err-keramahan_perawat"><i class="fas fa-exclamation-circle"></i> Berikan penilaian</div>
                            </div>
                            <div>
                                <label class="question-label"><span class="req">*</span> Ketanggapan perawat dalam merespons kebutuhan pasien</label>
                                <div class="scale-group" data-name="ketanggapan_perawat"></div>
                                <input type="hidden" name="ketanggapan_perawat" required>
                                <div class="error-msg" id="err-ketanggapan_perawat"><i class="fas fa-exclamation-circle"></i> Berikan penilaian</div>
                            </div>
                            <div>
                                <label class="question-label"><span class="req">*</span> Kompetensi pelayanan dokter</label>
                                <div class="scale-group" data-name="kompetensi_dokter"></div>
                                <input type="hidden" name="kompetensi_dokter" required>
                                <div class="error-msg" id="err-kompetensi_dokter"><i class="fas fa-exclamation-circle"></i> Berikan penilaian</div>
                            </div>
                            <div>
                                <label class="question-label"><span class="req">*</span> Keramahan dan sikap dokter</label>
                                <div class="scale-group" data-name="keramahan_dokter"></div>
                                <input type="hidden" name="keramahan_dokter" required>
                                <div class="error-msg" id="err-keramahan_dokter"><i class="fas fa-exclamation-circle"></i> Berikan penilaian</div>
                            </div>
                            <div>
                                <label class="question-label"><span class="req">*</span> Kejelasan penjelasan dokter mengenai kondisi dan pengobatan</label>
                                <div class="scale-group" data-name="kejelasan_dokter"></div>
                                <input type="hidden" name="kejelasan_dokter" required>
                                <div class="error-msg" id="err-kejelasan_dokter"><i class="fas fa-exclamation-circle"></i> Berikan penilaian</div>
                            </div>
                        </div>
                    </div>
                </section>

                <!-- LANGKAH 4: Pelayanan Penunjang -->
                <section class="form-step" data-step="4">
                    <div class="survey-card p-6 sm:p-8">
                        <div class="flex items-center gap-3 mb-2">
                            <div class="section-icon bg-amber-100 text-amber-700"><i class="fas fa-pills"></i></div>
                            <div>
                                <h2 class="text-lg font-bold text-gray-900">Pelayanan Penunjang</h2>
                                <p class="text-xs text-gray-400">Penilaian terhadap farmasi dan laboratorium</p>
                            </div>
                        </div>
                        <div class="bg-med-50 rounded-xl p-3 mb-6 text-xs text-med-800 font-medium flex items-center gap-2">
                            <i class="fas fa-info-circle"></i>
                            1 = Sangat Tidak Puas &nbsp;|&nbsp; 2 = Tidak Puas &nbsp;|&nbsp; 3 = Cukup Puas &nbsp;|&nbsp; 4 = Puas &nbsp;|&nbsp; 5 = Sangat Puas
                        </div>
                        <div class="space-y-7">
                            <div>
                                <label class="question-label"><span class="req">*</span> Keramahan petugas farmasi</label>
                                <div class="scale-group" data-name="keramahan_farmasi"></div>
                                <input type="hidden" name="keramahan_farmasi" required>
                                <div class="error-msg" id="err-keramahan_farmasi"><i class="fas fa-exclamation-circle"></i> Berikan penilaian</div>
                            </div>
                            <div>
                                <label class="question-label"><span class="req">*</span> Kecepatan pelayanan farmasi</label>
                                <div class="scale-group" data-name="kecepatan_farmasi"></div>
                                <input type="hidden" name="kecepatan_farmasi" required>
                                <div class="error-msg" id="err-kecepatan_farmasi"><i class="fas fa-exclamation-circle"></i> Berikan penilaian</div>
                            </div>
                            <div>
                                <label class="question-label"><span class="req">*</span> Kejelasan informasi cara penggunaan obat</label>
                                <div class="scale-group" data-name="kejelasan_info_obat"></div>
                                <input type="hidden" name="kejelasan_info_obat" required>
                                <div class="error-msg" id="err-kejelasan_info_obat"><i class="fas fa-exclamation-circle"></i> Berikan penilaian</div>
                            </div>
                            <div>
                                <label class="question-label"><span class="req">*</span> Pelayanan laboratorium / penunjang medis</label>
                                <div class="scale-group" data-name="pelayanan_lab"></div>
                                <input type="hidden" name="pelayanan_lab" required>
                                <div class="error-msg" id="err-pelayanan_lab"><i class="fas fa-exclamation-circle"></i> Berikan penilaian</div>
                            </div>
                        </div>
                    </div>
                </section>

                <!-- LANGKAH 5: Fasilitas & Lingkungan -->
                <section class="form-step" data-step="5">
                    <div class="survey-card p-6 sm:p-8">
                        <div class="flex items-center gap-3 mb-2">
                            <div class="section-icon bg-cyan-100 text-cyan-700"><i class="fas fa-building"></i></div>
                            <div>
                                <h2 class="text-lg font-bold text-gray-900">Fasilitas & Lingkungan</h2>
                                <p class="text-xs text-gray-400">Penilaian terhadap kebersihan dan fasilitas</p>
                            </div>
                        </div>
                        <div class="bg-med-50 rounded-xl p-3 mb-6 text-xs text-med-800 font-medium flex items-center gap-2">
                            <i class="fas fa-info-circle"></i>
                            1 = Sangat Tidak Puas &nbsp;|&nbsp; 2 = Tidak Puas &nbsp;|&nbsp; 3 = Cukup Puas &nbsp;|&nbsp; 4 = Puas &nbsp;|&nbsp; 5 = Sangat Puas
                        </div>
                        <div class="space-y-7">
                            <div>
                                <label class="question-label"><span class="req">*</span> Kebersihan ruang tunggu</label>
                                <div class="scale-group" data-name="kebersihan_ruang_tunggu"></div>
                                <input type="hidden" name="kebersihan_ruang_tunggu" required>
                                <div class="error-msg" id="err-kebersihan_ruang_tunggu"><i class="fas fa-exclamation-circle"></i> Berikan penilaian</div>
                            </div>
                            <div>
                                <label class="question-label"><span class="req">*</span> Kebersihan ruang perawatan / pemeriksaan</label>
                                <div class="scale-group" data-name="kebersihan_ruang_perawatan"></div>
                                <input type="hidden" name="kebersihan_ruang_perawatan" required>
                                <div class="error-msg" id="err-kebersihan_ruang_perawatan"><i class="fas fa-exclamation-circle"></i> Berikan penilaian</div>
                            </div>
                            <div>
                                <label class="question-label"><span class="req">*</span> Kenyamanan fasilitas ruang tunggu</label>
                                <div class="scale-group" data-name="kenyamanan_fasilitas"></div>
                                <input type="hidden" name="kenyamanan_fasilitas" required>
                                <div class="error-msg" id="err-kenyamanan_fasilitas"><i class="fas fa-exclamation-circle"></i> Berikan penilaian</div>
                            </div>
                            <div>
                                <label class="question-label"><span class="req">*</span> Kemudahan akses dan petunjuk arah di rumah sakit</label>
                                <div class="scale-group" data-name="kemudahan_akses"></div>
                                <input type="hidden" name="kemudahan_akses" required>
                                <div class="error-msg" id="err-kemudahan_akses"><i class="fas fa-exclamation-circle"></i> Berikan penilaian</div>
                            </div>
                            <div>
                                <label class="question-label"><span class="req">*</span> Ketersediaan sarana (toilet, parkir, tempat ibadah, dll)</label>
                                <div class="scale-group" data-name="ketersediaan_sarana"></div>
                                <input type="hidden" name="ketersediaan_sarana" required>
                                <div class="error-msg" id="err-ketersediaan_sarana"><i class="fas fa-exclamation-circle"></i> Berikan penilaian</div>
                            </div>
                        </div>
                    </div>
                </section>

                <!-- LANGKAH 6: Administrasi & Biaya -->
                <section class="form-step" data-step="6">
                    <div class="survey-card p-6 sm:p-8">
                        <div class="flex items-center gap-3 mb-2">
                            <div class="section-icon bg-violet-100 text-violet-700"><i class="fas fa-file-invoice-dollar"></i></div>
                            <div>
                                <h2 class="text-lg font-bold text-gray-900">Administrasi & Biaya</h2>
                                <p class="text-xs text-gray-400">Penilaian terhadap proses administrasi dan biaya</p>
                            </div>
                        </div>
                        <div class="bg-med-50 rounded-xl p-3 mb-6 text-xs text-med-800 font-medium flex items-center gap-2">
                            <i class="fas fa-info-circle"></i>
                            1 = Sangat Tidak Puas &nbsp;|&nbsp; 2 = Tidak Puas &nbsp;|&nbsp; 3 = Cukup Puas &nbsp;|&nbsp; 4 = Puas &nbsp;|&nbsp; 5 = Sangat Puas
                        </div>
                        <div class="space-y-7">
                            <div>
                                <label class="question-label"><span class="req">*</span> Kejelasan informasi biaya pelayanan</label>
                                <div class="scale-group" data-name="kejelasan_biaya"></div>
                                <input type="hidden" name="kejelasan_biaya" required>
                                <div class="error-msg" id="err-kejelasan_biaya"><i class="fas fa-exclamation-circle"></i> Berikan penilaian</div>
                            </div>
                            <div>
                                <label class="question-label"><span class="req">*</span> Kecepatan proses administrasi pembayaran</label>
                                <div class="scale-group" data-name="kecepatan_pembayaran"></div>
                                <input type="hidden" name="kecepatan_pembayaran" required>
                                <div class="error-msg" id="err-kecepatan_pembayaran"><i class="fas fa-exclamation-circle"></i> Berikan penilaian</div>
                            </div>
                            <div>
                                <label class="question-label"><span class="req">*</span> Kewajaran biaya pelayanan dibandingkan layanan yang diterima</label>
                                <div class="scale-group" data-name="kewajaran_biaya"></div>
                                <input type="hidden" name="kewajaran_biaya" required>
                                <div class="error-msg" id="err-kewajaran_biaya"><i class="fas fa-exclamation-circle"></i> Berikan penilaian</div>
                            </div>
                        </div>
                    </div>
                </section>

                <!-- LANGKAH 7: Penilaian Keseluruhan -->
                <section class="form-step" data-step="7">
                    <div class="survey-card p-6 sm:p-8">
                        <div class="flex items-center gap-3 mb-2">
                            <div class="section-icon bg-emerald-100 text-emerald-700"><i class="fas fa-star"></i></div>
                            <div>
                                <h2 class="text-lg font-bold text-gray-900">Penilaian Keseluruhan</h2>
                                <p class="text-xs text-gray-400">Kesan umum dan saran Anda</p>
                            </div>
                        </div>
                        <div class="bg-med-50 rounded-xl p-3 mb-6 text-xs text-med-800 font-medium flex items-center gap-2">
                            <i class="fas fa-info-circle"></i>
                            1 = Sangat Tidak Puas &nbsp;|&nbsp; 2 = Tidak Puas &nbsp;|&nbsp; 3 = Cukup Puas &nbsp;|&nbsp; 4 = Puas &nbsp;|&nbsp; 5 = Sangat Puas
                        </div>
                        <div class="space-y-7">
                            <div>
                                <label class="question-label"><span class="req">*</span> Secara keseluruhan, seberapa puas Anda dengan pelayanan kami?</label>
                                <div class="scale-group" data-name="kepuasan_keseluruhan"></div>
                                <input type="hidden" name="kepuasan_keseluruhan" required>
                                <div class="error-msg" id="err-kepuasan_keseluruhan"><i class="fas fa-exclamation-circle"></i> Berikan penilaian</div>
                            </div>
                            <div>
                                <label class="question-label"><span class="req">*</span> Apakah Anda akan merekomendasikan rumah sakit kami kepada keluarga atau teman?</label>
                                <div class="grid grid-cols-3 gap-3" data-group="rekomendasi">
                                    <div class="radio-card" data-value="Ya" tabindex="0" role="radio" aria-checked="false">
                                        <div class="radio-dot"></div><span class="font-medium text-sm">Ya</span>
                                    </div>
                                    <div class="radio-card" data-value="Mungkin" tabindex="0" role="radio" aria-checked="false">
                                        <div class="radio-dot"></div><span class="font-medium text-sm">Mungkin</span>
                                    </div>
                                    <div class="radio-card" data-value="Tidak" tabindex="0" role="radio" aria-checked="false">
                                        <div class="radio-dot"></div><span class="font-medium text-sm">Tidak</span>
                                    </div>
                                </div>
                                <input type="hidden" name="rekomendasi" required>
                                <div class="error-msg" id="err-rekomendasi"><i class="fas fa-exclamation-circle"></i> Pilih salah satu</div>
                            </div>
                            <div>
                                <label class="question-label"><span class="req">*</span> Saran dan masukan untuk peningkatan pelayanan</label>
                                <textarea name="saran" class="input-field" placeholder="Tuliskan saran atau masukan Anda di sini..." required></textarea>
                                <div class="error-msg" id="err-saran"><i class="fas fa-exclamation-circle"></i> Saran/masukan wajib diisi</div>
                            </div>
                        </div>
                    </div>
                </section>

                <!-- Navigasi -->
                <div class="flex items-center justify-between mt-6 gap-3" id="formNavigation">
                    <button type="button" id="btnPrev" class="btn-secondary" style="visibility: hidden;">
                        <i class="fas fa-arrow-left"></i> Sebelumnya
                    </button>
                    <button type="button" id="btnNext" class="btn-primary">
                        Selanjutnya <i class="fas fa-arrow-right"></i>
                    </button>
                    <button type="submit" id="btnSubmit" class="btn-primary" style="display: none;">
                        <span id="submitText">Kirim Survei</span>
                        <div class="spinner" id="submitSpinner" style="display: none;"></div>
                    </button>
                </div>

                <!-- Status bar pengiriman -->
                <div class="submit-status-bar" id="submitStatusBar"></div>
            </form>

            <!-- Halaman Terima Kasih -->
            <div class="thankyou-page" id="thankyouPage">
                <div class="survey-card p-8 sm:p-12">
                    <div class="checkmark-circle">
                        <i class="fas fa-check checkmark-icon"></i>
                    </div>
                    <h2 class="text-2xl sm:text-3xl font-extrabold text-gray-900 mb-3">Terima Kasih</h2>
                    <p class="text-gray-500 text-base sm:text-lg leading-relaxed max-w-md mx-auto mb-8">
                        Terima kasih atas partisipasi Anda. Masukan Anda sangat berharga bagi kami untuk meningkatkan kualitas pelayanan.
                    </p>
                    <div class="inline-flex items-center gap-2 text-sm text-med-700 bg-med-50 px-5 py-3 rounded-xl font-medium">
                        <i class="fas fa-shield-alt"></i>
                        Data Anda dijaga kerahasiaannya sesuai regulasi privasi
                    </div>
                </div>
            </div>
        </div>
    </main>

    <footer class="relative z-10 text-center py-6 px-4 border-t border-gray-100">
        <p class="text-xs text-gray-400">
            <i class="fas fa-lock text-gray-300 mr-1"></i>
            Data survei dijaga kerahasiaannya dan hanya digunakan untuk peningkatan pelayanan
        </p>
    </footer>

    <script>
    // ============================================================
    //  KONFIGURASI
    //  Ganti URL di bawah dengan URL Web App yang Anda dapatkan
    //  setelah deploy Google Apps Script (Langkah 2 di atas).
    // ============================================================
    const SCRIPT_URL = 'https://script.google.com/macros/s/AKfycbyV5YLGUGMdtZp6oMeg_uVJpQwQ4v8MqgTdKcE0od8jQcoh4kOkiowb9Q94Fg1fdps/exec';

    // Timeout pengiriman (15 detik)
    const SEND_TIMEOUT = 15000;

    const TOTAL_STEPS = 7;
    let currentStep = 1;

    const scaleLabels = ['Sangat Tidak Puas', 'Tidak Puas', 'Cukup Puas', 'Puas', 'Sangat Puas'];

    // Urutan kolom (harus sama persis dengan header di sheet)
    const COLUMN_ORDER = [
        'timestamp',
        'nama_lengkap', 'usia', 'jenis_kelamin', 'no_rm', 'tanggal_kunjungan',
        'unit_poliklinik', 'jenis_kunjungan',
        'kemudahan_pendaftaran', 'keramahan_petugas_pendaftaran', 'kecepatan_pendaftaran',
        'kejelasan_info_pendaftaran', 'keramahan_perawat', 'ketanggapan_perawat',
        'kompetensi_dokter', 'keramahan_dokter', 'kejelasan_dokter',
        'keramahan_farmasi', 'kecepatan_farmasi', 'kejelasan_info_obat', 'pelayanan_lab',
        'kebersihan_ruang_tunggu', 'kebersihan_ruang_perawatan', 'kenyamanan_fasilitas',
        'kemudahan_akses', 'ketersediaan_sarana',
        'kejelasan_biaya', 'kecepatan_pembayaran', 'kewajaran_biaya',
        'kepuasan_keseluruhan', 'rekomendasi', 'saran'
    ];

    document.addEventListener('DOMContentLoaded', () => {
        buildStepDots();
        buildScaleGroups();
        setupRadioCards();
        setupNavigation();
        updateProgress();
    });

    function buildStepDots() {
        const c = document.getElementById('stepDots');
        c.innerHTML = '';
        for (let i = 1; i <= TOTAL_STEPS; i++) {
            if (i > 1) { const l = document.createElement('div'); l.className = 'step-line'; l.id = `stepLine${i}`; c.appendChild(l); }
            const d = document.createElement('div');
            d.className = 'step-dot'; d.id = `stepDot${i}`; d.textContent = i;
            d.setAttribute('aria-label', `Langkah ${i}`); c.appendChild(d);
        }
    }

    function buildScaleGroups() {
        document.querySelectorAll('.scale-group').forEach(group => {
            const name = group.dataset.name;
            group.innerHTML = '';
            for (let i = 1; i <= 5; i++) {
                const btn = document.createElement('div');
                btn.className = 'scale-btn'; btn.dataset.val = i; btn.tabIndex = 0;
                btn.setAttribute('role', 'radio');
                btn.setAttribute('aria-label', `${scaleLabels[i - 1]} (${i})`);
                btn.setAttribute('aria-checked', 'false');
                btn.innerHTML = `<div class="num">${i}</div><div class="label">${scaleLabels[i - 1]}</div>`;
                btn.addEventListener('click', () => selectScale(group, name, i));
                btn.addEventListener('keydown', (e) => {
                    if (e.key === 'Enter' || e.key === ' ') { e.preventDefault(); selectScale(group, name, i); }
                });
                group.appendChild(btn);
            }
        });
    }

    function selectScale(group, name, value) {
        group.querySelectorAll('.scale-btn').forEach(b => { b.classList.remove('selected'); b.setAttribute('aria-checked', 'false'); });
        const sel = group.querySelector(`.scale-btn[data-val="${value}"]`);
        if (sel) { sel.classList.add('selected'); sel.setAttribute('aria-checked', 'true'); }
        const h = document.querySelector(`input[name="${name}"]`);
        if (h) h.value = value;
        clearError(name);
    }

    function setupRadioCards() {
        document.querySelectorAll('[data-group]').forEach(group => {
            const gn = group.dataset.group;
            group.querySelectorAll('.radio-card').forEach(card => {
                card.addEventListener('click', () => selectRadio(group, gn, card));
                card.addEventListener('keydown', (e) => {
                    if (e.key === 'Enter' || e.key === ' ') { e.preventDefault(); selectRadio(group, gn, card); }
                });
            });
        });
    }

    function selectRadio(group, gn, card) {
        group.querySelectorAll('.radio-card').forEach(c => { c.classList.remove('selected'); c.setAttribute('aria-checked', 'false'); });
        card.classList.add('selected'); card.setAttribute('aria-checked', 'true');
        const h = document.querySelector(`input[name="${gn}"]`);
        if (h) h.value = card.dataset.value;
        clearError(gn);
    }

    function setupNavigation() {
        document.getElementById('btnPrev').addEventListener('click', prevStep);
        document.getElementById('btnNext').addEventListener('click', nextStep);
        document.getElementById('surveyForm').addEventListener('submit', handleSubmit);
    }

    function validateStep(step) {
        const sec = document.querySelector(`.form-step[data-step="${step}"]`);
        let valid = true;
        sec.querySelectorAll('input[type="text"], input[type="number"], input[type="date"], select, textarea').forEach(inp => {
            if (inp.hasAttribute('required') && !inp.value.trim()) { showError(inp.name); inp.classList.add('error'); valid = false; }
            else { clearError(inp.name); inp.classList.remove('error'); }
        });
        sec.querySelectorAll('input[type="hidden"]').forEach(inp => {
            if (inp.hasAttribute('required') && !inp.value.trim()) { showError(inp.name); valid = false; }
        });
        const ua = sec.querySelector('input[name="usia"]');
        if (ua && ua.value) { const a = parseInt(ua.value); if (isNaN(a) || a < 1 || a > 120) { showError('usia'); ua.classList.add('error'); valid = false; } }
        if (!valid) {
            showToast('Harap lengkapi semua pertanyaan yang wajib diisi', 'error');
            const fe = sec.querySelector('.error-msg.visible');
            if (fe) fe.scrollIntoView({ behavior: 'smooth', block: 'center' });
        }
        return valid;
    }

    function showError(f) { const e = document.getElementById(`err-${f}`); if (e) e.classList.add('visible'); }
    function clearError(f) { const e = document.getElementById(`err-${f}`); if (e) e.classList.remove('visible'); const i = document.querySelector(`[name="${f}"]`); if (i) i.classList.remove('error'); }

    function nextStep() { if (!validateStep(currentStep)) return; if (currentStep < TOTAL_STEPS) goToStep(currentStep + 1); }
    function prevStep() { if (currentStep > 1) goToStep(currentStep - 1); }

    function goToStep(step) {
        const o = document.querySelector(`.form-step[data-step="${currentStep}"]`);
        const n = document.querySelector(`.form-step[data-step="${step}"]`);
        if (o) { o.classList.remove('active'); o.classList.add('exit-left'); setTimeout(() => o.classList.remove('exit-left'), 500); }
        currentStep = step;
        if (n) { n.classList.add('active'); window.scrollTo({ top: 0, behavior: 'smooth' }); }
        updateProgress();
    }

    function updateProgress() {
        const p = ((currentStep - 1) / (TOTAL_STEPS - 1)) * 100;
        document.getElementById('progressFill').style.width = `${p}%`;
        document.getElementById('stepLabel').textContent = `Langkah ${currentStep} dari ${TOTAL_STEPS}`;
        for (let i = 1; i <= TOTAL_STEPS; i++) {
            const d = document.getElementById(`stepDot${i}`);
            if (d) { d.classList.remove('active', 'completed'); if (i === currentStep) d.classList.add('active'); else if (i < currentStep) d.classList.add('completed'); }
            if (i > 1) { const l = document.getElementById(`stepLine${i}`); if (l) l.classList.toggle('completed', i <= currentStep); }
        }
        const bp = document.getElementById('btnPrev'), bn = document.getElementById('btnNext'), bs = document.getElementById('btnSubmit');
        bp.style.visibility = currentStep === 1 ? 'hidden' : 'visible';
        if (currentStep === TOTAL_STEPS) { bn.style.display = 'none'; bs.style.display = 'inline-flex'; }
        else { bn.style.display = 'inline-flex'; bs.style.display = 'none'; }
    }

    // ============================================================
    //  SUBMIT — Primary ke tab "Utama", Backup ke tab "Cadangan"
    //  Kedua tab berada di spreadsheet yang sama.
    // ============================================================

    async function handleSubmit(e) {
        e.preventDefault();
        if (!validateStep(currentStep)) return;

        const btn = document.getElementById('btnSubmit');
        const txt = document.getElementById('submitText');
        const spn = document.getElementById('submitSpinner');

        btn.disabled = true; txt.textContent = 'Mengirim...'; spn.style.display = 'block';

        const surveyData = collectFormData();

        updateSubmitStatus('sending', 'Mengirim data survei...');

        try {
            // TAHAP 1: Kirim ke tab "Utama" di spreadsheet
            const primaryOk = await sendToSheet('Utama', surveyData, false);

            if (primaryOk) {
                updateSubmitStatus('done', 'Data berhasil tersimpan');
                console.log('[Survei] Berhasil tersimpan di tab Utama');
            } else {
                // TAHAP 2: Gagal, kirim ke tab "Cadangan" dengan status_pengiriman
                console.warn('[Survei] Gagal kirim ke Utama, mengalihkan ke Cadangan...');
                updateSubmitStatus('sending', 'Mengirim ke penyimpanan cadangan...');

                const backupOk = await sendToSheet('Cadangan', surveyData, true);

                if (backupOk) {
                    updateSubmitStatus('done', 'Data berhasil tersimpan');
                    console.log('[Survei] Berhasil tersimpan di tab Cadangan');
                } else {
                    updateSubmitStatus('failed', 'Gagal menyimpan data. Silakan coba lagi.');
                    showToast('Terjadi kendala koneksi. Silakan coba kirim ulang.', 'error');
                    btn.disabled = false; txt.textContent = 'Kirim Survei'; spn.style.display = 'none';
                    return;
                }
            }

            showThankYouPage();
            launchConfetti();

        } catch (err) {
            console.error('[Survei] Error:', err);
            // Fallback terakhir ke Cadangan
            updateSubmitStatus('sending', 'Mengirim ke penyimpanan cadangan...');
            try {
                const lastOk = await sendToSheet('Cadangan', surveyData, true);
                if (lastOk) {
                    updateSubmitStatus('done', 'Data berhasil tersimpan');
                    showThankYouPage(); launchConfetti();
                } else {
                    updateSubmitStatus('failed', 'Gagal menyimpan data. Silakan coba lagi.');
                    showToast('Tidak dapat terhubung. Periksa koneksi internet Anda.', 'error');
                    btn.disabled = false; txt.textContent = 'Kirim Survei'; spn.style.display = 'none';
                }
            } catch (finalErr) {
                updateSubmitStatus('failed', 'Gagal menyimpan data. Silakan coba lagi.');
                showToast('Tidak dapat terhubung ke server.', 'error');
                btn.disabled = false; txt.textContent = 'Kirim Survei'; spn.style.display = 'none';
            }
        }
    }

    // Kirim data ke satu sheet tab
    // targetSheet: nama tab ("Utama" atau "Cadangan")
    // isBackup: jika true, tambahkan kolom status_pengiriman = "backup"
    async function sendToSheet(targetSheet, data, isBackup) {
        try {
            // Susun values sesuai COLUMN_ORDER
            let values = COLUMN_ORDER.map(col => data[col] || '');
            if (isBackup) values.push('backup');

            const payload = { targetSheet: targetSheet, values: values };

            const controller = new AbortController();
            const tid = setTimeout(() => controller.abort(), SEND_TIMEOUT);

            await fetch(SCRIPT_URL, {
                method: 'POST',
                mode: 'no-cors',
                headers: { 'Content-Type': 'application/json' },
                body: JSON.stringify(payload),
                signal: controller.signal
            });

            clearTimeout(tid);
            return true;

        } catch (err) {
            if (err.name === 'AbortError') {
                console.warn(`[Survei] Timeout mengirim ke tab "${targetSheet}"`);
            } else {
                console.warn(`[Survei] Gagal kirim ke tab "${targetSheet}":`, err.message);
            }
            return false;
        }
    }

    // Kumpulkan semua data form
    function collectFormData() {
        const data = {};
        const fd = new FormData(document.getElementById('surveyForm'));
        data.timestamp = new Date().toLocaleString('id-ID', {
            year: 'numeric', month: '2-digit', day: '2-digit',
            hour: '2-digit', minute: '2-digit', second: '2-digit'
        });
        for (const [k, v] of fd.entries()) data[k] = v.trim();
        COLUMN_ORDER.forEach(c => { if (!data[c]) data[c] = ''; });
        return data;
    }

    // Status bar pengiriman
    function updateSubmitStatus(state, message) {
        const bar = document.getElementById('submitStatusBar');
        bar.className = 'submit-status-bar';
        const icons = {
            sending: '<div class="spinner" style="width:16px;height:16px;border-width:2px;border-color:rgba(30,64,175,0.3);border-top-color:#1E40AF;"></div>',
            done: '<i class="fas fa-check-circle"></i>',
            failed: '<i class="fas fa-times-circle"></i>'
        };
        bar.innerHTML = `${icons[state] || ''}<span>${message}</span>`;
        bar.classList.add(state);
    }

    // Halaman terima kasih
    function showThankYouPage() {
        document.getElementById('surveyForm').style.display = 'none';
        document.getElementById('formNavigation').style.display = 'none';
        document.getElementById('submitStatusBar').style.display = 'none';
        document.getElementById('progressSection').style.display = 'none';
        document.getElementById('thankyouPage').classList.add('active');
        window.scrollTo({ top: 0, behavior: 'smooth' });
    }

    // Confetti
    function launchConfetti() {
        const colors = ['#10B981','#059669','#34D399','#6EE7B7','#FCD34D','#F472B6','#60A5FA'];
        for (let i = 0; i < 60; i++) {
            setTimeout(() => {
                const p = document.createElement('div');
                p.className = 'confetti-piece';
                const c = colors[Math.floor(Math.random() * colors.length)];
                const s = Math.random() * 8 + 6;
                const shape = Math.random() > 0.5;
                p.style.cssText = `width:${s}px;height:${s}px;background:${c};border-radius:${shape ? '50%' : '2px'};left:${Math.random()*100}vw;animation-duration:${Math.random()*2+2}s;animation-delay:${Math.random()*0.5}s;`;
                document.body.appendChild(p);
                setTimeout(() => p.remove(), 4000);
            }, i * 30);
        }
    }

    // Toast
    function showToast(message, type = 'info') {
        const c = document.getElementById('toastContainer');
        const t = document.createElement('div');
        t.className = `toast ${type}`;
        const icons = { error: 'fas fa-exclamation-triangle', success: 'fas fa-check-circle', info: 'fas fa-info-circle' };
        t.innerHTML = `<i class="${icons[type] || icons.info}"></i><span>${message}</span>`;
        c.appendChild(t);
        requestAnimationFrame(() => t.classList.add('show'));
        setTimeout(() => { t.classList.remove('show'); setTimeout(() => t.remove(), 400); }, 4000);
    }

    // Hapus error saat user mengetik/memilih
    document.addEventListener('input', (e) => { if (e.target.name) { clearError(e.target.name); e.target.classList.remove('error'); } });
    document.addEventListener('change', (e) => { if (e.target.name && e.target.tagName === 'SELECT') { clearError(e.target.name); e.target.classList.remove('error'); } });
    </script>
</body>
</html>
