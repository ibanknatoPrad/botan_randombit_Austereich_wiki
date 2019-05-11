NOTE: The commands below were written using bash.

These commands will produce a root for exampleco, a leading producer of examples based in Antarctica.  Change it or keep it, your choice.  Note that the policies on these will be very loose and should not be used for production purposes.

# Create the CA
This will generate a root certificate that should last until about 2040, which is similar in longevity to most "professional" root CAs on the internet.

### Create the root CA's password
If you want a random password, try running:

`botan rng --format=base64 100 | cut -c ${RANDOM:0:2} > capassword.txt`

Or simply:

`echo "battery horse stapler" > capassword.txt`

### Create the root CA's key
Many sites have moved from RSA to elliptic curve keys, so let's use a 384-bit one:

`botan keygen --algo=ECDSA --params=secp384r1 --passphrase=$(cat capassword.txt) > cakey.pem`

And then verify that the password works:

`botan pkcs8 --pass-in=$(cat capassword.txt) cakey.pem`

### Create the root CA certificate
Don't forget to change the name and country!  The path limit is 2 to allow for intermediate CAs, followed by end user certificates.

'botan gen_self_signed cakey.pem "exampleco root CA" --country=AQ --organization="exampleco" --ca --path-limit=2 --days=7300 --key-pass=$(cat capassword.txt) > cacert.pem'

View the new cert's info:

'botan cert_info cacert.pem'

### Protect the new files
To avoid any "oops" situations while playing around with these commands, go ahead and protect your work:

`chmod 400 capassword.txt & chmod 400 cakey.pem & chmod 444 cacert.pem`

# Create the Intermediate CA
To follow good practices, let's make an intermediate CA.

### Create the intermediate's password
Similar to the above, make a random one:

`botan rng --format=base64 100 | cut -c ${RANDOM:0:2} > intpassword.txt`

Or simply:

`echo "battery horse stapler" > intpassword.txt`

### Create the intermediate's key
Once again, with a 384-bit ECDSA key:

`botan keygen --algo=ECDSA --params=secp384r1 --passphrase=$(cat intpassword.txt) > intkey.pem`

and test it:

`botan pkcs8 --pass-in=$(cat intpassword.txt) intkey.pem`

### Create the intermediate's request
Don't forget to change out the name and country!  Note the path length has been decreased to 1.

`botan gen_pkcs10 intkey.pem "exampleco intermediate CA" --country=AQ --organization="exampleco" --ca --path-limit=1 --key-pass=$(cat intpassword.txt) > intreq.pem`

### Sign the intermediate's request
Like the root, this one will live till 2040.

`botan sign_cert --ca-key-pass=$(cat capassword.txt) --duration=7300 cacert.pem cakey.pem intreq.pem > intcert.pem`

And view it:

`botan cert_info intcert.pem`

### Write-protect the intermediate's info
To avoid any "oops" situations while playing around with these commands, go ahead and protect your work:

`chmod 400 intpassword.txt && chmod 400 intkey.pem & chmod 444 intcert.pem`

# Make the site's certificate
Finally, exampleco can get its intranet site certificate.  Unlike the root and intermediate CAs, this one won't have a password.

### Create the site's key
`botan keygen --algo=ECDSA --params=secp384r1 > sitekey.pem`

### Create the site's request
Once again, don't forget to change out the values for your own.

`botan gen_pkcs10 sitekey.pem "intranet.exampleco.test" --country=AQ --organization="exampleco" --dns=intranet.exampleco.test > sitereq.pem`

### Sign the site's request
`botan sign_cert --ca-key-pass=$(cat intpassword.txt) --duration=366 intcert.pem intkey.pem sitereq.pem > sitecert.pem`

### Write protect the site's info
`chmod 400 sitekey.pem & chmod 444 sitecert.pem`

# Share your new CA info
You're on your own for attaching the site key & cert to your servers, but we can help you silence some warnings from applications in your test environment.  Note that these have to be done on every client machine and chaining the files is being skipped since some applications get angry if they're not listed in a particular order.

### Add the root and intermediate certificates to the system store
This will make other CLI applications like curl and wget happy.

`sudo mkdir -p /usr/local/share/ca-certificates/exampleco`

`sudo cp cacert.pem /usr/local/share/ca-certificates/exampleco/exampleco-root.crt`

`sudo cp intcert.pem /usr/local/share/ca-certificates/exampleco/exampleco-int.crt`

`sudo update-ca-certificates`

### Add the root and intermediate certificates to Firefox
There are some heavy enterprise methods and hotshot hack scripts to trust local certificates, but the best method for testing is still (as of version 66):

1. Open preferences (about:preferences)
2. Type `view certificates` in the preferences search bar
3. Click view certificates, then import, and find your files
