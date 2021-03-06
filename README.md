libscrypt
=========
Linux scrypt shared library.

Full credit to algorithm designer and example code from Colin Percival here:
http://www.tarsnap.com/scrypt.html

Utilises BSD licensed BASE64 encoder here:
http://code.google.com/p/stringencoders/

Official project page, including stable tarballs found here:
http://www.lolware.net/libscrypt.html

Simple hashing interface

A hash can be generated using the following function:

    int libscrypt_scrypt(char *dst, char *passphrase, uint32_t N, uint8_t r, uint8_t p)

Sane constants have been created for N, r and p so you can create a has like this:

    libscrypt_scrypt(outbuf, "My cats's breath smells like cat food", SCRYPT_N, SCRYPT_r, SCRYPT_p);

Output stored in "outbuf" is stored in a standardised MCF form, which means includes the randomly created, 128 bit salt, all N, r and p values, and a BASE64 encoded version of the hash. The entire MCF can be stored in a database, and compared for use as below:

    retval = scrypt_check(mcf, "pleasefailme");
    retval < 0 error
    retval = 0 password incorrect
    retval > 0 pass

mcf should be defined as at least SCRYPT_MCF_LEN in size.

A number of internal functions are exposed, and users wishing to create more complex use cases should consult the header file, which is aimed at documenting the API fully.

The test reference is also aimed at providing a well documented use case.
Building
--------
    make
    make check
Check the Makefile for advice on linking against your application.

BUGS
----
SCRYPT_* constants are probably a little high for something like a Raspberry pi. Using '1' as SCRYPT_p is acceptable from a security and performance standpoint if needed. 

Notes on Code Development
------------------------

Code is now declared "stable", the master branch will always be "stable" and development will be done on branches.
The reference machines are CentOS and Raspbian, and the code is expected to compile and run on all of these before being moved to stable branch.
Testing has also confirmed that libscrypt does compile and run on MacOS with minor Makefile edits.
Full transparancy on the regular application of thorough testing can be found by reviewing recent test harness results here:
http://www.lolware.net/libscrypttesting.txt
