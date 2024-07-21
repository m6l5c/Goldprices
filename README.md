<!DOCTYPE html>
<html lang="ar">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>مجوهرات الحسناء الذهبية</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            margin: 0;
            padding: 0;
            background-color: #d32f2f; /* اللون العنابي */
            color: #fff;
        }
        .container {
            max-width: 800px;
            margin: 30px auto;
            padding: 20px;
            background-color: #fff;
            border-radius: 10px;
            box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
            color: #000; /* النص داخل الحاوية باللون الأسود */
        }
        h1 {
            text-align: center;
            margin-bottom: 10px;
            display: flex;
            justify-content: space-between;
            align-items: center;
        }
        .contact-info {
            text-align: center;
            margin-bottom: 20px;
            font-size: 1.1em;
        }
        table {
            width: 100%;
            border-collapse: collapse;
            margin-top: 20px;
        }
        th, td {
            padding: 12px;
            text-align: left;
            border-bottom: 1px solid #ddd;
        }
        th {
            background-color: #f4f4f4;
            color: #333;
            font-weight: bold;
        }
        tr:nth-child(even) {
            background-color: #f9f9f9;
        }
        tr:nth-child(odd) {
            background-color: #eeeeee;
        }
        tr:hover {
            background-color: #ddd;
        }
        .ounce-price {
            font-size: 0.6em; /* حجم الخط أصغر بمرتين */
            text-align: right; /* محاذاة إلى اليمين */
        }
        .ounce-price span {
            font-weight: bold;
            color: #007bff;
        }
    </style>
</head>
<body>

<div class="container">
    <h1>
        مجوهرات الحسناء الذهبية
        <span class="ounce-price">سعر الأونصة بالدولار: <span id="ounce-price">--</span></span>
    </h1>
    <div class="contact-info">
        <p>رقم الهاتف: 24752020</p>
        <p>رقم الهاتف: 55334534</p>
    </div>
    <table>
        <thead>
        <tr>
            <th>القيراط</th>
            <th>المبيع (دينار كويتي)</th>
            <th>الشراء (دينار كويتي)</th>
        </tr>
        </thead>
        <tbody id="gold-prices">
        <!-- سيتم تحديث الأسعار هنا -->
        </tbody>
    </table>
</div>

<script>
    async function fetchGoldPrices() {
        try {
            // جلب سعر الأونصة بالدولار الأمريكي
            const responseUSD = await fetch('https://www.goldapi.io/api/XAU/USD', {
                headers: {
                    'x-access-token': 'goldapi-1wm6lslyvc9oll-io', // استخدم مفتاح API الجديد هنا
                    'Content-Type': 'application/json'
                }
            });

            if (!responseUSD.ok) {
                console.error('Error fetching data from USD API:', responseUSD.status, responseUSD.statusText);
                throw new Error('Error fetching data from USD API');
            }

            const dataUSD = await responseUSD.json();
            const ouncePriceInUSD = dataUSD.price;

            // جلب سعر الأونصة بالدينار الكويتي
            const responseKWD = await fetch('https://www.goldapi.io/api/XAU/KWD', {
                headers: {
                    'x-access-token': 'goldapi-1wm6lslyvc9oll-io', // استخدم مفتاح API الجديد هنا
                    'Content-Type': 'application/json'
                }
            });

            if (!responseKWD.ok) {
                console.error('Error fetching data from KWD API:', responseKWD.status, responseKWD.statusText);
                throw new Error('Error fetching data from KWD API');
            }

            const dataKWD = await responseKWD.json();
            const ouncePriceInKWD = dataKWD.price;

            // تحويل سعر الأونصة إلى أسعار القيراطات المختلفة
            const prices = {
                "24": (ouncePriceInKWD / 31.1035).toFixed(2),
                "22": (ouncePriceInKWD / 31.1035 * 0.916).toFixed(2),
                "21": (ouncePriceInKWD / 31.1035 * 0.875).toFixed(2),
                "18": (ouncePriceInKWD / 31.1035 * 0.75).toFixed(2)
            };

            const goldPricesElement = document.getElementById('gold-prices');
            goldPricesElement.innerHTML = '';

            // إضافة السعر وشراء السعر
            for (const [carat, price] of Object.entries(prices)) {
                let buyPrice = (carat === "18" ? price - 0.750 : price - 0.500).toFixed(2);
                goldPricesElement.innerHTML += `<tr><td>${carat} قيراط</td><td>${price}</td><td>${buyPrice}</td></tr>`;
            }

            document.getElementById('ounce-price').innerText = ouncePriceInUSD.toFixed(2);

        } catch (error) {
            console.error('Error fetching gold prices:', error);
            document.getElementById('gold-prices').innerHTML = '<tr><td colspan="3">فشل في جلب البيانات. حاول مرة أخرى لاحقًا.</td></tr>';
            document.getElementById('ounce-price').innerText = '--';
        }
    }

    setInterval(fetchGoldPrices, 15000);
    fetchGoldPrices();  // Initial fetch
</script>

</body>
</html>
