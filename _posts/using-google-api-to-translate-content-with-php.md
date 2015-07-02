Using Google Translate API with PHP {.blog-post-title}
-----------------------------------

**This is a nice reference from: **[Jacek
Barecki](http://www.sitepoint.com/author/jbarecki/) ([http://www.sitepoint.com/using-google-translate-api-php/](http://www.sitepoint.com/using-google-translate-api-php/))


*Note: an advanced continuation of this topic is
accessible [here](http://www.sitepoint.com/auto-translating-user-submitted-content-using-google-translate-api/)*

If your site serves visitors from different countries, you may already
have translated all its static content into several languages. But what
to do with the content posted daily by the users in comments, opinions
and ratings? As this may be as valuable a part of your site as the
static content, you should think of finding a way to translate it into
other languages. One service that can help is, of course, [Google
Translate](https://developers.google.com/translate/).

After going through this tutorial you will be able to fetch translations
from the Google Translate API right from your app. You will learn how to
gain access to the API, how to use it and how to handle errors if they
occur.

### Creating a Google API account

In order to gain access to the Google Translate API, you will have to
create a new project on the [Google APIs
Console](http://code.google.com/apis/console) which requires an active
Google account. After creating a new project, just turn on Translate API
on the list of all the available APIs by flicking a switch.

![](http://dab1nmslvvntp.cloudfront.net/wp-content/uploads/2013/10/Enabling-Translate-API-2.png)

#### Pricing

Since the Google Translate API is a [paid
service](https://developers.google.com/translate/v2/pricing), you will
also need to enable billing in your project settings. To do so,
click *Billing* in the left menu on the Google APIs Console and
then *Enable billing*. You will be asked to enter the payment data such
as your address and credit card number. Different payment options are
available in various countries but credit card payment should be
recognised worldwide.

At the time of writing this article, the usage fee was \$20 per 1
million characters of translation or language detection. Which means
that translating a user comment of 300-400 characters would cost you
\$0.006 – \$0.008. Naturally, if you want to translate a text into more
than one target language, you will have to pay separately for each
translation.

If you fear getting billed too much for the translations you make, you
can control the API usage in your project by setting the maximum limit
of characters that can be translated daily. The entire configuration is
available on the Google APIs Console.

![](http://dab1nmslvvntp.cloudfront.net/wp-content/uploads/2013/10/Setting-limits.png)

#### Getting the API key

To access the Translate API from your app, you will need an API key
connected to the project you have created on the Google APIs Console. To
get the API key, just click *API Access* in the menu at the Google API
Console page. You will find the key you need under *Simple API access*.

![](http://dab1nmslvvntp.cloudfront.net/wp-content/uploads/2013/10/API-access.png)

### Accessing Translate API from your app

The Translate API offers 3 methods:\
 - *translate*, which translates the given text from one language to
another,\
 - *detect*, which detects the language of the given text,\
 - *languages*, which lists the source and target languages supported by
the API.

All the methods are called via GET requests. A common way of making such
a request in PHP is to use the[cURL
library](http://www.sitepoint.com/using-curl-for-remote-requests/),
which we will use in the examples below. The parameters passed to each
method need to be URL encoded which may be achieved in PHP using
the [rawurlencode()](http://www.php.net/manual/en/function.rawurlencode.php) function.
Remember that in each call you have to pass your API key as
a *key* parameter.

The results of each Google Translate API method are returned as a string
representing a JSON object. To parse it, we will use
the [json\_decode()](http://php.net/manual/en/function.json-decode.php) function.

#### Sample request

*Translate* and *detect* services are paid but we can use the third
method – *languages* – just to check if our app can connect with the
API. To do so, we will make a request to the following
URL:[https://www.googleapis.com/language/translate/v2/languages](https://www.googleapis.com/language/translate/v2/languages)

The entire code looks as follows:

    <?php
        $apiKey = '<paste your API key here>';
        $url = 'https://www.googleapis.com/language/translate/v2/languages?key=' . $apiKey;

        $handle = curl_init($url);
        curl_setopt($handle, CURLOPT_RETURNTRANSFER, true);     //We want the result to be saved into variable, not printed out
        $response = curl_exec($handle);                         
        curl_close($handle);

        print_r(json_decode($response, true));
    ?>

After executing the code above, you should see an array of all the
languages that can be processed by the Google Translate API. A similar
table is available in
the [documentation](https://developers.google.com/translate/v2/using_rest#target).
It is important to browse through that table because you will need to
include language codes stated there when submitting a request for
translating a specific text.

Getting the translations
------------------------

The core functionality of the Google Translate API is available through
its *translate* method. It is accessible under the following
URL: [https://www.googleapis.com/language/translate/v2](https://www.googleapis.com/language/translate/v2)

The *translate* method has several parameters. The most important are:\
 - *q* – the input text,\
 - *source* – the source language (if it's not specified, Google will
try to identify it automatically),\
 - *target* – the target language

If you want to get a translated text, you have to change the URL of the
request from the previous example. The rest of the code looks very
similar:

    <?php
        $apiKey = '<paste your API key here>';
        $text = 'Hello world!';
        $url = 'https://www.googleapis.com/language/translate/v2?key=' . $apiKey . '&q=' . rawurlencode($text) . '&source=en&target=fr';

        $handle = curl_init($url);
        curl_setopt($handle, CURLOPT_RETURNTRANSFER, true);
        $response = curl_exec($handle);                 
        $responseDecoded = json_decode($response, true);
        curl_close($handle);

        echo 'Source: ' . $text . '<br>';
        echo 'Translation: ' . $responseDecoded['data']['translations'][0]['translatedText'];
    ?>

A sample response containing the translated text looks as follows:

    {
     "data": {
      "translations": [{"translatedText": "Bonjour tout le monde!"}]
     }
    }

#### What happens if you don't set the source language?

If you decide not to include the source language
(the *source* parameter) in the request, two scenarios can happen:\
 1. Google will manage to detect the language by itself, the JSON
response will consequently contain an
additional *detectedSourceLanguage* property holding the source language
code.\
 2. The source language will not be successfully detected (e.g. when the
source text was too short) and the Google Translate API will return an
HTTP 500 error. This leads to the next part of the tutorial – handling
errors.

Handling errors
---------------

When your request cannot be processed, the Google Translate API returns
an HTTP response with a code representing the type of the error. After
executing a request using cURL, you can get the server response code
using
the [curl\_getinfo()](http://www.php.net/manual/en/function.curl-getinfo.php) function.
If the response code is different than **200**, it means that something
went wrong.

The Google Translate API can return following error codes:\
 - **400 (Bad request)** – your request is missing some parameters or
you have passed wrong values to the parameters present in the request
(e.g. an invalid language code),\
 - **403 (Forbidden)** – you have entered an incorrect API key or have
exceeded your quota,\
 - **500 (Internal Server Error)** – Google cannot identify the source
language of your text or another error occurred.

Additionally, when an error occurs, the Google Translate API returns a
JSON response containing an error description. For example, when one of
the required parameters is missing, the server will reply with a
following response:

    {
     "error": {
      "errors": [
       {
        "domain": "global",
        "reason": "required",
        "message": "Required parameter: target",
        "locationType": "parameter",
        "location": "target"
       }
      ],
      "code": 400,
      "message": "Required parameter: target"
     }
    }

So the best way of handling errors when querying the Google Translate
API service is just to combine checking the HTTP response code and
parsing the JSON response from the server. What is
important,*curl\_getinfo()* must be called before *curl\_close()*:

    <?php
        $apiKey = '<paste your API key here>';
        $text = 'Hello world!';
        $url = 'https://www.googleapis.com/language/translate/v2?key=' . $apiKey . '&q=' . rawurlencode($text) . '&source=en&target=fr';

        $handle = curl_init($url);
        curl_setopt($handle, CURLOPT_RETURNTRANSFER, true);
        $response = curl_exec($handle);
        $responseDecoded = json_decode($response, true);
        $responseCode = curl_getinfo($handle, CURLINFO_HTTP_CODE);      //Here we fetch the HTTP response code
        curl_close($handle);

        if($responseCode != 200) {
            echo 'Fetching translation failed! Server response code:' . $responseCode . '<br>';
            echo 'Error description: ' . $responseDecoded['error']['errors'][0]['message'];
        }
        else {
            echo 'Source: ' . $text . '<br>';
            echo 'Translation: ' . $responseDecoded['data']['translations'][0]['translatedText'];
        }
    ?>

### Multiple translations in one request

You can translate several texts in one request, which is undoubtedly
more efficient than executing separate request for each translation. To
do so, just pass a couple of *q* parameters, each containing one text to
translate.

However, it gets a little tricky here:\
 - if your source texts are all of the same language, you can pass
the *source* parameter containing the language code of all the texts;\
 - but if you want to translate a couple of texts in different
languages, you cannot pass several *source*parameters. In such case you
have to omit the *source* parameter and just let Google guess what the
languages of the source texts are.

Also note that you cannot get multiple translations of one source text
in a single request. If you want to translate one text to different
target languages, you have to make separate requests.

Conclusion
----------

Now you know the basics of connecting your app to the Google Translate
API. More complicated implementations of the API may include
auto-fetching translations when a user submits some content (or when a
site admin approves it) and saving translations to a database. We'll
cover such advanced examples in a future article (part
2 [here](http://www.sitepoint.com/auto-translating-user-submitted-content-using-google-translate-api/)).

If you plan to use the Google Translate API in your app, please remember
to read the [Terms of
service](https://developers.google.com/translate/v2/terms) and[Attribution
requirements](https://developers.google.com/translate/v2/attribution) which
contain several guidelines on how to display the translated content on a
webpage.

If you have any questions or comments regarding the article, feel free
to post a comment below or contact me
through [Google+](https://plus.google.com/112138584619019192671?rel=author).

Enviado por jguzman el Mié, 08/06/2014 - 18:48

-   [blog de
    jguzman](/es/blog/1 "Leer últimas entradas al blog de jguzman.")

