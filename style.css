<!DOCTYPE html>
<html lang="th">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>ระบบวัดระดับน้ำและค่า pH พร้อมคำนวณเวลาการใช้น้ำ</title>
    <style>
        body {
            font-family: 'Roboto', sans-serif;
            margin: 20px;
            text-align: center;
            background-color: #F0F8FF;
        }
        .container {
            margin: 20px auto;
            width: 80%;
        }
        .university-logo {
            margin-bottom: 20px;
        }
        .system-name {
            font-size: 24px;
            font-weight: bold;
            margin-bottom: 40px;
        }
        .box {
            border: 2px solid #000000;
            padding: 20px;
            margin-bottom: 20px;
            text-align: center;
        }
        .box h2 {
            font-size: 20px;
            margin-bottom: 10px;
        }
        .box p {
            font-size: 16px;
            margin-bottom: 10px;
        }
        .ph-value, .ultrasonic-value {
            font-size: 20px;
            font-weight: bold;
            color: red;
        }
        .graph {
            width: 100%;
            height: 200px;
            border: 1px solid #000;
            margin: 20px 0;
        }
        .info {
            font-size: 18px;
        }
        /* ซ่อนข้อมูล */
        #result1, #result2, #result3, th:nth-child(2) {
            display: none;
        }
    </style>
    <!-- Chart.js -->
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    <script>
        let distanceData = [];
        let timeLabels = [];
        let currentDate = '';


        function updateValues() {
            fetch('http://192.168.100.60/')  // Ensure this URL points to your ESP8266
                .then(response => {
                    if (!response.ok) {
                        throw new Error('Network response was not ok');
                    }
                    return response.json();
                })
                .then(data => {
                    // Update pH value
                    document.getElementById('ph-value').innerText = data.ph;


                    // Update ultrasonic distance
                    let distance = parseFloat(data.distance);
                    document.getElementById('ultrasonic-value').innerText = distance.toFixed(2) + ' m';


                    // เพิ่มบรรทัดนี้เพื่อแสดงค่า distanceRaw
                    let distanceRaw = data.distanceRaw;  // รับค่าจาก ESP8266
                    document.getElementById('distanceRaw').innerText = distanceRaw;  // แสดงในแท็ก distanceRaw


                    // Calculate water volume
                    let volume = (1 / 2) * 56 * distance;  // Assuming distance is depth
                    document.getElementById('water-volume').innerText = volume.toFixed(2) + ' ลูกบาศก์เมตร';


                    // Calculate water usage results
                    let results = [10, 20, 30].map(val => (volume / val).toFixed(2));
                    results.forEach((result, index) => {
                        document.getElementById(`result${index + 1}`).innerText = result;
                        let date = new Date();
                        date.setDate(date.getDate() + Math.ceil(result)); // Calculate end date
                        document.getElementById(`date${index + 1}`).innerText = date.toLocaleDateString('th-TH');
                    });


                    // Update chart data
                    const currentTime = new Date().toLocaleTimeString();
                    distanceData.push(distance);
                    timeLabels.push(currentTime);
                    if (distanceData.length > 10) {
                        distanceData.shift();
                        timeLabels.shift();
                    }
                    myChart.update();
                })
                .catch(error => console.error('Error:', error));
        }


        window.onload = function () {
            // Create chart when the page loads
            var ctx = document.getElementById('distanceChart').getContext('2d');
            window.myChart = new Chart(ctx, {
                type: 'line',
                data: {
                    labels: timeLabels,
                    datasets: [{
                        label: 'ความลึก (m)',
                        data: distanceData,
                        borderColor: 'rgba(75, 192, 192, 1)',
                        backgroundColor: 'rgba(75, 192, 192, 0.2)',
                        fill: true
                    }]
                },
                options: {
                    scales: {
                        y: {
                            beginAtZero: true
                        }
                    }
                }
            });


            // Display today's date
            const today = new Date().toLocaleDateString('th-TH');
            document.getElementById('current-date').innerText = today;


            // Start updating values every 5 seconds
            updateValues();
            setInterval(updateValues, 5000);
        };
    </script>
