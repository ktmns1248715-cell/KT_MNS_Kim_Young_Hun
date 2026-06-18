<html lang="ko">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>법인회선 재약정 견적서</title>
    <!-- 고해상도 그래픽 캡처 및 PDF 출력을 위한 표준 라이브러리 로드 -->
    <script src="https://cdnjs.cloudflare.com/ajax/libs/html2canvas/1.4.1/html2canvas.min.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js"></script>
    <style>
        @page {
            size: A4;
            margin: 0; 
        }

        body {
            font-family: -apple-system, BlinkMacSystemFont, "Apple SD Gothic Neo", "Pretendard Variable", Pretendard, "Malgun Gothic", "맑은 고딕", sans-serif;
            background-color: #f4f6f9;
            padding: 10px 0; 
            margin: 0;
            display: flex;
            flex-direction: column;
            align-items: center;
            -webkit-font-smoothing: antialiased;
        }

        /* 스마트폰/PC 다크모드 강제 무력화 선언 */
        @media (prefers-color-scheme: dark) {
            body { background-color: #f4f6f9 !important; }
            .invoice-container { background-color: #ffffff !important; color: #000000 !important; }
            th { background-color: #f1f5f9 !important; color: #000000 !important; }
            td { background-color: #ffffff !important; color: #000000 !important; border-color: #a0a0a0 !important; }
            input[type="text"], select { background-color: #f8fafc !important; color: #000000 !important; }
        }

        .invoice-container {
            width: 794px; 
            background-color: #ffffff;
            padding: 15px 30px 20px 30px; 
            box-shadow: 0 4px 20px rgba(0,0,0,0.08);
            box-sizing: border-box;
            image-rendering: -webkit-optimize-contrast;
        }

        .invoice-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 12px; 
            border-bottom: 3px solid #1c5280;
            padding-bottom: 5px;
        }
        
        .logo-area {
            font-size: 28px;
            font-weight: 900;
            color: #000000 !important; 
            font-family: 'Arial Black', Impact, sans-serif;
            letter-spacing: -2px;
            line-height: 1;
        }
        
        .title-area {
            font-size: 25px;
            font-weight: 800;
            color: #004b8d !important;
            text-align: center;
            flex-grow: 1;
            letter-spacing: -0.5px;
            margin-right: 40px;
        }

        table {
            width: 100%;
            border-collapse: collapse;
            font-size: 11px; 
            color: #000000 !important; 
            margin-bottom: 8px; 
            table-layout: fixed;
        }
        th, td {
            border: 1px solid #a0a0a0 !important; 
            padding: 4px 6px; 
            height: 26px; /* 행 높이를 26px로 넉넉하게 교정하여 글자 잘림 방지 */
            color: #000000 !important;
            vertical-align: middle !important;
            text-align: center !important; 
            overflow: hidden;
            white-space: nowrap;
        }
        
        th {
            background-color: #f1f5f9 !important;
            font-weight: 700 !important;
            color: #000000 !important;
            text-align: center !important;
        }

        .info-table th { width: 14%; }
        .info-table td { width: 36%; }

        /* 일반 입력창 및 선택창 기본 스타일 */
        input[type="text"], input[type="date"], select {
            width: 100%;
            height: 100%;
            border: none !important;
            background-color: #f8fafc !important; 
            padding: 0 4px;
            border-radius: 3px;
            font-size: 11px;
            font-family: inherit;
            color: #000000 !important;
            -webkit-text-fill-color: #000000 !important;
            opacity: 1 !important;
            font-weight: 600; 
            box-sizing: border-box;
            outline: none;
            text-align: center !important;
            box-shadow: none !important;
        }

        /* [핵심 교정] 이메일, 연락처, 날짜 등 자동 입력용 전용 텍스트 컨테이너 스타일 (찌그러짐 완전 소멸) */
        .text-display-cell {
            width: 100%;
            height: 100%;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 11px;
            font-weight: 700;
            box-sizing: border-box;
            background-color: #f1f5f9 !important;
        }

        input[type="date"] {
            color: transparent !important;
            -webkit-text-fill-color: transparent !important;
        }
        input[type="date"].has-value {
            color: #000000 !important;
            -webkit-text-fill-color: #000000 !important;
        }
        
        input:focus, select:focus {
            background-color: #e0f2fe !important; 
        }

        input[readonly], select[readonly], .lock-cell {
            color: #475569 !important; 
            -webkit-text-fill-color: #475569 !important;
            background-color: #f1f5f9 !important;
            font-weight: 600;
            cursor: not-allowed; 
        }
        
        .blue-readonly {
            color: #004b8d !important;
        }

        .benefit-highlight {
            color: #d91414 !important; 
            font-size: 13px;
            font-weight: 800;
            text-align: center !important; 
            background-color: #fef2f2 !important;
            white-space: nowrap !important;
            letter-spacing: 0.5px;
        }

        .product-table th {
            background-color: #f1f5f9 !important;
            color: #000000 !important;
            font-weight: 700;
            font-size: 11px;
            white-space: nowrap; 
        }

        .total-row {
            background-color: #e2e8f0 !important;
            font-weight: 700;
        }
        .total-row td {
            font-size: 11px;
            color: #000000 !important;
            text-align: center !important; 
        }

        .notice-text {
            font-size: 11px; 
            color: #222222 !important; 
            line-height: 1.45; 
            font-weight: 500;
        }
        .bg-alert {
            background-color: #fffbeb !important; 
            color: #b45309 !important;
        }

        .btn-area {
            margin-top: 15px;
            margin-bottom: 20px;
            width: 794px;
            display: flex;
            gap: 15px; 
        }
        .download-btn {
            flex: 1;
            background-color: #004b8d;
            color: white;
            border: none;
            padding: 14px;
            font-size: 16px;
            font-weight: 700;
            cursor: pointer;
            border-radius: 6px;
            box-shadow: 0 4px 12px rgba(0, 61, 141, 0.25);
            transition: background 0.2s;
        }
        .download-btn:hover {
            background-color: #143757;
        }
        .pdf-btn {
            background-color: #10b981;
            box-shadow: 0 4px 12px rgba(16, 185, 129, 0.25);
        }
        .pdf-btn:hover {
            background-color: #047857;
        }

        .responsive-wrapper {
            width: 100%;
            overflow-x: auto;
            display: flex;
            flex-direction: column;
            align-items: center;
            -webkit-overflow-scrolling: touch;
        }

        .clean-capture input, .clean-capture select {
            background-color: transparent !important;
            box-shadow: none !important;
            border: none !important;
        }
        .clean-capture .text-display-cell {
            background-color: transparent !important;
        }

        @media print {
            body { background: none; padding: 0; margin: 0; }
            .btn-area { display: none; } 
            .invoice-container { box-shadow: none; padding: 20px 30px; width: 100%; }
            input, select { background-color: transparent !important; }
            .text-display-cell { background-color: transparent !important; }
        }
    </style>
</head>
<body>

    <div class="responsive-wrapper">
        <div class="invoice-container" id="invoice-capture-area">
            
            <!-- 상단 헤더 -->
            <div class="invoice-header">
                <div class="logo-area">kt</div>
                <div class="title-area">법인회선 재약정 견적서</div>
            </div>

            <!-- 기본 정보 테이블 -->
            <table class="info-table">
                <tr>
                    <th>견적일자</th>
                    <!-- [교정] input을 div 순수 텍스트 래핑 구조로 대체 -->
                    <td><div id="invoice-date" class="text-display-cell blue-readonly"></div></td>
                    <th>사업자번호</th>
                    <td><div class="text-display-cell">120-87-09780</div></td>
                </tr>
                <tr>
                    <th>업체명</th>
                    <td><input type="text" value=" 귀하" id="client-name" onfocus="clearGuidance(this)" onblur="restoreGuidance(this)"></td>
                    <th>회사명</th>
                    <td><div class="text-display-cell">(주) KT M&S</div></td>
                </tr>
                <tr>
                    <th>사업자번호</th>
                    <td><input type="text" value="" placeholder="고객 사업자번호 입력"></td>
                    <th>대표자명</th>
                    <td><div class="text-display-cell">박성열</div></td>
                </tr>
                <tr>
                    <th>총 제공되는 혜택</th>
                    <td class="benefit-highlight" id="total-benefits-display">₩0</td>
                    <th>주소</th>
                    <td><div class="text-display-cell" style="font-size:10px;">경기도 성남시 분당구 불정로 90(정자동)</div></td>
                </tr>
                <tr>
                    <th>수수료</th>
                    <td><input type="text" id="fee-input" value="0" oninput="runBenefitCalculations(this)"></td>
                    <th>업종</th>
                    <td><div class="text-display-cell">정보통신업</div></td>
                </tr>
                <tr>
                    <th>통합사은품</th>
                    <td><input type="text" id="gift-input" value="0" oninput="runBenefitCalculations(this)"></td>
                    <th>담당부서</th>
                    <td><div class="text-display-cell">KT M&S 동부본부 동부법인지사</div></td>
                </tr>
                <tr>
                    <th rowspan="2">재약정<br>구비서류</th>
                    <td rowspan="2" class="notice-text lock-cell" style="background-color: #fafafa; font-size: 10px; line-height: 1.45; user-select: none;">
                        법인 대표자 신분증 / 명함<br>사업자 등록증 (사본가능)
                    </td>
                    <th>담당자</th>
                    <td>
                        <select id="manager-select" onchange="updateManagerInfo()" class="blue-readonly">
                            <option value="" data-phone="" data-email="" selected>-- 담당자 선택 --</option>
                            <option value="김영훈 과장" data-phone="010-8290-9971" data-email="1248715@ktmns.com">김영훈 과장</option>
                            <option value="김현태 센터장" data-phone="010-4583-0663" data-email="kht0663@ktmns.com">김현태 센터장</option>
                            <option value="박병규 차장" data-phone="010-3333-0859" data-email="lucky5763@ktmns.com">박병규 차장</option>
                            <option value="서동환 과장" data-phone="010-8871-3121" data-email="sdh8871@ktmns.com">서동환 과장</option>
                            <option value="장준성 과장" data-phone="010-6284-7934" data-email="1248733@ktmns.com">장준성 과장</option>
                            <option value="조창근 센터장" data-phone="010-2220-8900" data-email="jojocg@ktmns.com">조창근 센터장</option>
                            <option value="강수정 과장" data-phone="010-2860-8889" data-email="klklkil@ktmns.com">강수정 과장</option>
                        </select>
                    </td>
                </tr>
                <tr>
                    <th>연락처</th>
                    <!-- [교정] 찌그러짐을 유발하던 input을 완벽한 div 구조로 변경하여 폰트 선명도 보장 -->
                    <td><div id="manager-phone" class="text-display-cell blue-readonly">자동 입력</div></td>
                </tr>
                <tr>
                    <th style="user-select: none;">견적유효기간</th>
                    <td class="notice-text lock-cell" style="background-color: #ffffff; font-weight: bold; color: #004b8d !important; user-select: none;">
                        견적서 제출일로부터 30일 이내
                    </td>
                    <th>이메일주소</th>
                    <!-- [교정] 알파벳 'o', 'm' 깨짐을 원천 복구하기 위해 div 텍스트 렌더링 적용 -->
                    <td><div id="manager-email" class="text-display-cell blue-readonly" style="font-size:11px;">자동 입력</div></td>
                </tr>
            </table>

            <!-- 가입 상품 상세 테이블 (15개 행) -->
            <table class="product-table" id="product-list-table">
                <thead>
                    <tr>
                        <th style="width:18%">설치장소</th> 
                        <th style="width:15%">가입 상품</th> 
                        <th style="width:16%">약정만료기간</th> 
                        <th style="width:13%">상품 요금제</th>
                        <th style="width:11%">청구금액</th> 
                        <th style="width:12%">갱신시 요금</th> 
                        <th style="width:15%">기타 (요금변동)</th> 
                    </tr>
                </thead>
                <tbody>
                    <tr>
                        <td><input type="text"></td><td><input type="text"></td><td><input type="date" onchange="checkDateValue(this)"></td><td><input type="text"></td>
                        <td><input type="text" class="calc-charge" oninput="runCalculations(this)"></td><td><input type="text" class="calc-renew" oninput="runCalculations(this)"></td><td><input type="text" class="calc-diff" readonly placeholder="-"></td>
                    </tr>
                    <tr>
                        <td><input type="text"></td><td><input type="text"></td><td><input type="date" onchange="checkDateValue(this)"></td><td><input type="text"></td>
                        <td><input type="text" class="calc-charge" oninput="runCalculations(this)"></td><td><input type="text" class="calc-renew" oninput="runCalculations(this)"></td><td><input type="text" class="calc-diff" readonly placeholder="-"></td>
                    </tr>
                    <tr>
                        <td><input type="text"></td><td><input type="text"></td><td><input type="date" onchange="checkDateValue(this)"></td><td><input type="text"></td>
                        <td><input type="text" class="calc-charge" oninput="runCalculations(this)"></td><td><input type="text" class="calc-renew" oninput="runCalculations(this)"></td><td><input type="text" class="calc-diff" readonly placeholder="-"></td>
                    </tr>
                    <tr>
                        <td><input type="text"></td><td><input type="text"></td><td><input type="date" onchange="checkDateValue(this)"></td><td><input type="text"></td>
                        <td><input type="text" class="calc-charge" oninput="runCalculations(this)"></td><td><input type="text" class="calc-renew" oninput="runCalculations(this)"></td><td><input type="text" class="calc-diff" readonly placeholder="-"></td>
                    </tr>
                    <tr>
                        <td><input type="text"></td><td><input type="text"></td><td><input type="date" onchange="checkDateValue(this)"></td><td><input type="text"></td>
                        <td><input type="text" class="calc-charge" oninput="runCalculations(this)"></td><td><input type="text" class="calc-renew" oninput="runCalculations(this)"></td><td><input type="text" class="calc-diff" readonly placeholder="-"></td>
                    </tr>
                    <tr>
                        <td><input type="text"></td><td><input type="text"></td><td><input type="date" onchange="checkDateValue(this)"></td><td><input type="text"></td>
                        <td><input type="text" class="calc-charge" oninput="runCalculations(this)"></td><td><input type="text" class="calc-renew" oninput="runCalculations(this)"></td><td><input type="text" class="calc-diff" readonly placeholder="-"></td>
                    </tr>
                    <tr>
                        <td><input type="text"></td><td><input type="text"></td><td><input type="date" onchange="checkDateValue(this)"></td><td><input type="text"></td>
                        <td><input type="text" class="calc-charge" oninput="runCalculations(this)"></td><td><input type="text" class="calc-renew" oninput="runCalculations(this)"></td><td><input type="text" class="calc-diff" readonly placeholder="-"></td>
                    </tr>
                    <tr>
                        <td><input type="text"></td><td><input type="text"></td><td><input type="date" onchange="checkDateValue(this)"></td><td><input type="text"></td>
                        <td><input type="text" class="calc-charge" oninput="runCalculations(this)"></td><td><input type="text" class="calc-renew" oninput="runCalculations(this)"></td><td><input type="text" class="calc-diff" readonly placeholder="-"></td>
                    </tr>
                    <tr>
                        <td><input type="text"></td><td><input type="text"></td><td><input type="date" onchange="checkDateValue(this)"></td><td><input type="text"></td>
                        <td><input type="text" class="calc-charge" oninput="runCalculations(this)"></td><td><input type="text" class="calc-renew" oninput="runCalculations(this)"></td><td><input type="text" class="calc-diff" readonly placeholder="-"></td>
                    </tr>
                    <tr>
                        <td><input type="text"></td><td><input type="text"></td><td><input type="date" onchange="checkDateValue(this)"></td><td><input type="text"></td>
                        <td><input type="text" class="calc-charge" oninput="runCalculations(this)"></td><td><input type="text" class="calc-renew" oninput="runCalculations(this)"></td><td><input type="text" class="calc-diff" readonly placeholder="-"></td>
                    </tr>
                    <tr>
                        <td><input type="text"></td><td><input type="text"></td><td><input type="date" onchange="checkDateValue(this)"></td><td><input type="text"></td>
                        <td><input type="text" class="calc-charge" oninput="runCalculations(this)"></td><td><input type="text" class="calc-renew" oninput="runCalculations(this)"></td><td><input type="text" class="calc-diff" readonly placeholder="-"></td>
                    </tr>
                    <tr>
                        <td><input type="text"></td><td><input type="text"></td><td><input type="date" onchange="checkDateValue(this)"></td><td><input type="text"></td>
                        <td><input type="text" class="calc-charge" oninput="runCalculations(this)"></td><td><input type="text" class="calc-renew" oninput="runCalculations(this)"></td><td><input type="text" class="calc-diff" readonly placeholder="-"></td>
                    </tr>
                    <tr>
                        <td><input type="text"></td><td><input type="text"></td><td><input type="date" onchange="checkDateValue(this)"></td><td><input type="text"></td>
                        <td><input type="text" class="calc-charge" oninput="runCalculations(this)"></td><td><input type="text" class="calc-renew" oninput="runCalculations(this)"></td><td><input type="text" class="calc-diff" readonly placeholder="-"></td>
                    </tr>
                    <tr>
                        <td><input type="text"></td><td><input type="text"></td><td><input type="date" onchange="checkDateValue(this)"></td><td><input type="text"></td>
                        <td><input type="text" class="calc-charge" oninput="runCalculations(this)"></td><td><input type="text" class="calc-renew" oninput="runCalculations(this)"></td><td><input type="text" class="calc-diff" readonly placeholder="-"></td>
                    </tr>
                    <tr>
                        <td><input type="text"></td><td><input type="text"></td><td><input type="date" onchange="checkDateValue(this)"></td><td><input type="text"></td>
                        <td><input type="text" class="calc-charge" oninput="runCalculations(this)"></td><td><input type="text" class="calc-renew" oninput="runCalculations(this)"></td><td><input type="text" class="calc-diff" readonly placeholder="-"></td>
                    </tr>
                    <!-- 최종 합계 행 -->
                    <tr class="total-row">
                        <td colspan="4" class="text-center">최종합계</td>
                        <td id="total-charge">0</td>
                        <td id="total-renew">0</td>
                        <td id="total-diff">0</td>
                    </tr>
                </tbody>
            </table>

            <!-- 필수 안내 -->
            <table style="user-select: none;">
                <tr>
                    <th style="width:15%; color: #d91414 !important; background-color: #fef2f2 !important;">필수 안내</th>
                    <td class="notice-text bg-alert" style="text-align: left !important; padding: 10px; font-size: 11px; line-height: 1.5; font-weight: 700;">
                        • 수수료 지급 : 재약정 완료 ➔ 익월 말 ➔ 법인 대표님 휴대폰으로 신세계 모바일 상품권 발송<br>
                        • 사은품 지급 : 재약정 완료 ➔ 1주 이내 ➔ 법인 대표님 휴대폰으로 신세계 모바일 상품권 발송<br>
                        • <span style="color:#d91414;">[지급 제한 조건]</span> 수수료 및 통합 사은품은 법인 대표님의 명함에 기재된 명확한 전화번호로만 전송이 가능합니다.
                    </td>
                </tr>
            </table>

            <!-- 유의 사항 -->
            <table style="user-select: none;">
                <tr>
                    <th style="width:15%;">유의 사항</th>
                    <td class="notice-text" style="vertical-align: middle; padding: 10px; background-color: #fafafa; text-align: left !important; font-size: 11px; line-height: 1.5; color: #555 !important;">
                        1. 상기 법인 회선 재약정 기준 KT 정식 약정 기간은 총 3년(36개월)이며, 기타 상세 세부사항은 KT 이용약관에 준합니다.<br>
                        2. 재약정 시점에 따라 일부 가입 상품(기업용 TV 등)의 (구)요금제가  (신)요금제로 변경될 수 있습니다.<br>
                        3. 현장 설비 이상 및 장애 발생 시 즉시 KT 고객센터(국번없이 100번)를 통해 접수하시거나 담당부서로 상담 바랍니다.
                    </td>
                </tr>
            </table>

        </div>

        <!-- 다운로드 버튼 영역 -->
        <div class="btn-area">
            <button class="download-btn" onclick="downloadInvoiceJPG()">견적서 JPG 다운로드</button>
            <button class="download-btn pdf-btn" onclick="downloadInvoicePDF()">견적서 PDF 다운로드</button>
        </div>
    </div>

    <script>
        window.onload = function() {
            setTodayDate();       
            updateManagerInfo();  
            calculateBenefits();  
            calculateTableTotals();
        }

        function setTodayDate() {
            const today = new Date();
            const yyyy = today.getFullYear();
            const mm = String(today.getMonth() + 1).padStart(2, '0');
            const dd = String(today.getDate()).padStart(2, '0');
            document.getElementById('invoice-date').innerText = `${yyyy}-${mm}-${dd}`;
        }

        function clearGuidance(el) {
            if(el.value === " 귀하") { el.value = ""; }
        }
        function restoreGuidance(el) {
            if(el.value.trim() === "") {
                el.value = " 귀하";
            } else if(!el.value.endsWith(" 귀하")) {
                el.value = el.value + " 귀하";
            }
        }

        // [수정] value 대입 방식 대신 innerText 노출 방식으로 스크립트 전환
        function updateManagerInfo() {
            const select = document.getElementById('manager-select');
            const selectedOption = select.options[select.selectedIndex];
            const phone = selectedOption.getAttribute('data-phone') || '자동 입력';
            const email = selectedOption.getAttribute('data-email') || '자동 입력';
            document.getElementById('manager-phone').innerText = phone;
            document.getElementById('manager-email').innerText = email;
        }

        function checkDateValue(el) {
            if (el.value) { el.classList.add('has-value'); } else { el.classList.remove('has-value'); }
        }

        function runCalculations(el) {
            let cursorPosition = el.selectionStart;
            let originalLength = el.value.length;
            let value = el.value.replace(/[^0-9]/g, '');
            if (value !== "") { el.value = parseInt(value).toLocaleString(); } else { el.value = ""; }
            let newLength = el.value.length;
            el.setSelectionRange(cursorPosition + (newLength - originalLength), cursorPosition + (newLength - originalLength));
            
            calculateRowDifference(el.closest('tr'));
            calculateTableTotals();
        }

        function calculateRowDifference(row) {
            const chargeInput = row.querySelector('.calc-charge');
            const renewInput = row.querySelector('.calc-renew');
            const diffInput = row.querySelector('.calc-diff');
            
            if(!chargeInput || !renewInput || !diffInput) return;

            const charge = parseInt(chargeInput.value.replace(/,/g, '')) || 0;
            const renew = parseInt(renewInput.value.replace(/,/g, '')) || 0;
            
            if(chargeInput.value === "" && renewInput.value === "") {
                diffInput.value = "";
                return;
            }

            const diff = renew - charge;
            diffInput.value = (diff >= 0 ? "+" : "") + diff.toLocaleString();
        }

        function runBenefitCalculations(el) {
            let cursorPosition = el.selectionStart;
            let originalLength = el.value.length;
            let value = el.value.replace(/[^0-9]/g, '');
            if (value !== "") { el.value = parseInt(value).toLocaleString(); } else { el.value = "0"; }
            let newLength = el.value.length;
            el.setSelectionRange(cursorPosition + (newLength - originalLength), cursorPosition + (newLength - originalLength));
            calculateBenefits();
        }

        function calculateBenefits() {
            const feeString = document.getElementById('fee-input').value.toString().replace(/,/g, '') || "0";
            const giftString = document.getElementById('gift-input').value.toString().replace(/,/g, '') || "0";
            const fee = parseInt(feeString) || 0;
            const gift = parseInt(giftString) || 0;
            document.getElementById('total-benefits-display').innerText = '₩' + (fee + gift).toLocaleString();
        }

        function calculateTableTotals() {
            let chargeInputs = document.querySelectorAll('.calc-charge');
            let totalCharge = 0;
            chargeInputs.forEach(input => { totalCharge += parseInt(input.value.replace(/,/g, '')) || 0; });
            document.getElementById('total-charge').innerText = totalCharge.toLocaleString();

            let renewInputs = document.querySelectorAll('.calc-renew');
            let totalRenew = 0;
            renewInputs.forEach(input => { totalRenew += parseInt(input.value.replace(/,/g, '')) || 0; });
            document.getElementById('total-renew').innerText = totalRenew.toLocaleString();

            const totalDiff = totalRenew - totalCharge;
            const diffDisplay = document.getElementById('total-diff');
            
            if(totalCharge === 0 && totalRenew === 0) {
                diffDisplay.innerText = "0";
            } else {
                diffDisplay.innerText = (totalDiff >= 0 ? "+" : "") + totalDiff.toLocaleString();
            }
        }

        function downloadInvoiceJPG() {
            const captureArea = document.getElementById('invoice-capture-area');
            if (document.activeElement) { document.activeElement.blur(); }

            captureArea.classList.add('clean-capture');

            html2canvas(captureArea, {
                scale: 3,                 
                useCORS: true,
                backgroundColor: '#ffffff',
                logging: false,
                letterRendering: true     
            }).then(canvas => {
                const imageData = canvas.toDataURL('image/jpeg', 1.0); 
                const link = document.createElement('a');
                link.href = imageData;
                link.download = '법인회선_재약정_견적서.jpg';
                link.click();
                captureArea.classList.remove('clean-capture');
            }).catch(error => {
                console.error('오류 발생:', error);
                alert('이미지 저장 중 오류가 발생했습니다.');
                captureArea.classList.remove('clean-capture');
            });
        }

        function downloadInvoicePDF() {
            const { jsPDF } = window.jspdf;
            const captureArea = document.getElementById('invoice-capture-area');
            if (document.activeElement) { document.activeElement.blur(); }

            captureArea.classList.add('clean-capture');

            html2canvas(captureArea, {
                scale: 3,
                useCORS: true,
                backgroundColor: '#ffffff',
                letterRendering: true
            }).then(canvas => {
                const imgData = canvas.toDataURL('image/jpeg', 1.0);
                
                const pdf = new jsPDF('p', 'mm', 'a4');
                const pageWidth = pdf.internal.pageSize.getWidth();   
                const pageHeight = pdf.internal.pageSize.getHeight(); 
                
                const imgWidth = pageWidth;
                const imgHeight = (canvas.height * imgWidth) / canvas.width;
                
                let finalWidth = imgWidth;
                let finalHeight = imgHeight;
                let offsetX = 0;
                let offsetY = 0;

                if (imgHeight > pageHeight) {
                    finalHeight = pageHeight - 4; 
                    finalWidth = (canvas.width * finalHeight) / canvas.height;
                    offsetX = (pageWidth - finalWidth) / 2; 
                    offsetY = 2; 
                }
                
                pdf.addImage(imgData, 'JPEG', offsetX, offsetY, finalWidth, finalHeight, undefined, 'FAST');
                pdf.save('법인회선_재약정_견적서.pdf');
                
                captureArea.classList.remove('clean-capture');
            }).catch(error => {
                console.error('PDF 변환 중 치명적 오류:', error);
                alert('PDF 파일 생성 중 문제가 발생했습니다.');
                captureArea.classList.remove('clean-capture');
            });
        }
    </script>
</body>
</html>
