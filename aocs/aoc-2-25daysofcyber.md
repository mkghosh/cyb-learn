# 25 Days of cyber

## Day 1

Crismas Crisis

To decode hex string to ascii text in terminal we can use

*$echo "hex-string" | xxd -r -p*

## Day 2

### RCE

When you have the ability to upload files to a server, you have a path straight to RCE (Remote Command Execution). An upload form with no restrictions would mean that you could upload a script that, when executed, connects back to your attacking machine and gives you the ability to run any command you want. It would be very unusual to find a file upload with no filtering; but it's much less uncommon to find a file upload that employs flawed filtering techniques which can be circumbented to upload a malicious script. The script has to be written in a language which the server can execute. PHP is usually a good choice for this, as most websites are still written with a PHP backend.

- Double-barrelled extension (e.g .jpg.php)

## Upload files to the server

To be able to get upload in the server we need to provide id=given_id as get parameter to the url. Then we can bypass the upload filter by using the double-barrelled extension (e.g .jpg.php, .jpeg.php, .png.php) as the site only looks for files with extension .jpg, .jpeg or .png

Now upload the file to the server and 
