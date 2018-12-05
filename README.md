[![Minimum PHP Version](https://img.shields.io/badge/php-%3E%3D%205.4-8892BF.svg?style=flat-square)](https://php.net/)

# php-oauth-client
A PHP OAuth 1.0 Client refactored to use PSR-4 namespacing and installable with composer.

[raysolomon/php-oauth-client](https://packagist.org/packages/raysolomon/php-oauth-client) (Packagist)

```bash
composer require raysolomon/php-oauth-client
```

## How do I implement this?

Here is a two-legged auth example for the Yahoo Boss Web search api

```php
$clientId = ''; // fill in this
$clientSecret = '';  // fill in this

$consumer = new \RaySolomon\OAuthClient\OAuthConsumer($clientId, $clientSecret);

$url = "https://yboss.yahooapis.com/ysearch/web";

$query = 'Automotive Tune Up Service Mesa, AZ';

$args = [];
$args["q"] = $query;
$args["format"] = "json";
$args["count"] = 10;
$args["start"] = 0;

$request = \RaySolomon\OAuthClient\OAuthRequest::from_consumer_and_token($consumer, null, "GET", $url, $args);
$request->sign_request(new \RaySolomon\OAuthClient\OAuthSignatureMethod_HMAC_SHA1(), $consumer, null);
$url = sprintf("%s?%s", $url, \RaySolomon\OAuthClient\OAuthUtil::build_http_query($args));
$ch = curl_init();
curl_setopt($ch, CURLOPT_URL, $url);
curl_setopt($ch, CURLOPT_USERAGENT, 'MyUserAgent/1.2.1 (+https://www.example.com/bot.html)');
curl_setopt($ch, CURLOPT_AUTOREFERER, true);
curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);
curl_setopt($ch, CURLOPT_FAILONERROR, true);
curl_setopt($ch, CURLOPT_FOLLOWLOCATION, true);
curl_setopt($ch, CURLOPT_CONNECTTIMEOUT, 30);
curl_setopt($ch, CURLOPT_DNS_CACHE_TIMEOUT, 600);
curl_setopt($ch, CURLOPT_MAXREDIRS, 10);
curl_setopt($ch, CURLOPT_TIMEOUT, 600);
curl_setopt($ch, CURLOPT_SSL_VERIFYHOST, false);
curl_setopt($ch, CURLOPT_SSL_VERIFYPEER, false);
curl_setopt($ch, CURLOPT_HTTPHEADER,
    array(
        $request->to_header()
    )
);
$response = curl_exec($ch);
if (curl_errno($ch)) {
    throw new \Exception('YahooBoss: ' .
        sprintf('Request failed: (%d) %s.', curl_errno($ch), curl_error($ch)));
}
curl_close($ch);
var_dump($response);
```

Output of var_dump($response)

```json
string(6202) "{"bossresponse":{"responsecode":"200","web":{"start":"0","count":"10","totalresults":"6110000","results":[{"date":"","clickurl":"https:\/\/www.faithworksauto.com\/auto-tune-up-service","url":"https:\/\/www.faithworksauto.com\/auto-tune-up-service","dispurl":"https:\/\/www.faithworks<b>auto<\/b>.com\/<b>auto<\/b>-<b>tune<\/b>-<b>up<\/b>-<b>service<\/b>","title":"<b>Auto<\/b> <b>Tune<\/b> <b>Up<\/b> <b>Service<\/b> in <b>Mesa<\/b> <b>AZ<\/b> | Faith Works <b>Automotive<\/b>","abstract":"<b>Auto<\/b> <b>Tune<\/b> <b>up<\/b> <b>Service<\/b> Faith Works <b>Automotive<\/b> knows that a regular <b>tune<\/b>-<b>up<\/b> is important to your car's performance. Following your manufacturers <b>tune<\/b>-<b>up<\/b> schedule will extend the life of your car and will reduce the overall cost to maintain your car."},{"date":"","clickurl":"https:\/\/www.mechanicadvisor.com\/az\/mesa\/tune-up","url":"https:\/\/www.mechanicadvisor.com\/az\/mesa\/tune-up","dispurl":"https:\/\/www.mechanicadvisor.com\/<b>az<\/b>\/<b>mesa<\/b>\/<b>tune<\/b>-<b>up<\/b>","title":"10 Best <b>Mesa<\/b>, <b>AZ<\/b> <b>Tune<\/b> <b>up<\/b> Shops - Mechanic Advisor","abstract":"Find <b>Mesa<\/b>,<b>AZ<\/b> <b>Tune<\/b> <b>up<\/b> shops for your repair needs. Review <b>Mesa<\/b> repair shops that specialize in <b>Tune<\/b> <b>up<\/b>. ... Terrys <b>Auto Repair<\/b> and <b>Service<\/b> at 1625 N 40th St was ..."},{"date":"","clickurl":"https:\/\/www.yellowpages.com\/mesa-az\/automotive-tune-up-service","url":"https:\/\/www.yellowpages.com\/mesa-az\/automotive-tune-up-service","dispurl":"https:\/\/www.yellowpages.com\/<b>mesa<\/b>-<b>az<\/b>\/<b>automotive<\/b>-<b>tune<\/b>-<b>up<\/b>...","title":"<b>Automotive<\/b> <b>Tune<\/b> <b>Up<\/b> <b>Service<\/b> in <b>Mesa<\/b>, <b>AZ<\/b> - Yellowpages.com","abstract":"<b>Automotive<\/b> <b>Tune<\/b> <b>Up<\/b> <b>Service<\/b> in <b>Mesa<\/b> on YP.com. See reviews, photos, directions, phone numbers and more for the best <b>Automotive<\/b> <b>Tune<\/b> <b>Up<\/b> <b>Service<\/b> in <b>Mesa<\/b>, <b>AZ<\/b>."},{"date":"","clickurl":"https:\/\/www.yellowpages.com\/apache-junction-az\/automotive-tune-up-service","url":"https:\/\/www.yellowpages.com\/apache-junction-az\/automotive-tune-up-service","dispurl":"https:\/\/www.yellowpages.com\/...\/<b>automotive<\/b>-<b>tune<\/b>-<b>up<\/b>-<b>service<\/b>","title":"Best 30 <b>Automotive<\/b> <b>Tune<\/b> <b>Up<\/b> <b>Service<\/b> in Apache Junction, <b>AZ<\/b> ...","abstract":"<b>Automotive<\/b> <b>Tune<\/b> <b>Up<\/b> <b>Service<\/b> in Apache Junction on YP.com. See reviews, photos, directions, phone numbers and more for the best <b>Automotive<\/b> <b>Tune<\/b> <b>Up<\/b> <b>Service<\/b> in Apache Junction, <b>AZ<\/b>. Start your search by typing in the business name below."},{"date":"","clickurl":"https:\/\/crawfordsautoservice.com\/","url":"https:\/\/crawfordsautoservice.com\/","dispurl":"https:\/\/crawfordsauto<b>service<\/b>.com","title":"Crawfords Auto Repair <b>Mesa<\/b> <b>AZ<\/b> 85210, Quality Car Care","abstract":"Crawford's Auto Repair <b>Mesa<\/b> <b>AZ<\/b> 85210 85202 85233 85225 85224 is an auto <b>service<\/b> mechanic shop offering comprehensive services for car maintenance and repair"},{"date":"","clickurl":"https:\/\/gunnellstiresandservice.com\/automotive-tune-up-mesa-az","url":"https:\/\/gunnellstiresandservice.com\/automotive-tune-up-mesa-az","dispurl":"https:\/\/gunnellstiresand<b>service<\/b>.com\/<b>automotive<\/b>-<b>tune<\/b>-<b>up<\/b>...","title":"<b>Automotive<\/b> <b>Tune<\/b> Ups in <b>Mesa<\/b>, <b>AZ<\/b> | Gunnell's Tires &amp; <b>Service<\/b>","abstract":"<b>Tune<\/b> <b>Up<\/b> in <b>Mesa<\/b>, <b>AZ<\/b> <b>Tune<\/b>-ups can extend the life of your vehicle by replacing engine parts that are vulnerable to deteriorating. The majority of automobiles need to have <b>tune<\/b> ups every 30,000 miles."},{"date":"","clickurl":"https:\/\/www.bullittautomotive.com\/engine-tune-up-service\/","url":"https:\/\/www.bullittautomotive.com\/engine-tune-up-service\/","dispurl":"https:\/\/www.bullitt<b>automotive<\/b>.com\/engine-<b>tune<\/b>-<b>up<\/b>-<b>service<\/b>","title":"Engine <b>Tune<\/b> <b>Up<\/b> <b>Service<\/b> | Car <b>Tune<\/b> <b>Up<\/b> Tempe, <b>AZ<\/b> - Bullitt ...","abstract":"Engine <b>Tune<\/b> <b>Up<\/b> <b>Service<\/b>. At the heart of your vehicle is the engine, is it time for an for a Engine <b>Tune<\/b> <b>Up<\/b> <b>Service<\/b>. Over time, all the different components of your engine can wear out and make your vehicle lose fuel efficiency and power."},{"date":"","clickurl":"https:\/\/www.highlinecarcare.com\/","url":"https:\/\/www.highlinecarcare.com\/","dispurl":"https:\/\/www.highlinecarcare.com","title":"<b>Auto Repair<\/b> Shop in <b>Mesa<\/b>, <b>AZ<\/b> 85210 | Highline Car Care","abstract":"A <b>Mesa<\/b> <b>auto repair<\/b> shop servicing all vehicle makes and models. We are locally owned and provide the right <b>service<\/b> at the right time! Highline Car Car is best <b>auto repair<\/b> shop serving <b>Mesa<\/b>, <b>AZ<\/b> and the surrounding valley."},{"date":"","clickurl":"https:\/\/allbrandsauto.com\/service\/","url":"https:\/\/allbrandsauto.com\/service\/","dispurl":"https:\/\/allbrands<b>auto<\/b>.com\/<b>service<\/b>","title":"<b>Auto<\/b> <b>Services<\/b> &amp; Maintenance in <b>Mesa<\/b> <b>AZ<\/b> - All Brands <b>Auto<\/b>","abstract":"<b>Auto<\/b> <b>Services<\/b> &amp; Maintenance in <b>Mesa<\/b> <b>AZ<\/b>. Our All Brands <b>Auto<\/b> shop may be in a tough place because of the light rail construction going on on Main Street, but that shouldn’t stop you from checking us out."},{"date":"","clickurl":"https:\/\/www.mechanicadvisor.com\/az\/apache-junction\/tune-up","url":"https:\/\/www.mechanicadvisor.com\/az\/apache-junction\/tune-up","dispurl":"https:\/\/www.mechanicadvisor.com\/<b>az<\/b>\/apache-junction\/<b>tune<\/b>-<b>up<\/b>","title":"10 Best Apache Junction, <b>AZ<\/b> <b>Tune<\/b> <b>up<\/b> Shops - Mechanic Advisor","abstract":"<b>Tune<\/b> <b>up<\/b> in Apache Junction. ... <b>AZ<\/b> <b>auto<\/b> <b>tune<\/b> <b>up<\/b> . ... Friendly <b>Auto<\/b> Centers at 5026 E Main St was recently discovered under <b>Mesa<\/b> Honda CR-V <b>auto<\/b> <b>service<\/b>."}]}}}"
```

## Maintainers

[@RaySolomon](https://github.com/raysolomon)

