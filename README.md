<!DOCTYPE html>
<html lang="ar" dir="rtl" id="html-tag">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Control Risks | OFP Tracker</title>
    <!-- Bootstrap -->
    <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0/dist/css/bootstrap.rtl.min.css" id="bootstrap-rtl">
    <link href="https://fonts.googleapis.com/css2?family=Cairo:wght@400;700&display=swap" rel="stylesheet">
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <!-- SheetJS للتصدير -->
    <script src="https://cdnjs.cloudflare.com/ajax/libs/xlsx/0.18.5/xlsx.full.min.js"></script>

    <style>
        :root { --cr-olive: #3D4928; --cr-olive-light: #556B2F; --cr-dark: #1a1a1a; --cr-light: #f4f7f1; }
        body { font-family: 'Cairo', sans-serif; background-color: var(--cr-light); margin: 0; transition: all 0.3s ease; }
        
        #sidebar { width: 260px; background-color: var(--cr-dark); color: white; position: fixed; height: 100%; z-index: 1000; transition: 0.3s; }
        .sidebar-header { background: var(--cr-olive); padding: 30px 15px; text-align: center; border-bottom: 2px solid rgba(255,255,255,0.1); }
        .sidebar-header h5 { color: white; margin: 0; font-weight: bold; letter-spacing: 1px; }
        
        /* تنسيق أزرار القائمة الجانبية */
        #sidebar ul { list-style: none; padding: 0; margin-top: 10px; }
        #sidebar ul li a { 
            padding: 15px 20px; 
            display: block; 
            color: #cbd5e0; 
            text-decoration: none; 
            border-bottom: 1px solid #2d3748; 
            cursor: pointer; 
            transition: all 0.3s ease;
            border-right: 4px solid transparent; /* حافة شفافة للحفاظ على الأبعاد */
        }
        
        /* تأثير عند وضع الماوس (Hover) */
        #sidebar ul li a:hover { 
            background: rgba(85, 107, 47, 0.4); /* لون زيتوني شفاف */
            color: white; 
            padding-right: 25px; 
        }
        
        /* تأثير الصفحة النشطة (Active) */
        #sidebar ul li a.active { 
            background: var(--cr-olive-light); 
            color: white; 
            font-weight: bold;
            border-right: 4px solid white; /* شريط أبيض مميز */
            padding-right: 25px;
            box-shadow: inset 0 0 10px rgba(0,0,0,0.2);
        }
        
        .content { margin-right: 260px; min-height: 100vh; transition: 0.3s; }
        .top-navbar { background: white; padding: 15px 30px; display: flex; justify-content: space-between; align-items: center; box-shadow: 0 2px 10px rgba(0,0,0,0.05); }
        
        .btn-cr { background-color: var(--cr-olive); color: white; border: none; transition: 0.3s; }
        .btn-cr:hover { background-color: var(--cr-olive-light); transform: translateY(-2px); box-shadow: 0 4px 8px rgba(0,0,0,0.2); color: white; }
        
        .custom-card { background: white; border-radius: 12px; padding: 20px; box-shadow: 0 4px 20px rgba(0,0,0,0.04); margin-bottom: 20px; }
        
        #login-page { height: 100vh; display: flex; align-items: center; justify-content: center; background: linear-gradient(135deg, var(--cr-dark) 0%, var(--cr-olive) 100%); position: fixed; width: 100%; z-index: 2000; }
        .login-card { background: white; padding: 50px; border-radius: 20px; width: 100%; max-width: 450px; text-align: center; box-shadow: 0 20px 40px rgba(0,0,0,0.3); }
        .login-card h2 { color: var(--cr-olive); font-weight: bold; margin-bottom: 5px; }
        
        /* ضبط اتجاه اللغات LTR / RTL */
        .ltr-mode .content { margin-right: 0; margin-left: 260px; }
        .ltr-mode #sidebar { right: auto; left: 0; }
        .ltr-mode #sidebar ul li a { border-right: none; border-left: 4px solid transparent; padding-right: 20px; padding-left: 20px; }
        .ltr-mode #sidebar ul li a:hover { padding-left: 25px; padding-right: 20px; }
        .ltr-mode #sidebar ul li a.active { border-left: 4px solid white; padding-left: 25px; padding-right: 20px; }
        
        .ref-badge { font-size: 0.75rem; padding: 2px 8px; border-radius: 4px; background: #f0f4e8; color: var(--cr-olive); border: 1px solid var(--cr-olive); font-weight: bold; }
        
        /* فلاتر مصغرة */
        .filter-box { background: #f8f9fa; padding: 10px 15px; border-radius: 6px; border: 1px solid #dee2e6; margin-bottom: 15px; }
        .filter-box .form-control, .filter-box .form-select { font-size: 0.85rem; padding: 4px 8px; height: 30px; }
        .filter-box .btn { height: 30px; padding: 2px 10px; font-size: 0.85rem; }

        .names-preview { font-size: 0.75rem; background: #fafafa; border: 1px solid #eee; padding: 8px; border-radius: 6px; margin-top: 8px; max-height: 90px; overflow-y: auto; color: #444; line-height: 1.6; }
        .names-preview strong { color: var(--cr-olive); }
    </style>
</head>
<body class="rtl-mode text-start">

    <!-- صفحة تسجيل الدخول -->
    <div id="login-page">
        <div class="login-card shadow-lg">
            <h2>CONTROL RISKS</h2>
            <p class="text-muted mb-4 fw-bold">OFP Application Tracker</p>
            <form id="loginForm">
                <div class="mb-3 text-start"><label>اسم المستخدم</label><input type="text" id="user" class="form-control" required></div>
                <div class="mb-4 text-start"><label>كلمة المرور</label><input type="password" id="pass" class="form-control" required></div>
                <button type="submit" class="btn btn-cr w-100 p-2 fw-bold">دخول</button>
                <div class="mt-3"><a href="#" class="text-muted small text-decoration-none" onclick="alert('يرجى التواصل مع: compad.southiraq@controlrisks.com')">نسيت كلمة السر؟</a></div>
            </form>
        </div>
    </div>

    <!-- الواجهة الرئيسية -->
    <div id="main-dashboard" style="display: none;">
        <nav id="sidebar">
            <div class="sidebar-header"><h5>CONTROL RISKS</h5></div>
            <ul>
                <li><a onclick="showTab('authorities')" id="link-auth" class="active"><i class="fa fa-folder-open ms-2"></i> الطلبات للهيئات</a></li>
                <li><a onclick="showTab('permits')" id="link-permits"><i class="fa fa-id-card-clip ms-2"></i> التصاريح المستلمة</a></li>
                <li id="admin-menu" style="display: none;"><a onclick="showTab('users')" id="link-users"><i class="fa fa-users-gear ms-2"></i> إدارة الحسابات</a></li>
                <li><a onclick="logout()"><i class="fa fa-power-off ms-2"></i> خروج</a></li>
            </ul>
        </nav>

        <div class="content">
            <header class="top-navbar">
                <div><button class="btn btn-sm btn-outline-secondary" onclick="toggleLanguage()">EN / AR</button></div>
                <div>مرحباً: <strong id="user-name" class="text-success"></strong></div>
            </header>

            <div class="p-4">
                <!-- سجل الهيئات -->
                <div id="authorities-page">
                    <div class="d-flex justify-content-between mb-2 align-items-center">
                        <h3 class="fw-bold">سجل الطلبات للهيئات</h3>
                        <button class="btn btn-cr shadow-sm px-4" onclick="openModal('auth')"><i class="fa fa-plus-circle me-1"></i> إضافة طلب</button>
                    </div>
                    
                    <div class="filter-box row g-1 align-items-center">
                        <div class="col-md-3"><input type="text" id="f-auth-name" class="form-control" placeholder="اسم الكتاب..." onkeyup="render()"></div>
                        <div class="col-md-2"><input type="text" id="f-auth-memo" class="form-control" placeholder="المذكرة..." onkeyup="render()"></div>
                        <div class="col-md-3"><input type="date" id="f-auth-date" class="form-control" title="تاريخ الإرسال/الاستلام" onchange="render()"></div>
                        <div class="col-md-2">
                            <select id="f-auth-status" class="form-select" onchange="render()">
                                <option value="">كل الحالات</option>
                                <option value="Pending">Pending (معلق)</option>
                                <option value="Done">Completed (مكتمل)</option>
                            </select>
                        </div>
                        <div class="col-md-2"><button class="btn btn-secondary w-100" onclick="clearFilters('auth')"><i class="fa fa-sync"></i> مسح</button></div>
                    </div>

                    <div class="custom-card table-responsive">
                        <table class="table table-hover align-middle border text-center">
                            <thead class="table-dark">
                                <tr>
                                    <th>Ref</th>
                                    <th style="width: 30%;">اسم الكتاب وتفاصيل القوائم</th>
                                    <th>الهيئة / النوع</th>
                                    <th>المذكرة <i class="fa fa-paperclip"></i></th>
                                    <th>إرسال</th>
                                    <th>استلام</th>
                                    <th>الحالة</th>
                                    <th>الإجراء</th>
                                </tr>
                            </thead>
                            <tbody id="auth-table-body"></tbody>
                        </table>
                        <div class="mt-3 text-end">
                            <button class="btn btn-success btn-sm" onclick="exportData('auth', 'excel')"><i class="fa fa-file-excel"></i> Excel</button>
                            <button class="btn btn-secondary btn-sm" onclick="exportData('auth', 'csv')"><i class="fa fa-file-csv"></i> CSV</button>
                        </div>
                    </div>
                </div>

                <!-- سجل التصاريح -->
                <div id="permits-page" style="display: none;">
                    <h3 class="fw-bold mb-2" style="color: var(--cr-olive);">سجل التصاريح المستلمة</h3>
                    
                    <div class="filter-box row g-1 align-items-center">
                        <div class="col-md-3"><input type="text" id="f-perm-name" class="form-control" placeholder="اسم الكتاب..." onkeyup="render()"></div>
                        <div class="col-md-2"><input type="text" id="f-perm-ofp" class="form-control" placeholder="OFP..." onkeyup="render()"></div>
                        <div class="col-md-2"><input type="text" id="f-perm-memo" class="form-control" placeholder="المذكرة..." onkeyup="render()"></div>
                        <div class="col-md-2"><input type="date" id="f-perm-date" class="form-control" onchange="render()"></div>
                        <div class="col-md-2">
                            <select id="f-perm-status" class="form-select" onchange="render()">
                                <option value="">كل الحالات</option>
                                <option value="Pending">Pending</option>
                                <option value="Completed">Completed</option>
                            </select>
                        </div>
                        <div class="col-md-1"><button class="btn btn-secondary w-100" onclick="clearFilters('perm')"><i class="fa fa-sync"></i></button></div>
                    </div>

                    <div class="custom-card table-responsive">
                        <table class="table table-bordered align-middle border text-center">
                            <thead class="table-success">
                                <tr>
                                    <th>Ref / OFP</th>
                                    <th style="width: 30%;">التصريح الجديد وتفاصيل القوائم</th>
                                    <th>المذكرة <i class="fa fa-paperclip"></i></th>
                                    <th>إرسال</th>
                                    <th>استلام</th>
                                    <th>الحالة</th>
                                    <th>إجراء</th>
                                </tr>
                            </thead>
                            <tbody id="permits-table-body"></tbody>
                        </table>
                        <div class="mt-3 text-end">
                            <button class="btn btn-success btn-sm" onclick="exportData('permits', 'excel')"><i class="fa fa-file-excel"></i> Excel</button>
                            <button class="btn btn-secondary btn-sm" onclick="exportData('permits', 'csv')"><i class="fa fa-file-csv"></i> CSV</button>
                        </div>
                    </div>
                </div>

                <!-- إدارة الحسابات -->
                <div id="users-page" style="display: none;">
                    <h3 class="fw-bold mb-4">إدارة الحسابات</h3>
                    <div class="custom-card">
                        <div class="row g-3 mb-4 text-start">
                            <input type="hidden" id="edit-user-index" value="-1">
                            <div class="col-md-4"><label>اسم المستخدم</label><input id="nu" class="form-control"></div>
                            <div class="col-md-4"><label>الباسورد</label><input id="np" class="form-control"></div>
                            <div class="col-md-4 d-flex align-items-end"><button id="btn-save-user" class="btn btn-success w-100 fw-bold" onclick="addUser()">إضافة حساب</button></div>
                        </div>
                        <table class="table table-striped table-bordered text-center">
                            <thead class="table-dark"><tr><th>المستخدم</th><th>الباسورد</th><th>الرتبة</th><th>الإجراءات</th></tr></thead>
                            <tbody id="utb"></tbody>
                        </table>
                    </div>
                </div>
            </div>
        </div>
    </div>

    <!-- Modal Form -->
    <div class="modal fade" id="mainModal" tabindex="-1" aria-hidden="true">
        <div class="modal-dialog modal-xl">
            <div class="modal-content text-start">
                <div class="modal-header bg-dark text-white"><h5 class="modal-title">بيانات الطلب</h5><button type="button" class="btn-close btn-close-white" data-bs-dismiss="modal"></button></div>
                <div class="modal-body">
                    <form id="orderForm">
                        <input type="hidden" id="edit-id"><input type="hidden" id="form-type">
                        <div class="row g-3">
                            <div class="col-md-3"><label>رقم الكتاب</label><input type="text" id="f-bn" class="form-control" required></div>
                            <div class="col-md-3"><label>نوع التصريح</label><select id="f-pt" class="form-select"><option value="مؤقت">مؤقت</option><option value="دائمي">دائمي</option></select></div>
                            <div class="col-md-3"><label>الهيئة</label><select id="f-ta" class="form-select"><option value="BGC">BGC</option><option value="ROO">ROO</option><option value="SRC">SRC</option></select></div>
                            <div class="col-md-3" id="ofp-container"><label>OFP Number</label><input type="text" id="f-ofp" class="form-control"></div>
                            
                            <div class="col-md-3"><label>تاريخ الإرسال</label><input type="date" id="f-sd" class="form-control"></div>
                            <div class="col-md-3"><label>تاريخ الاستلام</label><input type="date" id="f-rd" class="form-control"></div>
                            <div class="col-md-3"><label>رقم المذكرة</label><input type="text" id="f-memo" class="form-control"></div>
                            <div class="col-md-3">
                                <label>مرفق المذكرة (اختياري)</label>
                                <input type="file" id="f-file" class="form-control" accept=".pdf, image/*">
                            </div>

                            <div class="col-12"><div class="alert alert-light border small text-center mt-2">انسخ القوائم من الوورد مباشرة في الخانات أدناه</div></div>
                            <div class="col-md-3"><label>العجلات</label><textarea id="f-w" class="form-control" rows="3"></textarea></div>
                            <div class="col-md-3"><label>الأجانب</label><textarea id="f-e" class="form-control" rows="3"></textarea></div>
                            <div class="col-md-3"><label>العراقيين</label><textarea id="f-l" class="form-control" rows="3"></textarea></div>
                            <div class="col-md-3"><label>الأسلحة</label><textarea id="f-wp" class="form-control" rows="3"></textarea></div>
                        </div>
                        <button type="submit" class="btn btn-cr w-100 mt-4 p-2 fw-bold">حفظ البيانات</button>
                    </form>
                </div>
            </div>
        </div>
    </div>

    <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0/dist/js/bootstrap.bundle.min.js"></script>
    <script>
        let authoritiesData = JSON.parse(localStorage.getItem('auth_data')) || [];
        let permitsData = JSON.parse(localStorage.getItem('perm_data')) || [];
        let users = JSON.parse(localStorage.getItem('users_data')) || [{u: 'admin', p: '1234', r: 'admin'}];
        
        let currentUser = null;
        let currentLang = 'ar';
        const modal = new bootstrap.Modal(document.getElementById('mainModal'));

        function saveAll() {
            localStorage.setItem('auth_data', JSON.stringify(authoritiesData));
            localStorage.setItem('perm_data', JSON.stringify(permitsData));
            localStorage.setItem('users_data', JSON.stringify(users));
        }

        function toggleLanguage() {
            currentLang = currentLang === 'ar' ? 'en' : 'ar';
            document.getElementById('html-tag').dir = currentLang === 'en' ? 'ltr' : 'rtl';
            document.getElementById('bootstrap-rtl').href = currentLang === 'en' ? "https://cdn.jsdelivr.net/npm/bootstrap@5.3.0/dist/css/bootstrap.min.css" : "https://cdn.jsdelivr.net/npm/bootstrap@5.3.0/dist/css/bootstrap.rtl.min.css";
            document.body.className = currentLang === 'en' ? 'ltr-mode text-start' : 'rtl-mode text-start';
        }

        document.getElementById('loginForm').onsubmit = (e) => {
            e.preventDefault();
            const u = document.getElementById('user').value, p = document.getElementById('pass').value;
            const f = users.find(x => x.u === u && x.p === p);
            if(f) {
                currentUser = f;
                document.getElementById('login-page').style.display = 'none';
                document.getElementById('main-dashboard').style.display = 'block';
                document.getElementById('user-name').innerText = f.u;
                if(f.r === 'admin') document.getElementById('admin-menu').style.display = 'block';
                render();
            } else alert("❌ خطأ في بيانات الدخول");
        };

        function showTab(t) {
            ['authorities','permits','users'].forEach(p => document.getElementById(p+'-page').style.display='none');
            document.getElementById(t+'-page').style.display='block';
            document.querySelectorAll('#sidebar a').forEach(a => a.classList.remove('active'));
            document.getElementById('link-'+t).classList.add('active');
        }

        function logout() { location.reload(); }

        function fileToBase64(file) {
            return new Promise((resolve, reject) => {
                const reader = new FileReader();
                reader.readAsDataURL(file);
                reader.onload = () => resolve(reader.result);
                reader.onerror = error => reject(error);
            });
        }

        async function openModal(type, id = null) {
            document.getElementById('orderForm').reset();
            document.getElementById('form-type').value = type;
            document.getElementById('edit-id').value = id || "";
            document.getElementById('ofp-container').style.display = (type === 'permits') ? 'block' : 'none';
            document.getElementById('f-file').removeAttribute('data-saved-file');

            if(id) {
                const item = (type === 'auth' ? authoritiesData : permitsData).find(i => i.id == id);
                if(item) {
                    document.getElementById('f-bn').value = item.bn_orig || "";
                    document.getElementById('f-pt').value = item.pt || "مؤقت";
                    document.getElementById('f-ta').value = item.ta || "BGC";
                    document.getElementById('f-memo').value = item.memo || "";
                    document.getElementById('f-ofp').value = item.ofp || "";
                    document.getElementById('f-sd').value = item.sd || "";
                    document.getElementById('f-rd').value = item.rd || "";
                    document.getElementById('f-w').value = item.w ? item.w.join('\n') : "";
                    document.getElementById('f-e').value = item.e ? item.e.join('\n') : "";
                    document.getElementById('f-l').value = item.l ? item.l.join('\n') : "";
                    document.getElementById('f-wp').value = item.wp ? item.wp.join('\n') : "";
                    if(item.fileData) document.getElementById('f-file').setAttribute('data-saved-file', item.fileData);
                }
            }
            modal.show();
        }

        document.getElementById('orderForm').onsubmit = async (e) => {
            e.preventDefault();
            const type = document.getElementById('form-type').value, id = document.getElementById('edit-id').value;
            const getL = (eid) => document.getElementById(eid).value.split('\n').filter(l => l.trim() !== "");
            let ex = id ? (type === 'auth' ? authoritiesData : permitsData).find(i => i.id == id) : null;

            const fileInput = document.getElementById('f-file');
            let fileData = fileInput.getAttribute('data-saved-file') || null;
            if(fileInput.files.length > 0) fileData = await fileToBase64(fileInput.files[0]);

            const entry = {
                id: id ? parseInt(id) : Date.now(), bn_orig: document.getElementById('f-bn').value, pt: document.getElementById('f-pt').value,
                ta: document.getElementById('f-ta').value, memo: document.getElementById('f-memo').value, ofp: document.getElementById('f-ofp').value,
                sd: document.getElementById('f-sd').value, rd: document.getElementById('f-rd').value, w: getL('f-w'), e: getL('f-e'), l: getL('f-l'), wp: getL('f-wp'),
                creator: ex ? ex.creator : currentUser.u, status: 'Pending', sysRef: ex ? ex.sysRef : `REF-${Math.floor(1000 + Math.random() * 9000)}`,
                fileData: fileData
            };

            const countStr = `${entry.e.length} أجانب - ${entry.w.length} عجلات - ${entry.l.length} عراقيين وفقط`;
            
            if(type === 'auth') {
                entry.bn = `TEMP - ${entry.bn_orig} - ${entry.pt} - ${countStr}`;
                entry.status = entry.rd ? 'Done' : 'Pending';

                if(id) {
                    authoritiesData[authoritiesData.findIndex(i => i.id == id)] = entry;
                    let pIdx = permitsData.findIndex(p => p.sysRef === entry.sysRef);
                    if(pIdx !== -1) {
                        permitsData[pIdx].pt = entry.pt; permitsData[pIdx].ta = entry.ta; permitsData[pIdx].w = entry.w; permitsData[pIdx].e = entry.e; permitsData[pIdx].l = entry.l; permitsData[pIdx].wp = entry.wp;
                        permitsData[pIdx].bn = `ISCO - ${permitsData[pIdx].bn_orig || ""} - ${entry.pt} - ${countStr}`;
                        permitsData[pIdx].fileData = entry.fileData;
                    }
                } else authoritiesData.push(entry);

                if(entry.status === 'Done') {
                    const exist = permitsData.find(p => p.sysRef === entry.sysRef);
                    if(!exist) {
                        let copy = JSON.parse(JSON.stringify(entry));
                        copy.id = Date.now() + Math.floor(Math.random() * 100); 
                        copy.bn_orig = ""; copy.bn = ""; copy.ofp = ""; copy.sd = ""; copy.rd = ""; copy.status = 'Pending';
                        permitsData.push(copy);
                    }
                }
            } else {
                entry.status = entry.rd ? 'Completed' : 'Pending'; 
                entry.bn = `ISCO - ${entry.bn_orig} - ${entry.pt} - ${countStr}`;
                permitsData[permitsData.findIndex(i => i.id == id)] = entry;
            }
            saveAll(); render(); modal.hide();
        };

        function deleteRecord(type, id) {
            if(confirm("هل أنت متأكد من الحذف نهائياً؟")) {
                if(type === 'auth') authoritiesData = authoritiesData.filter(i => i.id !== id);
                else permitsData = permitsData.filter(i => i.id !== id);
                saveAll(); render();
            }
        }

        function clearFilters(type) {
            if(type === 'auth') {
                document.getElementById('f-auth-name').value = ''; document.getElementById('f-auth-memo').value = '';
                document.getElementById('f-auth-date').value = ''; document.getElementById('f-auth-status').value = '';
            } else {
                document.getElementById('f-perm-name').value = ''; document.getElementById('f-perm-ofp').value = '';
                document.getElementById('f-perm-memo').value = ''; document.getElementById('f-perm-date').value = ''; document.getElementById('f-perm-status').value = '';
            }
            render();
        }

        function openFile(base64Data) {
            let win = window.open();
            win.document.write('<iframe src="' + base64Data + '" frameborder="0" style="border:0; top:0px; left:0px; bottom:0px; right:0px; width:100%; height:100%;" allowfullscreen></iframe>');
        }

        function generateNamesPreview(item) {
            let html = '';
            if(item.w.length > 0 || item.e.length > 0 || item.l.length > 0 || item.wp.length > 0) {
                html += `<div class="names-preview">`;
                if(item.w.length > 0) html += `<strong>🚗 العجلات:</strong> ${item.w.join(', ')}<br>`;
                if(item.e.length > 0) html += `<strong>👤 الأجانب:</strong> ${item.e.join(', ')}<br>`;
                if(item.l.length > 0) html += `<strong>👨🏽 العراقيين:</strong> ${item.l.join(', ')}<br>`;
                if(item.wp.length > 0) html += `<strong>🔫 الأسلحة:</strong> ${item.wp.join(', ')}<br>`;
                html += `</div>`;
            }
            return html;
        }

        function render() {
            const fAuthName = document.getElementById('f-auth-name').value.toLowerCase();
            const fAuthMemo = document.getElementById('f-auth-memo').value.toLowerCase();
            const fAuthDate = document.getElementById('f-auth-date').value;
            const fAuthStatus = document.getElementById('f-auth-status').value;

            const filteredAuth = authoritiesData.filter(i => {
                return (i.bn.toLowerCase().includes(fAuthName)) && ((i.memo || '').toLowerCase().includes(fAuthMemo)) &&
                       (fAuthDate ? (i.sd === fAuthDate || i.rd === fAuthDate) : true) && (fAuthStatus ? (i.status === fAuthStatus) : true);
            });

            const ab = document.getElementById('auth-table-body'); ab.innerHTML = '';
            filteredAuth.forEach(i => {
                const statusBadge = i.status === 'Done' ? '<span class="badge bg-success">Completed</span>' : '<span class="badge bg-warning text-dark">Pending</span>';
                const fileIcon = i.fileData ? `<a href="#" onclick="openFile('${i.fileData}')" class="text-danger ms-2" title="عرض المرفق"><i class="fa fa-paperclip fa-lg"></i></a>` : '';
                
                ab.innerHTML += `<tr>
                    <td><span class="badge bg-secondary">${i.sysRef}</span></td>
                    <td class="text-start"><span class="fw-bold">${i.bn}</span>${generateNamesPreview(i)}</td>
                    <td><span class="badge bg-secondary mb-1">${i.ta}</span><br><small class="text-muted">${i.pt}</small></td>
                    <td>${i.memo || '-'}${fileIcon}</td>
                    <td>${i.sd || '-'}</td><td>${i.rd || '-'}</td>
                    <td>${statusBadge}</td>
                    <td><div class="btn-group"><button class="btn btn-sm btn-outline-dark" onclick="openModal('auth', ${i.id})"><i class="fa fa-edit"></i></button>
                    <button class="btn btn-sm btn-outline-danger" onclick="deleteRecord('auth', ${i.id})"><i class="fa fa-trash"></i></button></div></td>
                </tr>`;
            });

            const fPermName = document.getElementById('f-perm-name').value.toLowerCase();
            const fPermOfp = document.getElementById('f-perm-ofp').value.toLowerCase();
            const fPermMemo = document.getElementById('f-perm-memo').value.toLowerCase();
            const fPermDate = document.getElementById('f-perm-date').value;
            const fPermStatus = document.getElementById('f-perm-status').value;

            const filteredPerm = permitsData.filter(i => {
                return (i.bn.toLowerCase().includes(fPermName)) && ((i.ofp || '').toLowerCase().includes(fPermOfp)) &&
                       ((i.memo || '').toLowerCase().includes(fPermMemo)) && (fPermDate ? (i.sd === fPermDate || i.rd === fPermDate) : true) &&
                       (fPermStatus ? (i.status === fPermStatus) : true);
            });

            const pb = document.getElementById('permits-table-body'); pb.innerHTML = '';
            filteredPerm.forEach(i => {
                const s = i.status === 'Completed' ? '<span class="badge bg-success">Completed</span>' : '<span class="badge bg-warning text-dark">Pending</span>';
                const fileIcon = i.fileData ? `<a href="#" onclick="openFile('${i.fileData}')" class="text-danger ms-2" title="عرض المرفق"><i class="fa fa-paperclip fa-lg"></i></a>` : '';

                pb.innerHTML += `<tr>
                    <td><span class="badge bg-secondary">${i.sysRef}</span><br><span class="text-primary fw-bold small">${i.ofp || 'No OFP'}</span></td>
                    <td class="text-start"><span class="fw-bold">${i.bn}</span>${generateNamesPreview(i)}</td>
                    <td>${i.memo || '-'}${fileIcon}</td>
                    <td>${i.sd || '-'}</td><td>${i.rd || '-'}</td><td>${s}</td>
                    <td><div class="btn-group"><button class="btn btn-sm btn-outline-dark" onclick="openModal('permits', ${i.id})"><i class="fa fa-edit"></i></button>
                    <button class="btn btn-sm btn-outline-danger" onclick="deleteRecord('permits', ${i.id})"><i class="fa fa-trash"></i></button></div></td>
                </tr>`;
            });
            renderUsersTable();
        }

        function renderUsersTable() {
            const utb = document.getElementById('utb'); utb.innerHTML = '';
            users.forEach((u, index) => {
                utb.innerHTML += `<tr>
                    <td>${u.u}</td><td>${u.p}</td><td>${u.r}</td>
                    <td>${u.r !== 'admin' ? `
                        <button class="btn btn-sm btn-warning" onclick="editUser(${index})"><i class="fa fa-edit"></i></button>
                        <button class="btn btn-sm btn-danger" onclick="deleteUser(${index})"><i class="fa fa-trash"></i></button>` : '-'}
                    </td>
                </tr>`;
            });
        }

        function editUser(index) {
            document.getElementById('nu').value = users[index].u; document.getElementById('np').value = users[index].p;
            document.getElementById('edit-user-index').value = index; document.getElementById('btn-save-user').innerText = "تحديث الحساب";
        }

        function addUser() {
            if(currentUser.r !== 'admin') return;
            const u = document.getElementById('nu').value, p = document.getElementById('np').value;
            const editIdx = parseInt(document.getElementById('edit-user-index').value);

            if(u && p) {
                if(editIdx !== -1) { users[editIdx].u = u; users[editIdx].p = p; document.getElementById('edit-user-index').value = -1; document.getElementById('btn-save-user').innerText = "إضافة حساب"; } 
                else users.push({u, p, r:'user'});
                document.getElementById('nu').value=''; document.getElementById('np').value='';
                saveAll(); renderUsersTable();
            }
        }
        function deleteUser(i) { users.splice(i, 1); saveAll(); renderUsersTable(); }

        function exportData(type, format) {
            let dataToExport = [];
            let sourceData = type === 'auth' ? authoritiesData : permitsData;
            let fileName = type === 'auth' ? 'Authorities_Requests' : 'Permits_Received';

            sourceData.forEach(item => {
                dataToExport.push({
                    "System Ref": item.sysRef, "Book Name": item.bn, "Authority": item.ta, "Type": item.pt,
                    "Memo No": item.memo || 'N/A', "OFP No": item.ofp || 'N/A', "Send Date": item.sd || 'N/A',
                    "Receive Date": item.rd || 'N/A', "Creator": item.creator, "Status": item.status,
                    "Wheels": item.w.join(", "), "Expats": item.e.join(", "), "Locals": item.l.join(", "), "Weapons": item.wp.join(", ")
                });
            });

            const ws = XLSX.utils.json_to_sheet(dataToExport);
            const wb = XLSX.utils.book_new(); XLSX.utils.book_append_sheet(wb, ws, "Data");
            if(format === 'excel') XLSX.writeFile(wb, fileName + ".xlsx"); else XLSX.writeFile(wb, fileName + ".csv");
        }

        render();
    </script>
</body>
</html>
