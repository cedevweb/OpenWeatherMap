# OpenWeatherMap
PHP ile OpenWeatherMap kullanarak Hava Tahmini


Bu eğiticide, bir API kullanarak hava durumu bilgilerini görüntülemek için bir PHP uygulaması oluşturacağız. PHP ile bunu uygulamak için OpenWeatherMap servisini kullandım. API tarafından sağlanan hava durumu bilgisini alıp uygulamada göstereceğiz.

Bu, hava durumu tahminlerini sağlayan en iyi API hizmetlerinden biridir. Düzenli olarak muazzam hava durumu verileri sağlar. Sınırlı erişimli ücretsiz bir hizmettir. Temel kullanım için yeterli olmalı ve ileride bunun için ödeme yapmanız gerekebilir. Bu API'yi bir PHP uygulaması ile entegre etmek kolaydır. Entegrasyon için aşağıdaki üç adım kullanılmıştır.

API anahtarını al
Şehir kimliğini bul
API anahtarı ve şehir kimliği göndererek hava tahmini isteyin

OpenWeatherMap API anahtarını alın
API anahtarını almak için OpenWeatherMap’e kaydolmamız gerekiyor . Kaydolduktan sonra, bizi profil ayarlarına yönlendirir.
Profil ayarları formunun üstünde, birkaç sekme içeren bir üst menü var . Click API anahtarları sekmesini ve API anahtarı kopyalamak. Bu daha sonra hava durumu tahminleri için API istemek için kullanılacaktır.


Şehir kimliğini bul
Aşağıdaki linke tıklayarak, şehirler listesi sıkıştırılmış biçimde indirilecektir. Dosyayı açın ve şehrin / eyaletin kimliğini alın.

http://bulk.openweathermap.org/sample/city.list.json.gz

Sıkıştırmayı açtıktan sonra, dosya bir konum dizisi içeren JSON formatlı verilere sahip olacaktır . Her dizi öğesi enlem, boylam, ülke, şehir / eyalet, şehir kimliği gibi coğrafi verileri içerir.

PHP Kodu, anahtarlar göndererek Hava Tahmini İsteğinde Bulunuyor
Bu, hava tahminlerini almak için OpenWeatherMap servisini istemek için kullanılan PHP kodudur. İstek gönderilirken, API anahtarı ve şehir kimliği, istek URL'si sorgu dizesiyle birlikte gönderilir.

API isteği göndermek için PHP CURL kullandım . CURL yanıtı bir JSON formatında olacaktır. JSON yanıtını çözerek, hava durumu verilerini alabilir ve tarayıcıda doldurabiliriz.



<?php
$apiKey = "API KEY";
$cityId = "CITY ID";
$googleApiUrl = "http://api.openweathermap.org/data/2.5/weather?id=" . $cityId . "&lang=en&units=metric&APPID=" . $apiKey;

$ch = curl_init();

curl_setopt($ch, CURLOPT_HEADER, 0);
curl_setopt($ch, CURLOPT_RETURNTRANSFER, 1);
curl_setopt($ch, CURLOPT_URL, $googleApiUrl);
curl_setopt($ch, CURLOPT_FOLLOWLOCATION, 1);
curl_setopt($ch, CURLOPT_VERBOSE, 0);
curl_setopt($ch, CURLOPT_SSL_VERIFYPEER, false);
$response = curl_exec($ch);

curl_close($ch);
$data = json_decode($response);
$currentTime = time();
?>


---------------------------------------------------------



HTML Code to Show Weather Forecasts
This HTML code is used to display the weather forecast by decoding the JSON object response. In this section, we access the location, weather description, icon, min-max ranges of the temperature, humidity and wind speed.


<!doctype html>
<html>
<head>
<title>Forecast Weather using OpenWeatherMap with PHP</title>
</head>
<body>
    <div class="report-container">
        <h2><?php echo $data->name; ?> Weather Status</h2>
        <div class="time">
            <div><?php echo date("l g:i a", $currentTime); ?></div>
            <div><?php echo date("jS F, Y",$currentTime); ?></div>
            <div><?php echo ucwords($data->weather[0]->description); ?></div>
        </div>
        <div class="weather-forecast">
            <img
                src="http://openweathermap.org/img/w/<?php echo $data->weather[0]->icon; ?>.png"
                class="weather-icon" /> <?php echo $data->main->temp_max; ?>°C<span
                class="min-temperature"><?php echo $data->main->temp_min; ?>°C</span>
        </div>
        <div class="time">
            <div>Humidity: <?php echo $data->main->humidity; ?> %</div>
            <div>Wind: <?php echo $data->wind->speed; ?> km/h</div>
        </div>
    </div>
</body>
</html>

PHP ile OpenWeatherMap kullanarak Hava Tahmini Çıkışı
Bu yukarıda gördüğümüz örnek programın çıktısıdır. API'nin JSON yanıtını çözerek hava durumu bilgisi sağlar.