</head>
<body>
    <div class="container">
        <div class="university-logo">
            <img src="Ratchaphat.png" width="200" alt="Logo">
        </div>
        <div class="system-name">มหาวิทยาลัยราชภัฎเพชรบุรี</div>
        <div class="system-name">ระบบวัดระดับน้ำและค่า pH พร้อมคำนวณเวลาการใช้น้ำ</div>


        <div class="box">
            <h2>แสดงการวัดค่า pH sensor</h2>
            <p>ค่า pH 0-6.9 มีค่าเป็นกรด</p>
            <p>ค่า pH 7.1-14 มีค่าเป็นด่าง</p>
            <div class="ph-value">ค่า pH ที่วัดได้: <span id="ph-value">กำลังโหลด...</span></div>
        </div>


        <div class="box">
            <h2>แสดงการวัดค่า Ultrasonic sensor</h2>
            <div class="info">ความลึกที่วัดได้: <span id="ultrasonic-value">กำลังโหลด...</span></div>
            <div class="info">ระยะห่างที่วัดได้: <span id="distanceRaw">กำลังโหลด...</span></div> 
            <canvas id="distanceChart" width="400" height="200"></canvas>
            <p class="info">ปริมาณน้ำจากการคำนวณ: <span id="water-volume">กำลังคำนวณ...</span></p>
            <p class="info">วันที่วัดค่า: <span id="current-date">กำลังโหลด...</span></p>
        </div>
        <div class="box">
            <table border="1" cellpadding="10" cellspacing="0" style="width: 100%; margin: 0 auto;">
                <thead>
                    <tr>
                        <th>วันที่ / เวลา</th>
                        <th>ค่า pH</th>
                        <th>ระดับน้ำ</th>
                        <th>ค่า pH</th>
                    </tr>
                </thead>
                <tbody>
                    <tr><td>1/08/67 09:00</td><td>3.46</td><td>4.44</td></tr>
                    <tr><td>1/08/67 18:00</td><td>3.45</td><td>4.89</td></tr>
                    <tr><td>2/08/67 09:00</td><td>3.45</td><td>3.33</td></tr>
                    <tr><td>2/08/67 18:00</td><td>3.47</td><td>3.90</td></tr>
                    <tr><td>3/08/67 09:00</td><td>3.49</td><td>3.74</td></tr>
                    <tr><td>3/08/67 18:00</td><td>3.45</td><td>3.34</td></tr>
                    <tr><td>4/08/67 09:00</td><td>3.46</td><td>5.66</td></tr>
                    <tr><td>4/08/67 18:00</td><td>3.45</td><td>5.55</td></tr>
                    <tr><td>5/08/67 09:00</td><td>3.45</td><td>6.59</td></tr>
                    <tr><td>5/08/67 18:00</td><td>3.50</td><td>7.09</td></tr>
                    <tr><td>6/08/67 09:00</td><td>3.48</td><td>3.55</td></tr>
                    <tr><td>6/08/67 18:00</td><td>3.50</td><td>4.79</td></tr>
                    <tr><td>7/08/67 09:00</td><td>3.46</td><td>6.59</td></tr>
                    <tr><td>7/08/67 18:00</td><td>3.47</td><td>4.23</td></tr>
                    <tr><td>8/08/67 09:00</td><td>3.48</td><td>7.64</td></tr>
                    <tr><td>8/08/67 18:00</td><td>3.45</td><td>6.44</td></tr>
                    <tr><td>9/08/67 09:00</td><td>3.49</td><td>6.52</td></tr>
                    <tr><td>9/08/67 18:00</td><td>3.45</td><td>6.99</td></tr>
                    <tr><td>10/08/67 09:00</td><td>3.46</td><td>3.77</td></tr>
                    <tr><td>10/08/67 18:00</td><td>3.45</td><td>7.65</td></tr>
                    <tr><td>11/08/67 09:00</td><td>3.50</td><td>4.72</td></tr>
                    <tr><td>11/08/67 18:00</td><td>3.45</td><td>7.64</td></tr>
                    <tr><td>12/08/67 09:00</td><td>3.50</td><td>3.76</td></tr>
                    <tr><td>12/08/67 18:00</td><td>3.48</td><td>4.67</td></tr>
                    <tr><td>13/08/67 09:00</td><td>3.45</td><td>6.73</td></tr>
                    <tr><td>13/08/67 18:00</td><td>3.48</td><td>5.57</td></tr>
                    <tr><td>14/08/67 09:00</td><td>3.49</td><td>4.47</td></tr>
                    <tr><td>14/08/67 18:00</td><td>3.45</td><td>4.22</td></tr>
                    <tr><td>15/08/67 09:00</td><td>3.47</td><td>3.63</td></tr>
                    <tr><td>15/08/67 18:00</td><td>3.48</td><td>4.10</td></tr>
                    <tr><td>16/08/67 09:00</td><td>3.51</td><td>5.28</td></tr>
                    <tr><td>16/08/67 18:00</td><td>3.49</td><td>2.65</td></tr>
                    <tr><td>17/08/67 09:00</td><td>3.46</td><td>3.63</td></tr>
                    <tr><td>17/08/67 18:00</td><td>3.46</td><td>4.81</td></tr>
                    <tr><td>18/08/67 09:00</td><td>3.48</td><td>7.70</td></tr>
                    <tr><td>18/08/67 18:00</td><td>3.49</td><td>6.89</td></tr>
                    <tr><td>19/08/67 09:00</td><td>3.49</td><td>4.77</td></tr>
                    <tr><td>19/08/67 18:00</td><td>3.46</td><td>6.55</td></tr>
                    <tr><td>20/08/67 09:00</td><td>3.48</td><td>5.47</td></tr>
                    <tr><td>20/08/67 18:00</td><td>3.47</td><td>6.89</td></tr>
                    <tr><td>21/08/67 09:00</td><td>3.50</td><td>3.59</td></tr>
                    <tr><td>21/08/67 18:00</td><td>3.45</td><td>3.08</td></tr>
                    <tr><td>22/08/67 09:00</td><td>3.46</td><td>6.35</td></tr>
                    <tr><td>22/08/67 18:00</td><td>3.45</td><td>7.93</td></tr>
                    <tr><td>23/08/67 09:00</td><td>3.51</td><td>4.88</td></tr>
                    <tr><td>23/08/67 18:00</td><td>3.45</td><td>5.45</td></tr>
                    <tr><td>24/08/67 18:00</td><td>3.55</td><td>7.97</td></tr>
                    <tr><td>24/08/67 09:00</td><td>3.50</td><td>8.76</td></tr>
                    <tr><td>25/08/67 18:00</td><td>3.48</td><td>3.48</td></tr>
                    <tr><td>25/08/67 09:00</td><td>3.45</td><td>4.75</td></tr>
                    <tr><td>26/08/67 18:00</td><td>3.45</td><td>7.58</td></tr>
                    <tr><td>26/08/67 09:00</td><td>3.46</td><td>6.45</td></tr>
                    <tr><td>27/08/67 18:00</td><td>3.50</td><td>3.34</td></tr>
                    <tr><td>27/08/67 18:00</td><td>3.49</td><td>4.22</td></tr>
                    <tr><td>28/08/67 09:00</td><td>3.65</td><td>3.34</td></tr>
                    <tr><td>28/08/67 18:00</td><td>3.50</td><td>4.22</td></tr>
                    <tr><td>29/08/67 09:00</td><td>3.48</td><td>5.57</td></tr>
                    <tr><td>29/08/67 18:00</td><td>3.47</td><td>6.05</td></tr>
                    <tr><td>30/08/67 09:00</td><td>3.46</td><td>3.76</td></tr>
                    <tr><td>30/08/67 18:00</td><td>3.45</td><td>3.63</td></tr>
                    <tr><td>31/08/67 09:00</td><td>3.48</td><td>4.67</td></tr>
                    <tr><td>31/08/67 18:00</td><td>3.46</td><td>5.21</td></tr>
                    <tr><td>26/11/67 09:00</td><td>3.48</td><td>4.67</td></tr>
                    <tr><td>26/11/67 18:00</td><td>3.46</td><td>5.21</td></tr>
                    <tr><td>27/11/67 09:00</td><td>3.48</td><td>4.67</td></tr>
                    <tr><td>27/11/67 18:00</td><td>3.46</td><td>5.21</td></tr>
                    <tr><td>28/11/67 09:00</td><td>3.48</td><td>4.67</td></tr>
                    <tr><td>28/11/67 18:00</td><td>3.46</td><td>5.21</td></tr>
                    <tr><td>29/11/67 09:00</td><td>3.48</td><td>4.67</td></tr>
                    <tr><td>29/11/67 18:00</td><td>3.46</td><td>5.21</td></tr>
                    <tr><td>30/11/67 09:00</td><td>3.48</td><td>4.67</td></tr>
                    <tr><td>30/11/67 18:00</td><td>3.46</td><td>5.21</td></tr>
                <tbody>    
        <div class="box">
                <h2>ผลการทดลองระดับน้ำและค่า pH</h2>
                <table border="1" cellpadding="10" cellspacing="0" style="width: 100%; margin: 0 auto;"> 
                    <thead>
                        <h2>กราฟสรุปผลการทดลอง </h2>
                    <div><img src="distance.jpeg" width="800">
                        <img src="pH.jpg" width="800">
                   </div>
                </thead>
            </table>
        </div>
        <div class="box">
            <tr>
                <h2>กราฟเปรียบเทียบค่า pH ในบ่อน้ำ </h2>
            <div><img src="27.png" width="800">
                 <img src="28.png" width="800">
                 <img src="29.png" width="800">
            </div>
            </tr>
        </div>    
        <div class="box">
            <tr>
                <h2>กราฟเปรียบเทียบค่า pH ในน้ำคาลิเบด </h2>
            <div><img src="6.86.jpg" width="800">
                 <img src="water.jpg" width="800">
            </div>
            </tr>
        </div>
        <div class="box">
            <h2>เหตุการณ์สมมุติ</h2>
            <table border="1" cellpadding="10" cellspacing="0" style="width: 100%; margin: 0 auto;">
                <tr>
                    <th>หากใช้น้ำ</th>
                    <th>ค่าที่วัดได้/ใช้น้ำ</th>
                    <th>น้ำสามารถใช้ได้ถึงวันที่</th>
                </tr>
                <tr>
                    <td>10 ลบ.ม.</td>
                    <td id="result1">กำลังโหลด...</td>
                    <td id="date1">กำลังโหลด...</td>
                </tr>
                <tr>
                    <td>20 ลบ.ม.</td>
                    <td id="result2">กำลังโหลด...</td>
                    <td id="date2">กำลังโหลด...</td>
                </tr>
                <tr>
                    <td>30 ลบ.ม.</td>
                    <td id="result3">กำลังโหลด...</td>
                    <td id="date3">กำลังโหลด...</td>
                </tr>
            </table>
        </div>
    </div>
</body>
</html>