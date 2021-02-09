# Google Recaptcha in Forms

## HTML Header

Add the following line in the header

```html
 <script src='https://www.google.com/recaptcha/api.js' async defer></script>
 ```

## Form

In the form, where you have form fields, add this line to make the Recaptcha check box appear

```html
<div class="g-recaptcha" data-sitekey="Your_Site_Key"></div>
```

Note: Your Site key is the key generated from https://www.google.com/recaptcha/about/

## Checking Recaptcha After Submission

Add the following codes when checking $_POST inputs after use pressed the Submit button.
```php

if(isset($_POST['g-recaptcha-response'])){
          $captcha=$_POST['g-recaptcha-response'];
        }
        if(!$captcha){
        $redirect_link = ''; // ADD REDIRECT LINK HERE
          echo '<html><script>alert("Please check the the captcha form.")</script></html>';
           echo '<meta http-equiv="refresh" content="0;url='.$redirect_link.'" />';
          exit;
        }
        $secretKey = "YOUR_SECRET_KET"; //Add your secret key here.
        $ip = $_SERVER['REMOTE_ADDR'];
        // post request to server
        $url = 'https://www.google.com/recaptcha/api/siteverify?secret=' . urlencode($secretKey) .  '&response=' . urlencode($captcha);
        $response = file_get_contents($url);
        $responseKeys = json_decode($response,true);
        // should return JSON with success as true
        if($responseKeys["success"]) {
                 // what to do if the recpathcha matched
        } else {
           $redirect_link = ''; // ADD REDIRECT LINK HERE
          echo '<html><script>alert("Wrong captcha. Please check the the captcha form properly.")</script></html>';
           echo '<meta http-equiv="refresh" content="0;url='.$redirect_link.'" />';
          exit;
        }
```

Note: YOUR_SECRET_KET is the key you got from https://www.google.com/recaptcha/about/
