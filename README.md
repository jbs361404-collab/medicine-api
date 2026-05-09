[index.html](https://github.com/user-attachments/files/27552899/index.html)
<!DOCTYPE html>
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <title>의약품 생산 실적 대시보드</title>
    <style>
        body { font-family: 'Malgun Gothic', sans-serif; line-height: 1.6; text-align: center; padding: 50px; background-color: #f4f7f6; }
        .container { background: white; border: 2px solid #4CAF50; padding: 30px; border-radius: 15px; display: inline-block; box-shadow: 0 4px 8px rgba(0,0,0,0.1); max-width: 800px; }
        h1 { color: #2E7D32; }
        button { background-color: #4CAF50; color: white; padding: 10px 20px; border: none; border-radius: 5px; cursor: pointer; font-size: 16px; margin: 20px 0; }
        button:hover { background-color: #45a049; }
        table { width: 100%; border-collapse: collapse; margin-top: 20px; display: none; }
        th, td { border: 1px solid #ddd; padding: 12px; text-align: left; }
        th { background-color: #f2f2f2; }
        .loading { display: none; color: #666; }
    </style>
</head>
<body>
    <div class="container">
        <h1>💊 의약품 생산·수입 실적 현황</h1>
        <p>식품의약품안전처 API를 활용하여 실시간 데이터를 불러옵니다.</p>
        
        <button onclick="fetchData()">실시간 데이터 불러오기</button>
        
        <div id="loading" class="loading">데이터를 불러오는 중입니다...</div>

        <table id="resultTable">
            <thead>
                <tr>
                    <th>업체명</th>
                    <th>제품명</th>
                    <th>금액 (원)</th>
                </tr>
            </thead>
            <tbody id="resultBody"></tbody>
        </table>
    </div>

    <script>
        async function fetchData() {
            // 1. 여기에 본인의 인증키를 넣으세요!
            const API_KEY = '3358d0a8ad766199bceba04104a7d6ff5c9851fd5a9de8a01f81ffa06a76edd8'; 
            const url = `http://apis.data.go.kr/1471000/MdcinPrductInptSttusService03/getMdcinPrductInptSttusList03?serviceKey=${API_KEY}&type=json&numOfRows=10&pageNo=1`;

            const loading = document.getElementById('loading');
            const table = document.getElementById('resultTable');
            const tbody = document.getElementById('resultBody');

            loading.style.display = 'block';
            table.style.display = 'none';
            tbody.innerHTML = '';

            try {
                const response = await fetch(url);
                const data = await response.json();
                const items = data.body.items;

                items.forEach(item => {
                    const row = `<tr>
                        <td>${item.ENTP_NAME}</td>
                        <td>${item.PRDUCT_NAME}</td>
                        <td>${Number(item.PRDUCT_PRICE).toLocaleString()}</td>
                    </tr>`;
                    tbody.innerHTML += row;
                });

                loading.style.display = 'none';
                table.style.display = 'table';
            } catch (error) {
                loading.style.display = 'none';
                alert('데이터를 불러오는데 실패했습니다. 인증키 활성화 대기 중이거나 키를 확인해주세요!');
                console.error(error);
            }
        }
    </script>
</body>
</html>
